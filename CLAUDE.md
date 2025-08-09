# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Running the Application
```bash
# Quick start (recommended) - creates docs/ directory if missing
./run.sh

# Manual start
cd backend && uv run uvicorn app:app --reload --port 8000

# Windows users: Use Git Bash for shell script execution
```

### Dependency Management
```bash
# Install/sync dependencies (uses pyproject.toml)
uv sync

# Add new dependency
uv add package-name

# Python 3.13+ required
```

### Environment Setup
Create `.env` file in root directory:
```
ANTHROPIC_API_KEY=your_anthropic_api_key_here
```

### Development Notes
- No testing framework configured currently
- No linting/formatting tools configured
- Uses uv for fast Python package management
- Backend runs on port 8000, serves both API and static frontend
- ChromaDB storage in `./chroma_db` directory (auto-created)

## Architecture Overview

This is a **RAG (Retrieval-Augmented Generation) chatbot** for course materials with a tool-based AI approach where Claude autonomously decides when to search course content.

### Core Architecture Pattern

The system uses a **two-stage AI processing** approach:
1. **Initial Claude Call**: Receives user query with available tools
2. **Tool Execution**: If needed, performs semantic search in course content
3. **Final Claude Call**: Generates response using tool results

### Key Components

**Frontend** (`frontend/`):
- Single-page web app with chat interface
- JavaScript handles API communication and UI updates
- Renders markdown responses with collapsible source citations

**Backend API** (`backend/app.py`):
- FastAPI server serving both API endpoints and static frontend
- Two main endpoints: `/api/query` for chat, `/api/courses` for stats
- Automatically loads course documents from `docs/` folder on startup

**RAG System** (`backend/rag_system.py`):
- Main orchestrator coordinating all components
- Manages session-based conversations with history
- Integrates tool manager for AI-driven search decisions

**Document Processing** (`backend/document_processor.py`):
- Parses structured course documents with metadata (title, instructor, lessons)
- Performs sentence-based text chunking with configurable overlap
- Extracts lesson boundaries using regex patterns like "Lesson X:"

**AI Integration** (`backend/ai_generator.py`):
- Anthropic Claude API wrapper with tool calling support
- Handles two-stage conversation: tool detection → execution → final response
- Uses static system prompt optimized for educational content

**Vector Storage** (`backend/vector_store.py`):
- ChromaDB integration for semantic search
- Stores both course metadata and content chunks with embeddings
- Supports filtering by course name and lesson number

**Search Tools** (`backend/search_tools.py`):
- Tool-based architecture where AI decides when to search
- `CourseSearchTool` provides semantic search with course/lesson filtering
- Returns formatted results with source attribution for UI display

### Document Format

Course documents in `docs/` follow this structure:
```
Course Title: [title]
Course Link: [url]
Course Instructor: [instructor]

Lesson 0: Introduction
Lesson Link: [optional lesson url]
[lesson content...]

Lesson 1: Next Topic
[lesson content...]
```

### Configuration

All settings in `backend/config.py`:
- `CHUNK_SIZE`: 800 characters (text chunk size)
- `CHUNK_OVERLAP`: 100 characters (overlap between chunks) 
- `MAX_RESULTS`: 5 (search results returned)
- `MAX_HISTORY`: 2 (conversation messages remembered)
- `ANTHROPIC_MODEL`: "claude-sonnet-4-20250514"
- `EMBEDDING_MODEL`: "all-MiniLM-L6-v2"

### Session Management

The system maintains conversation context through session IDs:
- Frontend tracks `currentSessionId` 
- Backend stores conversation history in memory
- Each session remembers last `MAX_HISTORY` exchanges

### Key Integration Points

When adding new features:
- **New tools**: Implement `Tool` interface in `search_tools.py`
- **Document types**: Extend `DocumentProcessor.process_course_document()`
- **AI behavior**: Modify system prompt in `AIGenerator.SYSTEM_PROMPT`
- **Search logic**: Update `VectorStore.search()` method
- **UI components**: Add to `frontend/index.html` with corresponding JavaScript

### Error Handling Strategy

The system uses graceful degradation:
- Tool search failures return "no results found" rather than crashing
- Missing API keys show clear error messages
- Frontend shows loading states and handles network failures
- Each layer catches exceptions and provides meaningful fallbacks