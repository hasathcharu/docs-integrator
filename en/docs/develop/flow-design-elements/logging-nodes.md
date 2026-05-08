---
title: Logging nodes
description: Logging nodes in the WSO2 Integrator flow designer for emitting log messages at info, error, warn, and debug severities.
keywords: [wso2 integrator, flow designer, logging, log info, log error, log warn, log debug]
---

# Logging nodes

Logging nodes emit a message to the integration's log output at a specific severity level. Each form takes a single required **Msg** field; additional options are available under **Advanced Configurations**.

## Log Info

Prints info logs.

![Log Info button in the Logging section](/img/develop/flow-design-elements/log-info-node.png)

| Field | Description |
|---|---|
| **Msg** | The message to be logged. |

![Log Info form](/img/develop/flow-design-elements/log-info-form.png)

## Log Error

Prints error logs.

![Log Error button in the Logging section](/img/develop/flow-design-elements/log-error-node.png)

| Field | Description |
|---|---|
| **Msg** | The message to be logged. |

![Log Error form](/img/develop/flow-design-elements/log-error-form.png)

## Log Warn

Prints warn logs.

![Log Warn button in the Logging section](/img/develop/flow-design-elements/log-warn-node.png)

| Field | Description |
|---|---|
| **Msg** | The message to be logged. |

![Log Warn form](/img/develop/flow-design-elements/log-warn-form.png)

## Log Debug

Prints debug logs.

![Log Debug button in the Logging section](/img/develop/flow-design-elements/log-debug-node.png)

| Field | Description |
|---|---|
| **Msg** | The message to be logged. |

![Log Debug form](/img/develop/flow-design-elements/log-debug-form.png)

## Show More Functions

If a function or node you need is not in the panel, select **Show More Functions** at the bottom of the node panel.

![Show More Functions link at the bottom of the node panel](/img/develop/flow-design-elements/show-more-functions.png)

This opens the full functions picker, which lists every function under **Within Project**, **Imported Functions**, and **Standard Library**.

![Functions picker view](/img/develop/flow-design-elements/show-more-functions-view.png)

## What's next

- [Statement nodes](./statement-nodes.md) — Function calls and variable updates.
- [Error handling nodes](./error-handling-nodes.md) — Pair logging with error handlers.
- [Control nodes](./control-nodes.md) — Conditional logging inside branches.
