# Fashion.AI (Raise Hackathon Project)

Fashion.AI is a fashion-focused AI-powered e-commerce MVP. Users can upload an image (and optionally use voice input) to receive product matches and styled recommendations.

This repo is organized into:
- `frontend/`: web UI
- `backend/`: FastAPI API + AI orchestration layer

## What’s in this repo (and what isn’t)

Some older docs mention Next.js and an `ai-model/` module. In **this** repo:
- The frontend is **Vite + React Router** (not Next.js).
- There is **no `ai-model/` folder**; AI logic lives under `backend/`.

## Tech stack (actual)

### Frontend (`frontend/`)
- **Framework**: React 18 + TypeScript
- **Build tool**: Vite (React SWC plugin)
- **Routing**: React Router
- **UI**: shadcn/ui (Radix UI primitives)
- **Styling**: Tailwind CSS + `tailwindcss-animate`
- **Server state**: TanStack React Query
- **Forms/validation**: React Hook Form + Zod

### Backend (`backend/`)
- **Language**: Python (Docker uses Python 3.10)
- **API framework**: FastAPI
- **Server**: Uvicorn
- **DB / metadata**: Supabase
- **Auth**: JWT (`python-jose`)
- **Jobs / scheduler**: APScheduler
- **HTTP client**: httpx
- **Config**: python-dotenv

### AI / retrieval (in backend)
- **Deep learning**: PyTorch
- **Object detection**: Ultralytics YOLO
- **Image processing**: OpenCV, Pillow
- **Embeddings / multimodal**: Transformers + OpenAI CLIP (GitHub install)
- **Vector search**: FAISS (CPU via `faiss-cpu`)
- **LLM API**: Groq SDK

## Project structure

- `frontend/` — React app (Vite)
- `backend/` — FastAPI app (`main.py`), handlers/controllers/services, agents, and scheduled jobs
- `docker-compose.yaml` — containerized backend (port `8000`)

## Setup

### 1) Environment variables

Copy `./.env.example` to `./.env` and fill in values.

Notes:
- The backend loads environment variables from `../.env` (so when you run from `backend/`, it expects `Fashion.AI/.env`).
- The frontend (Vite) expects `VITE_*` variables in `frontend/.env` (or your shell env) at dev/build time.

Key variables used by this repo:
- `SUPABASE_URL`, `SUPABASE_KEY`
- `GROQ_API_KEY`
- `SECRET_KEY`, `ALGORITHM` (JWT)
- `VITE_BACKEND_URL` (used by the frontend)

There are additional agent-specific `.env.example` files under `backend/agents/*/`.

### 2) Run the backend (local, recommended for dev)

```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

### 3) Run the frontend

```bash
cd frontend
npm install
npm run dev
```

## Run with Docker (backend only)

`docker-compose.yaml` currently runs the **backend** service on port `8000`.

```bash
docker compose up --build
```

Make sure the backend’s environment variables are available to the container (e.g. export them in your shell/session before running Compose, or extend `docker-compose.yaml` with an `environment:` / `env_file:` section).

## Scheduled job (user profile summarization)

The hourly user-profile summarization job lives under `backend/jobs/`.

From `backend/`:

```bash
python jobs/user_profile_summary_job_scheduler.py
```

## Docs / links

- **Contributing / running notes**: `Contributing.md`
- **Slides**: `./assets/Raise%20Your%20Hackathon%20Team%20Jroq-Prosus.pdf`

## Deployment note

If you’re looking for the team’s deployed version reference, an earlier note points to a personal deployment repo:
`https://github.com/ShuhaoZQGG/Fashion.AI`
