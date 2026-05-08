---
sidebar_position: 3
title: Create evaluations
description: Build evaluation functions in the WSO2 Integrator visual designer. Evalsets are auto-wired so you only design the scoring logic.
keywords: [wso2 integrator, ai agent, evaluation, visual designer, llm-as-judge]
---

# Create evaluations

An **evaluation** is the function that scores agent behaviour against an evalset. You build it in the visual designer. Evalsets you've already created are auto-wired into the evaluation, so you focus on the scoring logic, not the plumbing.

## Auto-wired evalsets

When you create a new evaluation, every evalset in the project is available as an input. Pick one or more to score against and the designer connects them for you.

<!-- TODO: Add screenshot of the evaluation creation dialog showing the evalset picker -->

## Build the scoring logic

Use the visual designer to assemble the checks that decide whether each case passes. Common building blocks include:

- **Tool-call assertions.** Verify the agent invoked the expected tool with the expected arguments.
- **Content checks.** Verify the response contains, or omits, specific terms.
- **LLM-as-judge scoring.** Call an LLM to score subjective dimensions like tone, completeness, or relevance on a numeric scale.

<!-- TODO: Add screenshot of the visual designer with a sample evaluation flow -->

## What's next

- [Run evaluations](running-evaluations.md) — Execute the evaluation and open the report.
- [Create evalsets](evalsets.md) — Add more evalsets to broaden coverage.
