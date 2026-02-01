# AI Project Boilerplate

A production-ready monorepo boilerplate for full-stack AI applications with Python/FastAPI backend, React/TypeScript frontend, and Docker orchestration. Clean architecture with organized directory structure for RAG, ML pipelines, and document processing.

## Project Structure

```
ai-project-boilerplate/
├── backend/                      # Python/FastAPI backend
│   ├── app.py                   # FastAPI application entry point
│   ├── core/                    # Core application modules
│   │   ├── __init__.py
│   │   ├── db.py               # Database connection
│   │   └── auth.py             # Authentication utilities
│   ├── src/
│   │   ├── routes/             # API endpoints organized by feature
│   │   │   ├── __init__.py
│   │   │   ├── auth.py         # /auth endpoints
│   │   │   ├── chat.py         # /chat endpoints
│   │   │   └── search.py       # /search endpoints
│   │   ├── scrapers/           # External data collection
│   │   │   ├── __init__.py
│   │   │   └── scraper.py
│   │   ├── document_processor/  # Document ingestion pipeline
│   │   │   ├── __init__.py
│   │   │   ├── parser.py       # Document parsing
│   │   │   ├── chunker.py      # Text chunking
│   │   │   └── embedder.py     # Embedding generation
│   │   ├── rag/                # Retrieval Augmented Generation
│   │   │   ├── __init__.py
│   │   │   ├── retriever.py    # Hybrid search
│   │   │   ├── prompt_engine.py # Prompt engineering
│   │   │   └── llm_orchestrator.py # LLM calls
│   │   ├── config/             # Shared configuration
│   │   │   ├── __init__.py
│   │   │   ├── settings.py     # App settings
│   │   │   └── prompts.yaml    # Prompt templates
│   │   └── utils/              # Utility functions
│   │       ├── __init__.py
│   │       └── helpers.py
│   ├── tests/                  # Unit & integration tests
│   │   ├── __init__.py
│   │   ├── test_routes.py
│   │   └── test_rag.py
│   ├── pyproject.toml          # Python dependency management
│   ├── requirements.txt        # Alternative: pip requirements
│   └── Dockerfile              # Backend container
│
├── frontend/                    # React/TypeScript frontend
│   ├── index.tsx               # React application entry point
│   ├── vite.config.ts          # Vite configuration & API proxy
│   ├── src/
│   │   ├── pages/              # Route-level views
│   │   │   ├── Chat.tsx
│   │   │   ├── Search.tsx
│   │   │   └── Dashboard.tsx
│   │   ├── components/         # Reusable UI components
│   │   │   ├── Header.tsx
│   │   │   ├── ChatBox.tsx
│   │   │   ├── ResultsPanel.tsx
│   │   │   └── SettingsPanel.tsx
│   │   ├── api/                # Backend communication layer
│   │   │   ├── client.ts       # Type-safe API client
│   │   │   └── hooks.ts        # API hooks
│   │   ├── types/              # TypeScript type definitions
│   │   └── utils/              # Utility functions
│   ├── package.json            # Node dependencies
│   ├── tsconfig.json           # TypeScript config
│   └── Dockerfile              # Frontend container
│
├── docker-compose.yml          # Orchestrates backend + frontend
├── .github/
│   └── workflows/              # CI/CD pipelines
│       ├── test.yml
│       └── deploy.yml
├── docs/                       # Documentation
│   ├── setup.md                # Setup instructions
│   ├── api.md                  # API documentation
│   └── architecture.md         # Architecture overview
├── .gitignore
├── README.md
└── LICENSE
```

## Quick Start

### Prerequisites
- Python 3.9+
- Node.js 18+
- Docker & Docker Compose
- Git

### Local Development

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/ai-project-boilerplate.git
cd ai-project-boilerplate
```

2. **Backend setup**
```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Or with Poetry
poetry install
```

3. **Frontend setup**
```bash
cd ../frontend
npm install
```

4. **Run with Docker Compose** (Recommended)
```bash
cd ..
docker-compose up --build
```

Access the application:
- Frontend: http://localhost:3000 (or 5173 for Vite dev)
- Backend: http://localhost:8000
- API Docs: http://localhost:8000/docs

## Backend Development

### Run Backend Server
```bash
cd backend
uv run fastapi dev app.py
# or
uvicorn app:app --reload
```

### Run Tests
```bash
cd backend
pytest tests/ -v
```

### Create New Route
1. Create endpoint file in `src/routes/new_feature.py`
2. Implement route with proper error handling
3. Import in `src/routes/__init__.py`
4. Add to FastAPI router in `app.py`

### Environment Variables
Create `.env` file in backend:
```
DATABASE_URL=postgresql://user:password@localhost/dbname
LLM_API_KEY=your-api-key
VECTOR_DB_URL=http://localhost:6333
```

## Frontend Development

### Run Frontend Dev Server
```bash
cd frontend
npm run dev
```

### Build for Production
```bash
npm run build
npm run preview
```

### API Integration
The frontend automatically proxies API calls to the backend via `vite.config.ts`:
```typescript
proxy: {
  '/api': {
    target: 'http://localhost:8000',
    changeOrigin: true,
    rewrite: (path) => path.replace(/^\/api/, '')
  }
}
```

## Docker Deployment

### Build and Run with Docker Compose
```bash
docker-compose up -d
docker-compose logs -f
```

### Individual Container Builds
```bash
# Backend
docker build -t ai-backend:latest backend/
docker run -p 8000:8000 ai-backend:latest

# Frontend
docker build -t ai-frontend:latest frontend/
docker run -p 3000:3000 ai-frontend:latest
```

## Key Components Explained

### Document Processing Pipeline
- **src/document_processor/**: Handles document ingestion
  - Parsing: Extract text from PDF, DOCX, images
  - Chunking: Split documents into meaningful chunks
  - Embedding: Generate vector embeddings
  - Storage: Save to vector database (Qdrant, Pinecone, etc.)

### RAG System
- **src/rag/**: Retrieval Augmented Generation
  - Retriever: Hybrid search (semantic + keyword)
  - Prompt Engine: Dynamic prompt construction
  - LLM Orchestrator: Handle API calls, retries, streaming

### Database Layer
- **core/db.py**: Connection pooling, transaction management
- Supports PostgreSQL, MongoDB, or other databases
- ORM: SQLAlchemy (SQL) or Pydantic (document-based)

## Best Practices

### Code Organization
✅ Separate concerns: routes, business logic, utilities
✅ Type hints: Use Python type annotations and TypeScript
✅ Error handling: Custom exceptions, proper HTTP status codes
✅ Logging: Structured logging for debugging

### Testing
✅ Unit tests for individual functions
✅ Integration tests for API endpoints
✅ Mock external services (LLM, databases)
✅ Test coverage > 80%

### Deployment
✅ Environment-specific configs
✅ Secret management: Use .env files or cloud secret managers
✅ Database migrations: Alembic for schema changes
✅ Monitoring: Logging, metrics, health checks

## Common Issues & Solutions

### Frontend can't connect to backend
- Check vite.config.ts proxy settings
- Ensure backend is running on port 8000
- Check CORS configuration in app.py

### Docker build fails
- Clear Docker cache: `docker system prune -a`
- Check Python/Node version compatibility
- Verify requirements.txt and package.json

### Port conflicts
- Change ports in docker-compose.yml
- Update frontend proxy configuration
- Update API client URLs

## Contributing

1. Create feature branch: `git checkout -b feature/amazing-feature`
2. Commit changes: `git commit -m 'Add amazing feature'`
3. Push to branch: `git push origin feature/amazing-feature`
4. Open Pull Request

## License

This project is licensed under the MIT License - see LICENSE file for details.

## Resources

- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [React Documentation](https://react.dev/)
- [Docker Documentation](https://docs.docker.com/)
- [Vector Databases](https://www.pinecone.io/) / [Qdrant](https://qdrant.tech/)
- [LLM APIs](https://platform.openai.com/) / [Anthropic](https://www.anthropic.com/)

## Support

For issues, questions, or suggestions, please create an GitHub Issue or contact the maintainers.
