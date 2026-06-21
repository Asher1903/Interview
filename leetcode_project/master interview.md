# 📋 LeetCode Recommender — Master Interview Prep Guide

> [!NOTE]
> This guide is structured in **Notion Style** (using callout alerts, code blocks, structured tables, and nested details). Use this master document to prepare for technical interviews, code walkthroughs, and system design discussions.

---

## 🛠️ 1. Technical Architecture & File Explanations

### Folder & Module Breakdown
The system follows a classic **Decoupled Client-Server (N-Tier) Architecture** with a clear separation of concerns between presentation, application logic, scoring logic, and data ingestion.

| Directory / File | Component Role | Detailed Mechanism |
| :--- | :--- | :--- |
| `api/main.py` | Application Bootstrapper | Configures CORS, initializes the FastAPI application, and loads the 3,900+ problem catalog from `data/problems_catalog.json` during the `lifespan` startup hook. |
| `api/routes.py` | API Controller Layer | Decouples incoming requests to separate routes. Handles request parameter resolution, initiates cache checks, triggers background DB tasks, and formats response models. |
| `api/user_pipeline.py` | ETL Coordinator | Implements the **Cache-Aside Pattern** via a local thread-safe dictionary with a 10-minute Time-To-Live (TTL). Coordinates the Extract, Transform, and Load steps. |
| `api/tracker.py` | DB Logging Client | Connects to Supabase using SQL clients and executes upsert logic on conflicts to log user performance stats. |
| `scraper/fetch_profile.py` | Ingestion Client | Construct custom GraphQL POST request queries, manages cookies, and utilizes an exponential backoff loop for network/rate-limiting resilience. |
| `scraper/tag_analyzer.py` | Metric Transformer | Runs relative max-scaling calculations to normalize user weakness scores from `0.0` (strongest) to `1.0` (weakest). |
| `model/weakness_scorer.py` | Problem Scorer | Loops over unsolved problems and assigns scores based on the mathematical average weakness of their tags. |
| `dashboard.py` | UI Presentation Client | Built in Streamlit. Connects to backend endpoints and plots data using Plotly. |

---

## ⚙️ 2. Core Data Flow & ETL Mechanics

### 1. Ingestion Phase (Extract)
When a cache miss occurs, the backend client sends a GraphQL request to LeetCode's endpoint:
```graphql
query userProblemsSolved($username: String!) {
  allQuestionsCount { difficulty count }
  matchedUser(username: $username) {
    submitStats { acSubmissionNum { difficulty count } }
  }
}
```
If the user cookie is set, it also retrieves their solved submissions list:
```graphql
query recentAcSubmissions($username: String!, $limit: Int!) {
  recentAcSubmissionList(username: $username, limit: $limit) {
    titleSlug
  }
}
```

### 2. Analytical Transformation Phase (Transform)
The solved questions are parsed into unique title slugs. The system maps slugs to topic tags using the global catalog database.
For each topic tag, the weakness score is calculated using relative max-scaling:

$$\text{Strength Score}_{\text{tag}} = \frac{\text{Solved Count}_{\text{tag}}}{\max(\text{Solved Counts})}$$

$$\text{Weakness Score}_{\text{tag}} = 1.0 - \text{Strength Score}_{\text{tag}}$$

Problems are scored based on tag overlaps:

$$\text{Problem Score} = \frac{\sum_{i=1}^{N} \text{Weakness Score}_{\text{tag}_i}}{N}$$

### 3. Storage & Response Phase (Load)
The transformation output is loaded into the in-memory cache and returned to the dashboard. An async background task is spawned to write the user's updated stats to the Supabase database.

---

## 💬 3. Top 50 Interview Questions & Cross-Questions

### 🏗️ Category 1: System Architecture & Design Patterns (Q1 - Q8)

#### Q1: What is the overall architectural pattern of your application?
* **Answer**: It is a decoupled, client-server N-tier architecture. The client presentation layer (Streamlit Cloud) is completely isolated from the backend application services layer (FastAPI on Render). The backend coordinates calculations using an ETL data pipeline and communicates with a database storage tier (Supabase PostgreSQL).
* **Interviewer Cross-Question**: Why not make it a monolith and run the backend inside the Streamlit app?
* **Counter-Answer**: Streamlit is designed for presentation. Mixing data scraping, caching, and model ranking logic inside the Streamlit run loop leads to poor responsiveness because Streamlit runs the entire script on every user interaction. Separating the backend into a FastAPI service allows independent scaling, API reusability, and professional lifecycle management.

#### Q2: How did you implement the Cache-Aside pattern?
* **Answer**: The backend checks if the request's username exists in our local dictionary cache. If it hits and is under 10 minutes old, the cached recommendation data is returned instantly. On a cache miss, the pipeline pulls fresh data from LeetCode, saves it to the cache with a timestamp, and returns the response.
* **Interviewer Cross-Question**: What happens if the cache fills up? How do you prevent out-of-memory (OOM) issues?
* **Counter-Answer**: Currently, it is a basic dictionary. To prevent OOM errors in production, I would use an eviction policy like **LRU (Least Recently Used)**. In Python, this can be implemented using `collections.OrderedDict` or by migrating the cache to Redis, which manages memory eviction automatically.

#### Q3: Why did you choose FastAPI over Flask or Django?
* **Answer**: FastAPI is built on ASGI (Asynchronous Server Gateway Interface), making it significantly faster than WSGI frameworks like Flask or Django for I/O-bound tasks. It also features native support for `async/await` syntax, auto-generates interactive Swagger documentation using Pydantic schemas, and handles concurrent connections efficiently.
* **Interviewer Cross-Question**: Django has async capabilities now. Why not use it to get built-in admin panels?
* **Counter-Answer**: Django is a "batteries-included" framework designed for large database-driven web apps. For a lightweight recommendation microservice that operates primarily on external APIs and in-memory scoring, Django introduces unnecessary bloat and overhead. FastAPI's minimal footprint was the right fit.

#### Q4: How is configuration managed across different deployment environments?
* **Answer**: I consolidated all configuration logic inside a single `config.py` module. It uses `python-dotenv` to load configurations from a local `.env` file during development, falling back to OS-level environment variables in production (such as Render or Streamlit Cloud).
* **Interviewer Cross-Question**: Why is importing config variables directly into individual routes a bad practice?
* **Counter-Answer**: Importing `.env` directly in multiple files makes it harder to audit settings, write unit tests, and change mock configurations. Centralizing config variables inside `config.py` acts as a single source of truth.

#### Q5: What is the purpose of the FastAPI lifespan event handler in `main.py`?
* **Answer**: The lifespan handler manages code that runs once during application startup and shutdown. In our case, it pre-loads the static global problem catalog into memory from a local JSON file at boot.
* **Interviewer Cross-Question**: What if the JSON file is corrupted or missing during startup?
* **Counter-Answer**: The lifespan handler catches file errors. If the file is missing or corrupted, it triggers a fallback query to download a fresh catalog directly from LeetCode and caches it locally, preventing boot crashes.

#### Q6: How does the dashboard interact with the backend API?
* **Answer**: Streamlit acts as a thin HTTP client. It sends GET requests to the FastAPI endpoints (`/recommend`, `/stats`) using `requests` or `httpx` and parses the returned JSON payload to render visual charts.
* **Interviewer Cross-Question**: What happens to the frontend UI if the backend API is slow or temporarily down?
* **Counter-Answer**: If the API call fails or times out, the dashboard catches the exception, displays a friendly warning banner, and prevents the app from crashing.

#### Q7: Why did you decouple the scraper logic into a separate package (`scraper/`)?
* **Answer**: It follows the **Single Responsibility Principle**. The scraping layer handles raw HTTP requests and data retrieval, while the pipeline and models focus on calculations. This allows changes to LeetCode's GraphQL schemas to be updated in the scraper without affecting the recommendation logic.
* **Interviewer Cross-Question**: If LeetCode changes their API format, how does your scraper handle it?
* **Counter-Answer**: The parser functions isolate the raw JSON extraction. If LeetCode changes a field key, I only need to update the mappings in `scraper/parse_submissions.py`.

#### Q8: How would you structure this application if you wanted to implement a microservices architecture?
* **Answer**: I would split the system into three services: a **Scraping Service** (fetching profile data), a **Recommendation Service** (holding the catalog in memory and ranking problems), and a **Dashboard Service** (frontend). They would communicate via REST APIs or message queues.
* **Interviewer Cross-Question**: Wouldn't that increase latency?
* **Counter-Answer**: Yes, network hops introduce some latency, but it allows you to scale the scraping service independently to handle rate limits while keeping the recommender service active.

---

### 📥 Category 2: Data Engineering & Scraping (Q9 - Q16)

#### Q9: How does your scraper query LeetCode?
* **Answer**: We send HTTP POST requests to `https://leetcode.com/graphql` with structured GraphQL queries to retrieve submission counts, matched user details, and recent submissions.
* **Interviewer Cross-Question**: Why use GraphQL instead of scraping their raw HTML pages?
* **Counter-Answer**: GraphQL returns structured, lightweight JSON data directly from their database. Parsing raw HTML is brittle, slow, and easily breaks when class names or styling rules change.

#### Q10: How do you bypass LeetCode's scraping protections?
* **Answer**: We supply browser-mimicking headers, including custom `User-Agent` strings and the `Referer` header set to `https://leetcode.com/`. We also pass the `LEETCODE_SESSION` cookie for authenticated queries.
* **Interviewer Cross-Question**: What happens if LeetCode still blocks the server's IP address?
* **Counter-Answer**: In production, I would configure a proxy rotator (like ScrapingBee or Bright Data) to distribute queries across a pool of IP addresses, preventing single-IP bans.

#### Q11: Explain your exponential backoff retry policy.
* **Answer**: When our GraphQL client encounters a transient connection error or an HTTP 429 rate limit, it catches the error and retries the request after an increasing delay:
  
  $$\text{Wait Time} = \text{Backoff Factor}^{\text{attempt number}}$$

* **Interviewer Cross-Question**: Why not use a fixed sleep interval, like always waiting 2 seconds?
* **Counter-Answer**: A fixed wait can worsen rate-limiting issues if the server is overloaded. Exponential backoff reduces query frequency dynamically, giving the target server time to recover.

#### Q12: Why does the API pre-fetch the global catalog at startup?
* **Answer**: The catalog contains over 3,900 problems. Fetching this volume of data from LeetCode's API in real time takes 30+ seconds. Pre-fetching and storing it locally allows us to serve recommendations instantly.
* **Interviewer Cross-Question**: How do you keep this local catalog up to date with newly added problems?
* **Counter-Answer**: I created a secure admin endpoint (`POST /admin/refresh-catalog`). A cron job can call this endpoint weekly to rebuild and cache the JSON catalog in the background without downtime.

#### Q13: How do you convert LeetCode's raw submissions JSON into unique solved problem lists?
* **Answer**: `scraper/parse_submissions.py` filters raw submissions to keep only successfully accepted ones (`status == "Accepted"`), extracts the unique problem slugs, and maps them to a set to remove duplicates.
* **Interviewer Cross-Question**: What happens if the submissions list only returns the last 20 submissions?
* **Counter-Answer**: LeetCode's public API limits recent submissions. To improve accuracy, we also query the user's solved question slugs directly from their profile statistics.

#### Q14: How does your code handle non-ASCII characters in console logging?
* **Answer**: Windows terminal environments can throw encoding crashes when printing unicode symbols. I replaced all console-logging symbols (like checkmarks or warnings) with standard ASCII equivalents.
* **Interviewer Cross-Question**: Isn't it better to just configure the terminal encoding in Python?
* **Counter-Answer**: While setting `sys.stdout` encoding works locally, it is less portable. Using standard ASCII characters ensures the code runs reliably across all terminal environments.

#### Q15: What is the benefit of using `httpx` instead of Python's standard `requests` library?
* **Answer**: `httpx` natively supports asynchronous requests, allowing us to make concurrent, non-blocking calls to external APIs.
* **Interviewer Cross-Question**: Are you using `httpx` asynchronously in your current code?
* **Counter-Answer**: The current client uses synchronous wrappers for simplicity, but the code is structured to transition to `async httpx.AsyncClient` easily.

#### Q16: How do you handle missing values or unexpected fields in the GraphQL response?
* **Answer**: We use Python's dictionary `.get()` method with default values (e.g., `data.get("matchedUser") or {}`) to handle missing data without throwing `KeyError` exceptions.
* **Interviewer Cross-Question**: Why not use Pydantic to validate the raw LeetCode responses?
* **Counter-Answer**: LeetCode's internal API schemas can change. Pydantic validation on raw external data would cause our app to crash if they added or renamed a field. Using loose parsing for ingestion and Pydantic for our own API responses offers better resilience.

---

### 🧮 Category 3: Mathematical Modeling & Algorithmic Design (Q17 - Q25)

#### Q17: Explain the math behind your tag weakness score.
* **Answer**: It is a relative strength calculation. First, we find the maximum number of problems solved in any single tag. We then calculate the score for all other tags relative to this max value:
  
  $$\text{Weakness Score}_{\text{tag}} = 1.0 - \frac{\text{Solved Count}_{\text{tag}}}{\max(\text{Solved Counts})}$$

* **Interviewer Cross-Question**: What is the drawback of this relative calculation?
* **Counter-Answer**: If a user has only solved 2 Array problems (max) and 1 String problem, their String weakness is calculated as `0.5`, which looks high even though they have barely solved any problems. The model works best for users with a more established history.

#### Q18: Why do unsolved tags receive a default weakness score of `1.0`?
* **Answer**: If a user has never solved a problem in a topic, it represents an absolute skill gap. Assigning `1.0` ensures the recommender prioritizes these topics.
* **Interviewer Cross-Question**: If there are many untouched tags, how does the system choose between them?
* **Counter-Answer**: It prioritizes them based on tag popularity in the global catalog, ensuring the user is directed to more fundamental topics (like Arrays or Trees) before niche ones.

#### Q19: How does the system score an unsolved problem with multiple tags?
* **Answer**: The problem score is the mathematical average of the weakness scores of its tags:
  
  $$\text{Problem Score} = \frac{\sum \text{Weakness Score}_{\text{tags}}}{n}$$

* **Interviewer Cross-Question**: If a problem has one weak tag (1.0) and one strong tag (0.0), its score is 0.5. How does this affect recommendation accuracy?
* **Counter-Answer**: Averaging tag scores balances topic difficulty. If you are strong in Arrays but weak in Dynamic Programming, an Array-DP problem is scored as a moderate challenge, which is a good candidate for progressive practice.

#### Q20: How do you filter out solved problems from the recommendations queue?
* **Answer**: We convert the user's solved question titles list into a hash set. During problem evaluation, we check if the candidate problem's title slug is in this set, which runs in $O(1)$ time.
* **Interviewer Cross-Question**: What is the space complexity of keeping this solved set in memory?
* **Counter-Answer**: Even for advanced users with 1,500 solved problems, the set size is under 100 KB, which has a negligible memory footprint.

#### Q21: What is the time complexity of your ranking algorithm?
* **Answer**: Let $N$ be the number of unsolved problems in the catalog (approx 3,900) and $T$ be the average number of tags per problem (approx 3). Scoring all candidate problems takes $O(N \times T)$ operations. Sorting the results takes $O(N \log N)$ time. The overall time complexity is:
  
  $$O(N \times T + N \log N)$$

* **Interviewer Cross-Question**: How would you optimize this if the catalog grew to 100,000 problems?
* **Counter-Answer**: I would use a **Priority Queue (Min-Heap)** to keep only the top $K$ recommendations. This reduces the sorting complexity from $O(N \log N)$ to $O(N \log K)$.

#### Q22: Why does the scorer assign a default score of `0.5` to problems without tags?
* **Answer**: Problems without tags represent unknown content. Assigning `0.5` places them in the middle of the recommendations queue, ensuring they don't block known weak areas or get buried at the bottom.
* **Interviewer Cross-Question**: Why not ignore untagged problems completely?
* **Counter-Answer**: Many newly added LeetCode questions do not have tags immediately. Including them with a neutral score ensures users still have access to new content.

#### Q23: How would you modify the scoring algorithm to support spaced repetition?
* **Answer**: I would store a `last_solved` timestamp for each tag in the database. I could then scale the tag weakness score based on how long it has been since the user practiced that topic:
  
  $$\text{Score}_{\text{final}} = \text{Weakness Score} \times (1 + \text{Days Since Last Solved} \times \lambda)$$

* **Interviewer Cross-Question**: How would you define the decay parameter $\lambda$?
* **Counter-Answer**: $\lambda$ would be adjusted based on standard forgetting curves (like SuperMemo SM-2), increasing the weakness score faster for topics the user hasn't practiced in over 14 days.

#### Q24: What are the drawbacks of using tag averages for scoring?
* **Answer**: Averaging can dilute the impact of critical weaknesses. If a problem has one tag with a weakness of `1.0` and three tags with `0.0` (strong), the average is `0.25`, placing it low in the queue.
* **Interviewer Cross-Question**: How would you fix this dilution issue?
* **Counter-Answer**: I could use a weighted average or a **max-pooling** strategy, where the problem's score is biased towards its weakest tag:
  
  $$\text{Problem Score} = \alpha \times \max(\text{Weaknesses}) + (1-\alpha) \times \text{Average}(\text{Weaknesses})$$

#### Q25: How would you recommend problems based on target company search tags?
* **Answer**: I would scrape or import company tag data (e.g., "Google", "Amazon") and add a filtering layer to the engine:
  
  $$\text{Problem Score} = \text{Weakness Score} \times \text{Company Weight}$$

* **Interviewer Cross-Question**: Where would you get this company tagging data?
* **Counter-Answer**: I could query LeetCode's company tags database via GraphQL or import it from community-curated datasets.

---

### ⚡ Category 4: Performance Optimization & Caching (Q26 - Q33)

#### Q26: Why did you choose an in-memory TTL cache over hitting the DB directly?
* **Answer**: Hitting the database on every request introduces network latency. In-memory caching stores data directly in the application's RAM, enabling **sub-millisecond lookups** and reducing DB load.
* **Interviewer Cross-Question**: What is the main drawback of an in-memory cache?
* **Counter-Answer**: If the server restarts or crashes, the cache is lost. It also doesn't scale well horizontally across multiple server instances.

#### Q27: How would you transition your cache to Redis?
* **Answer**: I would replace the `_cache` dictionary with a Redis client wrapper using `redis-py`:
  ```python
  import redis
  r = redis.Redis(host='localhost', port=6379)
  # Set with 10-minute expiry
  r.setex(username, 600, json.dumps(result))
  ```
* **Interviewer Cross-Question**: How does this improve horizontal scaling?
* **Counter-Answer**: Redis runs as a separate service. Multiple backend containers can query the same Redis cache instance, ensuring consistent data across all instances.

#### Q28: Explain the Cache-Aside pattern workflow in your codebase.
* **Answer**: The application first checks the cache. On a hit, it returns the cached data. On a miss, it fetches fresh data from LeetCode, updates the cache, and returns the response.
* **Interviewer Cross-Question**: What happens if two concurrent requests miss the cache for the same user at the same time?
* **Counter-Answer**: This is a **cache stampede**. Both requests will query LeetCode simultaneously. To prevent this, I would implement a lock (e.g., using `asyncio.Lock` or Redis distributed locks) to ensure only one worker scrapes the data while the other waits for the cache to update.

#### Q29: Why is loading the catalog from local JSON during startup faster than fetching it dynamically?
* **Answer**: Reading a local JSON file requires only disk I/O, which takes **< 10ms**. Fetching 3,900+ problems from LeetCode requires network calls and API parsing, which takes **30+ seconds**.
* **Interviewer Cross-Question**: How does this impact container deployment?
* **Counter-Answer**: Faster startup times allow container platforms (like Docker, Kubernetes, or Render) to boot, scale, and recover from failures quickly.

#### Q30: How does your code handle memory cleanup for expired cache keys?
* **Answer**: The current cache checks expiration lazily when a key is queried.
* **Interviewer Cross-Question**: What happens to keys that are never queried again? Do they leak memory?
* **Counter-Answer**: Yes, they remain in memory. To fix this, I would implement an active cleanup worker (such as an async background loop that clears expired keys hourly) or migrate to Redis, which handles expiration automatically.

#### Q31: Why did you choose 10 minutes as the cache TTL?
* **Answer**: Ten minutes balances profile freshness with server performance. A user's LeetCode solved counts don't change second-by-second, so a 10-minute TTL is sufficient.
* **Interviewer Cross-Question**: If a user finishes a problem and wants to see an immediate update, does the cache block them?
* **Counter-Answer**: We provided a `POST /update` route (triggered by the "Refresh" button in the UI) that purges the cache key for that username, forcing an immediate fetch.

#### Q32: How does Pydantic improve serialization performance in FastAPI?
* **Answer**: Pydantic v2 is written in Rust, making data validation and JSON serialization significantly faster than Python-based alternatives.
* **Interviewer Cross-Question**: How does Pydantic compare to standard Python `json.dumps`?
* **Counter-Answer**: Pydantic validates and coerces data types while serializing, ensuring data safety in addition to speed.

#### Q33: How would you handle a spike of 10,000 concurrent requests to your backend?
* **Answer**: I would run Uvicorn with multiple worker processes, configure a load balancer to distribute requests, and use Redis to serve cached queries without hitting LeetCode or our database.
* **Interviewer Cross-Question**: What is the bottleneck in this scenario?
* **Counter-Answer**: The bottleneck is the cache lookup speed. Offloading the cache to Redis ensures the app servers only perform CPU-bound rendering tasks.

---

### 🔒 Category 5: Database, Security & Reliability (Q34 - Q42)

#### Q34: What is Supabase and why did you choose it over local PostgreSQL?
* **Answer**: Supabase is an open-source Firebase alternative built on PostgreSQL. It provides hosted database services, RESTful APIs, and built-in user authentication out of the box, reducing setup overhead.
* **Interviewer Cross-Question**: What is the performance impact of using a hosted database?
* **Counter-Answer**: It introduces network latency. We mitigated this by running database logging in the background asynchronously.

#### Q35: Why did you disable Row Level Security (RLS) on your `users` table?
* **Answer**: RLS was disabled because our application currently runs in a public, single-user mode. Any user can type a username to view stats, requiring anonymous backend writes without session tokens.
* **Interviewer Cross-Question**: Is it safe to leave RLS disabled in production?
* **Counter-Answer**: It is acceptable for public stats, but not for sensitive user profiles. In a production app, I would enable RLS and require JWT session validation.

#### Q36: How does the admin route verify the `X-Admin-Secret` header?
* **Answer**: We use FastAPI's dependency injection to read the `X-Admin-Secret` header and verify it against our environment variable:
  ```python
  def verify_secret(x_admin_secret: str = Header(None)):
      if x_admin_secret != EXPECTED_SECRET:
          raise HTTPException(403)
  ```
* **Interviewer Cross-Question**: Why use a custom header instead of a standard OAuth Bearer token?
* **Counter-Answer**: Custom headers are simple and secure for backend-to-backend admin tasks, avoiding the overhead of setting up full OAuth scopes.

#### Q37: How do you prevent SQL injection attacks in your database integration?
* **Answer**: We use the Supabase PostgREST client library, which uses parameterized queries internally to prevent SQL injection.
* **Interviewer Cross-Question**: What if you had to write raw SQL?
* **Counter-Answer**: I would use parameterized query inputs (like `%s` in `psycopg2`) to ensure inputs are escaped safely, rather than using string formatting.

#### Q38: What happens if your Supabase database goes down?
* **Answer**: The database write fails, but the error is caught inside the background task. The user still receives their recommendation payload.
* **Interviewer Cross-Question**: How do you monitor these background failures?
* **Counter-Answer**: We log database errors to the console or an error-tracking service like Sentry to alert developers.

#### Q39: How do you protect user data privacy?
* **Answer**: We only store public LeetCode statistics and submission counts. We do not store email addresses, passwords, or personal details in our database.
* **Interviewer Cross-Question**: What if a user wants their profile deleted?
* **Counter-Answer**: Since we only store public stats, we can delete the row from the `users` table via the admin dashboard or by exposing a delete endpoint.

#### Q40: How does your database schema handle schema migrations?
* **Answer**: We use a `database_schema.sql` file containing our table definitions and policies.
* **Interviewer Cross-Question**: How would you handle migrations in a team environment?
* **Counter-Answer**: I would use a migration tool like **Alembic** or **dbmate** to version migrations and apply them systematically across staging and production.

#### Q41: Why did you set `SUPABASE_SERVICE_KEY` in your backend?
* **Answer**: The service key has administrative privileges and bypasses RLS, allowing the backend to perform logging tasks.
* **Interviewer Cross-Question**: Why shouldn't this key be used in client-side code?
* **Counter-Answer**: Exposing the service key allows anyone to read or modify your database. Client-side apps should only use the `anon` key.

#### Q42: How does the API validate incoming payloads?
* **Answer**: We use Pydantic models to validate the structure and data types of incoming requests and responses.
* **Interviewer Cross-Question**: What happens if the payload fails validation?
* **Counter-Answer**: FastAPI catches the validation error and automatically returns an HTTP 422 Unprocessable Entity response.

---

### 🌐 Category 6: Deployment, Scaling & Production Engineering (Q43 - Q50)

#### Q43: How do you run the Streamlit dashboard on Streamlit Community Cloud?
* **Answer**: Streamlit Cloud connects directly to our GitHub repository. We select the repository, set the branch to `main`, point the entry path to `dashboard.py`, and configure our environment variables in the App Secrets.
* **Interviewer Cross-Question**: What happens to the Streamlit app if the GitHub repository is updated?
* **Counter-Answer**: Streamlit Cloud automatically pulls the latest changes and redeploys the app, enabling continuous integration.

#### Q44: Why did you host the API on Render instead of Streamlit Cloud?
* **Answer**: Streamlit Cloud is designed specifically for rendering Streamlit dashboards. It cannot host general FastAPI backend microservices.
* **Interviewer Cross-Question**: Why not use Docker to run both in a single container?
* **Counter-Answer**: Hosting them separately allows us to scale the backend API independently and keeps the dashboard running on Streamlit's free hosting tier.

#### Q45: How does keeping only the API awake on Render save free monthly hours?
* **Answer**: Render's free plan allows 750 free hours across your account. Running two web services uses 1,440 hours, running out by mid-month. Moving the dashboard to Streamlit Cloud leaves only one service on Render, staying within the free limit.
* **Interviewer Cross-Question**: If the Render API goes to sleep, how does the Streamlit app wake it up?
* **Counter-Answer**: The Streamlit app sends an API request, which triggers the Render container to wake up (cold start).

#### Q46: How would you package this application using Docker?
* **Answer**: I would create a multi-stage `Dockerfile` to install dependencies and run the Uvicorn server:
  ```dockerfile
  FROM python:3.11-slim
  WORKDIR /app
  COPY requirements.txt .
  RUN pip install -r requirements.txt
  COPY . .
  CMD ["uvicorn", "api.main:app", "--host", "0.0.0.0", "--port", "8000"]
  ```
* **Interviewer Cross-Question**: Why use a `-slim` image?
* **Counter-Answer**: Slim images have a smaller footprint (approx 150MB vs 900MB), reducing deployment time and disk usage.

#### Q47: How would you monitor application performance (APM) in production?
* **Answer**: I would integrate a tool like **Prometheus** to collect metrics and **Grafana** to visualize CPU, memory, request rates, and error frequencies.
* **Interviewer Cross-Question**: How would you track query latency?
* **Counter-Answer**: I would use middleware in FastAPI to measure and log request processing times.

#### Q48: How would you design a CI/CD pipeline for this project?
* **Answer**: I would use GitHub Actions to run tests and linters (like `flake8` or `black`) on push, and then trigger a deployment webhook on Render.
* **Interviewer Cross-Question**: How would you handle database migrations in CI/CD?
* **Counter-Answer**: I would run migrations as a pre-deploy step to ensure the database schema is updated before the new code goes live.

#### Q49: What is the benefit of pinning dependencies in `requirements.txt`?
* **Answer**: Pinning versions prevents breaking changes when dependencies update, ensuring consistent builds across local and production environments.
* **Interviewer Cross-Question**: How do you monitor pinned dependencies for security vulnerabilities?
* **Counter-Answer**: I would configure GitHub **Dependabot** to scan dependencies and suggest updates for known vulnerabilities.

#### Q50: How would you scale this system to support 1 million active users?
* **Answer**: I would containerize the API with Docker, deploy it to a managed Kubernetes cluster with auto-scaling, set up a Redis cluster for shared caching, and use a Postgres replica pool to handle read requests.
* **Interviewer Cross-Question**: What is the most expensive part of this scaling setup?
* **Counter-Answer**: The scraping operations and LeetCode API limits. I would cache results aggressively and implement a queue system to manage scraping requests.
