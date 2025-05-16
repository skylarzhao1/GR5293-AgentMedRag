
## Data Pipeline

1. **Data Extraction**:
   - Run `download_pubmed.py` to fetch 2400 medical articles from PubMed
   - Output stored as JSON files

2. **QA Pair Generation**:
   - `generatequery.ipynb` creates ground truth QA pairs
   - Example pair format:
     ```json
     {
       "abstract": "Hepatolithiasis is prevalent in East Asian countries...",
       "question": "What are the common risk factors associated with hepatolithiasis?",
       "answer": "The common risk factors are infection, cholangitis... bile stasis."
     }
     ```
   - Output saved to `googledrive-finalproject/generated_qa_pairs.json`

## Environment Setup

```bash
# Core packages
pip install -U ragas langchain-openai langchain chromadb
pip install bitsandbytes accelerate transformers

# Additional requirements
pip install transformers==4.40.0 \
            accelerate==0.29.2 \
            bitsandbytes==0.42.0 \
            sentence-transformers==2.6.1 \
            chromadb==1.0.9 \
            faiss-cpu==1.8.0 \
            nest_asyncio==1.6.0 \
            huggingface-hub==0.21.4 \
            tqdm==4.66.1
  

 ```



## Troubleshooting Guide

| Problem | Solution |
|---------|----------|
| `ModuleNotFoundError: langchain_community` | Update LangChain: `pip install -U langchain` |
| `ValidationError` with `ChatOpenAI` | Use correct import: `from langchain_openai import ChatOpenAI` |
| `401 Error` (Gated model access) | Log in via `notebook_login()` or use public models |
| `AttributeError: 'str' object has no 'content'` | Update to compatible LangChain versions |
| Evaluation hangs or too slow | Reduce `top_k` or chunk size, use smaller models (e.g., GPT-3.5 instead of GPT-4) |
| Poor re-ranking results | 1. Adjust retrieval `k` parameter<br>2. Check context length (neither too short nor too long) |
| CUDA out of memory | 1. Reduce batch size<br>2. Use smaller models<br>3. Enable 4-bit quantization with `bitsandbytes` |
| API rate limits | 1. Add delays between requests<br>2. Use local models where possible |
| ChromaDB connection issues | 1. Verify Chroma server is running<br>2. Check port configurations |
| Metric scores inconsistent | 1. Verify ground truth data quality<br>2. Check token limits aren't truncating important content |


