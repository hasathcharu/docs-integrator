---
title: Statement nodes
description: Statement nodes in the WSO2 Integrator flow designer for working with variables, functions, connections, and data mapping.
keywords: [wso2 integrator, flow designer, statement nodes, connection, declare variable, update variable, call function, map data]
---

# Statement nodes

Statement nodes declare and update variables, call functions, transform data, and invoke actions on connections.

## Connection

A connection is a reusable client to an external system. Connections appear in the **Connections** section at the top of the node panel.

![mysqlClient connection in the Connections section](/img/develop/flow-design-elements/connection-node.png)

Selecting a connection lists its available actions. For a database client, the actions are **Query**, **Query Row**, **Execute**, **Batch Execute**, **Call**, and **Close**.

![Actions available on the mysqlClient connection](/img/develop/flow-design-elements/connection-actions.png)

## Declare Variable

Creates a new variable in the current flow scope.

![Declare Variable button in the Statement section](/img/develop/flow-design-elements/declare-variable-node.png)

| Field | Description |
|---|---|
| **Name** | Name of the variable. |
| **Type** | Type of the variable. |
| **Expression** | Initialize with value. |

![Declare Variable form](/img/develop/flow-design-elements/declare-variable-form.png)

## Update Variable

Updates the value of an existing variable.

![Update Variable button in the Statement section](/img/develop/flow-design-elements/update-variable-node.png)

| Field | Description |
|---|---|
| **Variable** | Name of the variable or field. |
| **Expression** | Update value. |

![Update Variable form](/img/develop/flow-design-elements/update-variable-form.png)

## Call Function

Calls a function defined in the project, an imported library, or the standard library.

![Call Function button in the Statement section](/img/develop/flow-design-elements/call-function-node.png)

The function picker lists functions under **Within Project**, **Imported Functions**, and **Standard Library**. Use **Create Function** to define a new one inline.

![Function picker showing Within Project, Imported Functions, and Standard Library](/img/develop/flow-design-elements/call-function-options.png)

## Map Data

Adds a data mapper call to transform data between record types.

![Map Data button in the Statement section](/img/develop/flow-design-elements/map-data-node.png)

The picker lists existing data mappers under **Within Project** and provides **Create Data Mapper** to define a new one.

![Data Mappers picker](/img/develop/flow-design-elements/map-data-view.png)

## What's next

- [Control nodes](./control-nodes.md) — Branching, loops, and returns.
- [AI nodes](./ai-nodes.md) — Model providers, RAG, and agents.
- [Logging nodes](./logging-nodes.md) — Emit log messages at different severities.
