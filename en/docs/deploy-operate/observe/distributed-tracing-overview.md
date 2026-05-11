---
sidebar_position: 4
title: Distributed Tracing
description: Configure OpenTelemetry-based distributed tracing for WSO2 Integrator.
---

# Distributed Tracing (OpenTelemetry, Jaeger, Zipkin)

Trace requests as they flow through your WSO2 Integrator services and downstream dependencies to identify bottlenecks and debug failures. Ballerina provides built-in support for distributed tracing using OpenTelemetry, with multiple backend options including Jaeger and Zipkin.

## Overview

Distributed tracing captures the end-to-end journey of a request across multiple services. Each trace consists of spans that represent individual operations. Ballerina provides built-in support for trace propagation using OpenTelemetry, automatically instrumenting HTTP services and clients.

Key benefits:

- **End-to-end visibility** -- See the full request path across services
- **Latency analysis** -- Identify which service or operation is slow
- **Error correlation** -- Connect errors to the specific span where they occurred
- **Dependency mapping** -- Understand service-to-service call patterns

## OpenTelemetry Integration

Ballerina integrates with OpenTelemetry for trace collection and export. Enable tracing in `Config.toml`:

```toml
# Config.toml
[ballerina.observe]
tracingEnabled = true
tracingProvider = "jaeger"

[ballerinax.jaeger]
agentHostname = "localhost"
agentPort = 6831
samplerType = "const"
samplerParam = 1.0
reporterFlushInterval = 1000
reporterBufferSize = 100
```

Add the Jaeger extension to your `Ballerina.toml`:

```toml
# Ballerina.toml
[[dependency]]
org = "ballerinax"
name = "jaeger"
version = "1.0.0"
```

### Automatic Instrumentation

Ballerina automatically creates spans for:

- **HTTP service resource functions** -- A span is created for each incoming request
- **HTTP client calls** -- A span is created for each outbound HTTP request
- **Database operations** -- SQL queries are captured as spans
- **gRPC calls** -- Both server and client spans are generated

No code changes are required for basic tracing. Trace context (W3C Trace Context headers) is propagated automatically across HTTP calls.

## Custom Spans

Add custom spans for application-specific operations:

```ballerina
import ballerina/observe;
import ballerina/http;

service /orders on new http:Listener(9090) {
    resource function post .(@http:Payload json order) returns json|error {
        // Automatic span created for this resource function

        // Create a custom span for a specific operation
        int spanId = check observe:startSpan("validateOrder");
        boolean valid = validateOrder(order);
        check observe:finishSpan(spanId);

        if !valid {
            return error("Invalid order");
        }

        // Create another custom span
        int paymentSpanId = check observe:startSpan("processPayment");
        check processPayment(order);
        check observe:finishSpan(paymentSpanId);

        return { status: "accepted" };
    }
}
```

## Adding Span Tags

Attach metadata to spans for filtering and searching in the tracing UI:

```ballerina
import ballerina/observe;

public function processOrder(json order) returns error? {
    int spanId = check observe:startSpan("processOrder");

    // Add tags to the span
    check observe:addTagToSpan(spanId, "orderId", check order.id);
    check observe:addTagToSpan(spanId, "customerId", check order.customerId);
    check observe:addTagToSpan(spanId, "orderTotal", check order.total.toString());

    // Process the order...

    check observe:finishSpan(spanId);
}
```

## Configuring Trace Exporters

### Jaeger Exporter

For Jaeger, configure the agent or collector endpoint:

```toml
# Config.toml -- Jaeger Agent (UDP)
[ballerinax.jaeger]
agentHostname = "jaeger-agent.observability"
agentPort = 6831

# Config.toml -- Jaeger Collector (HTTP)
[ballerinax.jaeger]
reporterEndpoint = "http://jaeger-collector.observability:14268/api/traces"
```

See **[Jaeger Setup](jaeger-distributed-tracing.md)** for complete Jaeger deployment and configuration.

### Zipkin Exporter

To export traces to Zipkin instead:

```toml
# Config.toml
[ballerina.observe]
tracingEnabled = true
tracingProvider = "zipkin"

[ballerinax.zipkin]
reporterEndpoint = "http://zipkin.observability:9411/api/v2/spans"
```

See **[Zipkin Setup](zipkin-tracing.md)** for Zipkin deployment and configuration.

### OpenTelemetry Collector

For a vendor-neutral approach, export traces to an OpenTelemetry Collector, which can then forward to any backend:

```toml
# Config.toml
[ballerina.observe]
tracingEnabled = true
tracingProvider = "jaeger"

[ballerinax.jaeger]
reporterEndpoint = "http://otel-collector.observability:14268/api/traces"
```

OpenTelemetry Collector configuration:

```yaml
# otel-collector-config.yaml
receivers:
  jaeger:
    protocols:
      thrift_http:
        endpoint: "0.0.0.0:14268"

processors:
  batch:
    timeout: 5s
    send_batch_size: 1024

exporters:
  jaeger:
    endpoint: "jaeger-collector:14250"
    tls:
      insecure: true
  otlp:
    endpoint: "tempo:4317"

service:
  pipelines:
    traces:
      receivers: [jaeger]
      processors: [batch]
      exporters: [jaeger, otlp]
```

## Jaeger Setup and Usage

For production-grade distributed tracing with Jaeger:

1. **Deploy Jaeger** – Run Jaeger as all-in-one container or production deployment
2. **Configure Ballerina** – Point to Jaeger agent or collector
3. **View Traces** – Open Jaeger UI to search and analyze traces
4. **Set Sampling** – Configure sampling strategy for production

See **[Jaeger Setup](jaeger-distributed-tracing.md)** for detailed instructions.

## Zipkin Setup and Usage

For lightweight distributed tracing with Zipkin:

1. **Deploy Zipkin** – Run Zipkin container
2. **Configure Ballerina** – Enable Zipkin tracing provider
3. **View Traces** – Open Zipkin UI to search traces
4. **Configure Sampling** – Set sampling strategy

See **[Zipkin Setup](zipkin-tracing.md)** for detailed instructions.

## Trace Analysis for Debugging

### Finding Slow Requests

In the Jaeger UI:

1. Select your service from the **Service** dropdown
2. Set a minimum duration (e.g., `>1s`) to find slow traces
3. Click **Find Traces** to see matching requests
4. Click a trace to see the span waterfall, showing where time was spent

### Correlating Traces with Logs

Include the trace ID in your log messages by enabling trace context logging:

```ballerina
import ballerina/log;
import ballerina/observe;

public function processOrder(string orderId) returns error? {
    // The trace ID is automatically included in structured logs
    // when both tracing and logging are enabled
    log:printInfo("Processing order", orderId = orderId);
}
```

### Sampling Configuration

In production, sampling reduces overhead. Configure the sampler:

```toml
# Constant sampling: capture all traces (1.0) or none (0.0)
[ballerinax.jaeger]
samplerType = "const"
samplerParam = 1.0

# Probabilistic sampling: capture 10% of traces
[ballerinax.jaeger]
samplerType = "probabilistic"
samplerParam = 0.1

# Rate-limiting sampling: 2 traces per second
[ballerinax.jaeger]
samplerType = "ratelimiting"
samplerParam = 2.0
```

## What's Next

- **[Jaeger Setup](jaeger-distributed-tracing.md)** – Production distributed tracing with Jaeger
- **[Zipkin Setup](zipkin-tracing.md)** – Lightweight tracing with Zipkin
- **[Logging](logging-overview.md)** -- Configure structured logging and correlate with traces
- **[Metrics](metrics-overview.md)** -- Monitor service health with Prometheus
- **[Datadog / New Relic / Splunk](third-party-overview.md)** -- Third-party observability integrations
