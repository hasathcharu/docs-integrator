---
title: Access paths
description: Open the data mapper as a reusable artifact, inline from a flow node, or with AI-assisted automatic mapping.
---

# Access paths

You can open the data mapper in three ways: as a reusable artifact, inline from a flow node, or through AI-assisted automatic mapping.

## Reusable data mapper

Create a reusable data mapper from the **Artifacts** page or the left sidebar. Configure the inputs and the output type, then open the data mapper view.

1. Open the **Artifacts** page and select **Data Mapper** under **Other Artifacts**, or click **+** next to **Data Mappers** in the left sidebar.

   ![Artifacts page with the Data Mapper option highlighted under Other Artifacts](/img/develop/integration-artifacts/supporting/data-mapper/reusable-datamapper-selection.png)

2. Fill in the **Create New Data Mapper** form.

   ![Create New Data Mapper form with Name, Public, Inputs, and Output fields](/img/develop/integration-artifacts/supporting/data-mapper/reusable-datamapper-form.png)

   | Field | Description |
   |---|---|
   | **Data Mapper Name** | A unique name for the mapping function (for example, `transform`). |
   | **Public** | Select **Make visible across the project** to use this mapper from other integrations. |
   | **Inputs** | Click **+ Add Input** to define one or more source variables. Each input has a name and a type. |
   | **Output** | The target type that the mapper produces. |

3. Click **Create**. The data mapper canvas opens with input fields on the left and output fields on the right.

   ![Data mapper canvas with input record on the left and output record on the right](/img/develop/integration-artifacts/supporting/data-mapper/datamapper-view.png)

## Inline data mapper

When a flow contains a **Declare Variable** or **Update Variable** node and the selected type is a record or record array, the side panel shows an **Open in Data Mapper** option. Click it to map the value inline without creating a separate artifact.

![Declare Variable side panel showing the Open in Data Mapper button next to a Lecturer-typed variable](/img/develop/integration-artifacts/supporting/data-mapper/inline-datamapper-path.png)

## AI-assisted mapping

Inside any data mapper view, click **Auto Map** in the top-right corner to generate mappings automatically using the WSO2 Integrator Copilot.

![Data mapper toolbar with the Auto Map button in the top-right corner](/img/develop/integration-artifacts/supporting/data-mapper/ai-map-option.png)

The Copilot panel opens with a `/datamap` command preloaded. Submit it to generate field mappings based on the input and output types.

![Copilot panel with the /datamap command ready to generate mappings](/img/develop/integration-artifacts/supporting/data-mapper/ai-map-view.png)

## What's next

- [Mapping capabilities](./mapping-capabilities.md) — Map fields one-to-one, combine inputs, and write expressions.
- [Array mappings](./array-mappings.md) — Map between arrays using iteration, joins, and aggregation.
