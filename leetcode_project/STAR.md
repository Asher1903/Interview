# STAR Interview Guide — Behavioral Q&A Template

Use this template to answer behavioral interview questions (e.g., *"Tell me about a time you solved a performance bottleneck,"* or *"Describe a project you built from scratch"*).

---

## 🌟 The STAR Method Breakdown

### 📍 Situation (The Context)
> "When preparing for software engineering technical interviews, developers often waste hundreds of hours 'blind grinding'—solving random coding problems without tracking their topic gaps. 
> 
> There was no automated tool that analyzed a user's LeetCode profile, diagnosed their exact algorithmic weaknesses, and recommended unsolved problems to directly target those gaps. 
> 
> I set out to build a data-driven LeetCode Recommender System, but faced two major constraints: LeetCode’s aggressive scraping rate limits and high query latency during real-time data processing."

### 🎯 Task (The Goal)
> "My task was to design, write, and deploy a production-grade system to:
> 1. Compute a relative weakness score for any user's profile across all algorithmic topics.
> 2. Sort and filter a database of 3,900+ problems in real time.
> 3. Ensure a fast user experience with sub-second API response times and robust error handling when third-party services failed or rate-limited our server."

### 🛠️ Action (What You Did)
> "I took the following steps to build and optimize the system:
>
> 1. **Decoupled Architecture**: I separated the frontend (Streamlit Cloud) from the backend (FastAPI) to ensure a modular design.
> 2. **Algorithm Design**: I designed a relative max-scaling algorithm to score tag weakness, meaning user strengths serve as a dynamic baseline rather than static counts.
> 3. **Startup Caching**: Downloading the 3,900+ problem catalog from LeetCode took 30+ seconds at boot. I implemented a local cache system (`problems_catalog.json`) loaded during the FastAPI lifespan startup event, which reduced boot times to under 1 second.
> 4. **Asynchronous Threading**: Writing query history to Supabase introduced a 200ms database latency. I offloaded these writes to FastAPI’s `BackgroundTasks` thread pool, allowing the server to respond to the client instantly while database logging ran in the background.
> 5. **Resilient HTTP Client**: To prevent server crashes during LeetCode rate-limiting (HTTP 429), I built a scraper client using custom browser headers and an exponential backoff retry loop."

### 📈 Result (The Business/Technical Outcome)
> "The project successfully achieved all technical goals:
> * **Latency Reduction**: Achieved **sub-10ms response times** for cached user queries (using a 10-minute in-memory TTL cache).
> * **Boot Optimization**: Dropped server boot and catalog load times by **over 95%** (from 30+ seconds to < 1 second).
> * **High Availability**: Successfully deployed 24/7. The scraper retry policy handles 100% of temporary rate-limiting issues automatically.
> * **Clean Code & Docs**: Fully documented with automated OpenAPI/Swagger docs and clean folder structures, ready for developers to audit or contribute to."
