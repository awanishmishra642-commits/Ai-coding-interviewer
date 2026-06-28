# AI Coding Interview Platform

A full-stack platform where software engineers practice coding interviews with an AI interviewer, and recruiters review candidate performance.

## Architecture

| Layer | Tech | Location |
|---|---|---|
| Frontend | React + Vite + TypeScript | `artifacts/interview-platform/` |
| API | Node/Express + TypeScript | `artifacts/api-server/` |
| Database | PostgreSQL via Drizzle ORM | `lib/db/` |
| API Contract | OpenAPI 3 → Zod + React Query | `lib/api-spec/`, `lib/api-zod/`, `lib/api-client-react/` |
| AI | Groq API (`llama3-70b-8192`) | `artifacts/api-server/src/lib/groq.ts` |
| Auth | JWT (`SESSION_SECRET`) + bcryptjs | `artifacts/api-server/src/lib/jwt.ts` |
| Real-time | Socket.io | `artifacts/api-server/src/app.ts` |

## Environment Variables / Secrets

| Variable | Description |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `SESSION_SECRET` | JWT signing secret |
| `GROQ_API_KEY` | Groq API key for AI features |
| `PORT` | Set automatically by Replit per artifact |

## Key Features

- **Candidate flow**: Register → Create interview (topic, difficulty, question count) → Start interview → Monaco editor with AI-generated DSA questions → Voice I/O (Web Speech API) → Submit code for AI review → Complete interview → View results
- **Recruiter flow**: Login → Dashboard with all candidates + all interviews → Candidate profile with full interview history
- **Real-time**: Socket.io updates scores and question arrivals live during interview sessions
- **Security**: JWT auth on all protected routes; ownership checks on every resource; Socket.io connections require valid JWT

## Development Workflows

- `artifacts/api-server: API Server` — Express backend on port 8080, path `/api` + `/socket.io`
- `artifacts/interview-platform: web` — Vite frontend on port 21137

## Regenerating API Client

After changing `lib/api-spec/openapi.yaml`:
```
pnpm --filter @workspace/api-spec run codegen
```

## DB Schema Changes

After changing schema files in `lib/db/src/schema/`:
```
pnpm --filter @workspace/db run push
```

## User Preferences

- Use Drizzle ORM (already in monorepo), not Prisma
- Use Groq API with model `llama3-70b-8192` for AI features
- JWT auth stored in `localStorage` as `auth_token`
- Voice I/O via Web Speech API (no third-party dependency)
