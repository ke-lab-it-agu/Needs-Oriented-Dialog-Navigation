# Project Title
Needs Oriented Navigation

## Overview

This repository contains the implementation used for evaluating **need-oriented dialog navigation** based on **Large Language Models (LLMs)** and **Retrieval-Augmented Generation (RAG)**.
The system infers human needs from user requests and predicts relevant objects and rooms in a household environment.

The code supports:
- **LLM-based inference**
- **RAG-based inference using embeddings + FAISS**
- Quantitative evaluation using Precision / Recall / F1
- Excel-based result logging


## Directory Structure

```text
.
├── need_oriented_environmental_knowledge_base.json
├── Evaluation_Needs_Rag.xlsx
├── Evaluation_Needs_LLM.xlsx
├── rag_experiment.py              # RAG-based evaluation
├── llm_experiment.py              # LLM-only evaluation
├── requirements.txt
└── README.md
```
## Requirements

Python
- Python 3.9 or later (recommended: 3.9–3.11)

- Python Libraries
  
Install all required libraries with:
```text
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

## OpenAI API Key

An OpenAI API key is required.
Set the API key directly in the code or via environment variable.

OPENAI_API_KEY = "your-api-key-here"


## Input Files

1. Knowledge Base
need_oriented_environmental_knowledge_base.json

Each entry must include:
```text
{
  "name": "Thirst",
  "action": "drink water",
  "explain": "the need to hydrate",
  "things": "water glass,mug,bottle"
}
```

2. Evaluation Excel Files
RAG-based evaluation
- Evaluation_Needs_Rag.xlsx

LLM-based evaluation
- Evaluation_Needs_LLM.xlsx

Each worksheet must contain:
- Column A: User request
- Column B: Ground-truth needs
- Column C: Ground-truth objects

The scripts automatically write:
- Predicted needs
- Predicted objects
- Room estimation
- Precision / Recall / F1
- Token counts

3. Run the Script

## RAG-based Evaluation

This script:
- Builds embeddings using text-embedding-3-large
- Indexes needs with FAISS
- Performs similarity-based retrieval
- Uses LLM for final object selection
```text
python rag_experiment.py
```
## LLM-based Evaluation

This script:
- Uses only LLM prompts
- Tests multiple prompt styles:
- Murray’s needs only
    - Needs + action keywords
    - Needs + explanations

python llm_experiment.py


## Experiment Settings
    1. Dataset: 93 evaluation samples extracted from OpenEQA.
    2. Large Language Models Used
        1.GPT-4o-mini
        2.GPT-4o
    3. Embedding Model Used
        1.text-embedding-3-large
    4. Prompt Variants (Need Information Embedded)
        1. "Murray’s Theory Description"
        2. "Needs and Action Names"
        3. "Needs and Explanations"

## Metrics
The following metrics are computed automatically:
- Precision
- Recall
- F1 score
Room prediction is derived deterministically from predicted objects and is not included in metric computation.

## Notes
- FAISS is used in CPU mode (faiss-cpu)
- Embeddings are computed at runtime (can be cached for efficiency)
- Token counts are computed using tiktoken (o200k_base)

