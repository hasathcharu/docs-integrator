---
sidebar_position: 9
title: Message Filter
description: "Implement the Message Filter pattern with WSO2 Integrator."
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Message Filter

Use the Message Filter pattern to evaluate each incoming message and continue the flow only for messages that satisfy the selected condition.

## Implementation overview

Place the filter where the message first has enough context for the decision. The source can be a service, an event handler, a file or database flow, or a consumer loop. Accepted messages can be forwarded, published, returned, stored, or passed to another function.

Choose the filtering construct based on where the decision belongs:

- Use [if/else statements](/docs/develop/design-logic/control-flow#ifelse-statements) for message-content or message-header checks. Model the accepted path as the first condition, and add `else if` or `else` branches only when other accepted outcomes need handling.
- Use [`match` expressions](/docs/develop/design-logic/control-flow#match-expressions) when the filter key is a discrete value. Handle matching cases and leave the default case empty when unmatched messages must be dropped.
- Use [query expressions](/docs/develop/design-logic/query-expressions) with `where` for collection-level filtering when messages are processed as records in a collection.
- Use listener-level or resource-level filtering when the input artifact can reject or separate messages before custom flow logic runs. For API-driven inputs, [HTTP services](/docs/develop/integration-artifacts/service/http) provide resource matching, guards, binding, and interceptors. Other service and event artifacts provide their own listener or handler selection points.
- Use protocol-native filters when the messaging system can scope delivery before the consumer flow runs. For RabbitMQ, route through [queues](/docs/develop/integration-artifacts/event/rabbitmq#service-configuration) and [exchanges with binding keys](/docs/connectors/catalog/messaging/rabbitmq/actions#exchange-management). For Kafka, use listener [topics and partition assignment strategy](/docs/develop/integration-artifacts/event/kafka#listener-configuration), or consumer [topic-partition assignments](/docs/connectors/catalog/messaging/kafka/actions#subscribe--assign) in manual consumer loops.
- Add manual `if` or `match` guards in consumer flows when the source cannot narrow delivery enough or when the filter depends on payload content.

## Design the integration

### Filter and forward matching messages

This example uses an HTTP resource as the message source and an HTTP call as the accepted path. The resource applies a priority condition before the forward action: only high-priority messages are forwarded, and all other messages complete without a forward action.

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. Create an HTTP service with a `POST` resource that accepts the example message.
2. Add a record type named `Message` with `id`, `source`, `subject`, and `priority` fields.
3. Add constants for the accepted priority values.
4. Because this example forwards accepted messages to an HTTP endpoint, add an HTTP connection named `outboundChannel`.
5. Add an **If** node to the resource flow.
6. Set the **If** condition to `message.priority == HIGH_PRIORITY`.
7. In the matching branch, add an HTTP **Post** operation that sends `message` to `/messages`.
8. Leave the non-matching branch empty so messages that fail the filter are not forwarded.

![Message Filter flow](/img/tutorials/patterns/message-filter-visual-designer.png)

</TabItem>
<TabItem value="code" label="Ballerina Code">

```ballerina
import ballerina/http;

const HIGH_PRIORITY = 1;
const MEDIUM_PRIORITY = 2;
const LOW_PRIORITY = 3;

type Message record {|
    string id;
    string source;
    string subject;
    HIGH_PRIORITY|MEDIUM_PRIORITY|LOW_PRIORITY priority;
|};

final http:Client outboundChannel = check new ("http://api.outbound.channel.com.balmock.io");

service /api/v1 on new http:Listener(8080) {
    resource function post message(Message message) returns error? {
        if message.priority == HIGH_PRIORITY {
            _ = check outboundChannel->/messages.post(message, targetType = http:Response);
        }
    }
}
```

</TabItem>
</Tabs>
