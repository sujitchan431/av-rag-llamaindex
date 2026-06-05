<div align="center">

# 🦙 Advanced RAG with LlamaIndex — 2 Systems

[![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)](https://python.org)
[![LlamaIndex](https://img.shields.io/badge/LlamaIndex-0.10.x-8A2BE2)](https://llamaindex.ai)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o-412991?logo=openai)](https://openai.com)
[![Cohere](https://img.shields.io/badge/Cohere-Reranker-blue)](https://cohere.com)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> Two enterprise-grade RAG systems: LIC insurance policy (3 embeddings × 2 LLMs compared) and Coca-Cola 10-K filings 2014–2023 (3 advanced retrievers + Cohere reranker + SubQuestion engine).

**🎓 Part of the [Analytics Vidhya GenAI Pinnacle Plus Program](https://www.analyticsvidhya.com/)**

</div>

---

## 📋 Overview

Two production-quality RAG systems demonstrating advanced LlamaIndex features far beyond basic VectorStoreIndex. Sub-1 does a systematic comparison of 6 embedding/LLM combos. Sub-2 implements 3 advanced retriever types, Cohere reranking, and multi-hop question decomposition over 10 years of financial filings.

---

## 📁 Sub-Projects

```
RAG LamdaIndex/
├── 1st/  ← LIC Policy RAG (embedding + LLM comparison)
│   ├── lic_rag_system.ipynb
│   └── Final Policy document_LICs New Jeevan Shanti.pdf
└── 2nd/  ← Coca-Cola 10-K RAG (advanced retrievers)
    └── coca_cola_10k_rag.ipynb
```

---

## 🔬 Sub-1 — LIC Insurance Policy RAG

**Document:** LIC New Jeevan Shanti policy (India)  
**Parsing:** LlamaParse (markdown quality output)  
**Chunking:** SentenceSplitter (512 tokens, 64 overlap)

### 6-Combination Comparison Matrix

| | GPT-4o | Mixtral-8x7B |
|--|--------|--------------|
| OpenAI embeddings | ✅ | ✅ |
| BGE embeddings (free, local) | ✅ | ✅ |
| Jina AI embeddings | ✅ | ✅ |

**Metrics:** Faithfulness, Relevancy, Response latency

### Prerequisites
```
OPENAI_API_KEY, LLAMA_CLOUD_API_KEY, TOGETHER_API_KEY, JINAAI_API_KEY
```

---

## 🏢 Sub-2 — Coca-Cola 10-K QA System (2014–2023)

**Data:** 10 years of SEC 10-K filings (auto-downloaded from EDGAR)

### System Architecture

| Stage | Component |
|-------|-----------|
| Ingestion | LlamaParse → markdown |
| Retriever A | **Sentence Window** (SentenceWindowNodeParser) |
| Retriever B | **AutoMerging** (HierarchicalNodeParser) |
| Retriever C | **Auto Retriever** (metadata-aware, year/filing type) |
| Post-Retrieval | **Cohere Reranker** |
| Query Engine A | Standard `RetrieverQueryEngine` |
| Query Engine B | **SubQuestionQueryEngine** (multi-hop) |
| LLM | GPT-4o-mini, temperature=0 |
| Evaluation | Faithfulness + Relevancy (LLM-as-judge) |

### SubQuestion Engine Example
> "Compare Coca-Cola's net revenue in 2018 vs 2022 and explain the difference"  
> → Decomposes into: [Q1: 2018 revenue] + [Q2: 2022 revenue] + [Q3: explanation]

### Prerequisites
```
OPENAI_API_KEY, LLAMA_CLOUD_API_KEY, COHERE_API_KEY
```

---

## ⚙️ Install

```bash
pip install llama-index-core llama-index-llms-openai llama-index-embeddings-openai \
    llama-index-postprocessor-cohere-rerank llama-parse nest-asyncio \
    requests beautifulsoup4 pandas matplotlib seaborn tqdm
```

---

## 💡 Key Learnings

- **LlamaParse** — why it produces better structured output than PyPDF2
- **Sentence Window Retriever** — retrieves single sentences, expands context window at synthesis
- **AutoMerging Retriever** — hierarchical nodes; retrieves leaves, merges to parent if enough leaves retrieved
- **Auto Retriever** — uses LLM to generate metadata filters before search
- **Cohere Reranker** — cross-encoder reranking beats bi-encoder similarity alone
- **SubQuestion Engine** — decomposes multi-part questions into sub-queries per document
- Index persistence with pickle — skip re-parsing on re-runs
- SEC EDGAR programmatic access with `requests` + `BeautifulSoup`

---

## 🎓 Program Context

**Analytics Vidhya GenAI Pinnacle Plus Program** — Advanced RAG with LlamaIndex module.

---

## 📄 License

MIT © 2026 [sujitchan431](https://github.com/sujitchan431)
