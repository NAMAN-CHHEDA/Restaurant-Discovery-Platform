# Yelp Prototype - Restaurant Discovery & Review Platform

A full-stack Yelp-style restaurant discovery and review platform built with **React**, **FastAPI**, **MySQL**, and an **AI Assistant** powered by Langchain.

> **If your partner sent you "Backend 2" and "Frontend 2" folders:** use **[RUN_PARTNER_APP.md](RUN_PARTNER_APP.md)** for step-by-step run instructions (same flow: database → backend → frontend).  
> **Architecture & data flow:** See [ARCHITECTURE.md](ARCHITECTURE.md) for request/response diagrams (if present).  
> **Submission checklist:** See [LAB1_CHECKLIST.md](LAB1_CHECKLIST.md) for feature verification (if present).

---

## End-to-End Setup Guide

Follow these steps **in order** to run the application locally.  
*(If you have `Backend 2` and `Frontend 2` instead of `backend` and `frontend`, follow [RUN_PARTNER_APP.md](RUN_PARTNER_APP.md) instead.)*

---

## Prerequisites

Install these before starting:

| Requirement | Version | Check Command |
|-------------|---------|---------------|
| Python | 3.10+ | `python3 --version` |
| Node.js | 18+ | `node --version` |
| npm | 9+ | `npm --version` |
| MySQL | 8.0+ | `mysql --version` |

---

## Step 1: Database Setup

### 1.1 Start MySQL

Ensure MySQL is running on your machine.

- **macOS (Homebrew):** `brew services start mysql`
- **Windows:** Start MySQL from Services or XAMPP
- **Linux:** `sudo systemctl start mysql`

### 1.2 Create Database and User

Open MySQL as root:

```bash
mysql -u root -p
```

Run these SQL commands:

```sql
CREATE DATABASE yelp_db;
CREATE USER 'yelp_user'@'localhost' IDENTIFIED BY 'yelp_password';
GRANT ALL PRIVILEGES ON yelp_db.* TO 'yelp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 1.3 Verify Connection

```bash
mysql -u yelp_user -pyelp_password yelp_db -e "SELECT 1;"
```

If this runs without error, the database is ready.

---

## Step 2: Backend Setup

### 2.1 Navigate to Backend

```bash
cd backend
```

### 2.2 Create Virtual Environment

```bash
python3 -m venv venv
```

### 2.3 Activate Virtual Environment

- **macOS/Linux:** `source venv/bin/activate`
- **Windows (CMD):** `venv\Scripts\activate.bat`
- **Windows (PowerShell):** `venv\Scripts\Activate.ps1`

You should see `(venv)` in your terminal prompt.

### 2.4 Install Dependencies

```bash
pip install -r requirements.txt
```

### 2.5 Create `.env` File

Copy the example file and edit it:

```bash
cp .env.example .env
```

Edit `.env` with your values. **Required variables:**

```env
DATABASE_URL=mysql+pymysql://yelp_user:yelp_password@localhost:3306/yelp_db
SECRET_KEY=your-secret-key-change-this
GROQ_API_KEY=your-groq-api-key
TAVILY_API_KEY=your-tavily-api-key
```

**Getting API keys (both free):**

1. **Groq:** Go to [console.groq.com](https://console.groq.com) → Sign up → API Keys → Create → Copy key
2. **Tavily:** Go to [tavily.com](https://tavily.com) → Sign up → API Keys → Create → Copy key

**Important:** No spaces around `=` in `.env` (e.g. `GROQ_API_KEY=gsk_xxx`, not `GROQ_API_KEY= gsk_xxx`)

### 2.6 Seed Sample Restaurants (Recommended)

```bash
python seed_restaurants.py
```

This adds 10 sample restaurants. Use `--force` to re-seed if data already exists.

### 2.7 Start the Backend

```bash
uvicorn main:app --reload --port 8000
```

You should see:

```
INFO:     Uvicorn running on http://127.0.0.1:8000
```

**Verify backend:**
- API docs: http://localhost:8000/docs
- Health check: http://localhost:8000/health

**Keep this terminal open.** The backend must stay running.

---

## Step 3: Frontend Setup

Open a **new terminal** (leave the backend running).

### 3.1 Navigate to Frontend

```bash
cd frontend
```

### 3.2 Install Dependencies

```bash
npm install
```

### 3.3 Start the Frontend

```bash
npm start
```

The app will open in your browser at **http://localhost:3000** (or http://localhost:5173 depending on your setup).

---

## Step 4: Run End-to-End

You should now have:

1. **Terminal 1:** Backend running (`uvicorn main:app --reload --port 8000`)
2. **Terminal 2:** Frontend running (`npm start`)
3. **MySQL:** Running in the background

### Quick Test Flow

1. **Sign up:** Click "Sign Up" → Create an account (e.g. user@test.com)
2. **Login:** Sign in with your credentials
3. **Browse:** Use the home page to search restaurants
4. **AI Assistant:** Click the red chat bubble → Ask "Best Italian restaurants near me"
5. **Add restaurant:** Sign up as "Restaurant Owner" → Add a restaurant
6. **Reviews:** Click a restaurant → Write a review

---

## Project Structure

```
lab-1/
├── backend/
│   ├── main.py              # FastAPI entry point
│   ├── config.py            # Environment config
│   ├── database.py          # DB connection
│   ├── models/              # SQLAlchemy models
│   ├── routers/             # API routes
│   ├── services/            # AI service
│   ├── .env                 # Your secrets (create from .env.example)
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   └── services/
│   └── package.json
└── README.md
```

---

## Tech Stack

| Layer    | Technology                          |
|----------|-------------------------------------|
| Frontend | React 18, TailwindCSS, Axios        |
| Backend  | Python 3.10+, FastAPI, SQLAlchemy   |
| Database | MySQL 8.0                           |
| AI       | Langchain, Groq, Tavily Search     |
| Auth     | JWT, bcrypt                         |

---

## Troubleshooting

### "ModuleNotFoundError: No module named 'langchain_groq'"

Make sure you activated the venv and installed dependencies:

```bash
cd backend
source venv/bin/activate
pip install langchain-groq
```

### "AI Assistant requires configuration"

- Check `.env` has `GROQ_API_KEY` and `TAVILY_API_KEY` with no spaces
- Restart the backend after changing `.env`

### "Can't connect to MySQL"

- Ensure MySQL is running
- Check `DATABASE_URL` in `.env` matches your MySQL user/password
- Verify: `mysql -u yelp_user -pyelp_password yelp_db -e "SELECT 1;"`

### "Port 8000 already in use"

Stop the process using the port, or run on a different port:

```bash
uvicorn main:app --reload --port 8001
```

Then update the frontend API base URL if needed.

### Frontend can't reach backend

- Ensure backend is running on port 8000
- Check CORS: backend allows `http://localhost:3000` and `http://localhost:5173`

---

## Features

- **Users:** Signup/Login, profile with photo, preferences, search, reviews, favorites, AI chatbot
- **Owners:** Restaurant management, claim restaurants, analytics
- **AI Assistant:** Natural language recommendations, uses preferences, Tavily web search

---

## Adding Restaurant Data

| Method | Command / Steps |
|--------|-----------------|
| Seed script | `cd backend && python seed_restaurants.py` |
| Via website | Sign up → Add Restaurant (or "Start a Project") |
| Via API | Use Swagger at http://localhost:8000/docs |

---

## API Quick Reference

| Endpoint | Method | Description |
|---------|--------|-------------|
| `/auth/login` | POST | Login |
| `/auth/signup` | POST | Sign up |
| `/restaurants/` | GET | List/search restaurants |
| `/restaurants/` | POST | Create restaurant (auth) |
| `/reviews/` | POST | Add review (auth) |
| `/ai-assistant/chat` | POST | AI chat (auth) |
| `/docs` | GET | Swagger UI |
