---
title: Normalizer
description: "Implement the Normalizer pattern with WSO2 Integrator."
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Normalizer

Use the Normalizer pattern to convert different accepted source message formats into one canonical payload before shared processing or downstream delivery.

## Implementation overview

In WSO2 Integrator, implement the Normalizer at the message boundary. The integration receives one of several accepted source representations, identifies the representation, extracts equivalent values, builds a canonical payload, and passes only that canonical payload to shared processing or downstream calls.

- Start the flow from the integration artifact that matches the source channel. For API-driven flows, use an [HTTP service](/docs/develop/integration-artifacts/service/http).
- Use [control flow](/docs/develop/design-logic/control-flow) in the **Visual Designer** to route each accepted representation to the matching extraction logic.
- Use the [data mapper](/docs/develop/integration-artifacts/supporting/data-mapper/) when the canonical payload is easier to maintain visually. For flexible payload structures, use [generic type mappings](/docs/develop/integration-artifacts/supporting/data-mapper/generic-type-mappings).
- Keep source-specific parsing, validation, and field extraction in the branch that handles that representation, and keep shared processing after the canonical payload is built.
- Keep endpoint URLs, credentials, and environment-specific values in [configurable variables](/docs/develop/integration-artifacts/supporting/configurations), and use [connections](/docs/develop/integration-artifacts/supporting/connections) for reusable downstream clients.

## Design the integration

<Tabs>
<TabItem value="code" label="Ballerina Code" default>

```ballerina
import ballerina/http;

configurable string targetApiUrl = ?;

final http:Client targetClient = check new (targetApiUrl);

service /api/v1 on new http:Listener(8080) {

    resource function post message(@http:Payload json|xml request) returns json|error {
        json normalizedMessage;

        if request is json {
            string title = check request.title;
            string body = check request.body;
            normalizedMessage = normalize(title, body);
        } else {
            string title = (request/<title>).data();
            string body = (request/<body>).data();
            normalizedMessage = normalize(title, body);
        }

        json response = check targetClient->/canonical-messages.post(normalizedMessage);
        return response;
    }
}

function normalize(string title, string body) returns json {
    return {
        message: {
            title,
            body
        }
    };
}
```

</TabItem>
<TabItem value="ui" label="Visual Designer">

1. In the integration, select **Add Artifact**.
2. Select **HTTP Service** under **Integration as API**.
3. Set **Service Base Path** to `/api/v1`.
4. Use a listener for the required port, or keep the shared listener if the default port is acceptable.
5. In the **Service Designer**, add a **POST** resource named `message`.
6. Set the payload parameter type to `json|xml` so the same resource can receive both supported source formats.
7. Open the `message` resource in the flow designer.
8. Add an **If** step.
9. Set the condition to `request is json`.
10. In the **True** branch, add **Variable** steps to extract the common fields from the JSON payload:
   - `title` with `check request.title`
   - `body` with `check request.body`
11. In the **False** branch, add **Variable** steps to extract the same fields from the XML payload:
   - `title` with `(request/<title>).data()`
   - `body` with `(request/<body>).data()`
12. In each branch, set `normalizedMessage` by calling the normalization function with the extracted values.
13. Select **Functions** in the project explorer.
14. Add a function named `normalize`.
15. Add `title` and `body` as `string` inputs.
16. Set the return type to `json`.
17. Build the canonical output with the data mapper or expression editor:
   - Map `title` to `message.title`.
   - Map `body` to `message.body`.
18. Return the canonical message from the function.
19. Add a configurable variable named `targetApiUrl` for the downstream base URL.
20. Add an HTTP connection that uses `targetApiUrl`.
21. In the `message` resource, add the HTTP **post** operation after the normalization branch.
22. Set the operation path to `/canonical-messages`.
23. Set the request payload to `normalizedMessage`.
24. Return the downstream response from the resource.

</TabItem>
</Tabs>
