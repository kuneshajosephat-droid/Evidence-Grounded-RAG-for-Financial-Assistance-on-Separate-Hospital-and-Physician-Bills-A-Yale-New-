
Evidence-Grounded RAG for Financial Assistance on Separate Hospital and Physician Bills
Overview
This project explores how Retrieval-Augmented Generation (RAG) can support questions about financial assistance when hospital and physician bills are handled separately.

Using public Yale New Haven Health (YNHH) financial-assistance documents, I built a source-grounded RAG system that retrieves evidence for questions about eligibility, applications, covered services, hospital discounts, and provider coverage.

A central part of the project is uncertainty handling. For physician coverage, the system distinguishes between:

Listed as covered
Explicitly listed as not covered
Cannot verify
Rather than treating a missing provider as automatically covered or not covered, the system returns Cannot verify when the available provider lists do not establish a reliable match.

Research Question
Given the same retrieved evidence, how differently do small open-source language models use that evidence when answering financial-assistance questions and classifying provider coverage?

What I Built
The system combines several NLP and retrieval components:

Sentence-transformer embeddings
FAISS vector indexing
BM25 lexical retrieval
Hybrid retrieval for general policy questions
Intent and entity parsing
Structured evidence lookup for eligibility and hospital discounts
Exact provider-list lookup
Evidence-grounded prompting
Explicit uncertainty handling
The knowledge base uses five public YNHH source types:

Financial Assistance Policy
Plain Language Summary and Application
Eligibility Guidelines
Participating Provider List
Non-Participating Provider List
RAG Pipeline
User question
      ↓
Intent + entity parsing
      ↓
Evidence routing
      ↓
Hybrid retrieval / structured lookup / provider lookup
      ↓
Source-grounded evidence
      ↓
Local language model
      ↓
Grounded response

For provider questions, the system uses exact provider-list evidence rather than relying only on semantic similarity.

Model Comparison

I compared three small open models running locally with Ollama:

Llama 3.2 3B
Qwen2.5 3B
Phi-3 Mini 3.8B

The formal experiment used 12 domain-specific questions.

Each model received the same question, prompt, and retrieved evidence, producing 36 responses.

Responses were evaluated separately for:

Correctness
Relevance
Evidence support
Results
Model	Correctness	Relevance	Evidence Support	Combined
Llama 3.2 3B	1.833	1.833	1.750	65 / 72
Phi-3 Mini 3.8B	1.750	2.000	2.000	69 / 72
Qwen2.5 3B	1.167	1.250	1.250	44 / 72

The comparison revealed different strengths across the models.

Llama 3.2 3B achieved the highest mean correctness.

Phi-3 Mini 3.8B achieved the highest overall score and the strongest relevance and evidence-support scores.

Qwen2.5 3B was less consistent in using the supplied evidence across the full question set.

All three models correctly distinguished the three formal provider outcomes:
Listed as covered
Explicitly listed as not covered
Cannot verify

Key Finding

One of the most interesting results was that retrieving the right evidence did not guarantee that every model would use it correctly.

The models received identical evidence but sometimes:

interpreted it differently,
omitted part of a multi-step answer,
returned Cannot verify despite sufficient evidence, or
produced a citation that was not supported by the supplied context.

This highlights an important RAG design issue: retrieval quality and generation quality should be evaluated separately.

Why This Project Matters

Financial-assistance information can be spread across policy documents, eligibility tables, application instructions, and provider lists.

This project demonstrates a way to combine those sources into a single evidence-grounded NLP pipeline while preserving an explicit Cannot verify option when the available evidence is insufficient.

The same design idea can extend to other domains where an AI system should retrieve authoritative evidence before answering rather than relying only on model memory.

Tech Stack
Python
Sentence Transformers
FAISS
BM25
Ollama
Llama 3.2 3B
Qwen2.5 3B
Phi-3 Mini
pandas
Next Step

The current work establishes the RAG architecture and controlled three-model comparison.

The next phase is focused on improving robustness to more varied user language and developing a simple interface for broader testing.

Repository

The repository includes:

source preprocessing and retrieval code
evidence construction and routing
provider lookup
evaluation questions
three-model comparison
scoring and retrieval evaluation
demo scripts
experiment results
project presentation
Disclaimer

This is an independent research and educational project using publicly available information.

It is not affiliated with or endorsed by Yale New Haven Health and should not replace official financial-assistance guidance or direct confirmation from the relevant provider or organisation.
