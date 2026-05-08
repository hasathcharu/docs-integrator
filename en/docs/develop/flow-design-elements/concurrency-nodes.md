---
title: Concurrency nodes
description: Concurrency nodes in the WSO2 Integrator flow designer for running work in parallel and serializing access to shared state.
keywords: [wso2 integrator, flow designer, concurrency, fork, wait, lock]
---

# Concurrency nodes

Concurrency nodes start parallel work, wait for futures to complete, and protect mutable state.

## Fork

Starts named workers that run in parallel.

![Fork button in the Concurrency section](/img/develop/flow-design-elements/fork-node.png)

Each worker is configured with a name and return type. Use **Add Worker** to add more workers.

| Field | Description |
|---|---|
| **Worker** | Name of the worker. |
| **Return Type** | Type of the return value. |

![Fork form with Worker 1 and Worker 2](/img/develop/flow-design-elements/fork-form.png)

## Wait

Waits for a set of futures to complete.

![Wait button in the Concurrency section](/img/develop/flow-design-elements/wait-node.png)

| Field | Description |
|---|---|
| **Future** | Wait for a set of futures to complete. |
| **Variable Name** | Name of the variable. |

![Wait form](/img/develop/flow-design-elements/wait-form.png)

## Lock

Allows access to mutable states safely. The operation does not require any parameters and does not return a result.

![Lock button in the Concurrency section](/img/develop/flow-design-elements/lock-node.png)

![Lock info panel showing Configuration Complete](/img/develop/flow-design-elements/lock-form.png)

## What's next

- [Control nodes](./control-nodes.md) — Branching and looping inside parallel workers.
- [Error handling nodes](./error-handling-nodes.md) — Handle errors raised inside workers.
- [Logging nodes](./logging-nodes.md) — Trace concurrent execution.
