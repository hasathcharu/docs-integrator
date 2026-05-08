---
sidebar_position: 2
title: "Build a Sentiment Analyzer"
description: Build your first AI integration — an HTTP service that uses a direct LLM call to classify customer reviews.
keywords: [wso2 integrator, ai, llm, direct llm call, sentiment analysis, quick start, ballerina ai]
---

import ThemedImage from '@theme/ThemedImage';
import useBaseUrl from '@docusaurus/useBaseUrl';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Build a Sentiment Analyzer

**Time:** Under 10 minutes | **What you'll build:** An HTTP service that listens on `POST /analyze`, sends a customer review to an LLM, and returns the sentiment as `POSITIVE`, `NEGATIVE`, or `NEUTRAL`.

A direct LLM call is the simplest way to use AI in an integration: you send a prompt, the model returns a value, and Ballerina enforces the return type. This quick start shows the full cycle: define the result type, configure a model provider, make the call, and test it.

:::info Prerequisites

- [WSO2 Integrator set up for AI](setting-up-ai.md)
- A project to work in. If you do not have one, see [Create a new integration](../../develop/create-integrations/create-a-new-integration.md).
:::

<Tabs>
<TabItem value="ui" label="Visual Designer" default>

## Step 1: Add an HTTP service

1. Select your integration from the project panel.
2. In the Design canvas, select **Add Artifact**.
3. Select **HTTP Service** under **Integration as API**.
4. Keep **Service Contract** as **Design From Scratch**.
5. Set **Service Base Path** to `/`.
6. Select **Create**.

<ThemedImage
    alt="Empty HTTP Service view with base path / and the default listener"
    sources={{
        light: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/create-http-service.png'),
        dark: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/create-http-service.png'),
    }}
/>

## Step 2: Define the Sentiment type

The LLM call returns one of three values. Define an enum so Ballerina can enforce the result.

1. In Project Explorer, click the **+** next to **Types**.
2. Set the kind to **Enum** and **Type name** to `Sentiment`.
3. Add three members: `POSITIVE`, `NEGATIVE`, `NEUTRAL`.
4. Select **Save**.

<ThemedImage
    alt="Sentiment enum with POSITIVE, NEGATIVE, and NEUTRAL members"
    sources={{
        light: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/create-sentiment-type.png'),
        dark: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/create-sentiment-type.png'),
    }}
/>

## Step 3: Add a POST resource

1. In the HTTP Service Design editor, select **+ Add Resource**.
2. Select **POST**.
3. Set **Resource Path** to `analyze`.
4. Select **+ Define Payload**, set the name to `AnalyzePayload`, and add a single field `text` of type `string`.
5. Select **Save** on the payload panel, then **Save** on the resource.

<ThemedImage
    alt="POST analyze resource configured with the AnalyzePayload request body"
    sources={{
        light: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/add-post-resource.png'),
        dark: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/add-post-resource.png'),
    }}
/>

## Step 4: Add a direct LLM call

1. Select **+** inside the resource flow.
2. Select **Model Provider** under **Direct LLM**.
3. Select **+ Add Model Provider**, then choose **WSO2 Model Provider**.
4. Keep the default name and select **Save**. The provider is added under **Connections**.
5. Select the **Generate** action.
6. Set **Prompt** to `Classify the sentiment of this customer review as POSITIVE, NEGATIVE, or NEUTRAL. Review: ${payload.text}`.
7. Set the result variable name to `td`.
8. Set **Expected Type** to `Sentiment`.
9. Select **Save**.

<ThemedImage
    alt="Configuring the Generate action with the classification prompt and Sentiment as the expected return type"
    sources={{
        light: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/add-prompt-and-return-type.png'),
        dark: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/add-prompt-and-return-type.png'),
    }}
/>

## Step 5: Return the result

1. Select **+** below the **ai:generate** node.
2. Select **Return**.
3. Set **Expression** to `td`.
4. Select **Save**.

<ThemedImage
    alt="Final flow with Start, ai:generate, Return, and the WSO2 model provider connection"
    sources={{
        light: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/final-view.png'),
        dark: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/final-view.png'),
    }}
/>

## Step 6: Run and test

1. Select **Run**.
2. Select **Try It** in the confirmation dialog.
3. Send a `POST` to `/analyze` with the body `{"text": "Absolutely loved this product! Best purchase I made all year."}`.
4. Confirm the response is `"POSITIVE"`.

<ThemedImage
    alt="Running the integration and testing it with the Try It panel showing a POSITIVE sentiment response"
    sources={{
        light: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/run-and-test.png'),
        dark: useBaseUrl('/img/genai/getting-started/build-a-sentiment-analyzer/run-and-test.png'),
    }}
/>

</TabItem>
<TabItem value="code" label="Ballerina Code">

The following Ballerina program produces the same integration shown in the Visual Designer steps. The code wraps the result in a `SentimentResponse` record so the response is `{"sentiment":"POSITIVE"}` instead of a bare JSON string.

`types.bal`:

```ballerina
public enum Sentiment {
    POSITIVE,
    NEGATIVE,
    NEUTRAL
}

public type ReviewRequest record {|
    string text;
|};

public type SentimentResponse record {|
    Sentiment sentiment;
|};
```

`connections.bal`:

```ballerina
import ballerina/ai;

final ai:Wso2ModelProvider aiWso2modelprovider = check ai:getDefaultModelProvider();
```

`main.bal`:

```ballerina
import ballerina/http;

listener http:Listener httpDefaultListener = http:getDefaultListener();

service / on httpDefaultListener {
    resource function post analyze(ReviewRequest payload) returns SentimentResponse|error {
        Sentiment sentiment = check aiWso2modelprovider->generate(
            `Classify the sentiment of this customer review as POSITIVE, NEGATIVE, or NEUTRAL.
            Review: ${payload.text}`
        );
        return {sentiment};
    }
}
```

Run and test the integration from WSO2 Integrator using the **Try It** panel as shown in Step 6. The response is `{"sentiment":"POSITIVE"}`.

</TabItem>
</Tabs>

## How it works

The model provider's `generate` method takes a backtick template prompt and an expected return type. Behind the scenes the LLM is instructed to produce output that conforms to that type, and the response is parsed and validated before being returned to your code.

Because the return type is an enum, the LLM cannot return free-form text — it must pick one of `POSITIVE`, `NEGATIVE`, or `NEUTRAL`. If the model returns anything else, the call fails with a typed error rather than silently passing bad data downstream.

This is what differentiates a direct LLM call from a raw chat completion: you write the call as if it were a normal function and let the type system do the work.

## What's next

- [Build a Sample Hotel Booking Agent](build-a-sample-hotel-booking-agent.md) — Add tools and memory for a multi-turn agent
- [What is an AI Agent?](/docs/genai/key-concepts/what-is-ai-agent) — Understand the agent architecture
- [What are Tools?](/docs/genai/key-concepts/what-are-tools) — Learn tool design patterns
