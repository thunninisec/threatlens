┌─────────────────────────────────────┐
│          Next.js Frontend           │
│  (Auth, Chat UI, Filters, Results)  │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│         FastAPI Backend             │
│  ┌───────────┐  ┌────────────────┐  │
│  │ RAG Engine │  │ Data Ingestion │  │
│  │ (query +   │  │ (scheduled     │  │
│  │  rerank)   │  │  pipeline)     │  │
│  └─────┬─────┘  └───────┬────────┘  │
│        │                │           │
│  ┌─────▼────────────────▼────────┐  │
│  │  Chunking + Embedding Layer   │  │
│  │  (text-embedding-3-small)     │  │
│  └─────┬─────────────────────────┘  │
└────────┼────────────────────────────┘
         │
┌────────▼─────────┐  ┌──────────────┐
│   Pinecone        │  │  Claude API  │
│   Vector Store    │  │  (Sonnet)    │
│   + Metadata      │  │  Generation  │
└──────────────────┘  └──────────────┘