# SmartSpidy

RAG-backed outreach assistant for NGO fundraising campaigns (Pads For Freedom, Bowls Of Hope, Wings Of Hope).

Live: https://smartspidy.smartbrew.in/

Built while working on SmartBrew products. Retrieves campaign-specific context from Supabase (pgvector), then generates replies / first DMs with OpenAI.

## Problem

Fundraiser chats need answers grounded in the right campaign stats and messaging. A generic LLM mixes campaigns and drifts. SmartSpidy scopes retrieval by campaign, then generates.

## Architecture

```
Vite/React UI
    → Express API (auth, chats, messages, notifications)
    → Supabase (users, chats, messages, embeddings)
    → OpenAI embeddings + chat
```

- Vector table / RPC: campaign-scoped similarity (`search_campaign_embeddings`)
- Backend: Express + JWT + rate limit + cron for inactive chats / notifications
- Frontend: React, Tailwind, Framer Motion

Details: `ENHANCED_RAG_SCHEMA.md`, `RAG_SETUP.md`, `backend/README.md`, `backend/full_schema_with_rls.sql`

## Stack

- Frontend: Vite, React 18, TypeScript, Tailwind
- Backend: Node.js, Express, Passport JWT, Joi
- Data: Supabase (Postgres + pgvector + RLS)
- LLM: OpenAI embeddings + GPT-4o (Gemini client also present in deps)

## How to run

### Frontend

```bash
npm install
cp .env.example .env   # if present; else create with VITE_SUPABASE_URL etc.
npm run dev
```

### Backend

```bash
cd backend
npm install
# create backend/.env from backend/README.md
npm run dev
```

Apply SQL in Supabase (`backend/full_schema_with_rls.sql` + RAG functions in the docs).

## Demo

https://smartspidy.smartbrew.in/

## Honest notes

- Repo name `Smart-Spidy-ay1` is a working copy name; product name is SmartSpidy.
- Root package.json still has the Vite starter name; rename when convenient.
- Prefer OpenAI/Supabase service keys on the backend only in any real deploy.
