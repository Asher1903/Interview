# K. Reena's Resume: Complete Explanation & Interview Preparation Guide

This guide provides an exhaustive, line-by-line breakdown of every single technical concept, project, database, architecture, and mathematical principle on your resume. Each section includes concrete code or mathematical examples, explanation of core concepts, and direct mapping to the **AWS AI Intern** role at **Royal Cyber**.

---

## Part 1: Resume Alignment with Royal Cyber's AWS AI Intern Role

Before diving into the details, here is how your profile aligns with the requirements of the AWS AI Intern role:

| Royal Cyber Job Requirement | Your Resume Alignment | Interview Strategy |
| :--- | :--- | :--- |
| **Develop AI/ML solutions using Amazon Bedrock** | Experience building RAG pipelines, LangChain, HuggingFace, and Generative AI Basics. | Emphasize that your RAG chatbot architecture can be directly ported to **Amazon Bedrock Knowledge Bases** and **Claude** models. |
| **Model development with Amazon SageMaker** | Experience with `scikit-learn`, Jupyter Notebooks, Linear Regression, and Classification. | Talk about using SageMaker Notebooks for exploratory data analysis (EDA) and training baseline models. |
| **Learn agentic AI systems and RAG architectures** | Built an end-to-end **RAG Chatbot** using FastAPI, LangChain, and ChromaDB. | Highlight your understanding of chunking, vector embeddings, similarity search, and prompt engineering. |
| **Proficiency in Python programming** | Core language on resume, FastAPI, Pydantic, CLI tool development, and data cleaning. | Showcase clean coding, object-oriented patterns, and backend testing with `pytest`. |
| **AWS services & Cloud Practitioner focus** | AWS Cloud Essentials, GitHub Actions CI/CD, and Render deployment. | Highlight your OCI AI and AWS Cloud Essentials foundations. |
| **Linear algebra, calculus, and statistics** | Listed under Technical Skills; used in correlation and regression analysis. | Be ready to explain dot products (for similarity), gradients (for training), and regression math. |

---

## Part 2: Section-by-Section Deep Dive

---

### Section 1: Professional Summary & Competitive Programming

#### 1. CGPA: 8.09/10 (B.Tech in Computer Science & Engineering)
*   **What it represents:** Academic discipline, strong foundation in core CS subjects (Operating Systems, Database Management Systems, Computer Networks, and Theory of Computation).
*   **Interview Tip:** If asked about your CGPA, focus on how you balanced academic excellence with practical, hands-on software development and hackathons.

#### 2. RESTful APIs & FastAPI
*   **The Concept:** 
    *   **REST (Representational State Transfer)** is an architectural style for designing networked applications. It relies on a stateless, client-server protocol—almost always HTTP.
    *   **FastAPI** is a modern, high-performance web framework for building APIs with Python 3.8+ based on standard Python type hints. It is built on top of **Starlette** (for web routing) and **Pydantic** (for data validation).
*   **Key Differences:**
    *   *Flask* is synchronous by default, requiring external libraries for validation (like Marshmallow) and documentation (like APISpec).
    *   *FastAPI* is asynchronous (`async/await`) out of the box, auto-generates interactive Swagger UI docs (`/docs`), and enforces type safety via Pydantic.
*   **Code Example (FastAPI REST Endpoint):**
    ```python
    from fastapi import FastAPI
    from pydantic import BaseModel

    app = FastAPI()

    # Pydantic schema for request validation
    class RecommendationRequest(BaseModel):
        user_id: int
        weakness_tag: str

    @app.post("/recommend")
    async def get_recommendation(request: RecommendationRequest):
        # Asynchronous database or cache lookup
        return {
            "user_id": request.user_id,
            "recommended_problem_id": 101,
            "title": "Symmetric Tree",
            "difficulty": "Easy"
        }
    ```

#### 3. ETL Pipelines (Extract, Transform, Load)
*   **The Concept:** ETL is the foundation of data engineering.
    *   **Extract:** Fetching data from source systems (e.g., scraping web pages using Playwright, querying LeetCode's GraphQL API).
    *   **Transform:** Cleaning, filtering, de-duplicating, and formatting the data (e.g., using `pandas` to drop null values, converting text into vector embeddings).
    *   **Load:** Writing the processed data into a destination store (e.g., loading relational data into PostgreSQL, indexing vector embeddings into ChromaDB).
*   **Royal Cyber Relevance:** SageMaker pipelines often start with ETL to prepare training datasets. Amazon Glue and SageMaker Data Wrangler are AWS's managed tools for ETL.

#### 4. Competitive Programming (LeetCode: 200+ Solved)
*   **Core Data Structures & Algorithms (DSA):**
    *   **Arrays:** Contiguous memory locations. You understand sliding window, two-pointer, and prefix sum techniques.
    *   **Stacks:** Last-In-First-Out (LIFO) structure. Used in problems like "Valid Parentheses" or tracking monotonic sequences.
    *   **Linked Lists:** Nodes pointing to the next node. You know how to detect cycles (Floyd's Cycle-Finding Algorithm) and reverse lists.
*   **Why it matters:** Demonstrates strong problem-solving skills, optimal code complexity (Big-O notation), and edge-case handling.

---

### Section 2: Technical Skills & Core Concepts

#### 1. GraphQL vs. REST APIs
*   **REST (Representational State Transfer):** Multiple endpoints return fixed data structures (e.g., GET `/users`, GET `/users/1/posts`). Can lead to *over-fetching* (getting more data than needed) or *under-fetching* (requiring multiple API calls).
*   **GraphQL:** A query language for APIs. The client defines the exact shape of the response in a single request.
*   **GraphQL Query Example (Used in your LeetCode Recommender):**
    ```graphql
    query getUserProfile($username: String!) {
      matchedUser(username: $username) {
        submitStats {
          acSubmissionNum {
            difficulty
            count
          }
        }
      }
    }
    ```

#### 2. JWT Authentication & bcrypt
*   **JWT (JSON Web Token):** A compact, URL-safe means of representing claims to be transferred between two parties. Composed of three parts: **Header** (algorithm & token type), **Payload** (user claims/metadata), and **Signature** (verifies the token hasn't been altered).
*   **bcrypt:** A password-hashing function designed to defend against brute-force attacks by incorporating a salt (random data) and utilizing an adjustable work factor (rounds).
*   **Authentication Flow:**
    1. User logs in with a password.
    2. Server hashes the input password using `bcrypt` and compares it to the stored hash in PostgreSQL.
    3. If valid, the server signs a JWT containing the user's ID and sends it back.
    4. For subsequent requests, the client sends the JWT in the `Authorization: Bearer <token>` header.

#### 3. PostgreSQL vs. ChromaDB (Relational vs. Vector Databases)
*   **PostgreSQL:** A relational database (RDBMS) that stores data in structured tables with rows and columns, enforcing relationships via foreign keys and constraints. Optimized for ACID-compliant transactional queries using SQL.
*   **ChromaDB:** An open-source vector database. It stores data as high-dimensional vector embeddings (arrays of floating-point numbers) rather than traditional rows. Optimized for fast similarity search using algorithms like Hierarchical Navigable Small World (HNSW).
*   **How Vector Search Works:** 
    *   If you search for *"capital of France"*, ChromaDB computes the cosine similarity between the query embedding and the stored document embeddings to retrieve *"Paris"* even if the word "capital" or "France" isn't explicitly in the document.

#### 4. Software Engineering & Testing (`pytest`)
*   **Object-Oriented Programming (OOP):**
    *   *Inheritance:* Subclasses inherit methods and attributes from a parent class.
    *   *Polymorphism:* Different classes can implement the same method interface (e.g., `BaseModel.predict()` implemented differently in `LinearRegression` and `RandomForest`).
    *   *Encapsulation:* Restricting direct access to some of an object's components (using private variables/methods in Python, e.g., `self.__private_var`).
    *   *Abstraction:* Hiding complex implementation details and showing only essential features.
*   **Design Patterns:**
    *   *Singleton:* Ensures a class has only one instance (e.g., a single shared database connection pool).
    *   *Factory:* Creates objects without specifying the exact class to be created.
*   **Unit Testing with `pytest`:** Writing automated tests to verify functions behave as expected.
    ```python
    # target_code.py
    def calculate_weakness_score(solved: int, total: int) -> float:
        if total == 0:
            return 0.0
        return round((1 - (solved / total)) * 100, 2)

    # test_target_code.py
    from target_code import calculate_weakness_score

    def test_calculate_weakness_score():
        assert calculate_weakness_score(2, 10) == 80.0
        assert calculate_weakness_score(0, 5) == 100.0
        assert calculate_weakness_score(5, 5) == 0.0
        assert calculate_weakness_score(0, 0) == 0.0
    ```

#### 5. AI / ML & Mathematics

##### Linear Algebra & Calculus
*   **Vectors & Matrices:** Datasets are represented as matrices $X$ (rows as samples, columns as features). Model weights are vectors $W$.
*   **Dot Product:** Used to calculate vector similarity and model predictions:
    $$\text{Prediction } y = W \cdot X + b = \sum_{i=1}^{n} w_i x_i + b$$
*   **Cosine Similarity Formula:** Used in your RAG pipeline to retrieve relevant chunks:
    $$\text{Cosine Similarity}(A, B) = \frac{A \cdot B}{\|A\| \|B\|}$$
*   **Gradients & Partial Derivatives (Calculus):** 
    *   **Gradient Descent:** An optimization algorithm used to minimize a loss function (like Mean Squared Error).
    *   The gradient represents the direction of steepest increase of the function. To minimize the loss, weights are updated in the opposite direction:
        $$W \leftarrow W - \alpha \nabla L(W)$$
        *(where $\alpha$ is the learning rate and $\nabla L(W)$ is the gradient of the loss function).*

##### Statistics
*   **Mean, Median, Mode:** Measures of central tendency.
*   **Variance & Standard Deviation:** Measures of data dispersion.
*   **Correlation Coefficient (Pearson's $r$):** Measures the linear relationship between two variables, ranging from -1 (perfect negative correlation) to +1 (perfect positive correlation).
*   **Linear Regression:** Fits a straight line to the data points to predict a continuous variable:
    $$y = mx + c$$
*   **Classification:** Predicting discrete class labels (e.g., spam vs. ham) using algorithms like Logistic Regression or Decision Trees.

##### Generative AI & RAG Concepts
*   **Embeddings:** Numerical representations of text meaning. Models like HuggingFace's `all-MiniLM-L6-v2` convert a sentence into a 384-dimensional vector.
*   **RAG (Retrieval-Augmented Generation):** Enhancing LLM outputs by retrieving relevant documents from a vector store and injecting them into the LLM's prompt context.
*   **LangChain:** A framework that provides ready-to-use abstractions for building LLM applications, including document loaders, text splitters, vector store integrations, and chain pipelines.

---

### Section 3: Work Experience

#### Data Analytics Intern - VOIS for Tech Program (Sept 2025 – Oct 2025)

##### Bullet 1: Data Preprocessing & Cleaning
*   **The Concept:** Preparing raw data for analysis.
    *   *Handling Missing Values:* Imputing missing data using the mean/median for numerical fields, or a placeholder/mode for categorical fields.
    *   *Removing Duplicates:* Dropping identical records that distort statistics.
    *   *Label Inconsistencies:* Standardizing values (e.g., mapping "USA", "United States", and "U.S." to a single category "US").
    *   *Feature Extraction:* Creating new features from existing ones (e.g., extracting "Day of Week" from a raw timestamp).
*   **Pandas Code Example:**
    ```python
    import pandas as pd

    df = pd.read_csv("raw_data.csv")

    # Drop duplicates
    df.drop_duplicates(inplace=True)

    # Handle missing values
    df["age"] = df["age"].fillna(df["age"].median())

    # Correct label inconsistencies
    df["country"] = df["country"].replace({"United States": "US", "USA": "US"})

    # Feature extraction
    df["signup_date"] = pd.to_datetime(df["signup_date"])
    df["signup_month"] = df["signup_date"].dt.month
    ```

##### Bullet 2: Exploratory Data Analysis (EDA) & Visualization
*   **The Concept:** Analyzing dataset characteristics, often visual, to discover patterns, spot anomalies, or test hypotheses.
*   **Visualizations Used:**
    *   *Bar Charts:* Compare categorical variables.
    *   *Line Plots:* Visualize trends over time.
    *   *Regression Plots:* Show data scatter along with a fitted line representing the relationship between the independent and dependent variables.
*   **Libraries:** `Matplotlib` (static plots), `Seaborn` (statistical styling), and `Plotly` (interactive HTML charts).

##### Bullet 3: Correlation & Regression Analysis
*   **The Concept:** Determining if and how variables change together.
    *   *Correlation:* Check if a change in variable $X$ is associated with a change in variable $Y$ (using `df.corr()`).
    *   *Regression:* Modeling the predictive relationship (e.g., how advertising spend predicts sales).
*   **Actionable Business Recommendations:** Translating statistical findings into business decisions. For example, if correlation analysis showed high engagement on short video content, the recommendation would be to focus content strategy on short-form videos.

---

### Section 4: Projects Deep Dive

#### Project 1: LeetCode Problem Recommender

```mermaid
graph TD
    A[Streamlit Frontend] -->|GraphQL Queries| B[FastAPI Backend]
    B -->|Check| C[Two-Tier Cache]
    C -->|Hit| B
    C -->|Miss| D[LeetCode GraphQL API]
    D -->|Store Data| C
    B -->|Compute Weakness Score| E[Weakness Scoring Engine]
    E -->|Filter & Recommend| A
```

##### 1. Two-Tier Caching System
*   **The Problem:** Querying the LeetCode GraphQL API directly for every user request is slow and rate-limited.
*   **The Solution:** A two-tier caching mechanism:
    *   **Tier 1 (In-Memory Cache - Fast):** Storing active user recommendations in application memory (e.g., a Python dictionary or Redis) for instant retrieval (latency < 5ms).
    *   **Tier 2 (Database Cache - PostgreSQL):** Storing parsed problem metadata. If the requested data isn't in memory, check the local database before calling the external API.
*   **Why it reduces latency:** Eliminates slow network round-trips to external APIs for recurring queries.

##### 2. Tag-Based Weakness Scoring Engine
*   **The Logic:** Calculate which topics (e.g., "Dynamic Programming", "Graphs") the user struggles with based on their submission history.
*   **Formula Example:**
    $$\text{Weakness Score for Tag } T = \left( 1 - \frac{\text{Problems Solved in } T}{\text{Total Problems Attempted in } T} \right) \times 100$$
    *   If a user attempted 10 "Dynamic Programming" problems but only passed 2, their weakness score is $(1 - 0.2) \times 100 = 80\%$.
    *   The recommender then filters LeetCode problems tagged with "Dynamic Programming" that are within the user's difficulty capability.

##### 3. Streamlit & Plotly Dashboard
*   **Streamlit:** A framework to turn data scripts into shareable web apps in minutes using pure Python.
*   **Plotly:** Interactive charting library. The dashboard allows users to hover over plots, zoom in on data points, and view their weakness distributions interactively.

---

#### Project 2: Company Based RAG Chatbot

```mermaid
graph TD
    A[Scrape Docs: Playwright] --> B[Text Splitter: LangChain]
    B --> C[Generate Embeddings: HuggingFace]
    C --> D[Store Vectors: ChromaDB]
    E[User Query] --> F[FastAPI Search]
    F -->|Query Embedding| D
    D -->|Top K Semantically Similar Chunks| F
    F -->|Construct Context Prompt| G[Groq LLM Inference]
    G -->|Hallucination-free Answer| H[User Response]
```

##### 1. Ingestion using Playwright Scraping
*   **Playwright:** A library to automate Chromium, Firefox, and WebKit browsers. Unlike simple libraries like `requests` or `BeautifulSoup`, Playwright executes JavaScript, allowing you to scrape modern Single Page Applications (SPAs).
*   **Process:** Open target web pages, wait for content to load, extract the raw text content, and pass it to the ingestion pipeline.

##### 2. Semantic Chunking & Vector Ingestion
*   **Why chunking is needed:** LLMs have context window limits, and passing an entire website's text to a model is expensive and noisy.
*   **Recursive Character Text Splitting:** Splits document text by checking common separators recursively (e.g., `\n\n`, `\n`, ` `) to ensure paragraphs and sentences remain intact.
    *   *Parameters:* `chunk_size=1000` (characters), `chunk_overlap=200` (to maintain contextual continuity across adjacent chunks).
*   **Vectorization:** Each chunk is converted to a vector embedding using a HuggingFace transformer model (e.g., `sentence-transformers/all-MiniLM-L6-v2`).
*   **Storage:** The vector embeddings and original text chunks are indexed in **ChromaDB**.

##### 3. Retrieval & Generation (RAG Pipeline)
*   **Retrieval:** When a user asks a question (e.g., *"What is the company's refund policy?"*), the query is embedded using the same model. ChromaDB performs a similarity search (like cosine distance) and returns the top $K$ (e.g., $K=3$) most relevant text chunks.
*   **Prompt Construction:** The retrieved chunks are formatted into a system template:
    ```text
    Context information is below.
    ---------------------
    {retrieved_chunks}
    ---------------------
    Given the context information and not prior knowledge, answer the query.
    Query: {user_query}
    Answer:
    ```
*   **Groq LLM Inference:** The prompt is sent to an LLM running on Groq (e.g., Llama-3) to generate a response. Because the model is constrained to the provided context, hallucinations are significantly reduced.

---

### Section 5: Awards, Certifications & Activities

#### 1. Hack for Impact Hackathon (2nd Place out of 50+ Teams)
*   **The Project:** A Startup-Investor Matchmaking Platform.
*   **The Value:** Demonstrates rapid prototyping skills, teamwork, and the ability to build functional MVPs under tight deadlines (typically 24–48 hours).
*   **Interview Hook:** Mentioning that the award was presented by Aman Gupta (CEO of BoAt) is a great icebreaker that displays your ability to pitch ideas effectively to business stakeholders.

#### 2. Key Certifications
*   **AWS Cloud Essentials / OCI AI Foundations:** Shows that you understand cloud infrastructure (virtual machines, object storage, security policies) and basic cloud-based AI service suites.
*   **GitHub Foundations:** Verifies knowledge of version control, branching strategies (GitHub Flow), pull request reviews, and repository security best practices.
*   **Postman API Fundamentals:** Confirms proficiency in writing API requests, organizing collection variables, and performing basic API testing.

---

## Part 3: Deep Dive into AWS AI Technologies (For Royal Cyber Prep)

Since this role is for an **AWS AI Intern**, the interviewers will want to see how you map your local/open-source experience (LangChain, ChromaDB, HuggingFace) to the **AWS Enterprise AI Ecosystem**.

### 1. Amazon Bedrock
*   **What it is:** A fully managed service that makes foundation models (FMs) from leading AI startups (Anthropic, Cohere, Meta, AI21 Labs, Stability AI) and Amazon available via an API.
*   **Key Services inside Bedrock:**
    *   **Knowledge Bases for Amazon Bedrock:** A fully managed RAG workflow. Instead of manually scraping, chunking, embedding, and setting up ChromaDB locally, AWS manages the ingestion, vector store syncing (into Amazon OpenSearch Serverless, Pinecone, or pgvector), and retrieval.
    *   **Agents for Amazon Bedrock:** Orchestrates multi-step tasks. It uses LLMs to dynamically decide which APIs (defined via OpenAPI schemas) to call to execute user actions.
*   **How to relate your projects:** 
    > *"In my Company-based RAG Chatbot, I built a custom pipeline with ChromaDB and LangChain. At Royal Cyber, I can implement this enterprise-wide using **Amazon Bedrock Knowledge Bases** combined with **Anthropic Claude** for generation and **Amazon OpenSearch Serverless** as the vector database."*

### 2. Amazon SageMaker
*   **What it is:** A fully managed service that provides every developer and data scientist with the ability to build, train, and deploy machine learning models quickly.
*   **Key Components:**
    *   **SageMaker Notebook Instances / Studio:** Managed Jupyter notebook environments.
    *   **SageMaker Training Jobs:** Offloads model training to distributed AWS compute instances (GPUs), saving models directly to S3.
    *   **SageMaker Real-Time Endpoints:** Deploys a trained model as a REST API endpoint behind an HTTPS connection, auto-scaling as traffic grows.
*   **How to relate your experience:**
    > *"I used `scikit-learn` and Pandas locally to build classification and regression pipelines. At Royal Cyber, I can run these workloads at scale using **Amazon SageMaker**, utilizing its training jobs to handle larger datasets and deploying model endpoints that can be consumed by FastAPI or other backend services."*

### 3. Amazon QuickSight (Referred to as "QuickSuite" in JD)
*   **What it is:** A fast, cloud-powered business intelligence (BI) service that makes it easy to deliver insights to everyone in your organization.
*   **Key Features:**
    *   Generates interactive dashboards.
    *   **QuickSight Q:** An AI-powered natural language query capability that allows business users to ask questions of their data (e.g., *"What were sales last quarter by region?"*) and receive instant visualizations.
*   **How to relate your experience:**
    > *"In my VOIS internship, I built static and interactive visualizations using Seaborn and Plotly. For corporate integration projects at Royal Cyber, I can leverage **Amazon QuickSight** to build production-grade dashboards, connecting them directly to data lakes in Amazon S3 or databases in Amazon RDS."*

---

## Part 4: Expected Technical Interview Questions & Answers

### Q1: Explain the difference between Vector Embeddings and Word Embeddings.
*   **Answer:**
    *   *Word Embeddings* (like Word2Vec or GloVe) assign a static vector to individual words. They fail to handle context. For example, the word "bank" has the same vector in "river bank" and "money bank".
    *   *Vector Embeddings* (generated by modern Transformers like BERT or HuggingFace models) are contextual. They represent the meaning of entire sentences or paragraphs. The context of surrounding words determines the vector representation of each word, making them ideal for semantic search in RAG.

### Q2: How does a two-tier caching system work in your LeetCode Recommender, and how do you handle cache invalidation?
*   **Answer:**
    *   *Tier 1:* An in-memory cache (like a Python dictionary or Redis) holds the final recommended output for active users.
    *   *Tier 2:* A relational database cache (PostgreSQL) stores the static problem details (problem ID, tags, description).
    *   *Execution:* When a user requests recommendations:
        1. We check Tier 1. If it's a hit, return immediately.
        2. If it's a miss, we compute the recommendations by querying PostgreSQL (Tier 2) for problems matching the user's weaknesses.
        3. If PostgreSQL doesn't have the problem list updated, we query the LeetCode API, update PostgreSQL, and update Tier 1.
    *   *Cache Invalidation:* Since LeetCode problems change slowly, we can use a Time-To-Live (TTL) of 24 hours for the in-memory cache, and update the PostgreSQL database weekly.

### Q3: How do you handle database connections efficiently in FastAPI?
*   **Answer:** We use **connection pooling** and FastAPI's **dependency injection** system (`Depends`).
    *   Using a library like `SQLAlchemy` or `psycopg2`, we initialize a engine/pool when the application starts.
    *   We write a generator function that yields a database session and closes it once the request is completed.
    ```python
    from sqlalchemy import create_engine
    from sqlalchemy.orm import sessionmaker

    DATABASE_URL = "postgresql://user:password@localhost/dbname"
    engine = create_engine(DATABASE_URL, pool_size=10, max_overflow=20)
    SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

    def get_db():
        db = SessionLocal()
        try:
            yield db
        finally:
            db.close()
    ```
    *   In the endpoint: `@app.get("/items") def read_items(db=Depends(get_db)): ...`

### Q4: What are the mathematical components of a simple Linear Regression model, and how do you evaluate it?
*   **Answer:**
    *   *Model equation:* $y = w \cdot x + b$, where $w$ is the slope weight, $b$ is the intercept bias, $x$ is the input feature, and $y$ is the predicted output.
    *   *Objective Function (Mean Squared Error - MSE):*
        $$\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$
    *   *Evaluation Metrics:*
        *   **$R^2$ Score (Coefficient of Determination):** Represents the proportion of variance in the dependent variable that is predictable from the independent variable. Ranges from 0 to 1 (with 1 being a perfect fit).
        *   **Mean Absolute Error (MAE):** The average magnitude of errors in predictions without considering their direction.

### Q5: How do you prevent your RAG pipeline from generating hallucinations?
*   **Answer:**
    1.  **Strict System Prompts:** Instruct the LLM to write "I don't know" or "Not found in context" if the answer is not present in the retrieved chunks.
    2.  **Context-Only Grounding:** Programmatically strip out general knowledge defaults by setting temperature to `0` for deterministic outputs.
    3.  **Citation and Source Verification:** Return the specific source web page URLs or chunk references alongside the generated answer so the user can verify the information.
    4.  **Reranking:** Implement a cross-encoder model to re-evaluate the relevance of retrieved chunks before sending them to the LLM, ensuring only highly relevant context is passed.
