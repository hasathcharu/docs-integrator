---
title: Error handling nodes
description: Error handling nodes in the WSO2 Integrator flow designer for catching, raising, and panicking on errors.
keywords: [wso2 integrator, flow designer, error handling, error handler, fail, panic]
---

# Error handling nodes

Error handling nodes catch and handle errors, fail the flow with an error value, or panic and stop execution.

## ErrorHandler

Catches and handles errors. The operation does not require any parameters and does not return a result.

![ErrorHandler button in the Error Handling section](/img/develop/flow-design-elements/error-handler-node.png)

![ErrorHandler info panel showing Configuration Complete](/img/develop/flow-design-elements/error-handler-info.png)

## Fail

Fails the execution with an error value.

![Fail button in the Error Handling section](/img/develop/flow-design-elements/fail-node.png)

| Field | Description |
|---|---|
| **Expression** | Fail value. |

![Fail form](/img/develop/flow-design-elements/fail-form.png)

## Panic

Panics and stops the execution.

![Panic button in the Error Handling section](/img/develop/flow-design-elements/panic-node.png)

| Field | Description |
|---|---|
| **Expression** | Panic value. |

![Panic form](/img/develop/flow-design-elements/panic-form.png)

## What's next

- [Control nodes](./control-nodes.md) — Branching and looping inside an Error Handler.
- [Logging nodes](./logging-nodes.md) — Log details about a caught error.
- [Statement nodes](./statement-nodes.md) — Variables and function calls.
