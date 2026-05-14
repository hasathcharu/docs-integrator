---
title: "Copilot Capabilities"
description: "Use WSO2 Integrator Copilot to write, test, debug, and fix integrations, and generate connectors from natural language prompts and OpenAPI specifications."
keywords: [wso2 integrator, copilot, ai, integration generation]
slug: /develop/wso2-integrator-copilot/copilot-capabilities
---

# Copilot Capabilities

WSO2 Integrator Copilot builds integrations from natural language prompts. It produces ready-to-use artifacts for your project. Iterate through follow-up prompts to refine logic, add features, or modify behavior.

![WSO2 Integrator Copilot panel open alongside an integration project in the IDE.](/img/develop/copilot/copilot-overview.png)

## Modes

Copilot has two modes. Switch between them with the toggle in the Copilot input bar.

- **Edit Mode**: Copilot starts generating immediately and applies the changes to your integration. Best for quick edits.
- **Plan Mode**: Copilot first proposes a high-level plan with a step-by-step task breakdown. Review or revise the plan, then approve it to begin generation.

  ![Plan mode showing a structured step-by-step breakdown of execution tasks.](/img/develop/copilot/plan-mode.png)

## Generating connectors

If you need to integrate with a service that does not have a pre-built connector, provide an OpenAPI specification and Copilot generates the custom connector code for you.

![Copilot generating custom connector code from an OpenAPI specification.](/img/develop/copilot/connector-generator.png)

## Using web tools

Copilot can use web tools to search the internet for external context or up-to-date documentation. It asks for permission before each search unless you enable the web tools toggle in the input bar.

![Web tools permission prompt in the Copilot input bar.](/img/develop/copilot/web-tool.png)

## Clarifying requirements

During the planning or generation phase, Copilot may identify missing information that is critical to the integration. If a requirement is ambiguous, it pauses and presents a list of suggested options. Select one, or select **Other** to type your own answer.

![Clarifying requirements prompt showing selection options.](/img/develop/copilot/clarifying-requirements.png)

## Reviewing

After generation completes, you can inspect exactly what was built before finalizing the changes. Review the generated artifacts as a diagram or as source code with a diff view.

![Review mode showing the generated integration diagram.](/img/develop/copilot/review-mode.png)

## Configuring

When you run or test the integration, Copilot identifies the required configurables and prompts you to enter them.

![Configuration collection prompt showing required fields for the integration.](/img/develop/copilot/config-collection.png)

## Testing

Copilot generates tests for your integration and runs them with the built-in test runner, so you can verify the generated artifacts immediately.

![Test runner showing generated tests and results.](/img/develop/copilot/running-tests.png)

## Trying your services

After your integration is running, you can send test requests to your services from Copilot. Describe what you want to test in plain language, and Copilot runs the appropriate `curl` commands against your service and returns the results.

![Copilot running curl commands against a running service.](/img/develop/copilot/try-it.png)

## Debugging

Copilot can run your integrations and read the runtime logs to debug issues as they occur.

![Copilot debugging an integration by reproducing the failing request, inspecting the HTTP response and service logs, and identifying a case-sensitivity bug in the team filter.](/img/develop/copilot/debugging-using-service-logs.png)

## Slash commands

Type `/` in the Copilot input bar to invoke a command for a specific task.

| Command | Description |
|---|---|
| `/ask` | Ask questions about Ballerina from the documentation. |
| `/doc` | Generate documentation for your integration. |
| `/openapi` | Import OpenAPI specifications. |
| `/typecreator` | Create custom types. |
| `/datamap` | [Generate data mappings](../integration-artifacts/supporting/data-mapper/ai-data-mapper.md). |
| `/natural-programming` | Experimental natural-language-to-code generation. |

:::note
`/ask` answers only from the Ballerina documentation and does not use your codebase context. For questions about your code or any other topic, message Copilot directly without a command.
:::

## See also

- [Getting started](getting-started.md) — Sign in to WSO2 Integrator Copilot.
- [Generate tests with AI](../test/ai-generated-cases.md) — Use Copilot to generate test cases.
- [AI data mapper](../integration-artifacts/supporting/data-mapper/ai-data-mapper.md) — Generate data mappings using AI.
- [Try-It tool](../test/built-in-try-it-tool.md) — Test services without leaving the IDE.
