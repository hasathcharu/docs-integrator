---
title: Control nodes
description: Control flow nodes in the WSO2 Integrator flow designer for branching, looping, and returning from a flow.
keywords: [wso2 integrator, flow designer, control nodes, if, match, while, foreach, return]
---

# Control nodes

Control nodes branch on conditions, repeat steps, or return a value from the flow.

## If

Branches the flow on a Boolean condition.

![If button in the Control section](/img/develop/flow-design-elements/if-node.png)

| Field | Description |
|---|---|
| **Condition** | Boolean condition. |

The form provides **Add Else IF Block** and **Add Else Block** to add more branches.

![If form](/img/develop/flow-design-elements/if-form.png)

## Match

Matches a value against one or more patterns.

![Match button in the Control section](/img/develop/flow-design-elements/match-node.png)

| Field | Description |
|---|---|
| **Target** | Match target expression. |
| **Pattern 1** | Binding pattern. |

The form provides **Add Case Block** and **Add Default Case Block** to add more cases.

![Match form](/img/develop/flow-design-elements/match-form.png)

## While

Loops over a block of code while a condition is true.

![While button in the Control section](/img/develop/flow-design-elements/while-node.png)

| Field | Description |
|---|---|
| **Condition** | Boolean condition. |

![While form](/img/develop/flow-design-elements/while-form.png)

## Foreach

Iterates over a block of code for each item in a collection.

![Foreach button in the Control section](/img/develop/flow-design-elements/for-each-node.png)

| Field | Description |
|---|---|
| **Collection** | Collection to iterate. |
| **Variable Name** | Name of the variable. |
| **Variable Type** | Type of the variable. |

![Foreach form](/img/develop/flow-design-elements/for-each-form.png)

## Return

Returns a value from the current function or service flow. The operation has no required parameters; optional settings can be configured below.

![Return button in the Control section](/img/develop/flow-design-elements/return-node.png)

| Field | Description |
|---|---|
| **Expression** | Return value. |

![Return form](/img/develop/flow-design-elements/return-form.png)

## What's next

- [Statement nodes](./statement-nodes.md) — Variables, function calls, and data mapping.
- [Error handling nodes](./error-handling-nodes.md) — Catch errors and raise failures.
- [Concurrency nodes](./concurrency-nodes.md) — Fork, wait, and lock for parallel work.
