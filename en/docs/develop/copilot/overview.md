---
title: "Using Copilot"
description: "AI-powered tool that generates integration artifacts from natural language prompts or OpenAPI specifications."
keywords: [wso2 integrator, copilot, ai, integration generation]
slug: /develop/copilot/using-copilot
---

# Using Copilot

The WSO2 Integrator Copilot is an AI-powered tool designed to accelerate the creation of integration scenarios. Users define integration requirements through natural language prompts or by uploading relevant files, such as OpenAPI specifications. The Copilot generates ready-to-use integration artifacts, which can be incorporated into existing projects. The tool supports iterative and incremental development, allowing for easy refinement, feature additions, or modifications through subsequent conversational prompts.

## Starting with Copilot

By default, Copilot operates in Edit Mode. This mode is optimized for making quick adjustments, applying bug fixes, or tweaking existing integration logic. Describe the change you want, and Copilot applies it directly to your flow.

For building new features or complex integrations from scratch, switch to Plan Mode.

## Planning

In Plan Mode, Copilot does not generate code immediately. Instead, it analyzes your request and provides a structured, step-by-step breakdown of the execution tasks. This gives you the opportunity to review the logic, add missing steps, or iterate on the plan before any artifacts are created.

![Plan mode showing a structured step-by-step breakdown of execution tasks.](/img/develop/copilot/plan-mode.png)

## Clarifying requirements

During the planning or generation phase, Copilot may identify missing information that is critical to the integration. If a requirement is ambiguous, it pauses and asks you to provide these details in the form of a selection.

![Clarifying requirements prompt showing selection options.](/img/develop/copilot/clarifying-requirements.png)

## Review

Once the generation process is complete, you can inspect exactly what was built before finalizing the changes. You can review the generated artifacts as a diagram or as source code with a diff view.

![Review mode showing the generated integration diagram.](/img/develop/copilot/review-mode.png)

## Configuring

When you are ready to run or test the integration, Copilot identifies configurations required to execute the flow and prompts you to enter the necessary configurables.

![Configuration collection prompt showing required fields for the integration.](/img/develop/copilot/config-collection.png)

## Testing

Copilot automates testing by generating tests for your integration and executing them using the built-in test runner, allowing you to immediately verify the functionality of the generated artifacts.

![Test runner showing generated tests and results.](/img/develop/copilot/running-tests.png)

## Try your services

Once your integration is running, you can try out your services directly through Copilot. Describe what you want to test in plain language, and Copilot runs the appropriate curl commands against your service and returns the results.

![Copilot running curl commands against a running service.](/img/develop/copilot/try-it.png)

## Web tools

If Copilot needs external context or up-to-date documentation, it can trigger web tools to search the internet. Copilot always asks for permission before performing a search. You can enable or disable this via the toggle in the input bar.

![Web tools permission prompt in the Copilot input bar.](/img/develop/copilot/web-tool.png)

## Connector Generator

If you need to integrate with a service that does not have a pre-built connector, Copilot can use the Connector Generator. By providing an OpenAPI specification, Copilot generates the custom connector code required to bridge the gap.

![Connector Generator generating custom connector code from an OpenAPI specification.](/img/develop/copilot/connector-generator.png)
