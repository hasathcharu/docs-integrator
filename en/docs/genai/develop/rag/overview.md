---
sidebar_position: 1
title: RAG
description: Overview of Retrieval-Augmented Generation support in WSO2 Integrator.
---

# RAG

RAG (Retrieval-Augmented Generation) is a technique that grounds LLM responses in your own documents. At query time, relevant passages are retrieved from a vector store and injected into the prompt, enabling the model to answer based on your content rather than its training data.

WSO2 Integrator provides the full RAG pipeline as built-in components on the BI canvas.

## In This Section

- **RAG Ingestion** — Load, chunk, embed, and store documents in a vector store.
- **RAG Query** — Retrieve relevant chunks, augment the user query, and generate a grounded response.
