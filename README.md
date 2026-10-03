# Grounded RAG & Routing Evaluation Benchmark

A reproducible, empirical benchmark designed to evaluate grounded retrieval, intent routing, and abstention quality across financial public corpora.

---

## 1. Corpus Selection & Manifest
* **Corpus Source:** Public Financial Dataset (SEC 10-K Filings / FinancialQA subset).
* **Format:** Raw JSON and Markdown documents with deterministic `sha256` hashing for full reproducibility.
* **Corpus Perturbation & Distractors:** 
  * Injection of synthetic/adversarial noise (15% distractor documents featuring high semantic similarity with conflicting facts).

---

## 2. Query Taxonomy & Test Benchmark
The benchmark dataset contains **150–200 curated queries** across three primary categories:
1. **Answerable Queries (60%):** Standard factoid and multi-step reasoning queries grounded directly in the corpus.
2. **Unanswerable Queries (20%):** Queries outside corpus boundaries or requesting missing data to evaluate abstention quality.
3. **Adversarially Similar Queries (20%):** Misleading or distractor queries designed to trigger false-positive retrievals or hallucinations.

---

## 3. Architecture & Baseline Comparison
The evaluation contrasts two distinct retrieval and generation strategies:
* **Baseline Approach:** Standard dense vector search (Naive Fixed-size Chunking + Dense Embeddings + Cosine Top-$k$).
* **Proposed Approach:** 
  * Semantic/Hierarchical Chunking with Metadata Enrichment.
  * Dynamic Query Intent Router (classifying queries into Direct Answer, Multi-Doc Synthesis, or Out-of-Scope).
  * Hybrid Retrieval (BM25 + Dense Embeddings with Reranking).

---

## 4. Evaluation Metrics
* **Retrieval Performance:** Precision@k, Recall@k, Mean Reciprocal Rank (MRR).
* **Generation & Faithfulness:** 
  * Answer Correctness (Semantic similarity / exact match evaluation).
  * Citation Faithfulness (Verifying that generated citations directly support claims).
* **Abstention Quality:** Precision and Recall on Refusal for Unanswerable and Adversarial queries.
* **Operational Metrics:** End-to-end Latency (ms), Total Prompt/Completion Token Usage, and API/Call Cost.

---

## 5. Experiment Log & Protocol Rules
* Evaluation protocol frozen prior to execution on held-out test sets.
* Detailed experiment log tracking hypotheses, configuration changes, failures, and residual failure modes (no claims of zero hallucination).
* Open-source execution code, SHA commits, corpus manifests, and raw model generations archived within this repository.
