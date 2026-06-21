# 📋 Simple Master Interview Prep Guide

> [!NOTE]
> This guide is structured in **Notion Style** with clean headings, bold summaries, callout boxes, and simple real-world analogies. It contains a complete walkthrough of the project and 50 questions with their expected follow-ups.

---

## 💡 Part 1: Explaining the Project in Plain English

### The 30-Second "Elevator Pitch"
> "I built an **intelligent practice assistant for LeetCode** that helps developers study smarter. 
> 
> Most people 'grind' LeetCode blindly by solving random problems or repeating topics they are already good at. 
> 
> My app analyzes a user's profile, calculates a **Weakness Score** for every coding topic (like Binary Search or Dynamic Programming) based on what they've practiced, and automatically recommends the best unsolved problems targeting their absolute biggest skill gaps. 
> 
> I built the frontend with **Streamlit**, the backend API with **FastAPI**, and integrated **Supabase** for database logging. To make it production-ready, I built a fast **local cache** that speed up startup from 30 seconds to under 1 second, and added an **asynchronous background logging thread** so database writes never slow down the user dashboard."

### The Analogy: The Automated Fitness Trainer
If an interviewer asks how it works, use this analogy:
* **The Traditional Way**: Imagine going to the gym and doing bicep curls every day because you like them and they are easy. Your legs remain weak.
* **Our App**: It acts as an automated trainer. It looks at your workout log (your LeetCode submissions), sees you have done 50 arm exercises but 0 leg exercises, calculates your leg weakness as $100\%$, searches the gym's database of 3,900 exercises (unsolved problems), and hands you a leg exercise card.

---

## 🔍 Part 2: How & Why Every Component Works

Here is a breakdown of the codebase, explained simply:

```
problem-recommender-system/
├── api/
│   ├── main.py              # The Host (Starts the server, loads the problems list)
│   ├── routes.py            # The Gatekeeper (Directs requests to the correct functions)
│   ├── tracker.py           # The DB Writer (Saves search history to Supabase)
│   └── user_pipeline.py     # The Manager (Checks the cache and coordinates the workflow)
│
├── scraper/
│   ├── fetch_profile.py     # The Messenger (Asks LeetCode for profile details)
│   ├── parse_submissions.py # The Filter (Extracts list of solved questions)
│   └── tag_analyzer.py      # The Calculator (Computes weakness scores)
│
└── model/
    ├── weakness_scorer.py   # The Grader (Grades unsolved questions based on weakness)
    └── problem_ranker.py    # The Sorter (Filters out solved questions and sorts the rest)
```

### 1. In-Memory Cache (The Notepad)
* **What it is**: A dictionary (`_cache = {}`) inside the server's memory.
* **Why it is there**: Scraping LeetCode takes 3–5 seconds. If a user refreshes the page or searches the same name twice, we don't want to make them wait again. We store their results for 10 minutes. If they request it again, we serve it in **under 10 milliseconds**.

### 2. Startup Catalog Caching (The Pre-Loaded List)
* **What it is**: Reading the list of all 3,900+ LeetCode problems from a local file (`data/problems_catalog.json`) when the server turns on.
* **Why it is there**: If we downloaded the list from LeetCode every time the server rebooted, it would take **30 seconds** for the server to start. By reading it from a local file, the server starts up in **less than 1 second**.

### 3. Background logging (The Async Mailman)
* **What it is**: Offloading database writes to a background worker thread using FastAPI's `BackgroundTasks`.
* **Why it is there**: Connecting to Supabase and writing user logs takes 200–500ms. If we do this synchronously, the user has to wait at a loading screen. Instead, we hand the data to our "Async Mailman" to write in the background, and immediately return the recommendations to the user.

### 4. Exponential Backoff Retries (The Patient Caller)
* **What it is**: If LeetCode blocks our request (HTTP 429), we wait 1 second and try again. If it fails again, we wait 2 seconds, then 4 seconds.
* **Why it is there**: It prevents our app from crashing due to minor network issues or LeetCode rate limits.

---

## 📊 Part 3: The Mathematical Model (Keep it Simple)

### 1. Tag Weakness Scoring
We calculate how weak you are in a topic by comparing it to your most practiced topic:

$$\text{Weakness} = 1.0 - \left( \frac{\text{Solved Count in this Topic}}{\text{Solved Count in your #1 Most Practiced Topic}} \right)$$

* **Example**: You have solved **10 Array** problems (your maximum) and **2 Hash Table** problems.
  * Your Array weakness: $1.0 - (10/10) = 0.0$ (No weakness, you are strong here).
  * Your Hash Table weakness: $1.0 - (2/10) = 0.8$ (High weakness, you need practice).
  * Any topic you have solved **0** problems in gets a default score of **1.0** (Maximum weakness).

### 2. Problem Weakness Scoring
To score an unsolved problem, we look at its topics and average your weakness scores:

$$\text{Problem Score} = \text{Average Weakness of the Problem's Topics}$$

* **Example**: An unsolved problem has tags `["Array", "Hash Table"]`.
  * Your Array weakness is `0.0`, and your Hash Table weakness is `0.8`.
  * The problem score is: $(0.0 + 0.8) / 2 = 0.4$.
  * If another problem has tags `["Hash Table", "Dynamic Programming"]` (where Dynamic Programming weakness is `1.0`):
    * The problem score is: $(0.8 + 1.0) / 2 = 0.9$.
  * The system will recommend the second problem (score `0.9`) because it targets your weaknesses much better.

---

## 💬 Part 4: Top 50 Interview Questions & Cross-Questions

### 🏗️ Topic A: Architecture & Design Decisions (Q1 - Q10)

#### Q1: Tell me about this project.
* **Answer**: I built an intelligent LeetCode Recommender System. It parses a developer's LeetCode profile, determines their weakest topics, and ranks unsolved problems in real-time to recommend the best questions for targeted practice. It uses FastAPI for the backend, Streamlit for the frontend, and Supabase for database logging.
* **Interviewer Cross-Question**: Why not use a standard database instead of scraping LeetCode directly?
* **Counter-Answer**: Users want up-to-date recommendations. If we only read from our database, we wouldn't know if a user solved new problems recently. Scraping LeetCode dynamically guarantees the recommendations are accurate to their latest activity.

#### Q2: Why did you decouple the frontend and the backend?
* **Answer**: It separates the visual layout from the computation. The Streamlit dashboard is just a presentation client, while the FastAPI backend handles the pipeline, caching, and ranking.
* **Interviewer Cross-Question**: What is the benefit during team development?
* **Counter-Answer**: It allows backend developers to optimize scoring algorithms or switch databases without changing any frontend UI code, and vice versa. It also lets us deploy them to different cloud servers.

#### Q3: Why did you choose Streamlit for the user interface?
* **Answer**: Streamlit allows Python developers to build interactive, responsive user interfaces quickly without writing HTML, CSS, or JavaScript.
* **Interviewer Cross-Question**: Streamlit re-runs the entire script from top to bottom on every click. Doesn't that hurt performance?
* **Counter-Answer**: Yes, that is a known behavior of Streamlit. We resolved this by moving all heavy data fetching and ranking logic to our FastAPI backend API and caching the results. Streamlit only renders the final data, keeping it fast and lightweight.

#### Q4: Why did you choose FastAPI for the backend?
* **Answer**: FastAPI is built on ASGI, making it extremely fast. It natively supports asynchronous operations and automatically generates interactive API documentation.
* **Interviewer Cross-Question**: Flask is also lightweight. Why not Flask?
* **Counter-Answer**: Flask is historically synchronous. If a route waits for a slow scraper, it blocks the thread. FastAPI handles I/O-bound tasks asynchronously, letting it handle many concurrent users much better.

#### Q5: What is the purpose of the `config.py` file?
* **Answer**: It is a single source of truth for reading configuration settings. It loads credentials from `.env` locally or environment variables in production.
* **Interviewer Cross-Question**: Why not just use `os.getenv` directly in your code files?
* **Counter-Answer**: If you use `os.getenv` in multiple files, it becomes difficult to track environment dependencies. If a variable name changes, you have to find and replace it everywhere. `config.py` isolates this logic.

#### Q6: How does the application start up quickly?
* **Answer**: During startup, the FastAPI lifespan event handler loads all 3,900+ LeetCode problems from a local JSON file (`data/problems_catalog.json`) into the application state.
* **Interviewer Cross-Question**: How did you get that JSON file in the first place?
* **Counter-Answer**: I wrote a scraper script that fetches the entire problem set from LeetCode, parses it, and saves it. We load this cached file at startup so we don't have to query LeetCode on boot.

#### Q7: What is the purpose of Pydantic in this project?
* **Answer**: We use Pydantic models to validate the structure of the data sent to and from our API endpoints.
* **Interviewer Cross-Question**: What happens if the data structure changes?
* **Counter-Answer**: Pydantic catches the data mismatch and returns a clear validation error, preventing corrupt data from passing through the API.

#### Q8: How would you design a notifications feature for this app?
* **Answer**: I would set up a worker process (like Celery) to check user stats daily. If a user hasn't solved a recommended problem in 3 days, it would send an email reminder.
* **Interviewer Cross-Question**: Would you run this worker inside your main API process?
* **Counter-Answer**: No, email sending and checking schedules are background operations. Running them inside the main API could block user requests. They should run in a separate background container.

#### Q9: How does the `/admin/refresh-catalog` route work?
* **Answer**: It is a POST route protected by an admin header that starts a background task to scrape LeetCode for new problems, updates the local JSON file, and refreshes the application state.
* **Interviewer Cross-Question**: Why is it a POST request and not a GET request?
* **Counter-Answer**: GET requests should only fetch data. POST requests are designed for operations that modify server state or run actions (like updating files).

#### Q10: What would happen if we run multiple instances of the backend API?
* **Answer**: The application will scale horizontally. However, since the user cache is currently in-memory, requests from the same user hitting different servers would result in cache misses.
* **Interviewer Cross-Question**: How would you fix that cache miss issue?
* **Counter-Answer**: I would replace the in-memory cache with a shared **Redis** instance, so all servers read from and write to the same central cache.

---

### 📥 Topic B: Data Engineering & Ingestion (Q11 - Q20)

#### Q11: How does the scraper retrieve data from LeetCode?
* **Answer**: We send HTTP POST requests to LeetCode's GraphQL endpoint (`https://leetcode.com/graphql`) containing custom query strings to request profile statistics and submissions.
* **Interviewer Cross-Question**: What is the difference between GraphQL and REST APIs?
* **Counter-Answer**: REST APIs return a fixed payload of data. GraphQL allows us to request only the specific fields we need (like solved counts and title slugs), reducing payload size and network latency.

#### Q12: Why did you add custom headers to your scraper client?
* **Answer**: LeetCode blocks automated bot requests. We add browser headers (like `User-Agent`) to make our requests look like they are coming from a normal web browser.
* **Interviewer Cross-Question**: Is this scraping setup completely foolproof?
* **Counter-Answer**: No. If the app receives high traffic, LeetCode might block the server's IP. In production, I would use a rotating proxy pool to distribute requests across multiple IPs.

#### Q13: What happens if LeetCode is temporarily down?
* **Answer**: The scraper attempts to retry the request. If it still fails, the API returns a clean HTTP 502 Bad Gateway response explaining that LeetCode is unreachable.
* **Interviewer Cross-Question**: How do you prevent the frontend from crashing in this case?
* **Counter-Answer**: The Streamlit frontend checks the status code of the response. If it is 502, it displays a warnings block in the UI instead of crashing.

#### Q14: How does the exponential backoff retry logic work?
* **Answer**: If a query fails due to a network glitch or a rate limit (HTTP 429), the scraper waits for a duration that doubles with each attempt (1s, 2s, 4s...) before trying again, up to a limit of 3 retries.
* **Interviewer Cross-Question**: Why wait longer each time instead of retrying immediately?
* **Counter-Answer**: If the server is rate-limiting you, retrying immediately will only result in more blocks. Waiting longer gives the target server time to recover.

#### Q15: Why did you replace unicode console log symbols with ASCII?
* **Answer**: Command prompts on Windows can crash with `UnicodeEncodeError` when trying to print special characters like checkmarks. Using standard ASCII characters makes the code portable across all terminal environments.
* **Interviewer Cross-Question**: Does this affect the data sent to the user?
* **Counter-Answer**: No, this only applies to the server's internal terminal logs, not the UI or API payloads.

#### Q16: How do you parse LeetCode's submissions list?
* **Answer**: We loop through the submission list and extract only the accepted ones (`status == "Accepted"`). We collect their unique question slugs and store them in a set.
* **Interviewer Cross-Question**: What if a user solved a problem a long time ago and it is not in their recent submissions?
* **Counter-Answer**: LeetCode's recent submissions list is limited. To ensure accuracy, we also query the user's solved question slugs directly from their profile statistics.

#### Q17: What is the benefit of using `httpx` instead of `requests`?
* **Answer**: `httpx` supports asynchronous requests, which allows our FastAPI app to perform non-blocking HTTP requests.
* **Interviewer Cross-Question**: Why did you use the synchronous client of `httpx` in the scraper?
* **Counter-Answer**: Synchronous calls are simpler to write and debug. Since we cache the results for 10 minutes, the slight performance difference is negligible for this scale.

#### Q18: What is the size of the global problems catalog cache?
* **Answer**: The cached file `data/problems_catalog.json` contains details for over 3,900 questions and is about 1.7 MB in size.
* **Interviewer Cross-Question**: Is it safe to load a 1.7 MB JSON file into RAM at startup?
* **Counter-Answer**: Yes. 1.7 MB is a tiny footprint for modern servers, which typically have gigabytes of RAM. It is much faster than scraping it on every request.

#### Q19: Why do you run the scraper in the user pipeline instead of the routing file?
* **Answer**: It separates ingestion logic from route controllers. `routes.py` only handles HTTP requests, while `user_pipeline.py` coordinates the scraping and transformation steps.
* **Interviewer Cross-Question**: What would change if you wanted to switch to a different coding platform scraper?
* **Counter-Answer**: I would only need to update the parser code in the scraper folder, leaving the API routes unchanged.

#### Q20: How do you handle cases where a user has no solved submissions?
* **Answer**: The parser returns an empty set. The system then treats all topics as complete weaknesses (score `1.0`) and recommends popular, fundamental problems first.
* **Interviewer Cross-Question**: Does the app crash?
* **Counter-Answer**: No. The code defaults to maximum weakness scores and handles empty sets gracefully.

---

### 🧮 Topic C: Scoring & Recommendation Logic (Q21 - Q30)

#### Q21: Walk me through the weakness score calculation.
* **Answer**: We count the problems solved in each tag. We find the max solved count across all tags and divide each tag's count by this max value to get a strength score. The weakness score is $1.0 - \text{strength score}$:
  
  $$\text{Weakness} = 1.0 - \left( \frac{\text{Solved Count in Tag}}{\text{Max Solved Count}} \right)$$

* **Interviewer Cross-Question**: What happens if the user's max solved count is 1?
* **Counter-Answer**: If the max is 1, a tag with 1 solved has weakness `0.0`, and all others have `1.0`. The scoring adapts dynamically to any profile size.

#### Q22: Why use relative strength instead of absolute solved counts?
* **Answer**: Absolute counts don't reflect a user's focus. A developer who has solved 200 array problems and 50 DP problems is relatively weaker in DP, even though 50 is a high absolute count. Relative scoring highlights their personal gaps.
* **Interviewer Cross-Question**: If someone has solved 500 problems in every topic, isn't their weakness score 0.0 everywhere?
* **Counter-Answer**: Yes, if they are equally balanced, their weakness scores will be near `0.0`. In this case, the system recommends problems based on catalog popularity.

#### Q23: How does the system score an unsolved problem?
* **Answer**: The problem's score is the average weakness score of all its tags. Problems with higher scores are prioritized.
* **Interviewer Cross-Question**: Why average the scores instead of taking the maximum?
* **Counter-Answer**: Averaging ensures the recommended problem matches the user's overall skill profile, rather than picking a problem with one weak tag and several advanced tags they aren't ready for yet.

#### Q24: How does the difficulty filter work?
* **Answer**: When a user selects a difficulty (e.g., Medium), the ranker filters the unsolved catalog to keep only Medium questions, ranks them, and returns the top result.
* **Interviewer Cross-Question**: Does filtering happen before or after ranking?
* **Counter-Answer**: We filter the list first and then rank the remaining candidates. This reduces the number of calculations, optimizing performance.

#### Q25: Why do tags without any user history default to a weakness score of `1.0`?
* **Answer**: An untouched tag represents a complete blind spot. Setting it to `1.0` ensures the system introduces the user to new topics.
* **Interviewer Cross-Question**: Could this overwhelm a beginner with advanced topics?
* **Counter-Answer**: Yes, that is possible. To address this, we could add a threshold to recommend popular or introductory questions for untouched tags first.

#### Q26: What is the time complexity of the ranking step?
* **Answer**: If the catalog has $N$ problems and each has $T$ tags, scoring takes $O(N \times T)$ time. Sorting takes $O(N \log N)$ time. The overall complexity is:
  
  $$O(N \times T + N \log N)$$

* **Interviewer Cross-Question**: Is this performance acceptable for a real-time web request?
* **Counter-Answer**: Yes. For $N \approx 3,900$, the calculation takes less than 15ms in Python, which is well within acceptable limits for a web response.

#### Q27: How does the system handle a problem with no tags at all?
* **Answer**: The scorer assigns it a neutral weakness score of `0.5`, placing it in the middle of the recommendations queue.
* **Interviewer Cross-Question**: Why not assign it a score of 1.0 to be safe?
* **Counter-Answer**: A score of `1.0` would push untagged problems to the top of the queue, potentially recommending questions that don't match the user's actual weaknesses.

#### Q28: How does the app prevent duplicate recommendations?
* **Answer**: When a user selects a problem, we store the recommended problem's ID in a cache pool. The dashboard displays the remaining top recommendations without repeating the selected one.
* **Interviewer Cross-Question**: How is this cache pool managed?
* **Counter-Answer**: It is stored in Streamlit's `session_state`, meaning it persists while the user's browser session is active.

#### Q29: What happens if the candidate queue runs out of problems?
* **Answer**: This only happens if a user has solved all 3,900+ problems. In this rare case, the API returns a friendly message suggesting they practice another list or platform.
* **Interviewer Cross-Question**: How does the code handle this without throwing index errors?
* **Counter-Answer**: We check if the ranked list is empty before accessing the first element (`list[0]`), preventing index crashes.

#### Q30: How would you add a "spaced repetition" factor to the scoring?
* **Answer**: I would record the date each problem was solved. During ranking, I would scale the weakness score of topics based on how long it has been since the user last solved a problem in that category.
* **Interviewer Cross-Question**: Would this data be stored locally or in the database?
* **Counter-Answer**: It would be stored in our Supabase database, allowing the system to track their practice history over time.

---

### ⚡ Topic D: Performance & Caching (Q31 - Q40)

#### Q31: What is the Cache-Aside pattern?
* **Answer**: The application checks the cache first. On a hit, it returns the cached data. On a miss, it fetches fresh data, updates the cache, and returns the response.
* **Interviewer Cross-Question**: What is the advantage over a Cache-Through pattern?
* **Counter-Answer**: Cache-Aside is simpler to implement and ensures our app server doesn't have to coordinate writes directly with the external database or APIs during standard read queries.

#### Q32: Why did you choose a 10-minute cache TTL?
* **Answer**: LeetCode stats don't change second-by-second. A 10-minute TTL is short enough to reflect new solves quickly, but long enough to protect the server from redundant requests.
* **Interviewer Cross-Question**: How does a user force an immediate update?
* **Counter-Answer**: Clicking the "Refresh" button calls `/update`, which deletes the user's cache key and triggers a fresh scrape.

#### Q33: How does in-memory caching affect memory usage?
* **Answer**: Each cached user profile payload is small (~15 KB). Caging 1,000 active users requires only ~15 MB of RAM, which has a negligible footprint on modern servers.
* **Interviewer Cross-Question**: What happens if you get 100,000 active users?
* **Counter-Answer**: Memory usage would grow to 1.5 GB. To prevent OOM crashes, I would use an eviction policy (like LRU) or migrate the cache to Redis.

#### Q34: What is a cache stampede?
* **Answer**: It occurs when a popular cache key expires, and multiple concurrent requests attempt to fetch and cache the data simultaneously, overloading the backend.
* **Interviewer Cross-Question**: How would you prevent this in your API?
* **Counter-Answer**: I would use locking mechanisms (such as `asyncio.Lock` or Redis locks) so only the first request performs the scrape, while subsequent requests wait and read the updated cache.

#### Q35: Why is loading the catalog JSON file faster than querying LeetCode on boot?
* **Answer**: Reading a local file requires only disk I/O, which takes milliseconds. Querying LeetCode requires network calls, GraphQL parsing, and page assembly, which takes 30+ seconds.
* **Interviewer Cross-Question**: What is the downside of loading the catalog at startup?
* **Counter-Answer**: If the server runs for months without restarting, the loaded catalog might become outdated.
* **Interviewer Cross-Question**: How do you fix that without restarting the server?
* **Counter-Answer**: We use the `/admin/refresh-catalog` endpoint, which updates the catalog in memory and saves it to disk without needing a server reboot.

#### Q36: How does Uvicorn manage worker processes?
* **Answer**: Uvicorn can run multiple worker processes, allowing the application to utilize multiple CPU cores and handle more concurrent requests.
* **Interviewer Cross-Question**: Does our in-memory cache dictionary share data across Uvicorn workers?
* **Counter-Answer**: No. Each worker runs in its own isolated OS process with its own memory space. An in-memory cache is not shared across workers, which is why migrating to Redis is recommended for scaling.

#### Q37: How does Pydantic improve data validation speeds in FastAPI?
* **Answer**: Pydantic v2 core is written in Rust, which validates and converts data types much faster than pure Python code.
* **Interviewer Cross-Question**: How does this speed up our API responses?
* **Counter-Answer**: It reduces the time spent converting Python dictionaries into JSON strings, lowering overall response latency.

#### Q38: Why is using thread pools for database logging beneficial?
* **Answer**: Database writes are I/O-bound. Running them in a background thread allows the main thread to handle incoming user requests without blocking.
* **Interviewer Cross-Question**: What happens if the thread pool gets overloaded?
* **Counter-Answer**: Tasks are queued. If the queue grows too large, it can consume memory, but this is rare for typical logging workloads.

#### Q39: What is the latency difference between a cache hit and a cache miss?
* **Answer**: A cache hit takes **< 10ms** because it reads directly from RAM. A cache miss takes **3–5 seconds** because it must fetch data from LeetCode.
* **Interviewer Cross-Question**: How does this impact the user experience?
* **Counter-Answer**: The first search has a slight delay, but subsequent page loads and navigation are instant.

#### Q40: How would you pre-warm the cache for popular users?
* **Answer**: I would set up a cron job to query the API for active users periodically, keeping their cache keys warm and preventing cold-start delays.
* **Interviewer Cross-Question**: Would this overload the backend?
* **Counter-Answer**: No, if scheduled during off-peak hours, it keeps the cache warm without impacting active users.

---

### 🔒 Topic E: Database, Security & Operations (Q41 - Q50)

#### Q41: Why did you choose Supabase for your database?
* **Answer**: Supabase provides a hosted PostgreSQL database, instant REST APIs, and authentication services with minimal configuration, accelerating development.
* **Interviewer Cross-Question**: Why not use a simpler database like SQLite?
* **Counter-Answer**: SQLite is serverless and stored in a local file. If we deploy the API to a platform like Render, the container's storage is ephemeral, meaning database changes are lost on restart. Supabase is hosted externally and persists data reliably.

#### Q42: What is Row Level Security (RLS)?
* **Answer**: RLS is a PostgreSQL feature that allows you to define policies restricting which rows a user can read, write, or update based on their authentication role.
* **Interviewer Cross-Question**: Why did you disable RLS on the `users` table?
* **Counter-Answer**: Since the app runs in public mode without login, the backend needs to write user statistics anonymously. Disabling RLS allows these anonymous database writes to complete.

#### Q43: How do you secure admin routes in your API?
* **Answer**: We require an `X-Admin-Secret` header in admin requests. The API compares this value with our server-side environment variable to authorize the request.
* **Interviewer Cross-Question**: Why use a header instead of a URL parameter?
* **Counter-Answer**: URL parameters are often logged by web servers, proxies, and browser histories, which could expose your admin secret. Headers are encrypted in transit via HTTPS and are not logged.

#### Q44: What is the purpose of the `requirements.txt` file?
* **Answer**: It lists all the Python packages and their specific versions required to run the application, ensuring consistent builds across different environments.
* **Interviewer Cross-Question**: Why pin dependency versions?
* **Counter-Answer**: If you don't pin versions, a package update could introduce breaking changes, causing your app to fail during deployment.

#### Q45: How does your database schema handle conflicts?
* **Answer**: We use PostgreSQL's `ON CONFLICT (username) DO UPDATE` (upsert) logic to update existing user rows instead of throwing duplicate key errors.
* **Interviewer Cross-Question**: Why is this better than checking if the user exists first?
* **Counter-Answer**: Checking first requires two database round-trips. An upsert executes the check and write in a single database operation, saving time.

#### Q46: How would you deploy this backend to AWS?
* **Answer**: I would package the FastAPI app in a Docker container, deploy it to **AWS ECS (Elastic Container Service)** with Fargate, and use an Application Load Balancer to distribute traffic.
* **Interviewer Cross-Question**: What about the database?
* **Counter-Answer**: I would use **AWS RDS** for a managed PostgreSQL database, keeping it close to the containers to minimize latency.

#### Q47: Why did you host the dashboard on Streamlit Cloud instead of Render?
* **Answer**: Streamlit Cloud is free, does not put apps to sleep, and has no monthly runtime limits. Render's free plan puts services to sleep after 15 minutes of inactivity.
* **Interviewer Cross-Question**: How does this save resources?
* **Counter-Answer**: Hosting the dashboard on Streamlit Cloud allows us to delete the Render dashboard service, saving our 750 free Render hours for the backend API.

#### Q48: How do you handle database connection timeouts?
* **Answer**: The Supabase client handles connection pooling and automatically reconnects if a connection drops.
* **Interviewer Cross-Question**: What if the database goes down completely?
* **Counter-Answer**: The backend logs the database failure and continues serving recommendations from the cache, ensuring the app remains online.

#### Q49: How would you implement user authentication in the future?
* **Answer**: I would enable Supabase Auth, verify user JWT tokens in the FastAPI backend using a dependency, and enable RLS policies to restrict database writes to authenticated sessions.
* **Interviewer Cross-Question**: Would you write custom login code?
* **Counter-Answer**: No, I would use Supabase's built-in login helpers (email, password, Google OAuth) to handle authentication securely without reinventing the wheel.

#### Q50: How do you monitor API errors in production?
* **Answer**: We log errors to the console. In production, I would integrate an error-tracking tool like **Sentry** to capture exceptions, tracebacks, and user contexts in real time.
* **Interviewer Cross-Question**: How does this help debug issues?
* **Counter-Answer**: Sentry aggregates errors, alerts developers, and tells us exactly which file and line of code caused the failure.

### ⚙️ Topic F: Operational Edge Cases & Data Freshness (Q51 - Q55)

#### Q51: Your application caches the global LeetCode problem catalog at startup from a local JSON file. What happens if LeetCode adds a new question? Does it get recommended?
* **Answer**: If LeetCode adds a new question, it will not be in our local JSON catalog, so it won't be recommended until the catalog is refreshed. To solve this without server reboots, we built a secure admin endpoint `POST /admin/refresh-catalog`. Running this endpoint scrapes the latest LeetCode catalog, updates the local JSON file, and updates the server's in-memory data dynamically.
* **Interviewer Cross-Question**: How would you automate this in a real production environment?
* **Counter-Answer**: I would set up a scheduled cron job (using a free tool like GitHub Actions, UptimeRobot, or cron-job.org) to ping the `POST /admin/refresh-catalog` endpoint every week with the `X-Admin-Secret` header, ensuring our list of LeetCode problems is always fresh.

#### Q52: What happens if the background logging task (`log_user`) fails or encounters a Supabase Row Level Security (RLS) policy error? Does it crash the user's dashboard?
* **Answer**: No. Because `log_user` runs asynchronously inside FastAPI's `BackgroundTasks`, any database errors or policy violations are caught inside that background worker thread. The API still returns the recommendations successfully to the user's dashboard, ensuring the user experiences zero downtime or crashes.
* **Interviewer Cross-Question**: If RLS is enabled on Supabase, why would the background log write fail?
* **Counter-Answer**: If RLS is enabled on the `users` table, queries made using the public `anon` key are blocked from writing rows unless there is a specific policy allowing them. To fix this in a single-user system, we run `ALTER TABLE public.users DISABLE ROW LEVEL SECURITY;` so the background logger can write stats freely.

#### Q53: If a user solves a new problem on LeetCode and wants to see their stats update immediately, how does the app handle it if their profile is cached?
* **Answer**: The cache normally expires after 10 minutes. However, to give the user immediate control, we added a "Refresh LeetCode Data" button in the UI. Clicking this triggers the `POST /update` API endpoint, which purges the cache key for that username and runs a fresh scrape instantly.
* **Interviewer Cross-Question**: What if the user spams the "Refresh" button? How do you prevent hitting LeetCode's rate limits?
* **Counter-Answer**: To prevent button spamming, I would disable the button in the UI for 30–60 seconds after a click (rate-limiting the UI actions) or implement a backend rate-limiter (like Slowapi or Redis Token Bucket) to block consecutive updates.

#### Q54: How do environment variables differ between your local development machine and your production cloud hosting?
* **Answer**: Locally, we read settings from a `.env` file that is kept safe and never committed to Git. In production (like Render or Streamlit Cloud), we define the exact same variables (like `SUPABASE_URL` and `LEETCODE_SESSION`) directly inside the host's Environment Variables settings page.
* **Interviewer Cross-Question**: Why is it crucial to keep the `.env` file out of Git?
* **Counter-Answer**: The `.env` file contains sensitive credentials like database keys and session cookies. If committed to public repositories, attackers can find the keys, hijack your database, or run up expensive API bills. We list `.env` inside `.gitignore` to guarantee it stays local.

#### Q55: If your Streamlit frontend is hosted on Streamlit Cloud and the FastAPI backend is on Render, how does the frontend know where to send requests?
* **Answer**: We use the `API_BASE` environment variable. Locally, it points to `http://127.0.0.1:8000`. In production on Streamlit Cloud, we set the `API_BASE` secret to point to your live Render URL (e.g., `https://your-api.onrender.com`), redirecting all network requests to the live server.
* **Interviewer Cross-Question**: How do you handle Cross-Origin Resource Sharing (CORS) when they are on different domains?
* **Counter-Answer**: I configured CORSMiddleware in the FastAPI backend (`api/main.py`) to allow requests from any origin (`allow_origins=["*"]`). This allows our Streamlit Cloud domain to make secure API requests to the Render server without CORS blocks.
