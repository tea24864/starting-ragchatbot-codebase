# Course Materials RAG System - IFLOW Context

## Project Overview

This is a Retrieval-Augmented Generation (RAG) system designed to answer questions about course materials using semantic search and AI-powered responses. It's a full-stack web application that enables users to query course materials and receive intelligent, context-aware responses.

The application uses:
- **ChromaDB** for vector storage of course content
- **Anthropic's Claude** for AI generation
- **FastAPI** for the backend API
- **Sentence Transformers** for embedding generation
- **HTML/CSS/JavaScript** for the frontend interface

## Architecture

The system consists of three main components:

1. **Frontend** (`frontend/`): A web interface for users to interact with the system
2. **Backend** (`backend/`): A FastAPI application that handles API requests and orchestrates the RAG system
3. **Vector Store** (`backend/vector_store.py`): ChromaDB implementation for storing and searching course content

The core RAG logic is implemented in `backend/rag_system.py`, which coordinates document processing, vector storage, and AI generation.

## Key Features

- Semantic search across course materials
- Conversation history management
- Course analytics and statistics
- Web-based user interface
- API endpoints for integration

## Prerequisites

- Python 3.13 or higher
- uv (Python package manager)
- An Anthropic API key (for Claude AI)

## Installation & Setup

1. **Install uv** (if not already installed):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

2. **Install Python dependencies**:
   ```bash
   uv sync
   ```

3. **Set up environment variables**:
   Create a `.env` file in the root directory:
   ```bash
   ANTHROPIC_API_KEY=your_anthropic_api_key_here
   ```

## Running the Application

### Quick Start

Use the provided shell script:
```bash
chmod +x run.sh
./run.sh
```

### Manual Start

```bash
cd backend
uv run uvicorn app:app --reload --port 8000
```

The application will be available at:
- Web Interface: `http://localhost:8000`
- API Documentation: `http://localhost:8000/docs`

## Development Conventions

- Python 3.13+ is required
- Dependencies are managed with `uv` and specified in `pyproject.toml`
- The backend follows FastAPI conventions
- Course materials are processed and stored in `docs/` directory
- Vector database is stored in `backend/chroma_db/`
- Environment variables are loaded from `.env` file

## API Endpoints

- `POST /api/query` - Process a query and return response with sources
- `GET /api/courses` - Get course analytics and statistics
- Static files are served from the `frontend/` directory

## Project Structure

```
.
├── backend/
│   ├── ai_generator.py      # AI response generation
│   ├── app.py              # FastAPI application
│   ├── config.py           # Configuration settings
│   ├── document_processor.py # Course document processing
│   ├── models.py           # Data models
│   ├── rag_system.py       # RAG system orchestration
│   ├── search_tools.py     # Search tool implementations
│   ├── session_manager.py  # Conversation session management
│   ├── vector_store.py     # ChromaDB vector storage
│   └── chroma_db/          # ChromaDB persistent storage
├── docs/                   # Course materials
├── frontend/               # Web interface
├── .env.example           # Example environment variables
├── pyproject.toml         # Python dependencies
├── README.md              # Project documentation
└── run.sh                 # Startup script
```