# Multilingual Document Q&A using RAG

## Overview

This project is a Retrieval-Augmented Generation (RAG) application that allows users to upload documents and ask questions about their content. The system retrieves relevant document chunks from a vector database and uses a Large Language Model (LLM) to generate answers with citations.

The project is built using FastAPI, LangChain, ChromaDB, HuggingFace Embeddings, Google Gemini, and Streamlit.

---

## Features

* Upload PDF, DOCX and TXT documents
* Automatic document chunking
* Embedding generation using Sentence Transformers
* ChromaDB vector storage
* Semantic search
* Gemini-powered Question Answering
* Source citations
* Multilingual Question Answering
* Confidence score
* Streamlit user interface
* FastAPI backend
* Swagger API documentation

---

## Tech Stack

### Backend

* FastAPI
* Python
* LangChain
* ChromaDB
* Sentence Transformers
* Google Gemini API

### Frontend

* Streamlit

### Vector Database

* ChromaDB

### Embedding Model

* sentence-transformers/all-MiniLM-L6-v2

### LLM

* Gemini 2.0 Flash

---

# Project Structure

```text
RAG-Document-QA/
│
├── backend/
│   ├── api/
│   │   ├── upload.py
│   │   ├── ask.py
│   │   └── contradict.py
│   │
│   ├── services/
│   │   ├── file_service.py
│   │   ├── document_loader.py
│   │   ├── text_splitter.py
│   │   ├── embedding_service.py
│   │   ├── vector_store.py
│   │   ├── retriever.py
│   │   ├── llm_service.py
│   │   ├── translator.py
│   │   └── confidence_service.py
│   │
│   ├── prompts/
│   │   ├── rag_prompt.py
│   │   └── contradiction_prompt.py
│   │
│   ├── models/
│   │   └── schemas.py
│   │
│   ├── database/
│   │   └── chroma_db/
│   │
│   ├── documents/
│   │
│   ├── app.py
│   ├── config.py
│   └── requirements.txt
│
├── frontend/
│   └── streamlit_app.py
│
├── examples/
│
├── README.md
│
└── .env
```

---

# System Architecture

```
User
 │
 ▼
Streamlit UI
 │
 ▼
FastAPI Backend
 │
 ├─────────────── Upload ───────────────┐
 │                                      │
 ▼                                      │
File Upload                             │
 ▼                                      │
Document Loader                         │
 ▼                                      │
Text Splitter                           │
 ▼                                      │
Embedding Model                         │
 ▼                                      │
ChromaDB                                │
                                        │
Question                                │
 ▼                                      │
Retriever                               │
 ▼                                      │
Relevant Chunks                         │
 ▼                                      │
Gemini                                  │
 ▼                                      │
Answer + Citations
```

---

# Chunking Strategy

The uploaded documents are split using LangChain's Recursive Character Text Splitter.

Configuration:

* Chunk Size: 1000 characters
* Chunk Overlap: 200 characters

### Why this strategy?

* Preserves semantic meaning.
* Avoids breaking sentences.
* Improves retrieval accuracy.
* Reduces hallucinations.
* Produces better citation quality.

Each chunk stores metadata:

* document_id
* filename
* page number
* chunk_id
* source

---

# Retrieval Pipeline

```
Question

↓

Embedding

↓

ChromaDB Similarity Search

↓

Top-K Chunks

↓

Context Builder

↓

Gemini

↓

Answer
```

---

# Multilingual Flow

```
User Question

↓

Language Detection

↓

Translate to English

↓

Semantic Retrieval

↓

Gemini

↓

Translate Back

↓

Final Answer
```

---

# Citation Format

Each answer includes citations in the following format:

```
File : AI.pdf

Page : 4

Chunk : AI_chunk_008

Snippet :
Machine Learning is a subset of Artificial Intelligence...
```

---

# REST APIs

## Upload Document

POST

```
/upload
```

Response

```json
{
    "message":"Document uploaded successfully",
    "document_id":"xxxxxxxx",
    "pages_loaded":12,
    "chunks_created":35
}
```

---

## Ask Question

POST

```
/ask
```

Request

```json
{
    "question":"What is Machine Learning?"
}
```

Response

```json
{
    "answer":"Machine Learning is a subset of Artificial Intelligence.",

    "confidence":0.93,

    "language":"en",

    "citations":[]
}
```

---

## Contradict Documents

POST

```
/contradict
```

This endpoint is currently a placeholder and is intended for future enhancement.

---

# Installation

Clone repository

```bash
git clone <repository-url>
```

Create virtual environment

```bash
python -m venv venv
```

Activate

Windows

```bash
venv\Scripts\activate
```

Install dependencies

```bash
pip install -r requirements.txt
```

Create `.env`

```env
GEMINI_API_KEY=YOUR_API_KEY

MODEL_NAME=gemini-2.0-flash

EMBEDDING_MODEL=sentence-transformers/all-MiniLM-L6-v2

UPLOAD_DIRECTORY=documents

CHROMA_DB_PATH=database/chroma_db

TOP_K=5

SIMILARITY_THRESHOLD=0.45
```

Run backend

```bash
cd backend

uvicorn app:app --reload
```

Backend URL

```
http://127.0.0.1:8000
```

Swagger

```
http://127.0.0.1:8000/docs
```

Run frontend

```bash
cd frontend

streamlit run streamlit_app.py
```

Frontend URL

```
http://localhost:8501
```

---

# Example Workflow

1. Upload a PDF.
2. The document is parsed.
3. Text is divided into chunks.
4. Embeddings are generated.
5. Chunks are stored in ChromaDB.
6. Ask a question.
7. The retriever finds relevant chunks.
8. Gemini generates an answer.
9. Citations are returned.

---

# Future Improvements

* Complete contradiction detection endpoint.
* Reranking model.
* Human-in-the-loop approval for low confidence.
* Retrieval evaluation dataset.
* Docker deployment.
* Authentication and user management.

---

# Known Limitations

* The contradiction endpoint is currently a placeholder.
* Duplicate detection is filename-based.
* Performance depends on the quality of uploaded documents.
* Large documents may increase indexing time.

---

# Author

**Sanket Pandit**

B.E. Artificial Intelligence & Data Science

Final Year Project – Retrieval-Augmented Generation (RAG) System
