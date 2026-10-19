# Capstone A: RAG on arXiv abstracts, with evaluation and failure analysis

**Goal:** Answer questions about database research using retrieval over arXiv paper titles and abstracts, and measure honestly where it works and where it breaks.

**Why this domain:** I work in data engineering, so I can write good test questions and judge the answers myself.

## Data
- Source: arXiv metadata (Kaggle: Cornell-University/arxiv). Check the license on the Kaggle page; the Hugging Face mirror card describes the metadata as CC0.
- Scope: about 5,000 papers with primary category cs.DB.
- Not committed to this repo. A download step is in the README.

## Pipeline
- Bronze: raw metadata as loaded.
- Silver: deduplicated by arXiv id, null abstracts handled, checked with five pipeline checks (row count, duplicates, null rate, freshness, schema).
- Index: embeddings over title, abstract, or both (compared), stored in a Vector Search index (or FAISS/Chroma if unavailable).
- Tracking: every configuration logged in MLflow.
- Governance: tables in a Unity Catalog schema, with grants and lineage documented.

## Test set (written by me)
- 30 answerable questions, each with the correct paper id.
- 10 unanswerable questions, to measure hallucination.

## Metrics
- Retrieval: hit@1, hit@3, hit@5, MRR.
- Generation: hallucination rate on the 10 unanswerable questions, before and after an "say you don't know" instruction.
- Cost and latency per question.

## Starting targets (adjust after the baseline, and report what I actually reach)
- hit@3 of at least 60% on the 30 answerable questions.
- Hallucination rate below 20% on the unanswerable questions after mitigation.

## Deliverables
- Repo with code, MLflow experiment summary, and results tables.
- Written failure analysis of the 10 worst answers (cause: parsing, representation, retrieval, or model).
- Architecture diagram.
- README with limitations and the dataset source and license.

## Results (fill in with real numbers only)
| Configuration | hit@1 | hit@3 | hit@5 | MRR |
|---|---|---|---|---|
| Title only | [X] | [X] | [X] | [X] |
| Abstract only | [X] | [X] | [X] | [X] |
| Title + abstract | [X] | [X] | [X] | [X] |

| Condition | Hallucination rate (10 unanswerable) |
|---|---|
| No instruction | [X]% |
| With "say you don't know" | [X]% |

## Non-goals
- No claim of state-of-the-art accuracy.
- No employer data, and no data committed to the repo.
