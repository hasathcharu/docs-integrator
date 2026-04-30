---
title: Data mapper
description: Transform data between record types visually using the WSO2 Integrator data mapper, with support for inline and AI-assisted mapping, array operations, generic JSON/XML payloads, and reusable submappings.
---

# Data mapper

The data mapper transforms data between different record types using a visual canvas. Map fields one-to-one, write inline expressions, iterate over arrays, aggregate values, and reuse common mappings as submappings — all without leaving the integration flow.

:::info Prerequisites
- WSO2 Integrator installed ([Install guide](../../../../get-started/install.md))
- An integration project with input and output types defined under **Types**
:::

## What you can do

| Topic | Description |
|---|---|
| [Access paths](./access-paths.md) | Open the data mapper as a reusable artifact, inline from a flow node, or with AI-assisted automatic mapping. |
| [Mapping capabilities](./mapping-capabilities.md) | One-to-one, many-to-one, expression editor, convert and map, custom functions, and transformation functions. |
| [Array mappings](./array-mappings.md) | Map between arrays with iteration, joins, nested iteration, single-element extraction, and aggregation. |
| [Generic type mappings](./generic-type-mappings.md) | Generate types from a sample JSON or XML payload and map between formats. |
| [Submappings](./submappings.md) | Define reusable mapping logic and apply it to multiple output fields. |

## When to use the data mapper

| Use case | Recommended approach |
|---|---|
| Transform a payload at the boundary of an integration | Reusable data mapper artifact |
| Shape a value within a flow (for example, on a **Declare Variable** node) | Inline data mapper |
| Bootstrap a complex mapping quickly | AI-assisted **Auto Map** |
| Reuse the same field-grouping logic in multiple outputs | Submapping |

## What's next

- [Access paths](./access-paths.md) — Choose how to open the data mapper for your scenario.
- [Query expressions](../../../design-logic/query-expressions.md) — Understand the Ballerina queries that power array mappings.
- [Expressions](../../../design-logic/expressions.md) — Write the inline expressions used in the expression editor.
