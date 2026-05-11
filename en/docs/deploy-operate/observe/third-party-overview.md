---
sidebar_position: 5
title: Third-Party Observability Tools
description: Integrate WSO2 Integrator with Datadog, New Relic, Elastic, and other observability platforms.
---

# Third-Party Observability Tools

WSO2 Integrator integrates with all major observability platforms through Ballerina's built-in observability framework and standard protocols (OpenTelemetry, Prometheus). Choose a platform based on your infrastructure, compliance requirements, and team expertise.

## Supported Platforms

| Platform | Metrics | Logs | Traces | Deployment |
|---|---|---|---|---|
| **Prometheus + Grafana** | Native | Via Loki | Via Tempo | Self-hosted |
| **Jaeger** | -- | -- | Native | Self-hosted |
| **Zipkin** | -- | -- | Native | Self-hosted |
| **Elastic Stack** | Via Metricbeat | Via Filebeat | Via APM | Self-hosted |
| **OpenSearch** | Via collector | Via Fluentd | Via Data Prepper | Self-hosted |
| **Datadog** | Via DogStatsD | Via Agent | Via APM | Managed Cloud |
| **New Relic** | Via OTLP | Via log forwarder | Via OTLP | Managed Cloud |
| **Splunk** | Via OTEL Collector | Via HEC | Via OTEL Collector | Managed Cloud |
| **WSO2 Devant** | Built-in | Built-in | Built-in | Managed Cloud |
| **WSO2 ICP** | Built-in | Built-in | Built-in | Self-hosted |

## Integration Approaches

### 1. Native Ballerina Extensions

Ballerina has built-in support for Jaeger and Prometheus. Enable them in `Config.toml`:

```toml
# Config.toml
[ballerina.observe]
metricsEnabled = true
metricsReporter = "prometheus"
tracingEnabled = true
tracingProvider = "jaeger"

[ballerinax.prometheus]
port = 9797

[ballerinax.jaeger]
agentHostname = "jaeger-agent"
agentPort = 6831
```

This approach requires no additional infrastructure beyond the tracing and metrics backends. See **[Metrics](metrics-overview.md)** and **[Distributed Tracing](distributed-tracing-overview.md)** for detailed configuration.

### 2. OpenTelemetry Collector

Use the OpenTelemetry Collector as a vendor-neutral pipeline to forward telemetry to any backend. This is the recommended approach when integrating with commercial observability platforms.

```yaml
# otel-collector-config.yaml
receivers:
  prometheus:
    config:
      scrape_configs:
        - job_name: "wso2-integrator"
          scrape_interval: 15s
          static_configs:
            - targets: ["integrator-app:9797"]
  jaeger:
    protocols:
      thrift_http:
        endpoint: "0.0.0.0:14268"

processors:
  batch:
    timeout: 5s
    send_batch_size: 1024
  resource:
    attributes:
      - key: service.name
        value: "wso2-integrator-app"
        action: upsert

exporters:
  # Datadog
  datadog:
    api:
      key: "${DD_API_KEY}"
      site: "datadoghq.com"
  # New Relic
  otlp/newrelic:
    endpoint: "otlp.nr-data.net:4317"
    headers:
      api-key: "${NR_API_KEY}"
  # Splunk
  sapm:
    access_token: "${SPLUNK_TOKEN}"
    endpoint: "https://ingest.us1.signalfx.com/v2/trace"

service:
  pipelines:
    traces:
      receivers: [jaeger]
      processors: [batch, resource]
      exporters: [datadog]
    metrics:
      receivers: [prometheus]
      processors: [batch]
      exporters: [datadog]
```

Deploy the collector as a sidecar or as a standalone service in your cluster:

```bash
# Deploy via Helm
helm install otel-collector open-telemetry/opentelemetry-collector \
  -f otel-collector-config.yaml \
  -n observability
```

### 3. Sidecar / Agent Pattern

Deploy vendor-specific agents alongside your integration containers:

- **Datadog Agent** -- Runs as a DaemonSet in Kubernetes, auto-discovers Prometheus endpoints and collects container logs
- **New Relic Infrastructure Agent** -- Collects host-level metrics and forwards logs
- **Elastic APM Agent** -- Java agent attached to the JVM for deep trace instrumentation
- **Splunk OTEL Collector** -- All-in-one agent for metrics, logs, and traces

Example Kubernetes DaemonSet annotation for Datadog auto-discovery:

```yaml
spec:
  template:
    metadata:
      annotations:
        ad.datadoghq.com/app.checks: |
          {
            "openmetrics": {
              "instances": [{
                "prometheus_url": "http://%%host%%:9797/metrics",
                "namespace": "wso2_integrator",
                "metrics": ["*"]
              }]
            }
          }
        ad.datadoghq.com/app.logs: |
          [{
            "source": "ballerina",
            "service": "wso2-integrator-app"
          }]
```

## Choosing a Platform

| Use Case | Recommended Approach |
|---|---|
| Open source, self-hosted | Prometheus + Grafana + Jaeger + ELK/OpenSearch |
| Managed cloud, AWS-centric | CloudWatch or Datadog |
| Managed cloud, multi-cloud | Datadog or New Relic |
| Full-text log search | Elastic Stack or OpenSearch |
| WSO2 managed deployment | Devant observability or ICP |
| Unified metrics, logs, traces | Splunk Observability Cloud |

## Vendor-Specific Guides

### Datadog

Datadog provides a full-stack observability platform with integrated metrics, logs, traces, and APM.

**Quick Start:**

```toml
# Config.toml -- route traces through Datadog Agent
[ballerina.observe]
tracingEnabled = true
tracingProvider = "jaeger"

[ballerinax.jaeger]
reporterEndpoint = "http://datadog-agent:14268/api/traces"
```

**Full Setup:** See **[Datadog Integration](datadog-integration.md)** for detailed configuration including:
- Datadog Agent installation
- Metrics collection via OpenMetrics
- Trace forwarding via Jaeger
- Log collection and tagging
- Dashboard creation and alerting

### New Relic

New Relic is a multi-cloud observability platform with APM, infrastructure monitoring, and log management.

**Quick Start:**

```toml
# Config.toml -- route traces through OTEL Collector to New Relic
[ballerina.observe]
tracingEnabled = true
tracingProvider = "jaeger"

[ballerinax.jaeger]
reporterEndpoint = "http://otel-collector:14268/api/traces"
```

**Full Setup:** See **[New Relic Integration](new-relic-integration.md)** for detailed configuration including:
- Metrics via Prometheus remote write
- Traces via OpenTelemetry Collector
- Logs via New Relic infrastructure agent or Fluent Bit
- NRQL dashboards and alerts

### Moesif API Analytics

Moesif specializes in API analytics, monitoring, and usage-based billing. Use it alongside other observability tools for API-specific insights.

**Setup:** See **[Moesif API Analytics](moesif-api-analytics.md)** for:
- HTTP interceptor configuration
- User and company context tracking
- API usage analytics
- Threshold-based alerts

## What's Next

- **[Datadog Integration](datadog-integration.md)** -- Full-stack observability platform setup
- **[New Relic Integration](new-relic-integration.md)** -- Multi-cloud observability setup
- **[Moesif API Analytics](moesif-api-analytics.md)** -- API-specific monitoring and analytics
- **[Metrics](metrics-overview.md)** -- Configure Prometheus metrics collection
- **[Distributed Tracing](distributed-tracing-overview.md)** -- Configure trace exporters
- **[Logging](logging-overview.md)** -- Configure structured logging
- **[Integration Control Plane](integration-control-plane-icp.md)** -- WSO2-native monitoring dashboard
