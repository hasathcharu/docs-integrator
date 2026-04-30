---
title: Array mappings
description: Transform arrays in the data mapper using iteration, direct assignment, nested iteration, conditional joins, single-element extraction, and aggregation.
---

# Array mappings

When the source or target field is an array, the data mapper offers dedicated options for the array shape you need. The available choices depend on whether you map array-to-array or array-to-singleton.

## Array to array

### Map each element

To transform each item in an input array into an item in an output array, use **Map Each Element**. The data mapper opens a focused view scoped to the array element types and generates a Ballerina query expression.

![Map Each Element opening a focused view for an Engineer array mapping](/img/develop/integration-artifacts/supporting/data-mapper/map-each-element.gif)

Within the focused view, refine the query using the available clauses:

| Clause | Purpose |
|---|---|
| **where** | Filter elements by a condition |
| **let** | Define local variables for use in the projection |
| **order by** | Sort the result |
| **limit** | Cap the number of output elements |
| **from** | Add another iteration source |
| **join** | Combine elements from a second array |
| **group by** | Group elements before projection |

### Assign as is

When the input and output array types are identical, use **Assign as is** to assign the array directly without iteration.

![Assign as is option mapping a Person array directly to an EngineerMapping array](/img/develop/integration-artifacts/supporting/data-mapper/assign-as-is.gif)

### Nested iterate

To iterate a second array on each iteration of an outer array, first map the outer array using **Map Each Element**. From the focused view, select the second array and map it to the target — the data mapper offers the **Nested Iterate** option to wrap it in an inner query.

![Nested Iterate prompt when mapping a second array inside the focused view](/img/develop/integration-artifacts/supporting/data-mapper/nested-iterate.gif)

### Join with condition

To join two arrays on a condition, map the first array with **Map Each Element** to enter the focused view. Then map the second array onto the target header — the data mapper offers **Join with Condition**. Define the join condition in the side panel.

![Join with Condition prompt and side panel for defining the join expression](/img/develop/integration-artifacts/supporting/data-mapper/join-with-condition.gif)

## Array to singleton

### Extract single element

To pull a single value out of an array, use **Extract Single Element**. This option appears whenever you map an array onto a singleton output (a dimension mismatch).

![Extract Single Element option appearing on a singleton output mapped from an array](/img/develop/integration-artifacts/supporting/data-mapper/extract-single-element.gif)

### Aggregate and map

To compute a single value from each element of an array (for example, sum, average, count), use **Aggregate and Map**. Aggregation supports primitive-type arrays mapped to a compatible primitive output. The data mapper opens a focused view and prompts for the aggregation operation.

![Aggregate and Map focused view with the available aggregation operations](/img/develop/integration-artifacts/supporting/data-mapper/aggregate-and-map.gif)

## What's next

- [Generic type mappings](./generic-type-mappings.md) — Map between JSON and XML payloads.
- [Submappings](./submappings.md) — Reuse mapping logic across multiple output fields.
- [Query expressions](../../../design-logic/query-expressions.md) — Understand the Ballerina queries the data mapper generates.
