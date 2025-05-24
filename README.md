# Pizza Restaurant Review RAG System

A Retrieval-Augmented Generation (RAG) system that answers questions about a pizza restaurant using customer reviews as context. The system uses LangChain, Ollama, and ChromaDB to provide intelligent responses based on real customer feedback.

## Features

- **Vector-based retrieval**: Uses embeddings to find relevant restaurant reviews
- **Local LLM integration**: Powered by Ollama's Llama 3.2 model
- **Persistent vector storage**: ChromaDB for efficient document storage and retrieval
- **Interactive Q&A**: Command-line interface for asking questions about the restaurant
- **Review-based context**: Answers are grounded in actual customer reviews

## Architecture

The system consists of two main components:

1. **Vector Store Setup** (`vector.py`): 
   - Loads restaurant reviews from CSV
   - Creates embeddings using Ollama's mxbai-embed-large model
   - Stores documents in ChromaDB for fast retrieval

2. **Question Answering** (`main.py`):
   - Retrieves relevant reviews based on user questions
   - Uses Llama 3.2 to generate context-aware responses
   - Provides an interactive chat interface

## Prerequisites

- Python 3.8+
- [Ollama](https://ollama.ai/) installed and running
- Required Ollama models:
  - `llama3.2` (for text generation)
  - `mxbai-embed-large` (for embeddings)

### Installing Ollama Models

```bash
# Install the required models
ollama pull llama3.2
ollama pull mxbai-embed-large
```

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd pizza-restaurant-rag
```

2. Install the required Python packages:
```bash
pip install -r requirements.txt
```

3. Ensure you have your restaurant reviews CSV file:
   - File should be named `realistic_restaurant_reviews.csv`
   - Required columns: `Title`, `Review`, `Rating`, `Date`

## Usage

1. **First Run** - Initialize the vector database:
```bash
python main.py
```
On the first run, the system will:
- Load reviews from the CSV file
- Generate embeddings for each review
- Create and populate the ChromaDB vector store

2. **Ask Questions**:
```
Ask a question (q to quit): What do customers think about the pizza quality?
```

3. **Exit the Program**:
```
Ask a question (q to quit): q
```

## Example Questions

- "What do customers think about the pizza quality?"
- "How is the service at this restaurant?"
- "Are there any complaints about delivery times?"
- "What are the most popular menu items?"
- "How do customers rate the value for money?"

## File Structure

```
├── main.py                           # Main application with Q&A interface
├── vector.py                         # Vector store setup and retrieval logic
├── requirements.txt                  # Python dependencies
├── realistic_restaurant_reviews.csv  # Restaurant reviews dataset
├── chroma_langchain_db/              # ChromaDB storage (auto-created)
└── README.md                         # This file
```

## Configuration

### Adjusting Retrieval Parameters

In `vector.py`, you can modify the retriever settings:

```python
retriever = vector_store.as_retriever(
    search_kwargs={"k": 5}  # Number of reviews to retrieve
)
```

### Changing the LLM Model

In `main.py`, you can switch to different Ollama models:

```python
model = OllamaLLM(model="llama3.2")  # Change to any Ollama model
```

### Customizing the Prompt Template

Modify the template in `main.py` to change how the AI responds:

```python
template = """
You are an expert in answering questions about a pizza restaurant
Here are some relevant reviews: {reviews}
Here is the question to answer: {question}
"""
```

## Data Format

Your CSV file should contain the following columns:

| Column | Description | Example |
|--------|-------------|---------|
| Title | Review title | "Great Pizza!" |
| Review | Full review text | "The margherita pizza was excellent..." |
| Rating | Numerical rating | 4.5 |
| Date | Review date | "2024-01-15" |

## Troubleshooting

### Common Issues

1. **Ollama not running**: Make sure Ollama is installed and the service is running
2. **Models not found**: Ensure you've pulled the required models (`llama3.2` and `mxbai-embed-large`)
3. **CSV file missing**: Verify `realistic_restaurant_reviews.csv` exists in the project directory
4. **Permission errors**: Check write permissions for the `chroma_langchain_db` directory

### Performance Tips

- The first run will be slower as it generates embeddings for all reviews
- Subsequent runs will be faster as the vector database is persisted
- For large datasets, consider increasing the chunk size or using more powerful hardware

## Dependencies

- `langchain`: Core LangChain framework
- `langchain-ollama`: Ollama integration for LangChain
- `langchain-chroma`: ChromaDB vector store integration
- `pandas`: Data manipulation and CSV handling

## Future Enhancements

- Web interface using Streamlit or FastAPI
- Support for multiple restaurants
- Advanced filtering by rating, date, or keywords
- Integration with live review APIs
- Sentiment analysis of reviews

