# Typesense RAG Pipeline Implementation

A comprehensive Retrieval-Augmented Generation (RAG) system using Typesense vector search, LangChain, and Google Generative AI for intelligent document search and question answering.

## 🚀 Project Overview

This project implements a complete RAG pipeline that allows you to:

- **Search and Index Documents**: Store documents in Typesense with vector embeddings
- **Intelligent Retrieval**: Find relevant documents using semantic similarity search  
- **Question Answering**: Generate answers using Google Generative AI based on retrieved context
- **Flexible Configuration**: Easy setup with environment variables

## 📋 Prerequisites

### Required Dependencies

Install the following Python packages:

```bash
pip install typesense
pip install langchain langchain-community
pip install langchain-google-genai
pip install langchain-huggingface
pip install python-dotenv
pip install sentence-transformers
```

### API Keys & Access

- **Typesense Account**: Sign up at [typesense.org](https://typesense.org)
- **Google Generative AI API Key**: Get from [Google AI Studio](https://makersuite.google.com/app/apikey)
- **Typesense Cloud Instance**: Either use provided cloud instance or set up your own

## 🛠️ Installation & Setup

### 1. Clone/Setup Project Structure

Ensure your project has the following structure:
```
Project/Yt_rag/
├── typesense.ipynb          # Main notebook
├── books.jsonl             # Book data for indexing (optional)
├── test.txt                # Sample text file for RAG testing
├── .env                    # Environment variables
└── README.md               # This file
```

### 2. Environment Configuration

Create a `.env` file in the project root:

```bash
# Google Generative AI API Key
GOOGLE_GENERATIVE_AI_API_KEY=your_google_api_key_here

# Typesense API Key
TYPESENSE_API_KEY=your_typesense_api_key_here
```

### 3. File Requirements

- **`books.jsonl`**: JSONL file containing book data (optional, for testing collection creation)
- **`test.txt`**: Text file containing documents for RAG testing
- Ensure files are located in the correct paths as referenced in the notebook

## 🔧 Configuration Details

### Typesense Client Configuration

The notebook uses a pre-configured Typesense cloud instance:
```python
client = typesense.Client({
    'nodes': [{
        'host': '92pg7uq8oxmi04sfp-1.a1.typesense.net',
        'port': '443',
        'protocol': 'https'
    }],
    'api_key': 'Xfzt4S6IVQKsnMyIogxZ6KS94LR1oZUx',
    'connection_timeout_seconds': 2
})
```

### Collection Schema

The notebook includes schema definitions for books with fields:
- `title` (string)
- `authors` (string[], faceted)
- `publication_year` (int32, faceted)
- `ratings_count` (int32)
- `average_rating` (float)

## 📖 Usage Guide

### 1. Basic Typesense Operations

#### Create Collection Schema
```python
books_schema = {
  'name': 'books',
  'fields': [
    {'name': 'title', 'type': 'string'},
    {'name': 'authors', 'type': 'string[]', 'facet': True},
    {'name': 'publication_year', 'type': 'int32', 'facet': True},
    {'name': 'ratings_count', 'type': 'int32'},
    {'name': 'average_rating', 'type': 'float'}
  ],
  'default_sorting_field': 'ratings_count'
}
client.collections.create(books_schema)
```

#### Import Documents
```python
with open('C:/Users/19736/Desktop/Project/Yt_rag/books.jsonl', 'r', encoding='utf-8') as jsonl_file:
    data = jsonl_file.read()
    client.collections['books'].documents.import_(data)
```

#### Search Documents
```python
search_parameters = {
    'q': "harry potter",
    'query_by': "title,authors",
    'sort_by': "ratings_count:desc",
    'filter_by': "publication_year:<1998"
}
results = client.collections['books'].documents.search(search_parameters)
```

### 2. RAG Pipeline Setup

#### Load and Process Documents
```python
from langchain_community.document_loaders import TextLoader
from langchain_community.vectorstores import Typesense
from langchain_text_splitters import CharacterTextSplitter
from langchain.embeddings import HuggingFaceEmbeddings

loader = TextLoader('C:/Users/19736/Desktop/Project/Yt_rag/test.txt')
documents = loader.load()
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=100)
docs = text_splitter.split_documents(documents)
```

#### Create Vector Store
```python
embeddings = HuggingFaceEmbeddings()
docsearch = Typesense.from_documents(
    docs, 
    embeddings,
    typesense_client_params={
        'host': '92pg7uq8oxmi04sfp-1.a1.typesense.net',
        'port': '443',
        'protocol': 'https',
        'typesense_api_key': typesense_api_key,
        'typesense_collection_name': "lang-chain"
    },
)
```

#### Perform Similarity Search
```python
query = "what is Artificial intelligence?"
found_docs = docsearch.similarity_search(query)
print(found_docs[0].page_content)
```

#### Use as Retriever
```python
retriever = docsearch.as_retriever()
query = "AI indepth explanation"
result = retriever.invoke(query)[0].page_content
```

## 🏗️ Code Structure Breakdown

### Section 1: Typesense Client Setup
- Initializes connection to Typesense cloud instance
- Configures API keys and connection parameters

### Section 2: Collection Schema & Creation
- Defines schema for book data with facets
- Provides example for creating collections

### Section 3: Document Import
- Imports JSONL data into Typesense collection
- Handles bulk document operations

### Section 4: Search Operations
- Demonstrates basic search with filters
- Shows sorting and query parameters

### Section 5: RAG Pipeline Integration
- LangChain integration with Typesense
- Document loading and preprocessing
- Embedding generation and vector storage

### Section 6: Retrieval & Search
- Similarity search implementation
- Retriever configuration for LLM integration

## 🔍 Example Workflows

### Workflow 1: Document Indexing
1. Prepare your documents (text files or JSONL)
2. Create appropriate collection schema
3. Import documents into Typesense
4. Verify indexing with search queries

### Workflow 2: RAG Question Answering
1. Load text documents using TextLoader
2. Split documents into chunks
3. Generate embeddings using HuggingFace
4. Store vectors in Typesense
5. Perform similarity search
6. Use retrieved context for LLM queries

## 🛠️ Troubleshooting

### Common Issues

#### Connection Errors
- Verify Typesense host and port
- Check API key permissions
- Ensure network connectivity

#### Import Failures
- Verify JSONL file format
- Check file encoding (use UTF-8)
- Ensure collection exists before import

#### Embedding Issues
- Confirm HuggingFace model availability
- Check memory requirements for embedding generation
- Verify Typesense vector field configuration

#### Environment Variable Problems
- Ensure .env file exists and is loaded
- Verify API key format and permissions
- Check for typos in variable names

### Performance Tips
- Use appropriate chunk sizes for document splitting
- Consider indexing strategy for large document collections
- Monitor Typesense memory usage for vector operations

## 📚 Additional Resources

- [Typesense Documentation](https://typesense.org/docs/)
- [LangChain Typesense Integration](https://python.langchain.com/docs/integrations/vectorstores/typesense/)
- [Google Generative AI Documentation](https://ai.google.dev/docs)
- [HuggingFace Embeddings Guide](https://huggingface.co/sentence-transformers)

## 🤝 Contributing

Feel free to extend this implementation with:
- Additional document loaders
- Different embedding models
- Enhanced search filters
- Performance optimizations
- Web interface for user interaction

## 📄 License

This project is open source and available under the MIT License.
