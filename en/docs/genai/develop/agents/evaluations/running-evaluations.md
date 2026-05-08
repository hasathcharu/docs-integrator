---
sidebar_position: 4
title: Run evaluations
description: Run AI agent evaluations in WSO2 Integrator and review per-case reports, score breakdowns, and regressions across runs.
keywords: [wso2 integrator, ai agent, evaluation, report, regression]
---

# Run evaluations

Once an evaluation is configured, run it on demand and review the report to see how the agent performed.

## Start a run

Trigger a run from the evaluation page. Each run executes every case in the wired evalsets and records the results.

<!-- TODO: Add screenshot of the Run button and run-in-progress indicator -->

## Review the report

The report breaks down results per case so you can isolate regressions quickly. Typical things to look for:

- **Pass / fail** for each case in the evalset.
- **Score breakdown** across the dimensions defined in the evaluation.
- **Comparison with previous runs** to spot regressions introduced by changes to instructions, tools, or the model.

<!-- TODO: Add screenshot of the report view with the run summary and per-case detail -->

## What's next

- [Observability](../observability.md) — Trace a single failed case to see exactly what the agent did.
- [Create evalsets](evalsets.md) — Expand coverage by adding new cases.
