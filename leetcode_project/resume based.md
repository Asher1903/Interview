# 🎯 Resume-Based Interview Q&A Guide

This guide prepares you for questions interviewers will ask based directly on the bullet points in your resume:

---

## 📌 Bullet 1: "Architected a two-tier caching system, ingesting 3,000+ LeetCode problems via GraphQL to enable low-latency personalized recommendations."

### Q1: What are the "two tiers" of your caching system, and why did you design it this way?
* **Simple Explanation**: 
  * **Tier 1 (The Global Catalog)**: We load the static list of all 3,900+ LeetCode problems from a local JSON file into the server's RAM during startup. This is cached **permanently** while the server runs, since the global list of problems rarely changes.
  * **Tier 2 (The User Cache)**: We cache individual user profiles and their generated recommendations in memory for **10 minutes** using an in-memory dictionary.
  * **Why**: Tier 1 prevents us from downloading the 3,900+ problems catalog on every startup (reducing boot time from 30 seconds to < 1 second). Tier 2 prevents us from repeatedly scraping LeetCode's API if the same user refreshes their dashboard within 10 minutes (reducing query time from 3 seconds to < 10 milliseconds).

### Q2: Why did you use GraphQL to ingest the LeetCode problems?
* **Simple Explanation**: LeetCode's API is GraphQL-based. Using GraphQL allowed us to write query payloads requesting only the specific fields we needed (such as `title`, `titleSlug`, `difficulty`, and `topicTags`), rather than pulling entire databases. This minimized network payload sizes and accelerated ingestion.

### Q3: What do you mean by "low-latency" recommendations? Can you quantify the performance improvement?
* **Simple Explanation**: Without caching, every user request forces a live web scrape to LeetCode, taking **3 to 5 seconds** due to network latency. With our two-tier caching system, cache hits retrieve data directly from the server's memory, reducing the response time to **less than 10 milliseconds**—a 99% reduction in latency.

---

## 📌 Bullet 2: "Implemented a tag-based weakness scoring engine, surfacing personalized results through a 4-endpoint RESTful FastAPI service with Pydantic data validation."

### Q4: How does your "tag-based weakness scoring engine" calculate weaknesses mathematically?
* **Simple Explanation**: It uses a relative scaling formula. We count how many problems a user has solved in each category. We find their most practiced category (for example, Arrays with 20 solved) to act as a baseline. We then calculate all other topics relative to this baseline:
  
  $$\text{Weakness} = 1.0 - \left( \frac{\text{Solved Count in Topic}}{\text{Max Solved Count}} \right)$$

  If they have only solved 2 problems in Dynamic Programming, their DP weakness is $1.0 - (2/20) = 0.9$. Any topic they have solved 0 problems in defaults to a maximum weakness of `1.0`.

### Q5: What are the 4 endpoints of your RESTful FastAPI service?
* **Simple Explanation**:
  1. `GET /recommend`: Returns the top recommended problem targeting the user's biggest weakness.
  2. `GET /recommend/{difficulty}`: Returns the top recommendation filtered by difficulty (Easy, Medium, Hard).
  3. `GET /stats`: Returns the user's solved question counts, completion percentages, and topic scores.
  4. `POST /update`: Deletes the user's cache key to force a fresh scrape and retrieve updated statistics.

### Q6: How does Pydantic validate data in your FastAPI service?
* **Simple Explanation**: We define Pydantic schemas (classes) specifying the exact data types we expect (e.g., `title` must be a string, `acceptance_rate` must be a float). If the incoming data or outgoing API response doesn't match this structure, Pydantic catches it and returns a validation error, preventing corrupt data from passing through the API.

---

## 📌 Bullet 3: "Developed an interactive Streamlit dashboard with 3 Plotly visualizations."

### Q7: What are the 3 Plotly visualizations you developed, and what insights do they show?
* **Simple Explanation**:
  1. **Weakest Areas Bar Chart**: A horizontal bar chart showing the user's top 12 weakest categories, sorted by weakness score, letting them see where their largest skill gaps are.
  2. **Skill Radar Map**: A circular radar chart mapping the user's strength across their top 8 practiced topics, showing a visual footprint of their coding profile.
  3. **Completion Progress Rings**: Three colored circular pie charts (Green for Easy, Amber for Medium, Red for Hard) showing the user's progress ratio against total available problems on LeetCode.

### Q8: Why did you choose Plotly instead of standard static Matplotlib charts?
* **Simple Explanation**: Plotly renders interactive, dynamic charts in the browser. Users can hover over bars or points to see exact metrics (like the number of problems solved in a specific tag) and zoom in on specific data. This provides a premium feel compared to static image charts.
