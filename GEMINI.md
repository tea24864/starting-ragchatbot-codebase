# GEMINI.md

## Project Overview

This project is a full-stack web application that implements a Retrieval-Augmented Generation (RAG) system. It's designed to answer questions about course materials. The application uses a Python backend with FastAPI and a vanilla JavaScript frontend.

**Key Technologies:**

*   **Backend:** Python, FastAPI, Uvicorn
*   **Frontend:** HTML, CSS, JavaScript
*   **AI/ML:**
    *   `anthropic` (for Claude AI)
    *   `chromadb` (for vector storage)
    *   `sentence-transformers` (for text embeddings)
*   **Dependency Management:** `uv`

**Architecture:**

The application is divided into a `backend` and a `frontend` directory.

*   The **backend** is a FastAPI application that exposes a REST API for querying the RAG system. It handles document processing, vectorization, and AI-powered response generation.
*   The **frontend** is a single-page web application that provides a chat interface for users to interact with the RAG system.

## Building and Running

### Prerequisites

*   Python 3.13 or higher
*   `uv` (Python package manager)
*   An Anthropic API key

### Installation

1.  **Install `uv`:**
    ```bash
    curl -LsSf https://astral.sh/uv/install.sh | sh
    ```

2.  **Install Python dependencies:**
    ```bash
    uv sync
    ```

3.  **Set up environment variables:**

    Create a `.env` file in the root directory with your Anthropic API key:
    ```
    ANTHROPIC_API_KEY=your_anthropic_api_key_here
    ```

### Running the Application

**Quick Start:**

```bash
chmod +x run.sh
./run.sh
```

**Manual Start:**

```bash
cd backend
uvicorn app:app --reload --port 8000
```

The application will be available at:

*   **Web Interface:** `http://localhost:8000`
*   **API Documentation:** `http://localhost:8000/docs`

## Development Conventions

*   **Backend:**
    *   The main application logic is in `backend/app.py` and `backend/rag_system.py`.
    *   The application uses a modular structure, with different components (e.g., `DocumentProcessor`, `VectorStore`, `AIGenerator`) in separate files.
*   **Frontend:**
    *   The frontend is built with vanilla HTML, CSS, and JavaScript.
    *   The main JavaScript file is `frontend/script.js`.
*   **Dependencies:**
    *   Python dependencies are managed with `uv` and defined in `pyproject.toml`.
*   **API:**
    *   The API is documented using OpenAPI (Swagger) and is available at `/docs`.
