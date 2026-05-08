---
title: AI nodes
description: AI nodes in the WSO2 Integrator flow designer for direct LLM calls, RAG, and agents.
keywords: [wso2 integrator, flow designer, ai nodes, model provider, knowledge base, data loader, augment query, agent]
---

# AI nodes

The **AI** section of the node panel is split into **Direct LLM**, **RAG**, and **Agent**.

## Model Provider

Defines a connection to an LLM provider.

![Model Provider button under Direct LLM](/img/develop/flow-design-elements/model-provider-node.png)

The picker lists the supported provider implementations.

| Provider | Description |
|---|---|
| **Default Model Provider (WSO2)** | WSO2 model provider implementation that provides chat completion capabilities. |
| **Anthropic Model Provider** | Client class that provides an interface for interacting with Anthropic large language models. |
| **Azure OpenAI Model Provider** | Client class that provides an interface for interacting with Azure-hosted OpenAI models. |
| **Deepseek Model Provider** | Client class that provides an interface for interacting with Deepseek large language models. |
| **Google Vertex Model Provider** | Client class that provides an interface for interacting with models hosted on Google Vertex AI. |
| **Mistral Model Provider** | Client class that provides an interface for interacting with Mistral AI large language models. |
| **Ollama Model Provider** | Client for interacting with Ollama language models. |
| **OpenAI Model Provider** | Client class that provides an interface for interacting with OpenAI large language models. |

![Model providers list](/img/develop/flow-design-elements/model-providers-offered.png)

## Knowledge Base

Defines a knowledge base used to retrieve context for RAG.

![Knowledge Base button under RAG](/img/develop/flow-design-elements/knowledge-base-node.png)

| Knowledge base | Description |
|---|---|
| **Vector Knowledge Base** | Represents a vector knowledge base for managing chunk indexing and retrieval. |
| **Azure AI Search Knowledge Base** | Represents the Azure AI Search Knowledge Base implementation. |

![Knowledge bases list](/img/develop/flow-design-elements/knowledge-bases-offered.png)

## Data Loader

Loads documents into a knowledge base.

![Data Loader button under RAG](/img/develop/flow-design-elements/data-loader-node.png)

| Data loader | Description |
|---|---|
| **Text Data Loader** | Data loader that can be used to load supported file types as `TextDocument`. |

![Data loaders list](/img/develop/flow-design-elements/data-loaders-offered.png)

## Augment Query

Augments the user's query with relevant context.

![Augment Query button under RAG](/img/develop/flow-design-elements/augment-query-node.png)

| Field | Description |
|---|---|
| **Context** | Array of matched chunks or documents to include as context. |
| **Query** | The user's original question. |
| **Result** | Name of the result variable. |
| **Result Type** | Type of the variable. |

![Augment Query form](/img/develop/flow-design-elements/augment-query-form.png)

## Agent

Executes the agent for a given user query.

![Agent button under Agent](/img/develop/flow-design-elements/agent-node.png)

| Field | Description |
|---|---|
| **Role** | Define the agent's primary function. |
| **Instructions** | Detailed instructions for the agent. |
| **Query** | The natural language input provided to the agent. |
| **Advanced Configurations** | Expand for additional settings. |
| **Result** | Name of the result variable. |

![Agent form](/img/develop/flow-design-elements/agent-form.png)

## What's next

- [Control nodes](./control-nodes.md) — Branching and looping in flows.
- [Error handling nodes](./error-handling-nodes.md) — Catch model and tool errors.
- [Statement nodes](./statement-nodes.md) — Variables and function calls.
