---
sidebar_position: 7
title: Configuration management
description: Externalize integration settings with configurable variables and Config.toml for environment-specific deployments.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Configuration management

Integration projects typically run in multiple environments — development, staging, and production — each with different database endpoints, API keys, and feature flags. 

WSO2 Integrator uses Ballerina's built-in configuration system to separate settings from code. You declare configurable variables in your source, provide values in a `Config.toml` file (or environment variables), and the runtime resolves them at startup.

## Configurable variables

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

Configurable variables defined in your integration project are accessible across all its flows and nodes.

![Config Editor UI in WSO2 Integrator showing configurable variables for the integration project and the panel for adding a new configurable variable](/img/develop/design-logic/configurations/config-editor-ui.png)

To add a configurable variable through the visual designer, see [Configurations](../integration-artifacts/supporting/configurations.md#adding-a-configuration). To supply values for declared variables, use the **Config Editor** UI (low-code) or edit the `Config.toml` file in the project root.

</TabItem>
<TabItem value="code" label="Ballerina Code">

Declare a configurable variable with the `configurable` keyword at the module level:

```ballerina
import ballerina/http;
import ballerina/log;

// Required -- no default, must be supplied via Config.toml or env var
configurable string dbHost = ?;
configurable string dbUser = ?;
configurable string dbPassword = ?;

// Optional -- has a default value
configurable int dbPort = 3306;
configurable int maxRetries = 3;
configurable decimal requestTimeoutSeconds = 30.0d;
configurable boolean enableCaching = true;
```

Config values must be provided through a supported configuration source (`Config.toml`, `BAL_CONFIG_VAR_*` env vars, `BAL_CONFIG_DATA`, or `-C` CLI args) for all variables with no default values (initialized with `?`); otherwise, the program fails with a runtime error at startup.

</TabItem>
</Tabs>

### Supported types

| Type | Example |
|---|---|
| `int` | `configurable int port = 8080;` |
| `float` | `configurable float threshold = 0.75;` |
| `decimal` | `configurable decimal taxRate = 0.08d;` |
| `string` | `configurable string apiKey = ?;` |
| `boolean` | `configurable boolean debug = false;` |
| `int[]`, `string[]` | `configurable string[] allowedOrigins = ["*"];` |
| `map<string>` | `configurable map<string> headers = {};` |
| Records | `configurable DatabaseConfig dbConfig = ?;` |
| Tables | `configurable table<Employee> key(id) employees = table [];` |

### Structured configuration

Group related settings into a structured configuration by declaring a record type:

```ballerina
type DatabaseConfig record {|
    string host;
    int port = 3306;
    string username;
    string password;
    string database;
    int maxConnections = 10;
|};

type ApiConfig record {|
    string baseUrl;
    string apiKey;
    decimal timeoutSeconds = 30.0d;
    int maxRetries = 3;
|};

configurable DatabaseConfig orderDb = ?;
configurable ApiConfig crmApi = ?;
```

Provide values in `Config.toml`:

```toml
[orderDb]
host = "db.dev.internal"
port = 3306
username = "app"
password = "dev-secret"
database = "orders_dev"
maxConnections = 5

[crmApi]
baseUrl = "https://sandbox.crm.example.com"
apiKey = "dev-key-123"
timeoutSeconds = 15.0
```

## Config.toml

`Config.toml` is the primary configuration file. Place it in the project root directory (alongside `Ballerina.toml`). The runtime reads it automatically at startup.

### Basic structure

```toml
dbHost = "localhost"
dbPort = 3306
dbUser = "root"
dbPassword = "dev-password"
enableCaching = true
maxRetries = 3

allowedOrigins = ["https://app.example.com", "https://admin.example.com"]

[orderDb]
host = "localhost"
port = 3306
username = "root"
password = "dev-password"
database = "orders_dev"
```

### Module-qualified keys

For multi-module projects, prefix keys with the module name:

```toml
apiPort = 8090

[myorg.myproject.billing]
taxRate = 0.08
currency = "USD"

[myorg.myproject.notifications]
smtpHost = "smtp.example.com"
smtpPort = 587
```

## Environment variables

Override any configurable variable with an environment variable using the pattern `BAL_CONFIG_VAR_<NAME>`, where `<NAME>` is the variable name in uppercase:

```bash
export BAL_CONFIG_VAR_DBHOST=db.prod.internal
export BAL_CONFIG_VAR_DBPASSWORD=prod-encrypted-password
bal run
```

Point to an alternative `Config.toml` file:

```bash
BAL_CONFIG_FILES=/etc/myapp/config.toml bal run
```

### Priority order

When the same variable is defined in more than one place, the runtime resolves it using the first source it finds from the list below (top → bottom, highest to lowest precedence):

| Source | Example | Typical use |
|---|---|---|
| Individual env vars (`BAL_CONFIG_VAR_*`) | `BAL_CONFIG_VAR_DBHOST=localhost` | CI/CD pipelines, containers, secrets |
| Command-line arguments | `bal run -- -CdbHost=localhost` | One-off overrides, local testing |
| Inline TOML (`BAL_CONFIG_DATA`) | `BAL_CONFIG_DATA='dbHost="localhost"'` | Containerized runs without a config file |
| TOML files (`Config.toml` / `BAL_CONFIG_FILES`) | `dbHost = "localhost"` | Per-environment configuration |
| Code defaults | `configurable string dbHost = "localhost";` | Development fallback |

## Per-environment configuration

```
my-integration/
├── Ballerina.toml
├── Config.toml              # Default / development
├── config/
│   ├── dev.toml
│   ├── staging.toml
│   └── prod.toml
└── main.bal
```

```toml
# config/dev.toml
dbHost = "localhost"
dbPort = 3306
dbUser = "root"
dbPassword = "dev-password"
dbName = "orders_dev"
crmBaseUrl = "https://sandbox.crm.example.com"
enableCaching = false
logLevel = "DEBUG"
```

```toml
# config/prod.toml
dbHost = "db.prod.internal"
dbPort = 3306
dbUser = "app_user"
dbPassword = "prod-encrypted-password"
dbName = "orders"
crmBaseUrl = "https://api.crm.example.com"
enableCaching = true
logLevel = "WARN"
```

```bash
BAL_CONFIG_FILES=config/dev.toml bal run
BAL_CONFIG_FILES=config/prod.toml bal run
```

## Secrets management

Never commit secrets to `Config.toml` in version control. Supply them through environment variables or a gitignored secrets file. For Kubernetes Secrets, HashiCorp Vault, AWS Secrets Manager, and TLS configuration, see [Secrets and encryption](../../deploy-operate/secure/secrets-encryption.md).

## Complete example

```ballerina
import ballerina/http;
import ballerina/log;
import ballerinax/mysql;
import ballerinax/mysql.driver as _;
import ballerina/sql;

configurable string dbHost = ?;
configurable int dbPort = 3306;
configurable string dbUser = ?;
configurable string dbPassword = ?;
configurable string dbName = ?;

configurable string crmBaseUrl = ?;

configurable int servicePort = 8090;
configurable boolean enableRequestLogging = false;

final mysql:Client orderDb = check new (
    host = dbHost, port = dbPort,
    user = dbUser, password = dbPassword,
    database = dbName
);

final http:Client crmClient = check new (crmBaseUrl, {
    httpVersion: http:HTTP_1_1
});

listener http:Listener httpListener = new (servicePort);

service /api on httpListener {

    resource function get orders() returns json|error {
        if enableRequestLogging {
            log:printInfo("GET /api/orders");
        }
        stream<record {|anydata...;|}, sql:Error?> resultStream = orderDb->query(`SELECT * FROM orders`);
        record {|anydata...;|}[] orderList = check from record {|anydata...;|} orderRow in resultStream
            select orderRow;
        return orderList.toJson();
    }
}

```

A matching `Config.toml` for the production environment:

```toml
dbHost = "db.prod.internal"
dbPort = 3306
dbUser = "app_user"
dbPassword = "prod-encrypted-password"
dbName = "orders"
crmBaseUrl = "https://api.crm.example.com"
servicePort = 8090
enableRequestLogging = false
```

## What's next

- [Configurations](../integration-artifacts/supporting/configurations.md) — Declare configurable variables and supply values through the visual designer.
- [Secrets and encryption](../../deploy-operate/secure/secrets-encryption.md) — Securely supply credentials and protect data in transit and at rest.
- [Connections](managing-connections.md) — Use configurable variables to parameterize connections.
