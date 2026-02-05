# RAG-QA System

A compact retrieval-augmented generation (RAG) demo that lets you upload documents and ask question(s) whose answers are grounded only in the uploaded content.

Why this exists: LLMs can hallucinate. This project constrains answers to retrieved passages so responses stay grounded in source documents.

--

## Preview 

<img width="1902" height="866" alt="Screenshot 2026-02-05 121259" src="https://github.com/user-attachments/assets/fc76048b-bcf8-4b55-a3ea-63cd40781201" />

## Quickstart

1. Create and activate a Python 3.10+ virtual environment.

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1   # PowerShell (Windows)
```

2. Install dependencies:

```powershell
pip install -r requirements.txt
```

3. Copy and configure environment variables (edit keys as needed):

```powershell
copy .env.example .env
# edit .env
```

4. Start the backend and the UI (optional):

```powershell
# API (FastAPI/uvicorn)
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000

# UI (Streamlit)
streamlit run ui/app.py
```

API docs: http://127.0.0.1:8000/docs
Streamlit UI: http://localhost:8501

--

## Features

- Upload PDF/TXT documents for ingestion
- Background ingestion and FAISS-based vector search
- Embeddings-based retrieval + LLM answer generation
- Responses include cited source chunks when available

--

## Basic Usage (HTTP)

Upload a file:

```bash
curl -X POST "http://127.0.0.1:8000/api/upload" -F "file=@/path/to/doc.pdf"
```

Ask a question:

```bash
curl -X POST "http://127.0.0.1:8000/api/query" -H "Content-Type: application/json" -d '{"question":"What does the document say about X?"}'
```

Typical response contains `answer` and `sources` (source file + chunk id).

--

## Project Structure (high level)

- `app/` — FastAPI app, routes, and services
- `app/services/` — ingestion, chunking, embeddings, retrieval, LLM glue
- `data/` — persisted FAISS index and uploaded files
- `ui/` — Streamlit demo UI
- `test_e2e.py` — end-to-end smoke tests

--

## Development notes

- Chunking and overlap are tuned for reasonable retrieval quality; adjust in `app/services/chunking.py`.
- FAISS index persists under `data/faiss_index/` — back up if needed.
- No authentication by default — add a reverse proxy or auth middleware for production.

--

## Contribute / Next steps

- Add authentication and rate limiting
- Add larger-scale tests and CI
- Introduce async ingestion workers for higher throughput

--

Licensed for demo use. For questions or help, open an issue or reach out in the project workspace.
