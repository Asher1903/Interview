# Step 1: The Ingestion Layer (Getting the Data)

"To make recommendations, we first need problem data. I designed an ingestion pipeline:"

- **GraphQL API Query:** Instead of using scraping scripts or bulky REST endpoints, I wrote a Python script that queries LeetCode's public GraphQL API endpoint.
- **Targeted Fetching:** I queried only the fields necessary: problem titles, IDs, difficulty level (Easy/Medium/Hard), and topic tags (e.g., Arrays, DP).
- **Database Storage:** This metadata for 3,000+ problems was stored in a relational PostgreSQL database (hosted on Supabase) to enable fast SQL queries.

# Step 2: The API & Caching Layer (Making it Fast)

"To serve user requests, I built a fast backend using a two-tier caching mechanism:"

- **Request Validation:** When a user logs in, their frontend sends a request to the FastAPI backend. The request is validated instantly using Pydantic schemas to ensure proper data types.
- **Tier 1 (In-Memory Cache):** The API first checks a local memory cache (using a Python dictionary or Redis). If the user's recommendations are already cached, it returns them instantly (taking less than 10 milliseconds).
- **Tier 2 (PostgreSQL):** If it's a cache miss, the system queries the PostgreSQL database to fetch the user's recent submission logs and the problem list.

# Step 3: The Recommendation Logic & Presentation

"Once the data is retrieved, the recommendation algorithm runs:"

- **Weakness Scoring:** The backend analyzes the user's submission history. If they fail a question under a tag (e.g., Dynamic Programming), the weakness score for DP increases. If they solve it, the score decreases.
- **Filtering & Ranking:** The system queries the problem database to find questions matching the user's weakest tag, filters them by their preferred difficulty (Easy/Medium/Hard), and returns the top 5.
- **Streamlit Frontend:** The list of recommended problems is sent as JSON to a Streamlit application, which renders the problems as clickable links and displays the user's progress with Plotly visualizations (like a radar chart of their weak areas).

# 💡 Key Design Decisions to Highlight

### Why GraphQL?

"It prevented over-fetching. We didn't need the full problem descriptions or solution codes, just titles and tags, which saved bandwidth."

### Why Caching?

"If 1,000 users request recommendations, hitting the database 1,000 times is slow and expensive. Caching user profiles in memory reduces database load by up to 80%."

### Why FastAPI?

"It is asynchronous, making it perfect for running database calls in parallel while validating input data automatically."
