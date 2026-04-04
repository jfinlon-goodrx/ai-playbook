> Consolidated from Local-AI-Stack reorganization

# MySty-RAG: Local Retrieval-Augmented Generation

## Overview

MySty integrates with Ollama to enable local Retrieval-Augmented Generation (RAG), combining document retrieval with LLM inference using models like Mistral or LLaMA. It's modular, private, and optimized for self-hosted setups.

---

## What Is RAG with MySty + Ollama?

**Retrieval-Augmented Generation (RAG)** enhances LLMs by grounding their responses in external documents. Instead of relying solely on pretraining, RAG fetches relevant context from your own data (PDFs, notes, etc.) and feeds it into the model for more accurate answers.

**MySty** is a modular framework that orchestrates this process, while **Ollama** runs the LLM locally---no cloud, no API costs.

---

## Architecture Overview

- **Ollama** runs as a Docker container exposing an API on port `11434`.
- **MySty** uses LangChain to interface with Ollama via its API.
- **Vector Store** (e.g., FAISS or ChromaDB) stores document embeddings.
- **RetrievalQA Chain** combines the retriever and Ollama model to generate answers.

### Core Components

| Layer | Tool/Module | Role |
|---|---|---|
| **LLM Inference** | Ollama (`mistral`, `llama3`) | Local model execution |
| **Orchestration** | MySty (LangChain-based) | Chains, prompts, routing |
| **Embedding** | `sentence-transformers` or `ollama-embed` | Converts docs to vectors |
| **Vector Store** | FAISS / ChromaDB | Stores and retrieves chunks |
| **Frontend/API** | FastAPI / Streamlit | UI or endpoint for queries |

### Benefits

- **Privacy-first**: No data leaves your machine.
- **Cost-free inference**: No API or token usage fees.
- **Model flexibility**: Swap models easily via config.
- **Offline capability**: Fully functional without internet.

Sources: [DeepWiki Ollama Integration](https://deepwiki.com/ajalaris/rag/3.2-ollama-llm-integration), [DEV Guide to Local RAG](https://dev.to/the_aayush_mishra/setting-up-rag-locally-with-ollama-a-beginner-friendly-guide-428m), [Ollama-RAG-Sync GitHub](https://github.com/Ollama-RAG-Sync/Ollama-RAG-Sync)

---

## Repo Structure

```bash
rag-mysty-ollama/
├── data/                  # Raw documents (PDFs, TXT, MD)
├── embeddings/            # Persisted vector store
├── models/                # Ollama model configs
├── mysty/                 # LangChain chains, retrievers
│   ├── loader.py          # Document loader + chunker
│   ├── embedder.py        # Embedding logic
│   ├── retriever.py       # FAISS/Chroma setup
│   └── qa_chain.py        # RAG pipeline
├── server/                # FastAPI or Streamlit app
│   └── main.py
├── docker-compose.yml     # Ollama container
└── README.md
```

---

## Ollama Setup

```yaml
# docker-compose.yml
services:
  ollama:
    image: ollama/ollama
    ports:
      - "11434:11434"
    volumes:
      - ./models:/root/.ollama
```

Pull models:

```bash
ollama pull mistral:7b-instruct
```

---

## LangChain RAG Chain

```python
from langchain.chains import RetrievalQA
from langchain.vectorstores import FAISS
from langchain.embeddings import OllamaEmbeddings
from langchain.llms import Ollama

llm = Ollama(
    base_url="http://localhost:11434",
    model="mistral:7b-instruct",
    temperature=0.2,
    system="You are a helpful assistant."
)

retriever = FAISS.load_local("embeddings/").as_retriever()
qa_chain = RetrievalQA.from_chain_type(llm=llm, retriever=retriever)
```

---

## Document Ingestion

```python
from langchain.document_loaders import TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter

loader = TextLoader("data/my_notes.txt")
docs = loader.load()

splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
chunks = splitter.split_documents(docs)

embeddings = OllamaEmbeddings(model="mistral")
db = FAISS.from_documents(chunks, embeddings)
db.save_local("embeddings/")
```

---

## Query Flow

```python
query = "What are the key points from my notes?"
response = qa_chain.run(query)
print(response)
```

---

## Optional Enhancements

- **Metadata filtering**: tag chunks by source, date, or topic.
- **Streaming UI**: use Streamlit for live chat interface.
- **Multi-model routing**: swap Ollama models based on query type.

---

## FastAPI Scaffold for Local RAG

### Directory Layout

```bash
server/
├── main.py              # FastAPI app entrypoint
├── rag_router.py        # RAG query endpoint
├── config.py            # Env + model config
├── ollama_client.py     # Ollama API wrapper
├── retriever.py         # FAISS retriever logic
├── qa_chain.py          # LangChain RAG chain
└── .env                 # Model + port config
```

### `.env` Example

```env
OLLAMA_URL=http://localhost:11434
OLLAMA_MODEL=mistral:7b-instruct
CHUNK_SIZE=500
CHUNK_OVERLAP=50
VECTOR_DB_PATH=../embeddings/
```

### `config.py`

```python
import os
from dotenv import load_dotenv

load_dotenv()

OLLAMA_URL = os.getenv("OLLAMA_URL")
OLLAMA_MODEL = os.getenv("OLLAMA_MODEL")
CHUNK_SIZE = int(os.getenv("CHUNK_SIZE", 500))
CHUNK_OVERLAP = int(os.getenv("CHUNK_OVERLAP", 50))
VECTOR_DB_PATH = os.getenv("VECTOR_DB_PATH")
```

### `ollama_client.py`

```python
from langchain.llms import Ollama
from config import OLLAMA_URL, OLLAMA_MODEL

def get_llm():
    return Ollama(
        base_url=OLLAMA_URL,
        model=OLLAMA_MODEL,
        temperature=0.2,
        system="You are a helpful assistant."
    )
```

### `retriever.py`

```python
from langchain.vectorstores import FAISS
from langchain.embeddings import OllamaEmbeddings
from config import VECTOR_DB_PATH, OLLAMA_MODEL

def get_retriever():
    embedder = OllamaEmbeddings(model=OLLAMA_MODEL)
    db = FAISS.load_local(VECTOR_DB_PATH, embedder)
    return db.as_retriever()
```

### `qa_chain.py`

```python
from langchain.chains import RetrievalQA
from ollama_client import get_llm
from retriever import get_retriever

def get_qa_chain():
    return RetrievalQA.from_chain_type(
        llm=get_llm(),
        retriever=get_retriever()
    )
```

### `rag_router.py`

```python
from fastapi import APIRouter, Query
from qa_chain import get_qa_chain

router = APIRouter()
qa_chain = get_qa_chain()

@router.get("/rag")
def query_rag(q: str = Query(..., description="Your question")):
    response = qa_chain.run(q)
    return {"query": q, "response": response}
```

### `main.py`

```python
from fastapi import FastAPI
from rag_router import router

app = FastAPI(title="MySty RAG API")
app.include_router(router, prefix="/api")
```

### Launch Script

```bash
uvicorn server.main:app --host 0.0.0.0 --port 8000 --reload
```

---

This setup is modular, repo-safe, and ready for GPU scaling.
