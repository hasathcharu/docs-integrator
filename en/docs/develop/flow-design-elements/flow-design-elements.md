---
title: Flow design elements
description: Reference for every node available in the WSO2 Integrator flow designer, grouped by category.
keywords: [wso2 integrator, flow designer, nodes, statement, control, ai, error handling, concurrency, logging]
---

# Flow design elements

The flow designer in WSO2 Integrator IDE represents an integration as a graph of nodes connected from a **Start** node down to an end terminator, with an **Error Handler** branch on the side. Each node performs one well-defined operation: declaring data, calling out to a service, branching the flow, handling errors, and so on.

This section catalogs every node available in the right-side **node panel**, grouped by the same categories the panel uses.

## Categories

| Category | Nodes | Use it for |
|---|---|---|
| [Statement nodes](./statement-nodes.md) | Connection, Declare Variable, Update Variable, Call Function, Map Data | Working with data and invoking actions |
| [Control nodes](./control-nodes.md) | If, Match, While, Foreach, Return | Branching, looping, and returning from a flow |
| [AI nodes](./ai-nodes.md) | Model Provider, Knowledge Base, Data Loader, Augment Query, Agent | Building AI-powered integrations |
| [Error handling nodes](./error-handling-nodes.md) | ErrorHandler, Fail, Panic | Catching, raising, and aborting on errors |
| [Concurrency nodes](./concurrency-nodes.md) | Fork, Wait, Lock | Running work in parallel and synchronizing access |
| [Logging nodes](./logging-nodes.md) | Log Info, Log Error, Log Warn, Log Debug | Emitting log messages at different severities |

## How to add a node

1. In the flow designer, select the **+** between any two nodes to open the node panel on the right.
2. Expand the relevant category.
3. Select the node you want to add.
4. Fill in the node's configuration form and select **Save**.

## What's next

- [Statement nodes](./statement-nodes.md) — Variables, function calls, and data mapping.
- [Control nodes](./control-nodes.md) — Conditionals, loops, and returns.
- [AI nodes](./ai-nodes.md) — LLM, RAG, and agent integration.
