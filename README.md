# Pinecone Vector Database Demo

This project is a small, beginner-friendly example of semantic search with
[Pinecone](https://www.pinecone.io/). The [`vectordb.ipynb`](./vectordb.ipynb)
notebook turns sample documents into vectors, stores them in a Pinecone index,
and searches for documents related to a question.

## What the notebook does

1. Loads sample documents and their metadata.
2. Creates an embedding (a list of numbers representing meaning) for each
   document.
3. Creates a Pinecone serverless index if it does not already exist.
4. Saves the document vectors and metadata to the index.
5. Turns a question into an embedding and asks Pinecone for the three closest
   documents whose metadata matches the filter.

The notebook uses the `text-embedding-3-small` model through Euri's
OpenAI-compatible API. Its 1,536-dimensional embeddings match the index
configuration in the notebook.

## Requirements

- Python 3.12 or newer
- [uv](https://docs.astral.sh/uv/)
- A Pinecone API key
- An Euri API key with access to the embedding model
- VS Code with the Jupyter extension, or another Jupyter notebook environment

## Setup

Install the project dependencies:

```bash
uv sync
```

Create a `.env` file in the project directory and add your API keys:

```dotenv
PINECONE_API_KEY=your-pinecone-api-key
EURI_API_KEY=your-euri-api-key
```

Keep `.env` private; do not commit API keys or share them.

## Run the notebook

1. Open `vectordb.ipynb` in VS Code.
2. Select the Python environment created by `uv sync` (the project `.venv`).
3. Run the notebook cells from top to bottom.
4. Change `query_text` in the final search cell to try another question.

Running the notebook connects to Pinecone and Euri and may incur usage charges.
It creates or uses the Pinecone index named `doc-index` in AWS `us-east-1`.

## Filter results by metadata

The final search applies this metadata filter:

```python
filter={"difficulty": {"$eq": "beginner"}}
```

It tells Pinecone to search only documents whose `difficulty` metadata is
`"beginner"`, then return the three closest matches. The sample documents
currently all have that difficulty value. To filter on another value, update
the filter and make sure at least one stored document has the matching
metadata. Other metadata fields in the sample include `category` and `tag`.

## Project files

- `vectordb.ipynb` — the end-to-end vector search example
- `pyproject.toml` — project metadata and dependencies
- `uv.lock` — locked dependency versions