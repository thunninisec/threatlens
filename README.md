# 🔍 ThreatLens

**AI-powered cybersecurity intelligence assistant** — query CVE databases, threat advisories, and attack frameworks using natural language.

![Python](https://img.shields.io/badge/Python-3.12+-blue?logo=python)
![Next.js](https://img.shields.io/badge/Next.js-14-black?logo=next.js)
![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-009688?logo=fastapi)
![License](https://img.shields.io/badge/License-MIT-green)

---

## The Problem

Security teams waste hours manually searching the National Vulnerability Database, reading CISA advisories, and cross-referencing threat intel across dozens of sources. When a new critical CVE drops, the workflow looks like:

1. Search NVD for the CVE details
2. Check if it's in CISA's Known Exploited Vulnerabilities catalog
3. Look up related MITRE ATT&CK techniques
4. Cross-reference affected products against your stack
5. Repeat for every advisory, every day

**ThreatLens replaces this with a single conversational interface.**

## What It Does

Ask questions in plain English. Get cited, accurate answers drawn from real threat intelligence sources.
```
> "What critical vulnerabilities affecting Apache were disclosed in the last 30 days?"

> "Show me all known exploited vulnerabilities related to authentication bypass"

> "What MITRE ATT&CK techniques are associated with CVE-2024-3400?"

> "Compare the severity of recent Log4j-related CVEs"
```

Every answer includes inline citations linking back to specific CVEs, advisories, and MITRE techniques.

## Key Features

- **Multi-source RAG pipeline** — Ingests and indexes data from NVD, CISA KEV, and MITRE ATT&CK
- **Hybrid search** — Vector similarity + structured metadata filtering (severity, date range, vendor, CWE type)
- **Citation-backed answers** — Every claim links to specific CVEs and advisories
- **Smart query analysis** — Automatically extracts filters from natural language
- **Threat dashboard** — Recent critical CVEs, ingestion stats, trending vulnerabilities
- **Conversation memory** — Multi-turn queries with full context
- **Streaming responses** — Real-time answer generation via SSE

## Architecture
```
┌──────────────────────────────────────────────┐
│              Next.js Frontend                 │
│       Chat UI  ·  Dashboard  ·  Auth          │
└──────────────────┬───────────────────────────┘
                   │
┌──────────────────▼───────────────────────────┐
│              FastAPI Backend                   │
│                                               │
│   RAG Engine              Data Ingestion      │
│   ├── Query Analysis      ├── NVD / CVE API   │
│   ├── Vector Retrieval    ├── CISA KEV        │
│   ├── Metadata Filtering  ├── MITRE ATT&CK   │
│   ├── Reranking           └── GitHub Sec.     │
│   └── Claude Generation                       │
│                                               │
│          Chunking + Embedding Layer            │
└──────────┬────────────────────┬──────────────┘
           │                    │
    ┌──────▼──────┐     ┌──────▼──────┐
    │  Pinecone   │     │  Claude API │
    │ Vector Store│     │  (Sonnet)   │
    └─────────────┘     └─────────────┘
```

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Python 3.12+, FastAPI, LangChain |
| Frontend | Next.js 14, TypeScript, Tailwind, shadcn/ui |
| Vector DB | Pinecone |
| Embeddings | OpenAI text-embedding-3-small |
| LLM | Claude Sonnet (Anthropic) |
| Auth | NextAuth.js + GitHub OAuth |
| Deployment | Vercel (frontend) + Railway (backend) |

## Data Sources

| Source | Description | Update Frequency |
|--------|------------|-----------------|
| [NVD](https://nvd.nist.gov/) | CVE details, CVSS scores, affected products | Incremental (daily) |
| [CISA KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) | Known Exploited Vulnerabilities catalog | Full refresh (daily) |
| [MITRE ATT&CK](https://attack.mitre.org/) | Adversary tactics, techniques, procedures | Periodic refresh |
| [GitHub Advisories](https://github.com/advisories) | Open-source package security advisories | Incremental |

## Getting Started

### Prerequisites

- Python 3.12+
- Node.js 18+
- Pinecone account (free tier works)
- Anthropic API key
- OpenAI API key (for embeddings)
- GitHub OAuth app (for auth)

### Backend
```bash
cd backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
# Fill in your API keys
uvicorn app.main:app --reload
```

### Frontend
```bash
cd frontend
npm install
cp .env.example .env.local
# Fill in your environment variables
npm run dev
```

### Data Ingestion
```bash
cd backend
python -m app.ingestion.pipeline
```

## How the RAG Pipeline Works

1. **Ingestion** — CVEs and advisories are fetched, normalized, chunked with rich metadata, embedded, and upserted into Pinecone.
2. **Query Analysis** — Claude extracts structured filters (severity, date, vendor, CWE) and rewrites the query for retrieval.
3. **Hybrid Retrieval** — Vector similarity search + metadata pre-filtering, returning top 20 candidates.
4. **Reranking** — Chunks re-scored for relevance, top 5 selected for context.
5. **Generation** — Claude generates an answer using only retrieved context, with mandatory inline citations.

## Screenshots

> *Coming soon*

## Roadmap

- [x] Project architecture and design
- [ ] NVD data ingestion pipeline
- [ ] CISA KEV ingestion
- [ ] Pinecone embedding pipeline
- [ ] RAG query engine with metadata filtering
- [ ] FastAPI endpoints
- [ ] Next.js chat interface
- [ ] Filter sidebar with structured search
- [ ] Citation cards with CVE deep links
- [ ] Threat dashboard
- [ ] MITRE ATT&CK integration
- [ ] Conversation persistence
- [ ] Deployment (Vercel + Railway)
- [ ] CI/CD pipeline

## License

MIT

---

Built by [@thunninisec](https://github.com/thunninisec)
