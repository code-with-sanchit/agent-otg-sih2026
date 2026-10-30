# Agent OTG

**Offline agentic AI workbench for industrial teams.**
Runs fully on your own machine. No cloud, no internet needed.

Smart India Hackathon 2026 | Problem Statement 26117 | MRPL

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-backend-009688)
![LangGraph](https://img.shields.io/badge/LangGraph-agents-purple)
![Ollama](https://img.shields.io/badge/Ollama-local%20LLM-black)
![Offline](https://img.shields.io/badge/Mode-100%25%20Offline-green)

---

## 1. What is Agent OTG?

Many industrial sites cannot send data to the cloud because of security rules.
Agent OTG gives them an AI assistant that works **completely offline**.

You can chat with it, upload files, ask questions about your own documents,
and let AI agents use tools to finish tasks. All data stays on your machine.

## 2. Key Features

- **Offline first:** an offline guard blocks outside network calls (TODO: confirm behavior in `offline_guard.py`)
- **Local LLM:** runs models through Ollama
- **Agent workflows:** built with LangGraph, with a router that picks the right path
- **Tools for agents:** the agent can call tools to do real work (see `backend/tools.py`)
- **Document Q&A (RAG):** search your own files using a local vector database (ChromaDB)
- **File reading:** reads uploaded files (TODO: list supported types from `file_readers.py`)
- **Artifacts:** generates outputs such as documents or code that you can reuse
- **Image generation:** local image generation module (TODO: confirm model used)
- **Chat history:** sessions are saved in a local SQLite database
- **Sync hub:** optional sync between machines (TODO: describe what `sync_hub.py` does)
- **Web UI:** React and Vite frontend (TODO: confirm React)

## 3. How It Works

```
 User (Browser)
      |
      v
 Frontend (Vite)  --->  FastAPI Backend
                             |
                      Router (picks path)
                             |
                     LangGraph Agent
                      /      |       \
                  Tools    RAG      Ollama
                          (Chroma)  (local LLM)
                             |
                      SQLite (chat history)
```

Simple flow:

1. You send a message from the UI.
2. The backend router decides what kind of task it is.
3. The LangGraph agent plans the steps.
4. It may use tools, search your documents, or ask the local model.
5. The answer comes back to the UI, and the chat is saved.

## 4. Tech Stack

| Part | Technology |
|------|------------|
| Backend | Python, FastAPI |
| Agents | LangGraph |
| Local LLM | Ollama |
| Vector DB | ChromaDB |
| Database | SQLite |
| Frontend | Vite (TODO: React?), ESLint |
| Tests | Pytest |
| Scripts | PowerShell (Windows) |

## 5. Project Structure

```
agent-otg-sih2026/
|-- backend/
|   |-- main.py              # FastAPI app entry
|   |-- router.py            # Decides which path a request takes
|   |-- langgraph_agent.py   # Agent graph
|   |-- tools.py             # Tools the agent can use
|   |-- offline_guard.py     # Keeps the system offline
|   |-- file_readers.py      # Reads uploaded files
|   |-- artifacts.py         # Output artifacts
|   |-- image_gen.py         # Image generation
|   |-- sync_hub.py          # Sync logic
|   |-- db.py, setup_db.py   # Database code and setup
|   |-- config.py            # Settings
|   |-- system_prompt.txt    # Base prompt for the agent
|   |-- rag/                 # Document search code
|   |-- tests/               # Backend tests
|   |-- docs/                # Extra notes
|   `-- requirements*.txt    # Dependencies
|-- frontend/
|   |-- src/                 # UI source code
|   |-- public/              # Static files
|   `-- vite.config.js
|-- start-agent-otg.ps1      # Starts the full app
|-- start-demo.ps1           # Starts the demo
`-- README.md
```

## 6. Requirements

- Python 3.10 or newer (TODO: confirm your version)
- Node.js 18 or newer
- [Ollama](https://ollama.com) installed
- At least one model pulled, for example: `ollama pull llama3` (TODO: use your real model name)
- Windows with PowerShell for the start scripts

## 7. Setup

**Step 1: Clone the repo**

```bash
git clone https://github.com/code-with-sanchit/agent-otg-sih2026.git
cd agent-otg-sih2026
```

**Step 2: Backend**

```bash
cd backend
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Optional extras:

```bash
pip install -r requirements-rag.txt    # for document search
pip install -r requirements-dev.txt    # for testing
```

**Step 3: Environment file**

```bash
copy .env.example .env
```

Open `.env` and fill in your values. Never upload this file to GitHub.

**Step 4: Create the database**

```bash
python setup_db.py
```

**Step 5: Frontend**

```bash
cd ../frontend
copy .env.example .env
npm install
```

## 8. Run the App

**Easy way (Windows):**

```powershell
./start-agent-otg.ps1
```

**Manual way:**

```bash
# Terminal 1: start Ollama
ollama serve

# Terminal 2: backend
cd backend
uvicorn main:app --reload --port 8000

# Terminal 3: frontend
cd frontend
npm run dev
```

Then open the link that Vite shows (usually `http://localhost:5173`).
Backend docs are at `http://localhost:8000/docs` (check your port).

**Demo mode:**

```powershell
./start-demo.ps1
```

## 9. Run Tests

```bash
cd backend
pytest
```

## 10. Offline Promise

- All models run locally through Ollama.
- The vector database and chat history are stored on your disk.
- The offline guard is there to stop outside network calls.
- Your documents never leave your machine.

## 11. Known Limits

- Answer quality depends on the local model you choose.
- Bigger models need a good GPU or lots of RAM.
- TODO: add the limits you actually found while testing.

## 12. Roadmap

- [ ] Support more file types
- [ ] Better agent tools for plant operations
- [ ] Role-based access
- [ ] Linux and macOS start scripts
- [ ] TODO: add your real plans



## 15. Acknowledgements

- Smart India Hackathon 2026
- MRPL (problem statement provider)
- Ollama, LangGraph, FastAPI and ChromaDB communities
