# LLAMA Index

LLAMA Index is a Python library for efficient document retrieval and querying using vector store indexes. This repository contains code snippets demonstrating how to use LLAMA Index to create a vector store index, load documents, query the index, and handle storage.

## Installation

To use LLAMA Index, follow these steps:

1. Install the required dependencies by running `pip install llama-index`.
2. Set up your OpenAI API key by creating a `.env` file and adding `OPENAI_API_KEY=your_api_key`.
3. Run the provided code snippets to create and query the vector store index.

## Usage

### 1. Creating a Vector Store Index

```python
import os
from dotenv import load_dotenv
from llama_index import VectorStoreIndex, SimpleDirectoryReader

# Load environment variables
load_dotenv()

# Set OpenAI API key
os.environ['OPENAI_API_KEY'] = os.getenv("OPENAI_API_KEY")

# Load documents
documents = SimpleDirectoryReader("data").load_data()

# Create vector store index
index = VectorStoreIndex.from_documents(documents, show_progress=True)
```

### 2. Querying the Index

```python
from llama_index.retrievers import VectorIndexRetriever
from llama_index.query_engine import RetrieverQueryEngine
from llama_index.indices.postprocessor import SimilarityPostprocessor

# Set up retriever and postprocessor
retriever = VectorIndexRetriever(index=index, similarity_top_k=4)
postprocessor = SimilarityPostprocessor(similarity_cutoff=0.80)

# Set up query engine
query_engine = RetrieverQueryEngine(retriever=retriever, node_postprocessors=[postprocessor])

# Query the index
response = query_engine.query("What is attention is all you need?")
```

### 3. Storing and Loading the Index

```python
import os.path
from llama_index import VectorStoreIndex, SimpleDirectoryReader, StorageContext, load_index_from_storage

# Define storage directory
PERSIST_DIR = "./storage"

# Check if storage exists
if not os.path.exists(PERSIST_DIR):
    # Load documents and create index
    documents = SimpleDirectoryReader("data").load_data()
    index = VectorStoreIndex.from_documents(documents)
    # Store index
    index.storage_context.persist(persist_dir=PERSIST_DIR)
else:
    # Load existing index
    storage_context = StorageContext.from_defaults(persist_dir=PERSIST_DIR)
    index = load_index_from_storage(storage_context)
```

## Contributing

Contributions to LLAMA Index are welcome! Please feel free to submit issues or pull requests for new features, improvements, or bug fixes.

## License

LLAMA Index is licensed under the [MIT License](LICENSE).