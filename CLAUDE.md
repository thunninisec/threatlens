# CLAUDE.md — ThreatLens Project Context

## What is this project?
ThreatLens is a production-grade RAG app for cybersecurity threat intelligence. Users ask natural language questions about vulnerabilities, exploits, and threats — the app retrieves relevant data from CVE/advisory databases and generates cited answers using Claude.

## Tech Stack
- Backend: Python 3.12+ / FastAPI
- Frontend: Next.js 14 (App Router) / TypeScript / Tailwind / shadcn/ui
- Vector DB: Pinecone (free tier)
- Embeddings: OpenAI text-embedding-3-small
- LLM: Claude Sonnet via Anthropic SDK
- Auth: NextAuth.js with GitHub OAuth
- Deployment: Vercel (frontend) + Railway (backend)

## Project Structure
- `backend/` — FastAPI app with RAG engine and data ingestion pipeline
- `frontend/` — Next.js app with chat UI, filters, and dashboard
- `ARCHITECTURE.md` — Full system design doc

## Code Conventions
- Python: Use async/await throughout. Type hints on all functions. Pydantic v2 for models.
- TypeScript: Strict mode. Use server components by default, client components only when needed.
- All API responses use consistent Pydantic response models.
- Environment variables in .env files, never hardcoded.
- Imports: absolute imports, grouped (stdlib / third-party / local).

## Data Sources
1. NVD (National Vulnerability Database) — CVEs via REST API
2. CISA KEV — Known Exploited Vulnerabilities catalog (JSON)
3. MITRE ATT&CK — Techniques/tactics via STIX data
4. GitHub Security Advisories — stretch goal

## Key Design Decisions
- One Pinecone namespace per data source for clean separation
- Metadata-rich chunks enable hybrid search (vector + filter)
- Query analysis step extracts structured filters from natural language before retrieval
- Citations are mandatory — every claim in generated answers links to specific CVEs
- Streaming responses from Claude to the frontend via SSE

## Common Tasks
- `cd backend && uvicorn app.main:app --reload` — run backend
- `cd frontend && npm run dev` — run frontend
- `cd backend && python -m app.ingestion.pipeline` — run data ingestion
- `cd backend && pytest` — run tests

## Current Phase
Phase 1 — Building the data ingestion pipeline. Start with NVD and CISA KEV sources.

## Important Notes
- NVD API rate limit: 5 req/30s without key, 50 req/30s with key
- Pinecone free tier: 1 index, 100k vectors — sufficient for MVP
- Always test RAG queries with diverse examples (severity filters, date ranges, vendor-specific, multi-hop)
- README with screenshots is critical — this is a portfolio piece