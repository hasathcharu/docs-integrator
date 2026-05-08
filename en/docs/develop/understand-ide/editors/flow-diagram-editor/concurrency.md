---
title: Concurrency
description: The Concurrency section in the WSO2 Integrator Flow Diagram editor for running work in parallel and serializing access to shared state.
keywords: [wso2 integrator, flow diagram editor, concurrency, fork, wait, lock]
---

# Concurrency

The **Concurrency** section of the node panel starts parallel work, waits for results, and protects mutable state. Use it when an integration needs to call multiple downstream services at the same time, gather their results, or guard shared state from concurrent updates.

## Fork

Spawns one or more named worker strands that execute in parallel with the main flow. Each worker has its own block of steps and runs independently until joined with **Wait**.

![Fork button in the Concurrency section](/img/develop/flow-design-elements/fork-node.png)

Each worker is configured with a name and a return type. Select **Add Worker** to add more workers.

| Field | Description |
|---|---|
| **Worker** | Name of the worker. |
| **Return Type** | Type of the value the worker returns. Define new types inline with the [Type editor](../type-editor.md). |

![Fork form with Worker 1 and Worker 2 entries and Add Worker action](/img/develop/flow-design-elements/fork-form.png)

Use **Fork** to fan out independent calls. For example, hit two different APIs in parallel and combine their results downstream.

## Wait

Waits for one or more worker strands started by **Fork** to complete and collects their return values. The matching **Wait** node is the join point for the workers spawned by an earlier **Fork**.

![Wait button in the Concurrency section](/img/develop/flow-design-elements/wait-node.png)

| Field | Description |
|---|---|
| **Future** | Wait expression that names the workers to join. Use the [Expression editor](../expression-editor.md) to write the wait expression. |
| **Variable Name** | Name of the variable that holds the joined result. |

![Wait form with Future and Variable Name fields](/img/develop/flow-design-elements/wait-form.png)

A wait expression with `{w1, w2}` collects all named workers and returns their values as a record. With alternation (`w1 | w2`), the wait returns as soon as any one worker completes.

## Lock

Acquires a lock to serialize access to a block of steps that touches shared mutable state. The lock is released when the block exits. Wrap any code path that mutates a shared variable from multiple strands in a **Lock** to prevent race conditions.

![Lock button in the Concurrency section](/img/develop/flow-design-elements/lock-node.png)

The configuration form requires no parameters and does not return a result. Add the steps that need protection inside the **Lock** block on the canvas.

![Lock info panel showing Configuration Complete](/img/develop/flow-design-elements/lock-form.png)

For most integration flows, prefer immutable values to avoid the need for locks entirely.

## What's next

- [Control](./control) — Branch and loop inside parallel workers.
- [Error handling](./error-handling) — Handle errors raised inside workers.
- [Logging](./logging) — Trace concurrent execution.
- [Statement](./statement) — Declare and update variables shared across workers.
