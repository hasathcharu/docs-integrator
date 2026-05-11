---
sidebar_position: 9
title: Message Filter
description: "Implement the Message Filter pattern with WSO2 Integrator."
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Message Filter

Use the Message Filter pattern to evaluate each incoming message and continue the flow only for messages that satisfy the selected condition. <a href="https://www.enterpriseintegrationpatterns.com/patterns/messaging/Filter.html" target="_blank" rel="noopener noreferrer" aria-label="Enterprise Integration Patterns Message Filter reference" title="EIP Reference"><svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" style={{ verticalAlign: '-2px' }}><path d="M15 3h6v6" /><path d="M10 14 21 3" /><path d="M21 14v5a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h5" /></svg></a>

In WSO2 Integrator, this maps to Ballerina constructs that decide whether a message should continue: conditional control flow for one-message predicates, query expressions for collections, listener or resource selection at the boundary, and connector or broker settings when the source can narrow delivery before the flow runs.

## Predicate-based filtering

Use predicate-based filtering with [if/else statements](/docs/develop/design-logic/control-flow#ifelse-statements) when each message carries the fields needed for a single boolean decision, such as priority, source, header, or status. The accepted path contains the forwarding or processing action; the rejected path does nothing or handles the rejection separately.

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. Open the flow and [add a step](/docs/develop/design-logic/visual-flow-designer#adding-steps-to-the-flow).
2. Add an [If node](/docs/develop/design-logic/control-flow#ifelse-statements) at the point where the message has enough data for the decision.
3. Set the condition to `message.priority == HIGH_PRIORITY`.
4. Add the accepted action inside the matching branch.
5. Leave the other branch empty when unmatched messages should be discarded.

<img
  src="/img/tutorials/patterns/message-filter-visual-designer.png"
  alt="Message Filter flow"
  width="560"
  style={{ display: 'block', margin: '0 auto' }}
/>

</TabItem>
<TabItem value="code" label="Ballerina Code">

```ballerina
// docs-fold-start: Supporting definitions
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
// docs-fold-end

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

## Collection-level filtering

Use collection-level filtering with [query expressions](/docs/develop/design-logic/query-expressions) when the flow already has a group of messages or records and only a subset should continue. Keep the predicate in the `where` clause so the result is the accepted collection.

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. Open the flow and [add a step](/docs/develop/design-logic/visual-flow-designer#adding-steps-to-the-flow).
2. Add a [Map Data or Declare Variable step](/docs/develop/design-logic/query-expressions#using-query-expressions-in-the-visual-designer).
3. Set the output type to the accepted collection type, such as `Message[]`.
4. Enter a query expression with a `where` clause for the filter predicate.
5. Use the resulting collection in the next processing or forwarding step.

</TabItem>
<TabItem value="code" label="Ballerina Code">

```ballerina
Message[] highPriorityMessages = from Message message in messages
    where message.priority == HIGH_PRIORITY
    select message;
```

</TabItem>
</Tabs>

## Boundary-level filtering

Use boundary-level filtering when the input artifact can reject or route messages before custom flow logic runs. For HTTP-facing inputs, use a [request interceptor](/docs/connectors/catalog/built-in/http/trigger-reference#interceptors) when the decision can be made from request metadata before the resource executes; other inputs can use their own handler, listener, or subscription selection points.

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. Add the source artifact, such as an [HTTP service](/docs/develop/integration-artifacts/service/http#creating-an-http-service).
2. Add a request interceptor for the service boundary.
3. Read the request metadata needed for the filter, such as a priority header.
4. Return a response for messages that should stop at the boundary.
5. Call the next service only for messages that should enter the resource flow.

</TabItem>
<TabItem value="code" label="Ballerina Code">

```ballerina
import ballerina/http;

listener http:Listener eventListener = new (8080,
    interceptors = [new HighPriorityFilter()]
);

service class HighPriorityFilter {
    *http:RequestInterceptor;

    resource function 'default [string... path](
            http:RequestContext ctx, http:Request req)
            returns http:NextService|http:Accepted|error? {
        if !req.hasHeader("x-priority") {
            return <http:Accepted>{body: {status: "filtered"}};
        }

        string priority = check req.getHeader("x-priority");
        if priority != "high" {
            return <http:Accepted>{body: {status: "filtered"}};
        }

        return ctx.next();
    }
}

service /events on eventListener {
    resource function post orders(OrderCreated event) returns error? {
        check processOrder(event);
    }
}
```

</TabItem>
</Tabs>

## Broker-side delivery filtering

Use broker-side delivery filtering when the messaging system can reduce what reaches the flow before consumption. RabbitMQ can narrow delivery through queues, exchanges, and binding keys; Kafka can narrow delivery through subscribed topics, partition assignment, or explicit topic-partition assignments. See the generic [Connections](/docs/develop/integration-artifacts/supporting/connections) page for connector creation, and the connector-specific configuration for [RabbitMQ listener queues](/docs/connectors/catalog/messaging/rabbitmq/triggers#service), [RabbitMQ binding keys](/docs/connectors/catalog/messaging/rabbitmq/actions#exchange-management), [Kafka listener topics](/docs/connectors/catalog/messaging/kafka/triggers#configuration), and [Kafka topic-partition assignments](/docs/connectors/catalog/messaging/kafka/actions#subscribe--assign).

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

1. Add the broker source from the event integration guides: [RabbitMQ](/docs/develop/integration-artifacts/event/rabbitmq#creating-a-rabbitmq-service) or [Kafka](/docs/develop/integration-artifacts/event/kafka#creating-a-kafka-consumer).
2. Configure the [connection](/docs/develop/integration-artifacts/supporting/connections) for the broker.
3. For RabbitMQ, set the queue in the [service configuration](/docs/develop/integration-artifacts/event/rabbitmq#service-configuration). Use exchange and binding-key setup when the broker should route only matching messages.
4. For Kafka, set the subscribed topics in the [listener configuration](/docs/develop/integration-artifacts/event/kafka#listener-configuration). Use explicit partition assignment when only selected partitions should be consumed.
5. Add processing steps only for the messages delivered by that broker-side filter.

</TabItem>
<TabItem value="code" label="Ballerina Code">

```ballerina
@rabbitmq:ServiceConfig {
    queueName: "high-priority-orders"
}
service rabbitmq:Service on rabbitmqListener {
    remote function onMessage(rabbitmq:AnydataMessage message) returns error? {
        check processOrder(message);
    }
}

listener kafka:Listener kafkaListener = new (kafka:DEFAULT_URL, {
    groupId: "orders",
    topics: ["high-priority-orders"]
});
```

</TabItem>
</Tabs>
