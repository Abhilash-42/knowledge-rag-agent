# Knowledge RAG Agent

A Retrieval-Augmented Generation (RAG) project that answers questions using a small collection of structured knowledge notes.

## Project Overview

This project explores how an AI agent can retrieve relevant information, generate answers grounded in retrieved notes, identify unverified knowledge, and abstain when the available evidence is insufficient.

## Architecture

1. **Knowledge base:** Five Markdown notes organized using the Open Knowledge Format (OKF) approach.
2. **Loading and splitting:** Ten searchable sections extracted from the notes.
3. **V1 — Keyword search:** Retrieves sections using word overlap.
4. **V2 — Semantic search:** Uses `all-MiniLM-L6-v2` embeddings and cosine similarity.
5. **Trust checks:** Flags draft, unchecked, and expired notes.
6. **Answer generation:** Uses `Qwen2.5-0.5B-Instruct` to generate answers from retrieved excerpts.
7. **Abstention:** A similarity threshold can prevent answers when retrieved knowledge appears insufficient.
8. **Evaluation:** Tests retrieval quality and selected unsupported questions.

## Technology Stack

- Python
- Google Colab
- Sentence Transformers
- Hugging Face Transformers
- PyTorch
- Markdown and YAML frontmatter

## Initial Retrieval Results

The initial evaluation used ten questions and measured Hit@3: whether the expected note appeared in the top three retrieved results.

| Method | Initial Hit@3 |
|---|---:|
| V1 — Keyword search | 90% (9/10) |
| V2 — Semantic search | 100% (10/10) |

These are results from a small initial test set, not a guarantee of performance on unseen questions.

## Trust and Abstention Tests

- Draft-note detection: passed.
- Missing verification metadata detection: passed.
- Unsupported question about the capital of France: the agent abstained after the relevance threshold was applied.
- The expired-note warning has been implemented but still needs a dedicated test.

## Documented Failure

Before adding a relevance threshold, the agent answered "Paris" when asked about the capital of France, even though the knowledge base contained only AI/ML notes. It also cited unrelated notes.

This showed that prompting alone did not ensure grounded answers. We added a retrieval relevance threshold, and the same question then produced an abstention with no sources.

## Limitations and Future Improvements

- Evaluate with a larger and more diverse question set.
- Test answer faithfulness against every cited passage.
- Tune the semantic similarity threshold using validation examples.
- Improve retrieval ranking and remove duplicate sources.
- Test expired-note warnings and prompt-injection resistance.
- Compare keyword, semantic, and hybrid retrieval.

## Run the Project

1. Open `knowledge_rag_agent.ipynb` in Google Colab.
2. Run the cells in order.
3. Ensure the `knowledge/` directory is available.
4. Install the notebook's dependencies.
5. Run retrieval, trust-check, answer-generation, and evaluation tests.

## Project Status

Prototype and experimentation stage. Results and limitations are documented honestly; further testing is required before treating the agent as reliably grounded.
