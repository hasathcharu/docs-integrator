---
sidebar_position: 1
title: Evaluations
description: Reference for AI agent evaluations in WSO2 Integrator. Create evalsets from session traces, design evaluations in the visual designer, and review run reports.
keywords: [wso2 integrator, ai agent, evaluations, evalset, golden dataset, llm-as-judge]
---

# Evaluations

**Evaluations** are how you keep an AI agent's quality from regressing. Unlike unit tests with deterministic outputs, agent behaviour has to be judged across several dimensions: correctness, tool selection, groundedness, safety, and tone. Evaluations in WSO2 Integrator give you a structured way to measure those dimensions and watch the trend over time.

The feature is built around three stages.

| Stage | What it is | Page |
|---|---|---|
| **Evalsets** | A golden dataset of conversations, captured from session traces and refined in a dedicated editor. | [Create evalsets](evalsets.md) |
| **Evaluations** | Evaluation functions, built in the visual designer, that score agent behaviour against an evalset. | [Create evaluations](creating-evaluations.md) |
| **Runs and reports** | Run an evaluation against the latest agent build and review the report to see what passed, what regressed, and why. | [Run evaluations](running-evaluations.md) |

## How the stages fit together

1. Chat with your agent and export the session traces into an **evalset**. Use the editor to rename the thread, edit messages, reorder or add turns, and update tool calls.
2. Create an **evaluation**. Existing evalsets are picked up automatically, so you focus on the checks in the visual designer.
3. **Run** the evaluation and open the report to see per-case results, score breakdowns, and regressions against previous runs.

## When to use evaluations

| Use evaluations when... | Look elsewhere when... |
|---|---|
| You're about to change instructions, tools, or the model and want a regression check. | You need a single deterministic unit test for a pure function. |
| You want a baseline of agent quality you can track over time. | You're debugging one specific failed run. Use [Observability](../observability.md). |
| You need to verify safety and refusal behaviour before shipping. | You're still prototyping and haven't picked the agent's tools yet. |

## What's next

- [Create evalsets](evalsets.md) — Capture real chats and curate them into a golden dataset.
- [Create evaluations](creating-evaluations.md) — Build evaluation functions in the visual designer.
- [Run evaluations](running-evaluations.md) — Execute runs and review the report.
