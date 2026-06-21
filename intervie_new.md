# AWS AI Intern Interview Preparation Guide

This guide is designed to help you prepare for your interview at **Royal Cyber** for the **AWS AI Intern** position. It connects your background (B.Tech, RAG Chatbot project, VOIS Internship, AWS Cloud Essentials certification) directly to the Job Description (JD) requirements.

---

## 🌟 Your Strongest Selling Points (Resume Alignment)
You are not a complete beginner! Your resume shows several elements that make you a strong candidate:
1. **RAG Chatbot Project**: You have hands-on experience with Retrieval-Augmented Generation (RAG) using LangChain, ChromaDB, and Groq. This directly maps to the JD requirement: *"Learn agentic AI systems and RAG architectures."*
2. **AWS Cloud Essentials**: This certification proves your familiarity with the cloud.
3. **Data Analytics Intern (VOIS)**: You have done data preprocessing, EDA, and statistics, which aligns with SageMaker data cleaning and ML foundations.
4. **Strong Python & Coding**: 200+ solved LeetCode problems and FastAPI experience show you have solid coding foundations.

---

## 🧠 Core Technical Topics (Simplified)

### 1. Retrieval-Augmented Generation (RAG)
* **What is it?**
  Imagine an open-book exam. A standard LLM (Large Language Model) like GPT-4 is like a student who memorized a library but can't access new or private books. **RAG** is when you give the student a search engine to look up private or real-time documents (a textbook) before writing their answer.
* **How it works step-by-step:**
  1. **Document Loading**: You read text files or websites (you used `Playwright` for scraping).
  2. **Chunking**: You break long documents into smaller, readable pieces (chunks).
  3. **Embeddings & Vector Database**: You convert these chunks into lists of numbers (embeddings) that represent their meaning. You store them in a **Vector Database** (like ChromaDB).
  4. **Retrieval**: When a user asks a question, you search the database for chunks that mean the same thing as the question.
  5. **Generation**: You feed the question + the retrieved chunks to the LLM (like Groq/Claude), which writes a correct answer.
* **AWS Equivalent**: **Amazon Bedrock Knowledge Bases**. Instead of writing custom LangChain and ChromaDB code, AWS allows you to point Bedrock to an S3 folder (where documents are stored), and AWS handles the chunking, embedding, vector database storage, and retrieval automatically.

---

### 2. Amazon Bedrock & Foundation Models
* **What is Amazon Bedrock?**
  Think of Bedrock as an **"App Store for AI Models."** Instead of downloading huge models and running them on your own expensive servers, Bedrock gives you a single API (a web link) to access state-of-the-art models from different AI companies (like Anthropic Claude, Meta Llama, AI21 Jurassic, and Amazon Titan).
* **Key Vocabulary:**
  * **Foundation Model (FM)**: A massive AI model trained on a huge amount of data that can perform many tasks (like summarizing, translating, writing code, or answering questions). Example: *Anthropic Claude 3.5 Sonnet*.
  * **API (Application Programming Interface)**: A way for two software applications to talk to each other. With Bedrock, you write Python code to send text to Bedrock, and it returns the model's answer.
  * **Fine-Tuning**: Taking an existing Foundation Model and training it further on your own small, private dataset so it learns your company's writing style or specific jargon.

---

### 3. Agentic AI Systems
* **What is it?**
  Traditional LLMs are *reactive*—they write a response and stop. **Agentic AI** (or AI Agents) is *proactive*. You give the LLM a goal, a set of **tools** (like a calculator, database search, or email sender), and a loop to think. The agent plans its steps, uses tools to gather information, evaluates its progress, and takes action until the goal is met.
  * *Simple Analogy*: A chatbot tells you the weather forecast. An AI Agent sees that it's going to rain, checks your calendar, finds you have an outdoor meeting, and emails the organizer to move it indoors.
* **AWS Equivalent**: **Agents for Amazon Bedrock**. You connect a foundation model to specific APIs (AWS Lambda functions) and describe what the APIs do. The Bedrock Agent decides when and how to call them to execute a user's task.

---

### 4. Amazon SageMaker
* **What is it?**
  If Bedrock is a store for ready-made models, **SageMaker** is the **factory** where you build, train, and deploy your *own* custom Machine Learning models from scratch.
* **The SageMaker Workflow:**
  1. **Build**: Use Jupyter Notebooks (SageMaker Studio) to write Python code, clean data (using Pandas/NumPy), and test small models.
  2. **Train**: SageMaker rents powerful computers (GPUs) automatically, trains your model on large datasets (e.g. using PyTorch or Scikit-Learn), and shuts down the computers when done so you don't waste money.
  3. **Deploy**: SageMaker hosts your trained model on a server and gives you a web link (endpoint). Now, other applications can send data to this link to get predictions (e.g., predicting house prices, detecting fraud).

---

### 5. Amazon QuickSight (mentioned as "QuickSuite" in the JD)
* **What is it?**
  Amazon QuickSight is a **Business Intelligence (BI)** tool. Think of it as a cloud-based Microsoft Excel + Power BI. It connects to your databases, reads tables, and lets you build interactive dashboards (charts, graphs) to show business metrics.
* **AI Integration (QuickSight Q)**:
  QuickSight uses generative AI so business users can ask questions in plain English (e.g., *"Show me our top-selling products in Chicago last month"*), and it automatically generates the correct charts and answers.

---

## 📐 Math & Machine Learning Fundamentals

If they ask you technical questions about basic ML or math, here are the core concepts explained simply:

### Linear Algebra & Calculus (The "Engine" of ML)
* **Vectors & Matrices**: A vector is a list of numbers (like coordinates: `[x, y]`). A matrix is a table of numbers. In ML, everything is converted into vectors. An image is a matrix of pixel colors; a word is a vector of meanings (embedding).
* **Calculus (Derivatives & Gradients)**: In calculus, a derivative tells you the slope or direction of a curve. In ML, models learn by making mistakes (loss). We use **Gradient Descent** (which uses calculus) to figure out which direction to tweak the model's settings (weights) to minimize its mistakes.

### Statistics (Understanding the Data)
* **Mean, Median, Mode**: Mean is the average, median is the middle value, and mode is the most common value.
* **Correlation**: A number between -1 and 1 showing how two things move together. (e.g., higher temperature is positively correlated with ice cream sales).
* **Overfitting vs. Underfitting**:
  * **Overfitting**: The model memorizes the training data too perfectly (including noise) and fails to perform on new data. (Like memorizing a practice test but failing the actual exam).
  * **Underfitting**: The model is too simple and doesn't learn the pattern at all. (Like not studying at all).

### Supervised vs. Unsupervised Learning
* **Supervised Learning**: You train the model with labeled data (data that has questions and answers).
  * *Regression*: Predicting a continuous number (e.g., predicting house prices).
  * *Classification*: Predicting a category (e.g., classifying an email as Spam or Not Spam).
* **Unsupervised Learning**: You give the model unlabeled data, and it finds patterns on its own.
  * *Clustering*: Grouping customers into different segments based on buying habits.

---

## 🚀 How to Introduce Yourself (90-Second Pitch)

When they say, *"Tell me about yourself,"* use this structured response:

> "Hello! I am Reena, a final-year B.Tech student in Computer Science and Engineering. Over the past year, I have focused on building backend systems and exploring Generative AI and RAG architectures.
> 
> Recently, I built a **Company-based RAG Chatbot** using FastAPI, LangChain, and ChromaDB. I scraped web pages, chunked the text, and generated semantic embeddings to enable accurate, domain-specific retrieval, which helped reduce AI hallucinations.
> 
> I also have practical data preprocessing and analysis experience from my internship with the VOIS program, and I hold certifications like **AWS Cloud Essentials** and **OCI AI Foundations**.
> 
> I am very excited about this role because it allows me to combine my passion for AI/ML with cloud technologies using services like Amazon Bedrock and SageMaker. I look forward to learning from the engineering team at Royal Cyber."

---

## 💬 Practice Interview Questions & Answers

### Q1: What is the difference between Amazon Bedrock and Amazon SageMaker?
* **Answer**: "Amazon Bedrock is a fully managed service that provides API access to pre-trained, high-performing foundation models from companies like Anthropic, Meta, and Amazon. It is best when you want to build generative AI applications quickly without training your own models. On the other hand, Amazon SageMaker is a complete machine learning platform where you can build, train, and deploy custom models from scratch. If I want to use Claude for a chatbot, I use Bedrock. If I want to train a custom model to predict customer churn, I use SageMaker."

### Q2: You built a RAG chatbot. How would you explain what RAG is to a non-technical person?
* **Answer**: *(Use the Open-Book Exam analogy)* "Imagine an AI is taking an open-book exam. Usually, the AI writes answers based only on what it remembers (which might be outdated or incorrect). With RAG, we first give the AI a search engine. When a question is asked, the AI searches our private database for relevant documents, reads them, and then writes an accurate answer based on those documents. This prevents it from making up facts."

### Q3: What are 'hallucinations' in LLMs, and how does RAG help reduce them?
* **Answer**: "Hallucination is when an LLM generates confident but incorrect or fabricated facts. This happens because LLMs predict the next most probable word rather than checking factual databases. RAG reduces hallucinations by providing the LLM with relevant, ground-truth context retrieved from our vector database, forcing the LLM to base its answer only on that trusted information."

### Q4: What is a Vector Database (like ChromaDB which you used) and why do we need it?
* **Answer**: "Traditional databases search for exact matches (like searching for the word 'dog'). A vector database stores text as numerical coordinates (embeddings) that capture the *semantic meaning* of words. This allows us to do semantic search. For example, if a user searches for 'puppy', a vector database understands that it is close in meaning to 'dog' and retrieves relevant articles even if the exact word 'puppy' isn't in them."

---

## 💡 Final Interview Tips
1. **Be Honest**: If they ask you about a tool you don't know (like a specific SageMaker feature), don't panic. Say: *"I haven't used that specific feature yet, but based on my experience with LangChain and general AWS Cloud Essentials, I understand the core concepts and am very eager to learn it quickly."*
2. **Slow Down**: When answering, take 2 seconds to structure your thoughts. Speak clearly.
3. **Ask Questions**: At the end, ask them:
   * *"What are some of the active projects the intern team will work on using Amazon Bedrock?"*
   * *"What does a typical day look like for an AWS AI Intern at Royal Cyber?"*
