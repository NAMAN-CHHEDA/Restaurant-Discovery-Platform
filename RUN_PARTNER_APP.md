# Run Your Partner's Lab 1 App (Backend 2 + Frontend 2)

Use this guide to run the app from **Backend 2** and **Frontend 2** the same way you ran the original (backend + frontend).

---

## Prerequisites

- Python 3.10+
- Node.js 18+
- MySQL 8.0+ (running)

---

## Step 1: Database

### Already have `yelp_db`?

If you created the database earlier (e.g. for the original LAB1 backend), you can **skip creating it again**. Just:

1. Make sure MySQL is running: `brew services start mysql`
2. In **Backend 2** `.env`, set `DATABASE_URL` to match how you connect:
   - If you use **root** (no password): `DATABASE_URL=mysql+pymysql://root@localhost:3306/yelp_db`
   - If you use **root** with a password: `DATABASE_URL=mysql+pymysql://root:YOUR_PASSWORD@localhost:3306/yelp_db`
   - If you already created **yelp_user**: `DATABASE_URL=mysql+pymysql://yelp_user:yelp_password@localhost:3306/yelp_db`

Then go to **Step 2: Backend**.

---

### First-time database setup

### Start MySQL

```bash
brew services start mysql
```

### Create database and user

```bash
mysql -u root -p
```

In MySQL:

```sql
CREATE DATABASE IF NOT EXISTS yelp_db;
CREATE USER 'yelp_user'@'localhost' IDENTIFIED BY 'yelp_password';
GRANT ALL PRIVILEGES ON yelp_db.* TO 'yelp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Verify

```bash
mysql -u yelp_user -pyelp_password yelp_db -e "SELECT 1;"
```

---

## Step 2: Backend (Backend 2)

### 2.1 Go to Backend 2

```bash
cd "LAB1/backend 2"
```

### 2.2 Virtual environment and install

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 2.3 Create `.env`

```bash
cp .env.example .env
```

Edit `.env` and set:

```env
DATABASE_URL=mysql+pymysql://yelp_user:yelp_password@localhost:3306/yelp_db
SECRET_KEY=your-secret-key-here
GROQ_API_KEY=your-groq-api-key
TAVILY_API_KEY=your-tavily-api-key
```

- **Groq:** [console.groq.com](https://console.groq.com) → API Keys (free).
- **Tavily:** [tavily.com](https://tavily.com) → API Keys (free).  
You can leave these empty; the AI Assistant will just show a “not configured” message.

### 2.4 Seed restaurants (optional)

```bash
python seed_restaurants.py
```

### 2.5 Start backend

```bash
uvicorn main:app --reload --port 8000
```

Leave this terminal open. Backend runs at **http://localhost:8000**.  
Check: http://localhost:8000/docs and http://localhost:8000/health

---

## Step 3: Frontend (Frontend 2)

Open a **new terminal**.

### 3.1 Go to Frontend 2

```bash
cd "LAB1/frontend 2"
```

### 3.2 Install and start

```bash
npm install
npm start
```

Browser should open at **http://localhost:3000**.  
If it doesn’t, open that URL manually.

---

## Step 4: Use the app

1. **Terminal 1:** Backend running in `backend 2` (`uvicorn main:app --reload --port 8000`).
2. **Terminal 2:** Frontend running in `frontend 2` (`npm start` → http://localhost:3000).
3. MySQL running in the background.

- Sign up / Log in.
- Browse and search restaurants.
- Use the AI Assistant (red chat) if you set `GROQ_API_KEY` and `TAVILY_API_KEY`.
- Add restaurants, write reviews, use Favorites and History.

---

## Quick reference

| What        | Command / URL |
|------------|----------------|
| Backend dir| `cd LAB1/backend 2` |
| Frontend dir | `cd LAB1/frontend 2` |
| Backend URL | http://localhost:8000 |
| Frontend URL | http://localhost:3000 |
| API docs   | http://localhost:8000/docs |
| Health    | http://localhost:8000/health |

---

## Troubleshooting

- **Port 8000 in use:** Run backend on another port, e.g. `uvicorn main:app --reload --port 8001`, then in Frontend 2 create `.env` with `REACT_APP_API_URL=http://localhost:8001` and restart `npm start`.
- **MySQL connection error:** Check `DATABASE_URL` in Backend 2 `.env` and that `yelp_user` / `yelp_password` match what you created.
- **Frontend can’t reach backend:** Ensure backend is on 8000 (or match `REACT_APP_API_URL`). CORS is set for `localhost:3000` and `localhost:5173`.
- **AI Assistant not working:** Add `GROQ_API_KEY` and `TAVILY_API_KEY` to Backend 2 `.env` and restart the backend.
