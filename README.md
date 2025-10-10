# yt-rag

This project features a sophisticated multi-level Retrieval-Augmented Generation (RAG) system built using Google’s Gemini 2.5 Flash model. The system is designed to provide efficient, context-aware AI responses with a focus on transparency and scalability.

## Key Features

*   **Multi-level RAG System**: Initiated with a core function for document retrieval and answer generation, then evolved into a robust, multi-level architecture.
*   **Gemini 2.5 Flash Integration**: Leverages Google's Gemini 2.5 Flash model for powerful and efficient AI response generation.
*   **Advanced Document Retrieval**: Includes features such as relevance filtering, source metadata inclusion, and confidence scoring to enhance the quality of retrieved information.
*   **Flexible Context Handling**: Offers an optional full context return for comprehensive understanding.
*   **Class-Based Architecture**: The entire system is encapsulated within a class, providing:
    *   **Streaming Answer Display**: Enables a dynamic and responsive user experience as answers are generated.
    *   **Citation Formatting**: Automatically formats citations for enhanced transparency and credibility.
    *   **Answer Summarization**: Provides concise summaries of generated answers.
    *   **Query History Tracking**: Maintains a record of past queries for easy reference.

## Project Description

The project began with basic document retrieval and answer generation capabilities. It was then significantly advanced by incorporating critical features such as relevance filtering for more precise results, the inclusion of source metadata for traceability, and confidence scoring to indicate the reliability of answers. The ability to return the full context was also added, allowing for deeper analysis when required. The culmination of this development is a well-structured class that manages all these functionalities, demonstrating a smooth integration of Gemini 2.5 for efficient, context-aware AI responses with transparency and scalability.

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
