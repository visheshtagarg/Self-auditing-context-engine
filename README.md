# Self-Auditing Context Engine

A production-ready framework for transparent, explainable, and auditable Retrieval-Augmented Generation (RAG).

## Overview

The Self-Auditing Context Engine addresses a key limitation of conventional RAG systems: retrieval decisions are often opaque.

Instead of silently selecting context, the system scores, ranks, logs, and explains both **selected and rejected document chunks** before generating an answer.

The overall pipeline is:

**Retrieve → Score → Extract → Audit → Answer**

## Key Features

- Semantic retrieval using Sentence Transformers (`all-MiniLM-L6-v2`)
- 384-dimensional embeddings
- Cosine similarity-based retrieval
- Multi-factor relevance scoring
- Source trustworthiness scoring
- Recency-based scoring
- Verification-status scoring
- Configurable Top-K and relevance thresholds
- Complete audit trail for selected and rejected chunks
- Structured entity extraction
- Verified vs. Derived fact classification
- LLM-grounded answer generation
- Plain-English explanations for retrieval decisions
- CSV, TXT, and JSON audit exports
- Streamlit-based interactive interface
- Deterministic retrieval and scoring for a fixed knowledge-base state

## Architecture

The system consists of six stages:

### 1. Query Input

Users submit a natural-language query through the Streamlit interface.

### 2. Semantic Embedding & Retrieval

Queries are encoded using `all-MiniLM-L6-v2` and compared against pre-computed document embeddings using cosine similarity.

### 3. Multi-Factor Relevance Scoring

Each candidate document chunk is evaluated using four dimensions:

- Semantic Similarity
- Source Trustworthiness
- Recency
- Verification Status

The scoring weights and relevance threshold are configurable.

### 4. Decision Logging

Every selected **and rejected** chunk is recorded with:

- Chunk ID
- Source/document
- Composite score
- Score breakdown
- Ranking position
- Selection/rejection reason

### 5. Entity & Fact Extraction

The system extracts structured information from retrieved context and classifies facts as:

- **Verified** — explicitly supported by the source
- **Derived** — inferred or extrapolated from the surrounding context

### 6. LLM Answer Generation

Validated facts, entities, and retrieval explanations are passed to an LLM to generate an answer grounded in the retrieved context.

Supported backends include:

- Google Gemini
- Ollama Mistral

## Retrieval & Scoring

Unlike standard RAG pipelines that primarily rely on embedding similarity, this system combines multiple signals to determine which sources should be used.

### Scoring Dimensions

| Dimension | Purpose |
|---|---|
| Semantic Similarity | Measures alignment between query and document chunk |
| Trustworthiness | Evaluates source reliability |
| Recency | Prioritizes timely information |
| Verification | Gives preference to verified/audited sources |

The system exposes the scoring process through its audit trail rather than hiding retrieval decisions.

## Audit Trail

A core differentiator of the system is that it records **both accepted and rejected retrieval candidates**.

### Selected Sources

The audit record contains:

- Source and chunk information
- Confidence score
- Similarity score
- Trust score
- Recency score
- Verification score
- Ranking position
- Human-readable selection reasons

### Rejected Sources

Rejected chunks include:

- Source/chunk information
- Composite score
- Ranking
- Specific rejection reason
- Threshold information

This makes retrieval decisions traceable and reviewable.

## Explainability

The Explainability Engine converts machine-generated scores into human-readable reasoning.

For each query, the system can provide:

- Why a source was selected
- Why another source was rejected
- Weighted score breakdown
- Confidence rating
- Rejection reasons

## Entity & Fact Extraction

The system performs structured extraction across six categories:

- Financial Metrics
- Percentages
- Time Periods
- Dates
- Company Names
- Business Metrics

Extracted facts are cross-validated against source context and tagged as **Verified** or **Derived**.

## Streamlit Interface

The project includes a browser-based Streamlit interface with:

- Natural-language query input
- Query history
- Top-K control
- Relevance threshold control
- Real-time pipeline progress
- Facts view
- Entities view
- Audit log
- Source ranking
- Explainability view
- JSON/TXT/CSV export

## Core Modules

```text
chunk_retriever.py
decision_logger.py
explainability_engine.py
entity_extractor.py
llm_answer_generator.py
chunk_embedding_generator.py
document_chunker.py
streamlit_main_app.py
