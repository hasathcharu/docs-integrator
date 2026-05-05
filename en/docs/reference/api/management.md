---
title: Management API
description: GraphQL API for managing WSO2 Integrator deployments via the Integration Control Plane (ICP).
---

# Management API

The Management API is a GraphQL endpoint exposed by the Integration Control Plane (ICP) server. It provides queries and mutations to manage runtimes, environments, projects, components, artifacts, loggers, secrets, and MI users.

## Endpoint

```
POST https://<icp-host>:9446/graphql
```

The default port is `9446`. All operations use the same endpoint via HTTP `POST`.

## Authentication

Before calling the GraphQL API, obtain a JWT access token from the ICP authentication service. See the [Authentication API](auth-api.md) for the full reference covering:

- Login (local credentials and OIDC/SSO)
- Token refresh and revocation
- Password management
- User, group, role, and permission management

Once you have a token, include it in every request:

```
Authorization: Bearer <jwt-access-token>
```

The token is signed with HMAC-SHA256 and validated against:

- **Issuer**: `icp-frontend-jwt-issuer`
- **Audience**: `icp-server`

## Queries

### Runtimes

#### `runtimes`

Returns a list of registered runtimes. All parameters are optional filters.

```graphql
query {
  runtimes(
    status: String
    runtimeType: String
    environmentId: String
    projectId: String
    componentId: String
  ): [Runtime!]!
}
```

**Example:**

```graphql
query {
  runtimes(environmentId: "env-001", componentId: "comp-abc") {
    runtimeId
    runtimeName
    runtimeType
    status
    version
    lastHeartbeat
    component { id name }
    environment { id name }
  }
}
```

#### `runtime`

Returns a single runtime by ID.

```graphql
query {
  runtime(runtimeId: String!): Runtime
}
```

#### `componentDeployment`

Returns deployment information for a component in a specific environment.

```graphql
query {
  componentDeployment(
    orgHandler: String!
    orgUuid: String!
    componentId: String!
    versionId: String!
    environmentId: String!
  ): ComponentDeployment
}
```

---

### Environments

#### `environments`

Returns all environments accessible to the authenticated user.

```graphql
query {
  environments(orgUuid: String, type: String, projectId: String): [Environment!]!
}
```

The `type` parameter accepts `"prod"` or `"non-prod"` to filter by production status.

**Example:**

```graphql
query {
  environments(type: "non-prod") {
    id
    name
    handler
    description
    isProduction
    critical
  }
}
```

#### `environmentByHandler`

Returns a single environment by its handler (slug).

```graphql
query {
  environmentByHandler(environmentHandler: String!): Environment
}
```

#### `environmentHandlerAvailability`

Checks if a handler string is available for a new environment.

```graphql
query {
  environmentHandlerAvailability(environmentHandlerCandidate: String!): EnvironmentHandlerAvailability!
}
```

---

### Projects

#### `projects`

Returns all projects accessible to the authenticated user.

```graphql
query {
  projects(orgId: Int): [Project!]!
}
```

#### `project`

Returns a single project by ID.

```graphql
query {
  project(orgId: Int, projectId: String!): Project
}
```

#### `projectByHandler`

Returns a project by its handler within an organization.

```graphql
query {
  projectByHandler(orgId: Int!, projectHandler: String!): Project
}
```

#### `projectCreationEligibility`

Checks whether the authenticated user is permitted to create a project.

```graphql
query {
  projectCreationEligibility(orgId: Int!, orgHandler: String!): ProjectCreationEligibility!
}
```

#### `projectHandlerAvailability`

Checks if a handler string is available for a new project within an organization.

```graphql
query {
  projectHandlerAvailability(orgId: Int!, projectHandlerCandidate: String!): ProjectHandlerAvailability!
}
```

---

### Components

#### `components`

Returns all components accessible to the user within an organization, with optional project filter and sort/pagination options.

```graphql
query {
  components(
    orgHandler: String!
    projectId: String
    options: ComponentOptionsInput
  ): [Component!]!
}
```

#### `component`

Returns a single component by ID, or by project ID and handler.

```graphql
query {
  component(
    componentId: String
    projectId: String
    componentHandler: String
  ): Component
}
```

#### `componentArtifactTypes`

Returns the artifact types available for a component, optionally filtered by environment.

```graphql
query {
  componentArtifactTypes(componentId: String!, environmentId: String): ArtifactTypes!
}
```

---

### Artifacts by Environment and Component

All artifact queries below share the same signature pattern and require both `environmentId` and `componentId`. State fields (e.g., `state`, `tracing`, `statistics`) are overlaid from the reconcile engine.

#### `servicesByEnvironmentAndComponent`

```graphql
query {
  servicesByEnvironmentAndComponent(environmentId: String!, componentId: String!): [Service!]!
}
```

#### `listenersByEnvironmentAndComponent`

```graphql
query {
  listenersByEnvironmentAndComponent(environmentId: String!, componentId: String!): [Listener!]!
}
```

#### `automationsByEnvironmentAndComponent`

```graphql
query {
  automationsByEnvironmentAndComponent(environmentId: String!, componentId: String!): [Automation!]!
}
```

#### `restApisByEnvironmentAndComponent`

```graphql
query {
  restApisByEnvironmentAndComponent(environmentId: String!, componentId: String!): [RestApi!]!
}
```

**Example:**

```graphql
query {
  restApisByEnvironmentAndComponent(environmentId: "env-001", componentId: "comp-abc") {
    name
    url
    context
    version
    state
    tracing
    statistics
    resources { path methods }
  }
}
```

#### `proxyServicesByEnvironmentAndComponent`

```graphql
query {
  proxyServicesByEnvironmentAndComponent(environmentId: String!, componentId: String!): [ProxyService!]!
}
```

#### `endpointsByEnvironmentAndComponent`

```graphql
query {
  endpointsByEnvironmentAndComponent(environmentId: String!, componentId: String!): [MIEndpoint!]!
}
```

#### `inboundEndpointsByEnvironmentAndComponent`

```graphql
query {
  inboundEndpointsByEnvironmentAndComponent(environmentId: String!, componentId: String!): [InboundEndpoint!]!
}
```

#### `sequencesByEnvironmentAndComponent`

```graphql
query {
  sequencesByEnvironmentAndComponent(environmentId: String!, componentId: String!): [Sequence!]!
}
```

#### `tasksByEnvironmentAndComponent`

```graphql
query {
  tasksByEnvironmentAndComponent(environmentId: String!, componentId: String!): [Task!]!
}
```

#### `templatesByEnvironmentAndComponent`

```graphql
query {
  templatesByEnvironmentAndComponent(environmentId: String!, componentId: String!): [Template!]!
}
```

#### `messageStoresByEnvironmentAndComponent`

```graphql
query {
  messageStoresByEnvironmentAndComponent(environmentId: String!, componentId: String!): [MessageStore!]!
}
```

#### `messageProcessorsByEnvironmentAndComponent`

```graphql
query {
  messageProcessorsByEnvironmentAndComponent(environmentId: String!, componentId: String!): [MessageProcessor!]!
}
```

#### `localEntriesByEnvironmentAndComponent`

```graphql
query {
  localEntriesByEnvironmentAndComponent(environmentId: String!, componentId: String!): [LocalEntry!]!
}
```

#### `dataServicesByEnvironmentAndComponent`

```graphql
query {
  dataServicesByEnvironmentAndComponent(environmentId: String!, componentId: String!): [DataService!]!
}
```

#### `dataSourcesByEnvironmentAndComponent`

```graphql
query {
  dataSourcesByEnvironmentAndComponent(environmentId: String!, componentId: String!): [DataSource!]!
}
```

#### `carbonAppsByEnvironmentAndComponent`

```graphql
query {
  carbonAppsByEnvironmentAndComponent(environmentId: String!, componentId: String!): [CarbonApp!]!
}
```

#### `carbonAppFaultStackTrace`

Returns the fault stack trace for a Carbon App on a specific runtime.

```graphql
query {
  carbonAppFaultStackTrace(runtimeId: String!, appName: String!): CarbonAppFaultStackTrace
}
```

#### `connectorsByEnvironmentAndComponent`

```graphql
query {
  connectorsByEnvironmentAndComponent(environmentId: String!, componentId: String!): [Connector!]!
}
```

#### `registryResourcesByEnvironmentAndComponent`

```graphql
query {
  registryResourcesByEnvironmentAndComponent(environmentId: String!, componentId: String!): [RegistryResource!]!
}
```

---

### Artifact Detail Queries

These queries fetch live details from the MI Management API for a single artifact. They accept an optional `environmentId`, `runtimeId`, and `packageName` to target a specific runtime.

#### `artifactSourceByComponent`

Returns the source definition of an artifact (e.g., sequence XML, API definition).

```graphql
query {
  artifactSourceByComponent(
    componentId: String!
    artifactType: String!
    artifactName: String!
    environmentId: String
    runtimeId: String
    packageName: String
    templateType: String
  ): String!
}
```

#### `artifactWsdlByComponent`

Returns the WSDL content for a proxy service artifact.

```graphql
query {
  artifactWsdlByComponent(
    componentId: String!
    artifactType: String!
    artifactName: String!
    environmentId: String
    runtimeId: String
    packageName: String
  ): String!
}
```

#### `localEntryValueByComponent`

Returns the value of a named local entry.

```graphql
query {
  localEntryValueByComponent(
    componentId: String!
    entryName: String!
    environmentId: String
    runtimeId: String
  ): String!
}
```

#### `artifactParametersByComponent`

Returns parameters for an inbound endpoint, message processor, or data source artifact.

```graphql
query {
  artifactParametersByComponent(
    componentId: String!
    artifactType: String!
    artifactName: String!
    environmentId: String
    runtimeId: String
    packageName: String
  ): [Parameter!]!
}
```

#### `dataSourceOverviewByComponent`

Returns overview fields (name, type, description, driverClass, userName, url) for a data source.

```graphql
query {
  dataSourceOverviewByComponent(
    componentId: String!
    dataSourceName: String!
    environmentId: String
    runtimeId: String
  ): [Parameter!]!
}
```

#### `messageStoreOverviewByComponent`

Returns overview fields (name, type, container, size) for a message store.

```graphql
query {
  messageStoreOverviewByComponent(
    componentId: String!
    storeName: String!
    environmentId: String
    runtimeId: String
  ): [Parameter!]!
}
```

#### `messageProcessorOverviewByComponent`

Returns overview fields (name, type, messageStore, status) for a message processor.

```graphql
query {
  messageProcessorOverviewByComponent(
    componentId: String!
    processorName: String!
    environmentId: String
    runtimeId: String
  ): [Parameter!]!
}
```

#### `dataServiceOverviewByComponent`

Returns structured overview (dataSources, queries, resources, operations) for a data service.

```graphql
query {
  dataServiceOverviewByComponent(
    componentId: String!
    dataServiceName: String!
    environmentId: String
    runtimeId: String
  ): MgmtDataServiceInfo!
}
```

---

### Loggers

#### `loggersByRuntime`

Returns loggers for a specific runtime. For MI runtimes, fetches live from the MI Management API. For BI runtimes, reads from the database with reconcile state overlaid.

```graphql
query {
  loggersByRuntime(runtimeId: String!): [Logger!]!
}
```

#### `loggersByEnvironmentAndComponent`

Returns loggers grouped by component name across all runtimes in an environment.

```graphql
query {
  loggersByEnvironmentAndComponent(environmentId: String!, componentId: String!): [LoggerGroup!]!
}
```

**Example:**

```graphql
query {
  loggersByEnvironmentAndComponent(environmentId: "env-001", componentId: "comp-abc") {
    loggerName
    componentName
    logLevel
    logLevelInSync
    runtimeIds
  }
}
```

---

### Log Files

#### `logFilesByRuntime`

Returns the list of available log files on an MI runtime. The runtime must be in `RUNNING` status.

```graphql
query {
  logFilesByRuntime(runtimeId: String!, searchKey: String): LogFilesResponse!
}
```

#### `logFileContent`

Returns the content of a specific log file. The `fileName` must be a plain filename with no path separators or traversal sequences.

```graphql
query {
  logFileContent(runtimeId: String!, fileName: String!): String!
}
```

---

### Registry Browser

These queries operate on the MI registry and require the runtime to be in `RUNNING` status.

#### `registryDirectory`

Lists the contents of a registry directory path.

```graphql
query {
  registryDirectory(runtimeId: String!, path: String!, expand: Boolean): RegistryDirectoryResponse!
}
```

#### `registryFileContent`

Returns the content of a registry file at the given path.

```graphql
query {
  registryFileContent(runtimeId: String!, path: String!): String!
}
```

#### `registryResourceMetadata`

Returns metadata (name and mediaType) for a registry resource.

```graphql
query {
  registryResourceMetadata(runtimeId: String!, path: String!): RegistryResourceMetadata!
}
```

#### `registryResourceProperties`

Returns the properties of a registry resource.

```graphql
query {
  registryResourceProperties(runtimeId: String!, path: String!): RegistryPropertiesResponse!
}
```

---

### Secrets

#### `orgSecrets`

Returns org-level secrets. Optionally filter by environment.

```graphql
query {
  orgSecrets(environmentId: String): [OrgSecretListEntry!]!
}
```

#### `componentSecrets`

Returns secrets bound to a specific component in an environment.

```graphql
query {
  componentSecrets(componentId: String!, environmentId: String!): [BoundSecretEntry!]!
}
```

---

### MI Runtime User Management

#### `getMIUsers`

Returns the list of users configured on an MI runtime.

```graphql
query {
  getMIUsers(componentId: String!, runtimeId: String!): MIUsersResponse!
}
```

**Example:**

```graphql
query {
  getMIUsers(componentId: "comp-abc", runtimeId: "runtime-001") {
    users {
      username
      domain
      isAdmin
    }
  }
}
```

---

### System

#### `systemInfo`

Returns the ICP server version.

```graphql
query {
  systemInfo {
    version
  }
}
```

---

## Mutations

### Runtime Mutations

#### `deleteRuntime`

Deletes a runtime by ID. If `revokeSecret` is `true` and the runtime's secret is no longer used by any other runtime, the secret is also revoked.

```graphql
mutation {
  deleteRuntime(runtimeId: String!, revokeSecret: Boolean): DeleteRuntimeResult!
}
```

**Example:**

```graphql
mutation {
  deleteRuntime(runtimeId: "runtime-001", revokeSecret: true) {
    deleted
    orphanedKeyId
    secretRevoked
  }
}
```

---

### Environment Mutations

#### `createEnvironment`

Creates a new environment. Requires environment management permission; production environments require full management permission.

```graphql
mutation {
  createEnvironment(environment: EnvironmentInput!): Environment
}
```

**Input:**

```graphql
input EnvironmentInput {
  name: String!
  environmentHandler: String!
  description: String
  isProduction: Boolean
}
```

#### `deleteEnvironment`

Deletes an environment by ID.

```graphql
mutation {
  deleteEnvironment(environmentId: String!): Boolean!
}
```

#### `updateEnvironment`

Updates the name, handler, or description of an environment.

```graphql
mutation {
  updateEnvironment(
    environmentId: String!
    name: String
    handler: String
    description: String
  ): Environment
}
```

#### `updateEnvironmentProductionStatus`

Promotes or demotes an environment to/from production status. Always requires full management permission.

```graphql
mutation {
  updateEnvironmentProductionStatus(environmentId: String!, isProduction: Boolean!): Environment
}
```

---

### Project Mutations

#### `createProject`

Creates a new project.

```graphql
mutation {
  createProject(project: ProjectInput!): Project
}
```

#### `deleteProject`

Deletes a project. Fails if the project still contains components.

```graphql
mutation {
  deleteProject(orgId: Int!, projectId: String!): DeleteResponse!
}
```

#### `updateProject`

Updates project name, version, or description.

```graphql
mutation {
  updateProject(project: ProjectUpdateInput!): Project
}
```

**Input:**

```graphql
input ProjectUpdateInput {
  id: String!
  orgId: Int
  name: String
  version: String
  description: String
}
```

---

### Component Mutations

#### `createComponent`

Creates a new component within a project. The name must be 3–64 characters.

```graphql
mutation {
  createComponent(component: ComponentInput!): Component
}
```

**Input:**

```graphql
input ComponentInput {
  projectId: String!
  name: String!
  displayName: String
  description: String
  orgId: Int
  orgHandler: String
  componentType: RuntimeType
  technology: String
  repository: String
  branch: String
  directoryPath: String
  secretRef: String
  isPublicRepo: Boolean
}
```

#### `deleteComponentV2`

Deletes a component. Returns a detailed response. Fails if the component has registered runtimes.

```graphql
mutation {
  deleteComponentV2(orgHandler: String!, componentId: String!, projectId: String!): DeleteComponentV2Response!
}
```

#### `updateComponent`

Updates component fields such as name, display name, or description.

```graphql
mutation {
  updateComponent(component: ComponentUpdateInput!): Component!
}
```

---

### Artifact Control Mutations

#### `updateArtifactStatus`

Enables or disables a named artifact across all MI runtimes of a component. Changes are propagated via the reconcile engine.

```graphql
mutation {
  updateArtifactStatus(input: ArtifactStatusChangeInput!): ArtifactStatusChangeResponse!
}
```

**Input:**

```graphql
input ArtifactStatusChangeInput {
  componentId: String!
  artifactType: String!
  artifactName: String!
  status: String!   # "active" or "inactive"
}
```

**Example:**

```graphql
mutation {
  updateArtifactStatus(input: {
    componentId: "comp-abc"
    artifactType: "api"
    artifactName: "OrderAPI"
    status: "inactive"
  }) {
    status
    message
    successCount
    failedCount
  }
}
```

#### `updateArtifactTracingStatus`

Enables or disables tracing on a specific artifact in an environment.

```graphql
mutation {
  updateArtifactTracingStatus(input: ArtifactTracingChangeInput!): ArtifactTracingChangeResponse!
}
```

**Input:**

```graphql
input ArtifactTracingChangeInput {
  componentId: String!
  environmentId: String!
  artifactType: String!
  artifactName: String!
  trace: String!   # "enable" or "disable"
}
```

#### `updateArtifactStatisticsStatus`

Enables or disables statistics collection on an artifact. Supported artifact types: `proxy-service`, `endpoint`, `api`, `sequence`, `inbound-endpoint`.

```graphql
mutation {
  updateArtifactStatisticsStatus(input: ArtifactStatisticsChangeInput!): ArtifactStatisticsChangeResponse!
}
```

**Input:**

```graphql
input ArtifactStatisticsChangeInput {
  componentId: String!
  environmentId: String!
  artifactType: String!
  artifactName: String!
  statistics: String!   # "enable" or "disable"
}
```

#### `triggerArtifact`

Triggers a scheduled task on all MI runtimes of a component. Commands are queued for offline runtimes and delivered on next heartbeat.

```graphql
mutation {
  triggerArtifact(input: ArtifactTriggerInput!): ArtifactTriggerResponse!
}
```

**Input:**

```graphql
input ArtifactTriggerInput {
  componentId: String!
  taskName: String!
}
```

---

### Listener Control Mutations

#### `updateListenerState`

Starts or stops a named listener on a set of runtimes.

```graphql
mutation {
  updateListenerState(input: ListenerControlInput!): ListenerControlResponse!
}
```

**Input:**

```graphql
input ListenerControlInput {
  runtimeIds: [String!]!
  listenerName: String!
  action: ControlAction!   # START or STOP
}
```

**Example:**

```graphql
mutation {
  updateListenerState(input: {
    runtimeIds: ["runtime-001", "runtime-002"]
    listenerName: "httpListener"
    action: STOP
  }) {
    success
    message
  }
}
```

---

### Logger Control Mutations

#### `updateLogLevel`

Updates the log level for MI loggers (applied immediately via the MI Management API) or BI component loggers (applied via the reconcile engine).

```graphql
mutation {
  updateLogLevel(input: UpdateLogLevelInput!): UpdateLogLevelResponse!
}
```

**Input:**

```graphql
input UpdateLogLevelInput {
  runtimeIds: [String!]!
  componentType: RuntimeType    # MI or BI; inferred from first runtime if omitted
  componentName: String         # required for BI
  loggerName: String            # required for MI
  loggerClass: String           # optional: provide to add a new MI logger
  logLevel: LogLevel!
}
```

**Example — update an MI logger:**

```graphql
mutation {
  updateLogLevel(input: {
    runtimeIds: ["runtime-001"]
    loggerName: "org.apache.synapse"
    logLevel: DEBUG
  }) {
    success
    message
  }
}
```

**Example — add a new MI logger with a class:**

```graphql
mutation {
  updateLogLevel(input: {
    runtimeIds: ["runtime-001"]
    loggerName: "com.example.MyLogger"
    loggerClass: "com.example.MyLogger"
    logLevel: INFO
  }) {
    success
    message
  }
}
```

---

### Secret Mutations

#### `createOrgSecret`

Creates an org-level secret for an environment. If `componentId` is provided, the secret is bound to that component.

```graphql
mutation {
  createOrgSecret(environmentId: String!, componentId: String): String!
}
```

Returns the secret value as a plain string (shown only once).

#### `revokeOrgSecret`

Revokes an org-level secret by its key ID.

```graphql
mutation {
  revokeOrgSecret(keyId: String!): Boolean!
}
```

---

### MI Runtime User Management Mutations

#### `addMIUser`

Creates a new user on an MI runtime.

```graphql
mutation {
  addMIUser(
    componentId: String!
    runtimeId: String!
    username: String!
    password: String!
    isAdmin: Boolean
    domain: String
  ): MIUserOperationResponse!
}
```

#### `deleteMIUser`

Deletes a user from an MI runtime.

```graphql
mutation {
  deleteMIUser(
    componentId: String!
    runtimeId: String!
    username: String!
    domain: String
  ): MIUserOperationResponse!
}
```

---

## Key Types

### `Runtime`

| Field | Type | Description |
|-------|------|-------------|
| `runtimeId` | `String!` | Unique runtime identifier |
| `runtimeName` | `String` | Human-readable name |
| `runtimeType` | `String!` | `"MI"` or `"BI"` |
| `status` | `String!` | Current status (e.g., `RUNNING`, `OFFLINE`) |
| `version` | `String` | Runtime version |
| `component` | `Component!` | The component this runtime belongs to |
| `environment` | `Environment!` | The environment this runtime is deployed in |
| `platformName` | `String` | Host platform name |
| `registrationTime` | `String` | ISO 8601 timestamp when the runtime registered |
| `lastHeartbeat` | `String` | ISO 8601 timestamp of last heartbeat |

### `Environment`

| Field | Type | Description |
|-------|------|-------------|
| `id` | `String!` | Unique environment ID |
| `name` | `String!` | Display name |
| `handler` | `String!` | URL-safe identifier (slug) |
| `description` | `String` | Optional description |
| `isProduction` | `Boolean!` | Whether this is a production environment |
| `critical` | `Boolean` | Same as `isProduction`; controls permission level required |

### `Component`

| Field | Type | Description |
|-------|------|-------------|
| `id` | `String!` | Unique component ID |
| `name` | `String!` | Component name |
| `displayName` | `String` | Human-readable display name |
| `componentType` | `RuntimeType!` | `MI` or `BI` |
| `projectId` | `String!` | Parent project ID |

### `LogLevel` (enum)

`OFF` | `FATAL` | `ERROR` | `WARN` | `INFO` | `DEBUG` | `TRACE`

### `ControlAction` (enum)

`START` | `STOP`

### `RuntimeType` (enum)

`MI` | `BI`

### `ArtifactState` (enum)

Possible values: `active`, `inactive` (rendered as strings in the schema).

---

## Permissions

Operations are protected by role-based access control (RBAC). The key permission levels are:

| Permission | Covers |
|------------|--------|
| `integration_mgt:view` | Reading artifact and runtime data |
| `integration_mgt:edit` | Modifying artifacts, log levels, MI users |
| `integration_mgt:manage` | Full control including deletes and secret management |
| `environment_mgt:manage` | Creating and deleting production environments |
| `environment_mgt:manage_nonprod` | Managing non-production environments |
| `project_mgt:manage` | Creating and deleting projects |
| `project_mgt:edit` | Updating project metadata |

Insufficient permissions return a GraphQL error with the message `Access denied: ...`.

## Error Handling

GraphQL errors are returned in the standard format:

```json
{
  "errors": [
    {
      "message": "Access denied: insufficient permissions to delete runtime",
      "locations": [{ "line": 2, "column": 3 }],
      "path": ["deleteRuntime"]
    }
  ],
  "data": null
}
```

Common error messages:

| Message pattern | Cause |
|-----------------|-------|
| `Access denied: ...` | Insufficient RBAC permission |
| `Runtime not found` | Invalid or deleted `runtimeId` |
| `Integration not found` | Invalid `componentId` |
| `Runtime is not online` | Operation requires `RUNNING` status |
| `Authorization header missing in request` | Missing or malformed `Authorization` header |
| `At least one runtime ID must be provided` | Empty `runtimeIds` list in mutation input |
