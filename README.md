## 🚀 Fintech Frontend + Backend Prototype

**Fintech** is a hackathon prototype designed to connect startups with investors using AI in a FinTech ecosystem. It features a JavaScript-based frontend (built with Vite) and a Python FastAPI backend. Startups can upload their pitch decks as PDFs, and the system intelligently matches them with investors based on their investment thesis and preferences.

To enable rapid development, the backend uses an in-memory data store (a simple Python dictionary) instead of a full database. While this keeps the prototype fast and lightweight, it is designed to be replaced with a persistent database in a production-ready system. Additionally, the platform integrates an AI-powered introduction service — using the `/intro/{startup_id}/{investor_id}` endpoint, it generates personalized draft emails to connect startups with relevant investors.

---

## 🏗️ System Architecture

The application follows a **three-layer architecture**:

- **Frontend (Client Layer):**  
  A browser-based UI (powered by Vite) where users can sign in as either a startup or an investor.

- **Backend (API Layer):**  
  Built using FastAPI, it handles file uploads, user data, matching logic, and AI-based services.

- **Data Layer:**  
  Uses an in-memory data structure for storing startup and investor information during the prototype phase.

### 🔄 Workflow

- Startups upload pitch decks (PDF) via `POST /startup/upload-deck`  
- Investors register via `POST /investor/register` with details like firm name, thesis, and preferences  
- Matching system:
  - `GET /startup/{id}/matches` → finds relevant investors  
  - `GET /investor/{id}/dealflow` → shows startup pipeline  

- AI integration:
  - `GET /intro/{startup_id}/{investor_id}` → generates a personalized intro email  

All API routes are auto-documented using FastAPI’s Swagger UI at `/docs`, making testing and exploration easy.

---

## 🔐 Authentication Flow

This prototype uses a **simplified local authentication system**:

- **Login:**  
  Users can enter any non-empty name, email, and password to proceed

- **Role Selection:**  
  - Startup → redirected to Startup Dashboard  
  - Investor → redirected to Investor Dashboard  

- **Session Management:**  
  - Stored in `localStorage`  
  - Logout clears session data  

- **Dashboards:**  
  - Startups → upload pitch decks  
  - Investors → register preferences and view dealflow  

---

## 📡 Key API Endpoints

- `POST /startup/upload-deck`  
  Upload a startup pitch deck (PDF via form-data)

- `GET /startup/{id}/matches`  
  Retrieve investor matches for a startup

- `POST /investor/register`  
  Register an investor with firm details and preferences

- `GET /investor/{id}/dealflow`  
  Get startup pipeline for an investor

- `GET /intro/{startup_id}/{investor_id}`  
  Generate an AI-powered intro email

- `GET /docs`  
  Access Swagger UI for testing all endpoints

---

💡 All endpoints return JSON responses. After creating a startup or investor, you’ll receive an ID (e.g., `startup_id`, `investor_id`) which is used for further API interactions.
## Backend setup

```bash
cd backend
pip install -r requirements.txt
cp .env.example .env
# paste your real OPENAI_API_KEY in backend/.env
uvicorn main:app --reload --port 8000
```

API base URL from frontend: `http://localhost:8000`

### Key endpoints

- `POST /startup/upload-deck` — upload a startup pitch deck PDF
- `GET /startup/{id}/matches` — get investor matches for a startup
- `POST /investor/register` — register investor thesis/preferences
- `GET /investor/{id}/dealflow` — get startup dealflow for investor
- `GET /intro/{startup_id}/{investor_id}` — generate intro email
- `GET /docs` — interactive Swagger docs

## Frontend setup

```bash
npm install
npm run dev
```

Open Vite local URL (typically `http://localhost:5173`).

### Auth flow notes

- Frontend auth is currently local/demo mode (no backend auth API).
- Use any non-empty `name`, `email`, and `password` to continue.
- Role selected on sign-in decides dashboard route:
  - `startup` → startup dashboard
  - `investor` → investor dashboard
- Session is persisted in localStorage; use the `Logout` button to clear it.

## Auth + role dashboards (frontend)

- Users first land on a simple auth screen and choose role (`startup` or `investor`).
- Startup users are routed to startup dashboard flow.
- Investor users are routed to investor dashboard flow.

## 3-minute API demo (copy/paste)

After backend is running on `http://localhost:8000`, run the following in a new terminal.

### 1) Register an investor

```bash
curl -s -X POST http://localhost:8000/investor/register \
  -H "Content-Type: application/json" \
  -d '{
    "partner_name":"Priya Mehta",
    "firm_name":"Nexus Venture Partners",
    "thesis":"We back fintech infrastructure startups in India.",
    "sectors":"Fintech, SaaS",
    "stages":"Seed, Series A"
  }'
```

Copy the `investor_id` from response.

### 2) Upload startup deck

```bash
curl -s -X POST http://localhost:8000/startup/upload-deck \
  -F "file=@/absolute/path/to/pitch-deck.pdf"
```

Copy the `startup_id` from response.

### 3) Get startup -> investor matches

```bash
curl -s "http://localhost:8000/startup/<startup_id>/matches"
```

### 4) Get investor dealflow

```bash
curl -s "http://localhost:8000/investor/<investor_id>/dealflow"
```

### 5) Generate intro email draft

```bash
curl -s "http://localhost:8000/intro/<startup_id>/<investor_id>"
```

### 6) (Optional) Case #6 demo endpoints

```bash
curl -s "http://localhost:8000/india-investor/opportunity-radar"
curl -s "http://localhost:8000/india-investor/chart-patterns/RELIANCE"
curl -s -X POST "http://localhost:8000/india-investor/market-chat" \
  -H "Content-Type: application/json" \
  -d '{"question":"Any near-term breakout opportunities?","portfolio":["INFY","HDFCBANK"]}'
```
