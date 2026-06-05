# RAG LlamaIndex — 2 Sub-Projects

## Project Overview

Two advanced RAG systems built with LlamaIndex, comparing embeddings, retrievers, LLMs, and rerankers. Sub-1 covers an LIC insurance policy document; Sub-2 covers 10 years of Coca-Cola 10-K SEC filings.

## Sub-Project Structure

```
RAG LamdaIndex/
├── 1st/  ← LIC New Jeevan Shanti Policy RAG (3 embeddings × 2 LLMs = 6 combos)
└── 2nd/  ← Coca-Cola 10-K (2014–2023) RAG (3 retrievers + reranker + SubQuestion)
```

---

## Sub-Project 1 — LIC Policy RAG System

**File:** `lic_rag_system.ipynb`  
**Document:** LIC New Jeevan Shanti policy PDF (Indian insurance)

### Comparative Matrix

| Stage | Options |
|-------|---------|
| Parsing | LlamaParse (markdown output) |
| Chunking | SentenceSplitter (512 tokens, 64 overlap) |
| Embeddings | OpenAI `text-embedding-3-small` / BGE `bge-small-en-v1.5` (local) / Jina AI `jina-embeddings-v2-base-en` |
| LLMs | GPT-4o / Mixtral-8x7B-Instruct-v0.1 (via Together AI) |
| Evaluation | Faithfulness, Relevancy, Response time |

**6 combinations tested** (3 embeddings × 2 LLMs)

### APIs Used
- OpenAI, LlamaCloud (LlamaParse), Together AI (Mixtral), Jina AI

---

## Sub-Project 2 — Coca-Cola 10-K QA System

**File:** `coca_cola_10k_rag.ipynb`  
**Data:** 10 years of Coca-Cola 10-K filings from SEC EDGAR (CIK: 0000021344), FY 2014–2023

### System Architecture

| Stage | Component |
|-------|-----------|
| Ingestion | LlamaParse → markdown |
| Retriever A | **Sentence Window Retriever** (SentenceWindowNodeParser) |
| Retriever B | **AutoMerging Retriever** (HierarchicalNodeParser) |
| Retriever C | **Auto Retriever** (metadata-aware, knows year/filing type) |
| Post-Retrieval | **Cohere Reranker** |
| Query Engines | Standard + **SubQuestion Query Engine** (decomposes multi-part questions) |
| LLM | GPT-4o-mini (temperature=0) |
| Embeddings | OpenAI `text-embedding-3-small` |
| Evaluation | Faithfulness + Relevancy (LLM-as-judge) |

### Advanced Features

- **SubQuestionQueryEngine** — decomposes "Compare revenue in 2020 vs 2022" into sub-questions per filing
- **Metadata-aware retrieval** — auto retriever filters by year, filing type
- **Cohere reranker** — post-retrieval reranking for precision
- **Index persistence** — saves/loads index with `pickle` to avoid re-parsing
- **SEC EDGAR scraping** — `requests` + BeautifulSoup to fetch 10 filings automatically

---

## Tech Stack Summary

| Technology | Sub-1 | Sub-2 |
|-----------|-------|-------|
| LlamaIndex Core | ✓ | ✓ |
| LlamaParse | ✓ | ✓ |
| OpenAI embeddings + LLM | ✓ | ✓ |
| BGE (HuggingFace) embeddings | ✓ | — |
| Jina AI embeddings | ✓ | — |
| Mixtral via Together AI | ✓ | — |
| Cohere Reranker | — | ✓ |
| SubQuestion Query Engine | — | ✓ |
| SEC EDGAR scraping | — | ✓ |
| W&B / eval metrics | ✓ | ✓ |

## Work Completed

- [x] LlamaParse PDF extraction (markdown-quality parsing)
- [x] 3-embedding comparison with index building + timing
- [x] 2-LLM comparison (GPT-4o vs Mixtral)
- [x] 6-combination evaluation matrix (faithfulness, relevancy, latency)
- [x] 3 advanced retriever types (sentence window, auto-merging, auto retriever)
- [x] Cohere reranker post-processing
- [x] SubQuestion engine for multi-hop 10-year comparisons
- [x] SEC EDGAR automated filing download
- [x] Index persistence with pickle

## Complexity

**Very High** — Two enterprise-grade RAG systems. Sub-1: systematic comparison of 6 embedding/LLM combinations. Sub-2: multi-retriever architecture with reranking, metadata filtering, and multi-hop question decomposition over a 10-year financial corpus.
