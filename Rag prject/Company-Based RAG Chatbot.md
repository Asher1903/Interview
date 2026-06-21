# Deep Dive: Company-Based RAG Chatbot

This document provides a highly technical breakdown of your **Company-based RAG Chatbot** project. Use this to study the exact code architecture and underlying math/concepts so you can answer deep technical questions.

---

## 1. Architectural Data Flow

```
[Web Pages] ──(Playwright)──> [Raw HTML/Text]
                                    │
                                (Recursive Splitter: 500 chars / 50 overlap)
                                    │
                                    ▼
[User Query]                    [300+ Text Chunks]
     │                              │
(all-MiniLM-L6-v2)              (all-MiniLM-L6-v2)
     │                              │
     ▼                              ▼
[Query Vector] ──(Cosine Similarity Search)──> [Top 3 Relevant Chunks]
                                                      │
                                                      ▼
                                            [Prompt Template + Context]
                                                      │
                                                  (Groq LLM)
                                                      │
                                                      ▼
                                             [Final Answer]
```

---

## 2. Technical Breakdown & Sample Code

Here is the exact structure of the code you wrote for this project. Be ready to explain these blocks.

### Step A: Scraping with Playwright
We use Playwright because it runs a headless browser, allowing us to scrape dynamic, JavaScript-rendered websites (which tools like `requests` or `BeautifulSoup` fail to scrape).

```python
import asyncio
from playwright.async_api import async_playwright

async def scrape_page(url):
    async with async_playwright() as p:
        browser = await p.chromium.launch(headless=True)
        page = await browser.new_page()
        await page.goto(url, wait_until="networkidle")
        
        # Extract plain text from the body
        text_content = await page.locator("body").inner_text()
        await browser.close()
        return text_content
```

### Step B: Chunking and Embedding (LangChain)
We use `RecursiveCharacterTextSplitter` because it splits text by searching for double newlines `\n\n`, single newlines `\n`, spaces ` `, and characters in order, preserving semantic integrity (paragraphs/sentences are kept together).

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_community.embeddings import HuggingFaceEmbeddings

# 1. Split text into chunks
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,        # Max size of each chunk (characters)
    chunk_overlap=50,      # Overlap between consecutive chunks
    length_function=len
)
chunks = text_splitter.split_text(raw_scraped_text)

# 2. Initialize the Embedding Model (runs locally)
# Dimensions: 384-dimensional vector space
embeddings = HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")
```

### Step C: Vector Database (ChromaDB)
ChromaDB index searches are performed using **Cosine Similarity**, calculating the angle between the query vector and document vectors.

```python
from langchain_community.vectorstores import Chroma

# Initialize local vector store
vector_store = Chroma.from_texts(
    texts=chunks, 
    embedding=embeddings, 
    persist_directory="./chroma_db"
)

# How search works under the hood:
query = "What services does the company offer?"
docs = vector_store.similarity_search(query, k=3)  # Retrieves top 3 most relevant chunks
```

### Step D: FastAPI and Groq LLM Inference
We use Groq because it uses LPU (Language Processing Unit) hardware, which is up to 10x faster than standard GPU-based API endpoints.

```python
from fastapi import FastAPI
from langchain_core.prompts import ChatPromptTemplate
from langchain_groq import ChatGroq

app = FastAPI()

# Initialize Groq LLM (e.g., Llama 3)
llm = ChatGroq(
    temperature=0,                 # 0 = Deterministic/Factual; 1 = Creative
    model_name="llama3-8b-8192", 
    api_key="gsk_..."
)

@app.get("/chat")
async def chat_endpoint(query: str):
    # 1. Retrieve relevant context from ChromaDB
    docs = vector_store.similarity_search(query, k=3)
    context = "\n\n".join([doc.page_content for doc in docs])
    
    # 2. Construct strict prompt template to prevent hallucinations
    prompt = f"""
    You are a helpful company assistant. Answer the user's question based ONLY on the following context.
    If you do not know the answer based on the context, say "I cannot find that information in our records."
    
    Context:
    {context}
    
    Question: {query}
    Answer:
    """
    
    response = llm.invoke(prompt)
    return {"response": response.content}
```

---

## 3. Advanced Questions they might ask to test your depth

### Q1: Why did you choose a chunk size of 500 characters?
* **Answer**: *"A chunk size of 500 characters (roughly 80-100 words) is ideal for web pages because it is large enough to contain a complete sentence or short paragraph (preserving context) but small enough to fit multiple chunks into the LLM's context window without sending noisy, irrelevant information."*

### Q2: What embedding model did you use, and what is its dimension size?
* **Answer**: *"I used **`all-MiniLM-L6-v2`** from HuggingFace/SentenceTransformers. It is a highly optimized model with **384 dimensions**, which makes it extremely fast and lightweight to run locally compared to larger models like OpenAI’s text-embedding-ada-002 (1536 dimensions)."*

### Q3: How did you prevent the LLM from making up answers (hallucinating) when it didn't find the info in the vector DB?
* **Answer**: *"First, I set the LLM **temperature to 0** to make the generation deterministic. Second, I engineered the **prompt template** with strict instructions: 'Answer the question based *only* on the provided context, and if the answer is not in the context, state that you do not know.' This restricts the LLM from using its pre-trained general knowledge."*
