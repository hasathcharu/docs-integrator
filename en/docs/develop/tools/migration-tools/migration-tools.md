---
sidebar_position: 1
title: Migration tools
description: Migrate integrations from MuleSoft, TIBCO BusinessWorks, and Azure Logic Apps to WSO2 Integrator.
---

# Migration Tools

WSO2 Integrator provides migration tools to help you move existing integrations from other platforms to WSO2 Integrator. These tools analyze your integration artifacts, generate equivalent Ballerina code, and produce a migration report highlighting items that require manual attention.


## Supported platforms and features

| Source platform | WSO2 Integrator | CLI | AI Enhancement | Command |
|---|:---:|:---:|:---:|---|
| [MuleSoft](migrate-from-mulesoft.md) | ✓ | ✓ | ✓ | `bal migrate-mule` |
| [TIBCO BusinessWorks](migrate-from-tibco-businessworks.md) | ✓ | ✓ | ✓ | `bal migrate-tibco` |
| [Azure Logic Apps](migrate-from-azure-logic-apps.md) |  | ✓ | ✓ | `bal migrate-logicapps` |

**Legend:**
- **WSO2 Integrator:** Migration supported via the WSO2 Integrator migration wizard UI.
- **CLI:** Migration supported via the Ballerina CLI tool.
- **AI Enhancement (WSO2 Integrator only):** AI-powered migration enhancement is available only in the WSO2 Integrator for MuleSoft and TIBCO.


## Migration workflow

Migration can be initiated using either the WSO2 Integrator migration wizard (UI) or the CLI tool. The table above shows the capabilities supported by each migration tool. For step-by-step instructions and screenshots, refer to the dedicated migration pages for each platform.

After migration, complete the following post-migration steps:

1. **Review:** Check the migration report and migration summary markdown files to understand what was done during migration and identify any items that may need manual attention.
2. **Implement:** Complete any manually flagged items (custom logic, complex transformations, or unsupported elements).
3. **Configure:** Set up `Config.toml` with connection details and environment-specific values.
4. **Test:** Review any auto-migrated tests, and add or update tests as needed to ensure the migrated integrations behave as expected compared to the source system.
5. **Deploy:** Deploy to the WSO2 Integrator runtime.

## Tool summary

### MuleSoft
- Converts MuleSoft Anypoint flows (XML configurations) to Ballerina code.
- Handles HTTP listeners, request connectors, DataWeave transformations, routers, error handling patterns, and more.
- Migration is supported via both the WSO2 Integrator wizard (UI) and the CLI tool.
- AI enhancement is available only in the wizard (UI) to further automate migration and resolve unmapped elements.
- See [Migrate from MuleSoft](migrate-from-mulesoft.md) for detailed instructions.

### TIBCO BusinessWorks
- Converts TIBCO BusinessWorks process definitions to Ballerina code.
- Handles process flows, activities, transitions, shared resources, error handling configurations, and more.
- Migration is supported via both the WSO2 Integrator wizard (UI) and the CLI tool.
- AI enhancement is available only in the wizard (UI) to further automate migration and resolve unmapped elements.
- See [Migrate from TIBCO BusinessWorks](migrate-from-tibco-businessworks.md) for detailed instructions.

### Azure Logic Apps
- Converts Logic Apps workflow definitions (ARM templates and workflow JSON) to Ballerina code.
- Handles triggers, actions, connectors, control flow, error handling patterns, and more.
- Migration is currently supported only via the CLI tool. Wizard (UI) and AI enhancement are not available for Logic Apps at this time.
- See [Migrate from Azure Logic Apps](migrate-from-azure-logic-apps.md) for detailed instructions.

## Command reference

| Command | Description |
|---|---|
| `bal migrate-mule <source-project-directory-or-file>` | Migrate from MuleSoft |
| `bal migrate-tibco <source-project-directory-or-file>` | Migrate from TIBCO BusinessWorks |
| `bal migrate-logicapps <source-project-directory-or-file>` | Migrate from Azure Logic Apps |
| `-o, --out <output-directory>` | Output directory of the migration |
| `-v, --verbose` | Enable verbose output |
| `-m, --multi-root` | (MuleSoft/TIBCO) Treat each child directory as a separate project and convert each.<br/>(Logic Apps) Treat each JSON file in the directory as a separate project and convert each. |
| `-d, --dry-run` | (MuleSoft/TIBCO) Analyze and generate a migration report without generating Ballerina code |
| `-k, --keep-structure` | (MuleSoft/TIBCO) Preserve original project structure |
