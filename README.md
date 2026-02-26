VeriRAG: Trust-Calibrated Retrieval Evaluation Framework
Overview

VeriRAG is a quantitative reliability evaluation framework designed to assess the trustworthiness of Retrieval-Augmented Generation (RAG) outputs.

Rather than focusing solely on answer generation, VeriRAG introduces a multi-layer trust scoring architecture that evaluates:

Retrieval quality

Evidence grounding strength

Logical consistency

Hallucination risk

Confidence calibration

The system computes a Composite Trust Score (CTS) to differentiate grounded responses from hallucinated outputs under controlled evaluation settings.

Motivation

Standard RAG pipelines assume that relevant retrieval implies reliable generation.
However, retrieval strength does not guarantee semantic grounding or logical consistency.

VeriRAG addresses this gap by introducing structured reliability diagnostics across multiple layers of the pipeline.

System Architecture

VeriRAG consists of four evaluation layers:

1. Retrieval Diagnostics

FAISS cosine similarity indexing

Retrieval Coverage Score (RCS)

Stability metrics (Std Dev + Top1–TopK gap)

Retrieval risk classification

2. Evidence Alignment

Evidence Alignment Score (EAS)

Semantic similarity between answer and aggregated retrieved context

3. Hallucination Modeling

Semantic gap analysis (RCS − EAS)

Numeric claim verification

NLI-based contradiction detection

Hallucination Probability Score (HPS v1–v3)

4. Composite Trust Calibration

Weighted aggregation:

CTS = w₁(RCS) + w₂(EAS) + w₃(1 − HPS) + w₄(Stability)

This produces an interpretable trust score between 0 and 1.

Dataset & Infrastructure

MS MARCO Passage Collection (200K subset)

384-dimensional transformer embeddings

FAISS vector indexing

Transformer-based NLI model for contradiction detection

Tech stack:

Python

SentenceTransformers

FAISS

NumPy / Pandas

Scikit-learn

HuggingFace Transformers

Experimental Evaluation

Controlled evaluation across grounded, hard hallucinated, and subtle hallucinated responses.

Results Summary
Evaluation Type	Avg CTS
Grounded	0.7788
Hard Hallucination	0.3446
Subtle Hallucination	0.6714
NLI-Enhanced Subtle	0.5789
Statistical Validation

Separation Margin: 0.4341

ROC-AUC: 1.0

Classification Accuracy: 1.0

Threshold Sensitivity Sweep performed

Calibration via quantile binning

No overlap observed between grounded minimum and hallucinated maximum CTS.

Key Contributions

Designed a multi-metric trust scoring framework for RAG evaluation.

Integrated semantic alignment, numeric verification, and contradiction detection.

Demonstrated statistically significant separation between grounded and hallucinated outputs.

Provided calibration analysis for trust score reliability.

Built a scalable evaluation pipeline over large semantic corpora.

Limitations

Synthetic hallucinations used for controlled testing.

Threshold selection manually defined.

No real-time LLM integration in evaluation loop.

External benchmark comparison not included.

Repository Structure
notebooks/     → End-to-end evaluation notebook  
src/           → Modular scoring & retrieval components  
docs/          → Project documentation  
results/       → Evaluation summaries  
Future Directions

Real LLM output benchmarking

Automatic threshold optimization

Brier score and advanced calibration metrics

Cross-dataset generalization testing

Production-grade API deployment
