# Taskly: To-Do App with AI

A full-stack to-do application with user and admin roles, plus AI features powered by Google Gemini.

**Live app:** _add your hosted link here_

## Features
- Register and log in (JWT authentication), with User and Admin roles
- Create, edit, delete, search and filter tasks (status, priority, due date)
- Admin dashboard: view all users and their tasks
- **AI Smart Add:** type a sentence like "submit report by Friday, urgent" and AI fills in the title, priority and due date
- **AI Plan my day:** AI orders your open tasks and gives a short reason for each

## Tech stack
- **Frontend:** React 18, TypeScript, Vite
- **Backend:** FastAPI, SQLAlchemy, Alembic
- **Database:** TiDB Cloud (MySQL compatible)
- **AI:** Google Gemini API (Grok is also supported in the backend)

## AI integration
The AI calls run only on the backend (`backend/app/ai.py`), so API keys never reach the browser.
- `POST /api/ai/parse-task`: turns text into task fields
- `POST /api/ai/plan`: orders the logged-in user's open tasks

Safety measures: login required, only the user's own tasks are sent to the AI, AI output is validated before use, and requests are rate limited per user.

## Run locally

### Backend
```
cd backend
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```
Create `backend/.env` (see `backend/.env.example`):
```
DATABASE_URL=your_database_url
JWT_SECRET_KEY=a_long_random_secret
GEMINI_API_KEY=your_gemini_key
CORS_ORIGINS=http://localhost:5173
```
Then run:
```
uvicorn app.main:app --reload
```

### Frontend
```
cd frontend
npm install
npm run dev
```
For a hosted frontend, set `VITE_API_URL` to your backend URL.

## Environment variables
| Name | Where | Purpose |
|---|---|---|
| `DATABASE_URL` | backend | database connection |
| `JWT_SECRET_KEY` | backend | signs login tokens |
| `GEMINI_API_KEY` | backend | Gemini API key (never commit it) |
| `GEMINI_MODEL` | backend | optional model name |
| `XAI_API_KEY` | backend | optional Grok key |
| `CORS_ORIGINS` | backend | allowed frontend URLs |
| `VITE_API_URL` | frontend | backend URL for hosted builds |
