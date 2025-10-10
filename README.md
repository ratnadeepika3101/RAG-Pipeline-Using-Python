# yt-rag

A Python project implementing a Retrieval-Augmented Generation (RAG) pipeline. This project demonstrates how to ingest documents, embed them, store them in a vector database, and retrieve relevant documents based on a query.

## Project Structure

```
yt-rag/
├── .gitignore
├── .python-version
├── main.py                 # Simple "Hello World" script
├── pyproject.toml         # Project build system and metadata
├── README.md              # This file
├── requirements.txt       # Python dependencies
├── uv.lock               # Dependency lock file for uv
├── data/
│   ├── pdf_files/        # Directory for PDF documents
│   │   ├── Introduction_to_Python_Programming_-_WEB.pdf
│   │   ├── sample_pdf-1.pdf
│   │   ├── sample_pdf-2.pdf
│   │   └── sample_pdf-3.pdf
│   ├── text_files/       # Directory for text documents
│   │   ├── Machinelearning_intro.txt
│   │   └── python_intro.txt
│   └── vector_store/     # ChromaDB persistent storage
│       ├── chroma.sqlite3
│       └── 2959224d-c294-4eeb-91bf-baa5c66a75f5/
├── notebook/
│   └── document.ipynb    # Jupyter Notebook with RAG implementation details
└── main.py               # Main Python script (currently a simple hello world)
```

## Setup

### Prerequisites

*   Python 3.x
*   `pip` or `uv` (preferred for dependency management)

### Installation

1.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd yt-rag
    ```

2.  **Install dependencies:**
    Using `uv` (recommended):
    ```bash
    uv pip install -r requirements.txt
    ```
    Or using `pip`:
    ```bash
    pip install -r requirements.txt
    ```

## RAG Pipeline Overview

The core RAG logic is implemented in the `notebook/document.ipynb` Jupyter Notebook. The pipeline consists of the following main steps:

### 1. Data Ingestion

Documents (both text and PDF) are loaded into the system.
*   **Text Files**: Loaded using `langchain_community.document_loaders.TextLoader`.
*   **PDF Files**: Loaded using `langchain_community.document_loaders.PyMuPDFLoader`.

The notebook includes example code to create sample text files (`data/text_files/python_intro.txt`, `data/text_files/Machinelearning_intro.txt`) and load existing PDF files from `data/pdf_files/`.

### 2. Embedding Generation

Document content is converted into numerical embeddings using sentence transformers.
*   **Class**: `EmbeddingManager` in `notebook/document.ipynb`
*   **Model**: Uses `all-MiniLM-L6-v2` from `sentence-transformers` to generate dense vector representations of the text. This allows for semantic similarity comparisons.

### 3. Vector Store

Documents and their corresponding embeddings are stored in a vector database for efficient retrieval.
*   **Class**: `VectorStore` in `notebook/document.ipynb`
*   **Database**: Uses `ChromaDB` as the persistent vector store.
*   **Functionality**: The `VectorStore` class handles initializing the ChromaDB client, creating a collection, and adding documents with their embeddings and metadata. The data is persisted in the `data/vector_store/` directory.

### 4. Retrieval

When a user query is made, the system retrieves the most relevant documents from the vector store.
*   **Class**: `RAGRetriever` in `notebook/document.ipynb`
*   **Process**:
    1.  The query text is embedded using the same `EmbeddingManager`.
    2.  A similarity search (e.g., cosine similarity) is performed against the embeddings in the vector store.
    3.  The top-k most similar documents are returned, along with their content, metadata, and similarity scores.

## Running the Example

The `main.py` script currently contains a simple "Hello World" message:
```bash
python main.py
```

The detailed RAG implementation and examples are within the `notebook/document.ipynb` file. To run the cells in the notebook, you can use Jupyter or JupyterLab:
```bash
jupyter notebook notebook/document.ipynb
# or
jupyter lab notebook/document.ipynb
```

## Key Components (from `notebook/document.ipynb`)

*   **Document**: Langchain's `Document` object is used to store page content and metadata (e.g., source, author, creation date).
*   **EmbeddingManager**: Manages loading the sentence transformer model and generating embeddings for text.
*   **VectorStore**: Manages the ChromaDB connection, document storage (IDs, content, metadata, embeddings), and retrieval.
*   **RAGRetriever**: Provides the `retrieve` method to find relevant documents for a given query, utilizing the `EmbeddingManager` and `VectorStore`.

## Next Steps & Potential Enhancements

*   **Integrate with a Language Model**: Connect the retrieved documents to a Large Language Model (LLM) like GPT, LLaMA, or an open-source alternative to generate contextual answers. This is the "Augmented Generation" part of RAG.
*   **Main Script Enhancement**: Expand `main.py` to include a command-line interface or a simple web interface for querying the RAG system.
*   **Advanced Document Processing**: Implement more sophisticated document chunking strategies for better handling of large documents.
*   **Error Handling & Logging**: Add more robust error handling and logging throughout the pipeline.
*   **Configuration Management**: Use configuration files (e.g., YAML, JSON) for managing model names, database paths, and other parameters.
*   **Unit Tests**: Write unit and integration tests for the `EmbeddingManager`, `VectorStore`, and `RAGRetriever` classes.
