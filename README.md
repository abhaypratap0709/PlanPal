# PlanPal – Monorepo (Frontend + Backend)

A modern group planning app with real-time chat, events, polls, and AI trip assistant.

## Overview
- Frontend: React (Vite), Tailwind, Framer Motion, Supabase client
- Backend: Node.js/Express, Supabase (Postgres + Auth + Realtime)
- AI: Gemini (Google Generative AI) for chatbot (`@bot`)

## Repo Structure
```
.
├─ frontend/                 # React (Vite) app
│  ├─ src/
│  ├─ package.json
│  └─ (expects .env in this folder)
├─ config/, routes/, services/, middleware/  # Backend code
├─ server.js                 # Backend entrypoint
├─ package.json              # Backend package.json
├─ schema.sql                # Database schema + policies (Supabase)
├─ frontend.env.example      # Copy → frontend/.env (Vite)
└─ backend.env.example       # Copy → .env (repo root)
```

## Environment Setup

### 1) Frontend (.env)
Copy `frontend.env.example` to `frontend/.env` and fill values:
```
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-supabase-anon-key
VITE_APP_NAME=PlanPal
```

### 2) Backend (.env)
Copy `backend.env.example` to `.env` (repo root) and fill values:
```
PORT=8000
NODE_ENV=production

# Supabase (service-role key only on server)
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key

# Auth/JWT
JWT_SECRET=replace-with-strong-secret

# Gemini AI (chatbot)
GEMINI_API_KEY=your-gemini-api-key
# Optional: force a bot UUID (else created automatically)
BOT_USER_ID=

# Optional external APIs
TMDB_API_KEY=
GOOGLE_API_KEY=
```

Notes:
- Never commit real `.env` files; keep only the `*.example` files in git.
- Ensure Supabase Realtime is enabled for `chat_messages` (or `messages`) table.

## Install & Run Locally

Open two terminals.

### Backend
```
# From repo root
npm install
npm run dev
# or: npm start
# API: http://localhost:8000
```

### Frontend
```
cd frontend
npm install
npm run dev
# App: http://localhost:5173 (default Vite port)
```

## Production Builds
- Frontend: `cd frontend && npm run build` → deploy `frontend/dist` to static hosting (e.g., Vercel/Netlify)
- Backend: run `npm start` on your server platform with `.env` configured (Render/Railway/DigitalOcean, etc.)

## Supabase Setup (Quick)
1. Create project → copy project URL and keys
2. Run SQL from `schema.sql`
3. Enable Email auth (or your chosen provider)
4. Enable Realtime on your chat table (`chat_messages` preferred)

## Chatbot (@bot)
- Model: `gemini-2.5-flash-lite`
- Trigger by typing `@bot ...` in group chat
- Backend saves bot messages using service-role (persists across refresh and visible to all members)

## GitHub Push
To push this repo to your GitHub:
```
# set remote to your repo
git init
git remote add origin https://github.com/abhaypratap0709/PlanPal.git

# add and commit
git add .
git commit -m "chore: monorepo README, env examples, chatbot persistence"

# push (main branch)
git branch -M main
git push -u origin main
```

## Troubleshooting
- 400/401/403 from backend → verify `.env` and Supabase keys, and JWT in Authorization header
- Bot message not persisting → ensure backend runs with service-role key; check Realtime enabled; verify `.env` `GEMINI_API_KEY`
- Frontend cannot connect to Supabase → verify `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`

## License
MIT
