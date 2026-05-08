---
title: Error handling
description: The Error Handling section in the WSO2 Integrator Flow Diagram editor for catching, raising, and panicking on errors.
keywords: [wso2 integrator, flow diagram editor, error handling, error handler, fail, panic]
---

# Error handling

The **Error Handling** section of the node panel covers nodes that catch errors in the flow, raise an error value to the caller, or abort the strand entirely. Every flow in WSO2 Integrator has an automatic **Error Handler** branch on the side of the canvas that catches any error raised in the main flow. Use the nodes here to refine that behavior or to raise errors yourself.

## ErrorHandler

Catches and handles errors that occur during the flow's execution. The default branch appears on the canvas without configuration; add steps inside it to log, transform, or compensate when an error is caught.

![ErrorHandler button in the Error Handling section](/img/develop/flow-design-elements/error-handler-node.png)

The configuration form requires no parameters and does not return a result. The form confirms this with a **Configuration Complete** message.

![ErrorHandler info panel showing Configuration Complete](/img/develop/flow-design-elements/error-handler-info.png)

To customize how errors are handled, add steps inside the **ErrorHandler** branch. For example, log the error with the nodes in [Logging](./logging.md), transform it into an HTTP error response, or trigger a compensating action.

## Fail

Raises a Ballerina error value that propagates up the call stack until an enclosing **ErrorHandler** catches it or the error is returned to the caller. Use **Fail** when the integration cannot proceed but you want callers (or an enclosing handler) to recover or report meaningfully.

![Fail button in the Error Handling section](/img/develop/flow-design-elements/fail-node.png)

| Field | Description |
|---|---|
| **Expression** | Fail value. Construct an error with `error("message")`, or pass an existing error variable. Use the [Expression editor](../expression-editor.md) for type-aware suggestions. |

![Fail form with Expression field](/img/develop/flow-design-elements/fail-form.png)

## Panic

Aborts the current strand immediately and propagates the panic up the call stack. Unlike a regular error from **Fail**, a panic is not normally caught. Reserve **Panic** for unrecoverable conditions such as violated invariants, missing required configuration, or programming bugs. For expected error conditions, use **Fail**.

![Panic button in the Error Handling section](/img/develop/flow-design-elements/panic-node.png)

| Field | Description |
|---|---|
| **Expression** | Panic value. |

![Panic form with Expression field](/img/develop/flow-design-elements/panic-form.png)

## What's next

- [Control](./control) — Branch and loop inside an **ErrorHandler**.
- [Logging](./logging) — Log details about a caught error.
- [Statement](./statement) — Variables and function calls.
- [Expression editor](../expression-editor) — Author error expressions with assistance.
