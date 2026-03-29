# Fintech — AI Venture Matching Prototype

> A full-stack hackathon prototype that helps **startups get investor-intro-ready** and helps **investors prioritize high-fit dealflow** using AI.

---

## ✨ Why this project is interesting

Most early-stage fundraising is noisy, slow, and manually filtered.

This prototype compresses the workflow into a single product loop:

1. **Startup uploads a pitch deck (PDF)**
2. **AI extracts structured company metadata**
3. **Investors register thesis + stage preferences**
4. **AI scores startup–investor fit in both directions**
5. **One-click warm intro email draft is generated**

It also includes a dedicated **“AI for the Indian Investor”** case track with signal feed, chart pattern intelligence, and portfolio-aware market chat endpoints.

---

## 🧱 Tech Stack

### Frontend
- **React 19 + Vite**
- Lightweight single-page flow with role-based onboarding
- Session persistence via `localStorage`

### Backend
- **FastAPI**
- **OpenAI API** for extraction, matching reasoning, and intro email generation
- **PyPDF2** for deck text extraction
- In-memory storage (`dict`) for rapid prototyping

---

## 🔄 Product Flow

### Startup side
- Authenticate (demo auth)
- Complete onboarding
- Upload deck (`POST /startup/upload-deck`)
- See matched investors (`GET /startup/{id}/matches`)

### Investor side
- Authenticate (demo auth)
- Register thesis and preferences (`POST /investor/register`)
- View ranked dealflow (`GET /investor/{id}/dealflow`)

### Shared workflow
- Generate introduction draft (`GET /intro/{startup_id}/{investor_id}`)

---

## 🧪 API Surface (Core)

| Endpoint | Method | Purpose |
|---|---|---|
| `/health` | GET | Service heartbeat + in-memory counts |
| `/startup/upload-deck` | POST | Upload & parse startup deck PDF |
| `/startup/{startup_id}` | GET | Fetch startup profile |
| `/startup/{startup_id}/matches` | GET | Ranked investor matches for startup |
| `/investor/register` | POST | Register investor thesis/preferences |
| `/investor/{investor_id}` | GET | Fetch investor profile |
| `/investor/{investor_id}/dealflow` | GET | Ranked startup dealflow for investor |
| `/intro/{startup_id}/{investor_id}` | GET | AI-generated warm intro email |
| `/all` | GET | Debug endpoint: all in-memory records |

### India investor track

| Endpoint | Method | Purpose |
|---|---|---|
| `/india-investor/opportunity-radar` | GET | Signal-first opportunity feed |
| `/india-investor/chart-patterns/{symbol}` | GET | Pattern detection + risk note |
| `/india-investor/market-chat` | POST | Portfolio-aware market Q&A response |

Docs UI: `http://localhost:8000/docs`

---

## 🚀 Local Setup

## 1) Backend setup

```bash
cd backend
python -m venv .venv
source .venv/bin/activate    # Windows: .venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env         # if not present, create .env manually
# set OPENAI_API_KEY=your_key_here
uvicorn main:app --reload --port 8000
```

Backend runs at: `http://localhost:8000`

## 2) Frontend setup

```bash
# from repo root
npm install
npm run dev
```

Frontend runs at: `http://localhost:5173` (default Vite port)

---

## ⚡ Quick API Demo (copy/paste)

### Register investor
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

### Upload startup deck
```bash
curl -s -X POST http://localhost:8000/startup/upload-deck \
  -F "file=@/absolute/path/to/pitch-deck.pdf"
```

### Match startup to investors
```bash
curl -s "http://localhost:8000/startup/<startup_id>/matches"
```

### Investor dealflow
```bash
curl -s "http://localhost:8000/investor/<investor_id>/dealflow"
```

### Intro draft
```bash
curl -s "http://localhost:8000/intro/<startup_id>/<investor_id>"
```

---

## 🛡️ Current Prototype Constraints

- Uses **in-memory storage** (resets on backend restart)
- Uses simplified frontend auth (demo mode, local only)
- Some India investor track responses are deterministic simulated outputs
- Not production-hardened (no RBAC, no persistent DB, no background jobs)

---

## 🗺️ Suggested Next Steps

- Replace in-memory data with PostgreSQL + SQLModel/SQLAlchemy
- Add JWT auth + role-based route protection
- Add async task queue for deck parsing + scoring
- Introduce retrieval-backed market intelligence for source-grounded responses
- Add test coverage (backend integration + frontend component tests)
- Containerize with Docker and ship CI/CD pipeline

---

## 📁 Repository Layout

```text
.
├── backend/
│   ├── main.py
│   └── requirements.txt
├── src/
│   ├── App.tsx
│   ├── pages/
│   └── assets/
├── package.json
└── README.md
```

---

## 🏁 Bottom line

This is a strong foundation for an AI-native private-market product: it already demonstrates ingestion, understanding, matching, ranking, and communication generation in one cohesive flow.

If you’re evaluating this for a hackathon, incubator demo, or MVP seed build — it’s a compelling starting point.
