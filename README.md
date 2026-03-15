<div align="center">

# 🧠 WikiMind

### RAG Knowledge Base Chatbot

A production-grade Retrieval-Augmented Generation system. Upload any documents — PDF, Markdown, DOCX, CSV — and chat with them instantly. Built entirely on free, open-source tooling. Deployable in minutes.

[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![ChromaDB](https://img.shields.io/badge/ChromaDB-FF6F00?style=for-the-badge&logo=databricks&logoColor=white)](https://www.trychroma.com/)
[![HuggingFace](https://img.shields.io/badge/HuggingFace-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black)](https://huggingface.co/)
[![Groq](https://img.shields.io/badge/Groq-000000?style=for-the-badge&logo=groq&logoColor=white)](https://groq.com/)
[![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge)](LICENSE)
[![Cost](https://img.shields.io/badge/Cost-$0%2Fmonth-brightgreen?style=for-the-badge)](#features--stack)

</div>

---

## Architecture

```
INGEST                        RETRIEVE                      GENERATE
─────                        ────────                      ────────
PDF · DOCX · MD              Query Embedding               Context Assembly
HTML · CSV · JSON · TXT      (same model)                  System Prompt
        │                           │                      + Source Labels
        ▼                           ▼                      + Chat History
Recursive Chunker            ChromaDB ANN Search                  │
512 tokens · 64 overlap      HNSW index                           ▼
        │                           │                      Groq llama-3.1-8b
        ▼                           ▼                      or Ollama local
BAAI/bge-small-en-v1.5       Top-5 Chunks                        │
33M params · 384 dims        score ≥ 0.30                         ▼
        │                                                  Server-Sent Events
        ▼                                                  Token streaming
ChromaDB (cosine)                                                 │
                                                                  ▼
                                                            React UI
```

## Features & Stack

| Feature | Implementation | Cost |
|---|---|---|
| Multi-format ingestion | PDF, DOCX, Markdown, HTML, CSV, JSON, TXT | $0 |
| Embeddings | BAAI/bge-small-en-v1.5 (HuggingFace) — 33M params, 384 dims | $0 |
| Vector Store | ChromaDB — local, persistent, cosine similarity via HNSW | $0 |
| LLM Inference | Groq free tier (6k req/day) OR Ollama local — OpenAI fallback | $0 |
| Streaming | Server-Sent Events → real-time typewriter effect in React UI | $0 |
| Source Citations | Every answer shows document name + relevance % score | $0 |
| REST API | FastAPI + Pydantic · OpenAPI docs auto-generated at `/api/docs` | $0 |
| CI/CD Pipeline | GitHub Actions → pytest → build → HuggingFace Spaces deploy | $0 |
| Docker | CPU-only PyTorch image · pre-downloads embedding model at build | $0 |

## Quick Start

### 1. Get a Free Groq API Key

Head to [console.groq.com](https://console.groq.com) — no credit card required. 6,000 requests/day free.

### 2. Clone & Install

```bash
git clone https://github.com/vikramjeet/wikimind.git
cd wikimind
pip install torch --index-url https://download.pytorch.org/whl/cpu
pip install -r backend/requirements.txt
```

### 3. Configure Environment

```bash
cp backend/.env.example backend/.env
# Paste your GROQ_API_KEY into backend/.env
```

### 4. Start the Backend

```bash
uvicorn app.main:app --reload --port 8000
# API docs → http://localhost:8000/api/docs
```

### 5. Start the Frontend

```bash
cd frontend && npm install && npm run dev
# UI → http://localhost:5173
```

### 6. Upload & Chat

Drag any PDF, DOCX, or Markdown file into the sidebar and start asking questions.

## Project Structure

```
wikimind/
├── backend/
│   ├── app/
│   │   ├── api/              # chat.py · documents.py · health.py
│   │   ├── core/             # vector_store · llm_client · chunker · config
│   │   ├── services/         # rag_service.py (RAG orchestrator)
│   │   └── main.py           # FastAPI app entry point
│   ├── tests/                # pytest suite with mocks
│   └── requirements.txt
├── frontend/src/App.jsx      # React streaming chat UI
├── docker/Dockerfile         # CPU PyTorch · pre-downloads embeddings
├── .github/workflows/        # CI/CD → test → build → HF Spaces deploy
└── README.md
```

## Demo Scenarios

Six real experiments captured from the live system:

### 01 · Document Summarization

> Fed a 200-page novel (*Pride and Prejudice*) into the system — retrieved the most relevant chunks and synthesised a structured summary with Key Events, Characters, and Themes.

### 02 · Factual Retrieval from Research Papers

> Asked "What problem does RAG solve?" across multiple loaded papers. The system located the exact answer inside a 15-page academic PDF, showing `RAG_Research_paper.pdf` at 69% relevance with source attribution.

### 03 · Benchmark Result Extraction

> Extracted all 8 benchmark datasets and exact Natural Questions scores (RAG-Sequence 44.5 EM, RAG-Token 44.1 EM) from a dense results table buried on page 5.

### 04 · Cross-Document Synthesis

> Asked how the Transformer architecture relates to RAG's answer generation. Retrieved from **both** `transformers_paper.pdf` (74%) and `RAG_Research_paper.pdf` (73%) simultaneously, synthesising a coherent cross-document explanation.

### 05 · Personal Resume Q&A

> Loaded a resume alongside a research paper. The system cross-referenced professional experience with RAG concepts, identifying relevant enterprise RAG work at 68% relevance.

### 06 · Conversational Memory + Anti-Hallucination

> Multi-turn follow-ups correctly resolved pronoun references from chat history. When asked about the FIFA World Cup (not in any loaded document), the system responded: *"The provided context does not contain this information."*

## Resume Bullet Point

> Built WikiMind, a production RAG system using FastAPI, ChromaDB, and HuggingFace sentence-transformers; supports multi-format document ingestion (PDF/DOCX/Markdown), real-time streaming responses via Server-Sent Events, and multi-provider LLM switching (Groq/Ollama/OpenAI); deployed on HuggingFace Spaces with GitHub Actions CI/CD — zero infrastructure cost.

## License

[MIT](LICENSE)

---

<div align="center">

**Built by [Vikramjeet Singh Raina](https://github.com/vikramjeet)**

FastAPI · ChromaDB · sentence-transformers · React · Groq · HuggingFace Spaces · Docker · GitHub Actions

</div>
