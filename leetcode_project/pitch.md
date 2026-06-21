# How to Pitch & Explain Your Project

This guide provides structured templates and talking points for explaining your LeetCode Recommender project during job interviews or networking events. It is broken down by the amount of time you have.

---

## ⏱️ 1. The 30-Second Elevator Pitch
*Use this when an interviewer says: "Tell me about this project on your resume."*

> "I built a **data-driven LeetCode Recommender System** designed to eliminate 'blind grinding' and help developers target their exact algorithmic weak spots. 
>
> Instead of solving random problems, the system scrapes a user's LeetCode submission history, calculates a normalized weakness score for every topic tag, and dynamically ranks over 3,900 unsolved problems to recommend the one that targets their biggest skill gap. 
>
> On the technical side, I built it using **FastAPI** and **Streamlit**, optimized it with **startup caching** and **asynchronous database logging**, and designed it to be highly resilient against API rate limits."

---

## 🗣️ 2. The 2-Minute Project Walkthrough
*Use this during technical deep-dives or when asked: "Walk me through how it is built and the architecture."*

### 1. The Problem
> "When preparing for technical interviews, most developers fall into the trap of 'blind grinding'—solving random problems or practicing topics they are already comfortable with. There was no easy way to get a personalized diagnostic of your weak spots and get direct problem recommendations targeting those gaps."

### 2. My Solution
> "I built an automated coaching tool. A user enters their LeetCode username, and the dashboard displays their solved metrics, a Plotly-powered skill radar map, a ranked list of their weakest topics, and direct links to their next recommended Easy, Medium, and Hard questions."

### 3. The Architecture & Data Flow
> "Architecturally, it is structured as a decoupled backend and frontend:
> * **The Frontend**: Built with **Streamlit** and hosted on Streamlit Cloud, styled with a premium dark-mode editorial theme. It makes HTTP calls to the backend.
> * **The Backend**: A **FastAPI** server hosted on Render. When a request comes in, it checks an in-memory TTL cache first. On a cache miss, it scrapes LeetCode’s GraphQL endpoint, runs an ETL pipeline to calculate tag weakness scores, and filters the global catalog of 3,900+ questions to find the best unsolved matches.
> * **The Database**: We log user search history and performance stats to **Supabase**."

### 4. Technical Wins (What I'm Proud Of)
> "To make this project production-grade, I solved three key engineering challenges:
> 1. **Zero-Delay Startup**: Fetching the 3,900+ problem catalog from LeetCode took over 30 seconds at boot. I resolved this by implementing **Startup Catalog Caching**, dropping server startup time to **under 1 second**.
> 2. **Latency-Free UI**: Database logging to Supabase introduced a 200ms delay. I offloaded these writes to FastAPI's **BackgroundTasks** thread pool, enabling the API to respond instantly while the DB write happens in the background.
> 3. **Scraper Resiliency**: LeetCode frequently blocks or rate-limits automated requests. I protected our scraper using custom browser headers and an **exponential backoff retry algorithm** to handle temporary network issues or rate-limiting (HTTP 429) automatically."

---

## 💡 3. Key Talking Points (What to Brag About)

* **Performance-First Mindset**: Highlight that you didn't just write a scraper; you implemented **caching** (in-memory TTL cache) and **asynchronous patterns** (FastAPI BackgroundTasks) because user experience and latency matter.
* **Resilience**: Talk about handling HTTP 429 errors. Interviewers love hearing about how you handle **failing dependencies** (like a third-party API going offline or rate-limiting you) using exponential backoffs.
* **Separation of Concerns**: Mention that the backend is a pure API and the frontend is a thin client. This makes the system modular, meaning you could swap Streamlit for a React mobile app without changing a single line of backend logic.
