# RAG HR Chatbot

This project is a **Retrieval-Augmented Generation (RAG) chatbot** that answers employee queries based on an HR Policy document.  
It demonstrates how HR teams can deploy an internal AI assistant that retrieves policy information and generates concise, citation-backed answers.

---

## 📌 Features
- **PDF ingestion**: extract and clean HR policy text  
- **Chunking**: split into overlapping chunks for semantic search  
- **Embeddings + FAISS**: vector index for fast retrieval  
- **Re-ranking**: hybrid approach (BM25 + cosine similarity) for accuracy  
- **Backend**: FastAPI `/query` endpoint (answer, sources, latency)  
- **Frontend**: Streamlit chat-like interface  
- **Caching**: in-memory LRU cache (with optional Redis)  
- **Dockerized**: single `docker-compose up` runs everything  

---

## 🗂 Project Structure
rag-hr-chatbot/
├─ data/
│ ├─ hr_policy.pdf # Input HR Policy
│ └─ extracted_text/ # Intermediate JSON
├─ ingestion/ # Extraction + cleaning + chunking
├─ embeddings/ # Embeddings (OpenAI / sentence-transformers)
├─ index/ # FAISS index utilities
├─ reranker/ # BM25 + cosine reranker
├─ backend/ # FastAPI backend + cache
├─ frontend/ # Streamlit UI
├─ Dockerfile
├─ docker-compose.yml
├─ requirements.txt
└─ README.md

yaml
Copy code

---

## ⚡ Quickstart

### Local Run
```bash
# 1. Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate   # Linux/macOS
.venv\Scripts\activate      # Windows

# 2. Install dependencies
pip install -r requirements.txt
Add your HR policy at:

bash
Copy code
data/hr_policy.pdf
Build the index:

bash
Copy code
# Extract text
python ingestion/extract_text.py --pdf data/hr_policy.pdf --out data/extracted_text/hr_policy_pages.json

# Chunk text
python ingestion/chunker.py --pages data/extracted_text/hr_policy_pages.json --out data/extracted_text/chunks.json

# Build FAISS index
python index/build_faiss.py --chunks data/extracted_text/chunks.json --out_dir index
Run services:

bash
Copy code
# Backend
uvicorn backend.api:app --reload   # → http://localhost:8000/docs

# Frontend
streamlit run frontend/app.py      # → http://localhost:8501
Docker (Recommended)
bash
Copy code
docker-compose up --build
Backend → http://localhost:8000

Frontend → http://localhost:8501

🔑 Configuration
OpenAI API Key (optional, improves quality):

bash
Copy code
export OPENAI_API_KEY="your_api_key"
Embeddings: text-embedding-3-small

LLM: gpt-4o-mini

Offline mode: uses sentence-transformers + deterministic summarizer if no API key is set.

🎯 Demo Scenarios
“How many earned leaves do I get?” → 18 per year (with citation)

“What is the maternity leave policy?” → 26 weeks if employed >1 year (with citation)

“Can I get 1 year of paternity leave?” → Graceful fallback answer

Repeat queries → Demonstrates cache (faster response)

📊 Tech Stack
Python 3.11

FAISS (vector search)

rank_bm25 (lexical reranker)

FastAPI (backend)

Streamlit (frontend)

Docker + docker-compose

✅ Notes
Keep /data private (contains HR documents)

Add authentication for production deployments

Ensure no sensitive data (PII) is embedded

✨ Recruiter Value
This project highlights my ability to:

Ingest & preprocess unstructured data (PDF policies)

Build retrieval pipelines with FAISS + reranking

Integrate LLMs for grounded, reliable answers

Deliver a full-stack solution (backend + frontend + Docker)

Balance demo simplicity with production-readiness

🔗 GitHub Repo: github.com/faisalimam1/rag-hr-chatbot
