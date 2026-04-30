---
title: Generic type mappings
description: Map between generic JSON or XML payloads by pasting a sample structure and letting the data mapper generate compatible record types.
---

# Generic type mappings

When the input or output is a generic JSON or XML payload, paste a sample structure into the canvas. The data mapper generates compatible record types from the sample, and you map the fields visually.

## Generate types from a sample

1. Create a data mapper with the relevant generic types as input and output.
2. In the data mapper view, paste a sample JSON or XML structure for the field you want to type.
3. The data mapper builds compatible record types from the sample and exposes the fields on the canvas.
4. Map the input fields to the output fields as you would for any record type.

![Data mapper generating record types from a pasted JSON sample and producing XML output](/img/develop/integration-artifacts/supporting/data-mapper/json-to-xml-mapping.gif)

## What's next

- [Submappings](./submappings.md) — Reuse mapping logic across multiple output fields.
- [Mapping capabilities](./mapping-capabilities.md) — Combine fields, write expressions, and convert primitive types.
