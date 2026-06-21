# RAG Chatbot Architecture

## Phase 1: The Ingestion Pipeline (Data Preparation)

The first phase is preparing the data. Because we want the chatbot to understand specific company pages, we run an ingestion pipeline.

### 1. Scraping

We use **Playwright** to programmatically open company web pages, wait for JavaScript content to render, and extract clean text from the HTML body.

### 2. Chunking

The scraped text is too large to send directly to an LLM. To solve this, we split the content using a **Recursive Character Text Splitter** into chunks of **500 characters** with a **50-character overlap** to preserve context between chunks.

### 3. Embedding

Each chunk is passed through a local embedding model, **all-MiniLM-L6-v2**, which converts the text into a **384-dimensional vector representation** that captures the semantic meaning of the content.

### 4. Vector Database Storage

The generated vectors, along with their corresponding text chunks (metadata), are stored and indexed inside **ChromaDB** for efficient retrieval.

---

## Phase 2: The Retrieval Pipeline (Semantic Search)

The second phase occurs in real time whenever a user asks a question.

### 1. Query Embedding

When a user submits a question (e.g., *"What is your refund policy?"*), the same embedding model (**all-MiniLM-L6-v2**) converts the query into a vector representation.

### 2. Similarity Search

The query vector is sent to **ChromaDB**, where a **Cosine Similarity** search is performed against all indexed document vectors.

### 3. Context Retrieval

ChromaDB returns the **top 3 most relevant text chunks** whose vectors are closest to the query vector. These chunks contain the information most likely to answer the user's question.

---

## Phase 3: The Generation Pipeline (Response Synthesis)

The final phase generates a human-readable answer.

### 1. Prompt Construction

The retrieved text chunks are combined with the user's original question using a structured **Prompt Template**. The retrieved chunks serve as the contextual knowledge base.

### 2. LLM Synthesis

The constructed prompt is sent to the **Groq API**, running **Llama 3-8B**. The model is instructed to:

> Answer the question using ONLY the provided context.

### 3. FastAPI Delivery

The generated response is returned through a **FastAPI REST endpoint**, delivering the answer to the user in under a second.

---

# Why This Architecture Works

## No Retraining Required

There is no need for expensive fine-tuning or retraining. Instead, the LLM receives relevant context dynamically at inference time.

## Low Latency

Embeddings are generated locally, and Groq's high-speed **LPU hardware** enables response times of less than **0.5 seconds**.

## Reduced Hallucinations

Because the model is constrained to answer using only the retrieved context, it significantly reduces the likelihood of generating incorrect or fabricated information.

## Scalable Knowledge Base

Adding new knowledge simply requires re-running the ingestion pipeline and updating ChromaDB. No model retraining is necessary.

---

## Technology Stack

| Component | Technology |
|------------|------------|
| Web Scraping | Playwright |
| Text Chunking | LangChain Recursive Character Text Splitter |
| Embeddings | all-MiniLM-L6-v2 |
| Vector Database | ChromaDB |
| Similarity Metric | Cosine Similarity |
| LLM | Llama 3-8B |
| Inference Provider | Groq |
| API Layer | FastAPI |
| Architecture Pattern | Retrieval-Augmented Generation (RAG) |
