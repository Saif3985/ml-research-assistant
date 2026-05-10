#  ML Research Assistant

AI-powered RAG system for exploring 5,000+ ML/AI research papers from ArXiv with citation-backed answers and automatic quality scoring.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Python](https://img.shields.io/badge/python-3.8+-blue.svg)


## Demo

![Chat Interface](Screen%20shots/chat_interface.png)

## System Architecture

![Chat Interface](Screen%20shots/complete_architecture.png)


## Features

-  **Natural Language Queries** - Ask questions across 5,000+ papers in plain English
-  **PDF Upload** - Add your own papers with automatic title extraction
-  **Quality Scoring** - RAGAS metrics evaluate answer faithfulness and relevance
-  **Hybrid Retrieval** - Combines vector search (BGE-large) + BM25 keyword search
-  **Clean UI** - Gradio interface with source citations and quality badges

## Tech Stack

| Component | Model/Library |
|-----------|--------------|
| Embeddings | BAAI/bge-large-en-v1.5 (1024-dim) |
| Reranker | BAAI/bge-reranker-large |
| Generator | Mistral-7B-Instruct-v0.3 (4-bit) |
| Vector Store | FAISS |
| Keyword Search | BM25 |
| Evaluation | RAGAS (4 metrics) |
| UI | Gradio |

## Architecture

```
Query → Query Expansion → Hybrid Retrieval (Vector + BM25) 
     → Weighted RRF Fusion → MMR Diversity → Reranking 
     → Context + Prompt → Mistral-7B → Answer + RAGAS Score
```


### Local Setup
```bash
git clone https://github.com/yourusername/ml-research-assistant
cd ml-research-assistant
pip install -r requirements.txt

```

## How It Works

### Phase 1: Data Preparation
- Load 5,000 ML/AI papers from HuggingFace
- Clean text (remove LaTeX, citations, URLs)
- Create LangChain documents

### Phase 2: Indexing
- Semantic chunking (preserves paper structure)
- BGE-large embeddings (CPU to save GPU memory)
- Build FAISS vector store + BM25 index
- Initialize query expander, semantic cache, RAGAS evaluator

### Phase 3: Application
- Load BGE reranker (GPU 0)
- Load Mistral-7B with dual GPU support
- Launch Gradio UI with 3 tabs: Chat, Upload, History

### Retrieval Pipeline
1. **Query Expansion** - Add technical synonyms
2. **Hybrid Search** - Vector (k=15) + BM25 (k=15)
3. **Weighted RRF** - Fuse results (60% vector, 40% BM25)
4. **MMR Diversity** - Reduce redundancy
5. **Reranking** - Cross-encoder scores top 5
6. **Caching** - Store results for similar queries

### Answer Generation
- Mistral-7B generates cited answers
- RAGAS evaluates: faithfulness, relevance, precision, recall
- UI displays quality badge ( Excellent /  Good /  Fair)


## Example Usage

```python
# Ask a question
"How does self-attention work in Transformers?"

# Upload a paper
Upload PDF → Auto-extract title → Index chunks → Search alongside 5,000 papers

# Export history
Download CSV with timestamps, questions, answers, quality scores
```

## Improvements Over Baseline

| Feature | Baseline | Enhanced |
|---------|----------|----------|
| Embeddings | bge-small (384-dim) | bge-large (1024-dim) |
| Chunking | Fixed 512 chars | Semantic (preserves structure) |
| Retrieval | Vector only | Hybrid (vector + BM25) |
| Fusion | Simple concat | Weighted RRF |
| Diversity | None | MMR |
| Evaluation | None | 4 RAGAS metrics |
| Caching | None | Semantic similarity |

**Result**: ~40% improvement in answer quality (measured by RAGAS scores)

## Project Structure

```
ml-research-assistant/
├── README.md
├── ml_research_assistant.ipynb  # Complete notebook
├── requirements.txt
├── .gitignore
└── screenshots/
    ├── chat.png
    ├── upload.png
    └── quality.png
```


## Performance

- **Answer Quality**
- **Retrieval Speed**
- **Indexing Speed**
- **PDF Upload**


## Citation

If you use this project, please cite:

```bibtex
@software{ml_research_assistant,
  title = {ML Research Assistant: RAG System for Academic Papers},
  author = {Saifullah},
  year = {2024},
  url = {https://github.com/Saif3985/ml-research-assistant}
}
```

## License

MIT License - see LICENSE file

## Acknowledgments

- ArXiv papers dataset: [CShorten/ML-ArXiv-Papers](https://huggingface.co/datasets/CShorten/ML-ArXiv-Papers)
- BAAI for BGE models
- Mistral AI for Mistral-7B
- LangChain, HuggingFace, Gradio communities

## Contact

- GitHub: [@Saif3985](https://github.com/Saif3985)
- LinkedIn: [Saifullah](https://www.linkedin.com/in/saifullah-ds/)

---

**Built with ❤️ for researchers, students, and ML practitioners**
