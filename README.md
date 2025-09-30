# RAG HR Chatbot

This project is a **Retrieval-Augmented Generation (RAG) chatbot** that answers employee queries based on an HR Policy document.  
It demonstrates how HR teams can deploy an internal AI assistant that retrieves policy information and generates concise, citation-backed answers.
## 📌 Features
- PDF ingestion: extract and clean HR policy text
- Chunking: split into overlapping chunks for semantic search
- Embeddings + FAISS: vector index for fast retrieval
- Re-ranking: BM25 + cosine similarity hybrid approach
- Backend: FastAPI `/query` endpoint (answers + sources)
- Frontend: Streamlit chat-like interface
- Caching: in-memory LRU cache (with optional Redis)
- Dockerized: one `docker-compose up` runs everything
## 🗂 Project Structure
rag-hr-chatbot/
├─ data/
│  ├─ hr_policy.pdf  
│  └─ extracted_text/       
├─ ingestion/              
├─ embeddings/          
├─ index/                  
├─ reranker/           
├─ backend/         
├─ frontend/         
├─ Dockerfile
├─ docker-compose.yml
├─ requirements.txt
└─ README.md
##  Quickstart

### Local Run
```bash
# Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate   # Linux/macOS
.venv\Scripts\activate      # Windows

# Install dependencies
pip install -r requirements.txt
#Add your HR policy at:
data/hr_policy.pdf
#Build the index:
# Extract text
python ingestion/extract_text.py --pdf data/hr_policy.pdf --out data/extracted_text/hr_policy_pages.json

# Chunk text
python ingestion/chunker.py --pages data/extracted_text/hr_policy_pages.json --out data/extracted_text/chunks.json

# Build FAISS index
python index/build_faiss.py --chunks data/extracted_text/chunks.json --out_dir index
# Run services:
# Backend
uvicorn backend.api:app --reload   # → http://localhost:8000/docs

# Frontend
streamlit run frontend/app.py      # → http://localhost:8501
docker-compose up --build
Backend → http://localhost:8000

Frontend → http://localhost:8501
```
## 🔑 Configuration
- **OpenAI API Key** (optional, improves quality)
  ```bash
  export OPENAI_API_KEY="your_api_key"
Embeddings: text-embedding-3-small

LLM: gpt-4o-mini

Offline mode: uses sentence-transformers + summarizer if no key is set
---

###  Demo Scenarios
```markdown
##  Demo Scenarios
- “How many earned leaves do I get?” → *18 per year* (with citation)  
- “What is the maternity leave policy?” → *26 weeks if employed >1 year* (with citation)  
- “Can I get 1 year of paternity leave?” → Graceful fallback answer  
- Repeat queries → Faster (cache demo)
##  Tech Stack
- Python 3.11
- FAISS (vector search)
- rank_bm25 (lexical reranker)
- FastAPI (backend)
- Streamlit (frontend)
- Docker + docker-compose
##  Notes
- Keep `/data` private (HR documents)
- Add authentication for production use
- Ensure no PII (sensitive data) is embedded

##  Recruiter Value
This project highlights my ability to:
- Ingest & preprocess unstructured data (PDFs)
- Build retrieval pipelines with FAISS + reranking
- Integrate LLMs for grounded answers
- Deliver full-stack solutions (backend + frontend + Docker)
- Balance demo simplicity with production-readiness
```
[GitHub Repo](https://github.com/faisalimam1/rag-hr-chatbot)

