# Lab 1 – Yelp Prototype: Analysis & Where to Start Coding

This document summarizes the **Lab1-Yelp.pdf** assignment and provides a clear, step-by-step plan for where to start coding.

---

## Project Overview

**Goal:** Build a Yelp-style restaurant discovery and review platform with:
- **Backend:** Python + FastAPI + MySQL
- **Frontend:** React (with TailwindCSS or Bootstrap)
- **AI Assistant:** Langchain + Tavily web search
- **Personas:** User (Reviewer) and Restaurant Owner

**Due Date:** March 24, 2026 | **Points:** 40

---

## Recommended Folder Structure

```
LAB1/
├── backend/                 # FastAPI
│   ├── main.py
│   ├── database.py
│   ├── models.py
│   ├── schemas.py
│   ├── auth.py
│   ├── routers/             # auth, users, restaurants, reviews, ai_assistant
│   ├── requirements.txt
│   └── create_db.sql
├── frontend/                # React
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── context/
│   │   └── services/
│   └── package.json
└── LAB1_YELP_ANALYSIS_AND_CODING_GUIDE.md   # This file
```

---

## Where to Start Coding (Recommended Order)

### Phase 1: Backend Foundation

| Step | Task | Why first |
|------|------|-----------|
| **1** | Create `requirements.txt` | Define FastAPI, SQLAlchemy, PyMySQL, bcrypt, python-jose (JWT), uvicorn |
| **2** | Design & create MySQL schema (`create_db.sql`) | Tables: `users`, `restaurants`, `reviews`, `user_preferences`, `favourites`, `user_history`, `sessions` (if session-based) |
| **3** | `database.py` – SQLAlchemy engine + session | Central place for DB connection |
| **4** | `models.py` – SQLAlchemy models | Users, Restaurants, Reviews, Preferences, Favourites |
| **5** | `main.py` – Minimal FastAPI app | CORS, base routes, import routers |

### Phase 2: Authentication

| Step | Task | Why |
|------|------|-----|
| **6** | `auth.py` – bcrypt password hashing, JWT create/verify | Security baseline |
| **7** | `routers/auth.py` – POST `/signup`, POST `/login`, POST `/logout` | Must be logged in for most features |
| **8** | Middleware/dependency for protected routes | Reusable auth check |

### Phase 3: Core APIs (User Features)

| Step | Task | Why |
|------|------|-----|
| **9** | `routers/users.py` – Profile GET/PUT, preferences GET/PUT | Profile page needs these |
| **10** | `routers/restaurants.py` – CRUD, search (name, cuisine, keywords, location) | Core discovery feature |
| **11** | `routers/reviews.py` – Create, list (by restaurant), update, delete (own only) | Reviews are central to Yelp |
| **12** | `routers/favourites.py` – Add/remove favourite, list favourites | Simple feature; builds on restaurants |
| **13** | `routers/history.py` – List user’s reviews & added restaurants | User history tab |

### Phase 4: Frontend Foundation

| Step | Task | Why |
|------|------|-----|
| **14** | Create React app (Vite + React Router) | Project scaffolding |
| **15** | AuthContext – login state, user, token | Shared auth across pages |
| **16** | API service (Axios) – base URL, auth header, error handling | Centralized API calls |
| **17** | Public pages: Explore/Search, Restaurant Details | Main landing + detail view |

### Phase 5: User Pages (Logged In)

| Step | Task | Why |
|------|------|-----|
| **18** | Signup/Login pages | Required for logged-in features |
| **19** | Profile + Preferences page | Uses users/preferences APIs |
| **20** | Add Restaurant form | Uses POST restaurants API |
| **21** | Write Review form (on Restaurant Details) | Uses reviews API |
| **22** | Favourites tab, History tab | Uses favourites/history APIs |

### Phase 6: AI Assistant (Later)

| Step | Task | Why |
|------|------|-----|
| **23** | `routers/ai_assistant.py` – POST `/ai-assistant/chat` | Langchain + Tavily integration |
| **24** | AI Chat UI component – chat window, input, loading state | Prominent on home/explore |
| **25** | Connect UI to `/ai-assistant/chat` | End-to-end AI flow |

### Phase 7: Owner Features (Optional/Stretch)

| Step | Task |
|------|------|
| **26** | Owner signup/login, claim restaurant, owner dashboard |
| **27** | Owner pages: profile management, reviews dashboard, analytics |

---

## Suggested First Code to Write

1. **`create_db.sql`** – Database tables (users, restaurants, reviews, preferences, favourites).
2. **`requirements.txt`** – Dependencies.
3. **`database.py`** – SQLAlchemy connection.
4. **`models.py`** – User, Restaurant, Review, UserPreference, Favourite.
5. **`main.py`** – Minimal FastAPI app that loads routers and connects to DB.

After that, implement `auth.py` and `routers/auth.py` so you can protect the rest of the routes.

---

## Key Technical Requirements

### Backend

- **Password hashing:** bcrypt
- **Auth:** JWT or session-based
- **APIs:** RESTful, documented with Swagger (built-in) or Postman
- **Security:** Validation, error handling, secure endpoints

### Frontend

- **UI:** Responsive, TailwindCSS or Bootstrap
- **API:** Axios or Fetch
- **Routing:** React Router
- **Structure:** components/pages/services separation

### AI Assistant

- **Endpoint:** `POST /ai-assistant/chat`
- **Input:** `{ "message": "...", "conversation_history": [...] }`
- **Stack:** Langchain (NLU + query interpretation)
- **Extra:** Tavily web search for hours, events, etc.
- **Behavior:** Load user preferences, query DB, rank results, conversational responses

---

## Frontend (Complete)

The frontend is built end-to-end. To run:

```bash
cd LAB1/frontend && npm install && npm run dev
```

Visit http://localhost:5173. API requests proxy to the backend at http://localhost:8000.

## Quick Start Checklist

- [x] LAB1 folder with `backend/` and `frontend/` subfolders
- [ ] MySQL database created and schema applied
- [ ] Backend: `requirements.txt`, `database.py`, `models.py`, `main.py`
- [ ] Auth: signup, login, logout working
- [ ] At least one working route: e.g. GET restaurants (with search)
- [ ] React app with Explore page and Restaurant Details
- [ ] README.md with run instructions
- [ ] Don’t commit `venv` or `__pycache__`

---

## Reference

- Yelp structure: https://www.yelp.com/
- Tavily: https://www.tavily.com/#features
- Your HW4 structure can serve as a template for FastAPI + React + MySQL
