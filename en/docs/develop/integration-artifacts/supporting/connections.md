---
title: Connections
description: Centralize and reuse connection configurations for databases, HTTP clients, and message brokers.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Connections

Connection artifacts centralize the configuration for external systems. Define connections once and reuse them across multiple services, event handlers, and functions in your project.

## Adding a connection

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. Open an integration/library in **WSO2 Integrator**.

   ![WSO2 Integrator sidebar showing the project structure with Connections listed](/img/develop/integration-artifacts/supporting/connections/step-1.png)

2. Click **+ Add Artifact** in the canvas or click **+** next to **Connections** in the sidebar

3. In the **Add Connection** panel, select how to define the connector:

   ![Add Connection panel showing connector options](/img/develop/integration-artifacts/supporting/connections/step-2.png)

   **Create New Connector**

   | Option | Description |
   |---|---|
   | **Connect via API Specification** | Import an OpenAPI or WSDL file to generate a typed HTTP client connector. For more information, see the [OpenAPI tool](../../tools/openapi-tool.md) and [WSDL tool](../../tools/wsdl-tool.md). |
   | **Connect to a Database** | Enter database credentials to introspect schema and create a typed database connector. Supports MySQL, MSSQL, and PostgreSQL. |

   **Pre-built Connectors**

   Browse the connector library using the **All**, **Standard**, or **Organization** tabs. Available connectors include HTTP, GraphQL, WebSocket, TCP, UDP, FTP, and many more. Use the search box to filter by name. For the full list, see the [Connector Catalog](../../../connectors/overview.md).

4. Select a connector type. A configuration form appears with fields specific to that connector (for example, base URL and authentication for HTTP, or host, port, and credentials for a database).

5. Fill in the required fields and click **Create** (or **Save**).

6. The new connection appears under **Connections** in the sidebar and is available for use in any service, function, or event handler in your project.

</TabItem>
<TabItem value="code" label="Ballerina Code">

Import the appropriate connector module and declare the client as a module-level `final` variable. For the complete list of available modules, see the [Connector Catalog](../../../connectors/overview.md).

```ballerina
// connections.bal
import ballerinax/mysql;
import ballerina/http;
import ballerinax/kafka;

// Database connection
configurable string dbHost = ?;
configurable int dbPort = 3306;
configurable string dbUser = ?;
configurable string dbPassword = ?;
configurable string dbName = ?;

final mysql:Client orderDb = check new (
    host = dbHost,
    port = dbPort,
    user = dbUser,
    password = dbPassword,
    database = dbName
);

// HTTP client connection
configurable string crmBaseUrl = ?;
configurable string crmApiKey = ?;

final http:Client crmClient = check new (crmBaseUrl, {
    auth: {token: crmApiKey},
    timeout: 30,
    retryConfig: {
        count: 3,
        interval: 2,
        backOffFactor: 2.0
    }
});

// Kafka producer connection
configurable string kafkaBrokers = "localhost:9092";

final kafka:Producer kafkaProducer = check new ({
    bootstrapServers: kafkaBrokers,
    acks: kafka:ACKS_ALL,
    retryCount: 3
});
```

</TabItem>
</Tabs>

## Connection types

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

The **Add Connection** panel organizes connectors into two categories:

- **Create New Connector** — generate a connector from an API spec or by introspecting a database.
- **Pre-built Connectors** — select from the connector library. Use the **All**, **Standard**, or **Organization** tabs to filter by source.

Pre-built connectors come from several sources:

| Source | Description | Examples |
|---|---|---|
| **`ballerina`** | Standard library connectors maintained as part of the Ballerina platform. | `ballerina/http`, `ballerina/graphql`, `ballerina/tcp` |
| **`ballerinax`** | Extended library connectors for popular third-party systems. | `ballerinax/mysql`, `ballerinax/kafka`, `ballerinax/rabbitmq`, `ballerinax/ftp` |
| **`wso2`** | Connectors published by WSO2 for enterprise systems and WSO2 products. | WSO2 organization connectors on the connector registry |
| **Custom / Organization** | Connectors developed in-house and published to your organization's registry, or generated from an OpenAPI/WSDL spec or database schema. | Your organization's private connectors |

For the complete list of available connectors, see the [Connector Catalog](../../../connectors/overview.md). To build and publish your own, see [Build your own connector](../../../connectors/build-your-own/index.md).

</TabItem>
<TabItem value="code" label="Ballerina Code">

Connector modules are distributed under different organizations on [Ballerina Central](https://central.ballerina.io/):

| Organization | Import prefix | Description |
|---|---|---|
| **Ballerina standard library** | `ballerina/*` | Core protocol and platform modules maintained with the language (for example, `ballerina/http`, `ballerina/graphql`, `ballerina/tcp`). |
| **Ballerina extended library** | `ballerinax/*` | Connectors for popular third-party systems (for example, `ballerinax/mysql`, `ballerinax/kafka`, `ballerinax/rabbitmq`, `ballerinax/ftp`). |
| **WSO2** | `wso2/*` | Connectors published by WSO2 for enterprise systems and WSO2 products. |
| **Custom / Organization** | `<your-org>/*` | Connectors developed in-house and published to your organization's registry, or generated from an OpenAPI or WSDL spec. |

For the complete list of modules and their APIs, see the [Connector Catalog](../../../connectors/overview.md). To build and publish your own, see [Build your own connector](../../../connectors/build-your-own/index.md).

</TabItem>
</Tabs>

## Best practices

| Practice | Description |
|---|---|
| **Dedicated file** | Keep all connections in a `connections.bal` file |
| **Use `configurable`** | Externalize host, port, and credentials so they vary by environment |
| **Use `final`** | Declare connections as `final` to initialize them once at startup |
| **Retry configuration** | Add retry and timeout settings for resilient connections |
| **Connection pooling** | Database clients manage connection pools automatically |
