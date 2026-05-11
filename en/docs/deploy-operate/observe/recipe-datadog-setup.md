---
title: Recipe - Datadog Full-Stack Observability
description: Quick setup guide for monitoring WSO2 Integrator with Datadog.
---

# Recipe: Datadog Full-Stack Observability

Complete observability setup with Datadog for unified metrics, logs, and distributed tracing. Minimal configuration with automatic collection and built-in dashboards.

## Architecture

```
WSO2 Integrator
├── Metrics (9797) ──┐
├── Logs (stdout) ──┤──▶ Datadog Agent ──▶ Datadog Cloud
└── Traces (6831) ──┘
```

## Prerequisites

- Datadog account (free or paid)
- Datadog API key
- Kubernetes cluster or Docker/VM

## Step 1: Get Your Datadog API Key

1. Log in to [Datadog](https://app.datadoghq.com)
2. Navigate to **Organization Settings** > **API Keys**
3. Create or copy your API key

## Step 2: Install Datadog Agent

### Option A: Docker Compose

Create `docker-compose.yml`:

```yaml
version: "3.8"

services:
  integrator:
    image: ballerina:latest
    ports:
      - "9090:9090"
      - "9797:9797"
    environment:
      - BALLERINA_OBSERVE_METRICS_ENABLED=true
      - BALLERINAX_PROMETHEUS_PORT=9797
      - BALLERINA_OBSERVE_TRACING_ENABLED=true
      - BALLERINAX_JAEGER_AGENT_HOSTNAME=datadog-agent
      - BALLERINAX_JAEGER_AGENT_PORT=6831
      - BALLERINA_LOG_LEVEL=INFO
    depends_on:
      - datadog-agent

  datadog-agent:
    image: datadog/agent:latest
    environment:
      - DD_API_KEY=${DD_API_KEY}
      - DD_SITE="datadoghq.com"
      - DD_DOGSTATSD_NON_LOCAL_TRAFFIC=true
      - DD_APM_ENABLED=true
      - DD_APM_NON_LOCAL_TRAFFIC=true
      - DD_LOGS_ENABLED=true
      - DD_CONTAINER_EXCLUDE="image:datadog/agent"
    ports:
      - "8126:8126/tcp"   # APM/Traces
      - "8125:8125/udp"   # DogStatsD
      - "5000:5000"       # Listener
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /proc:/host/proc:ro
      - /sys/fs/cgroup/:/host/sys/fs/cgroup:ro
```

Start services:
```bash
export DD_API_KEY=<your-api-key>
docker-compose up -d
```

### Option B: Kubernetes Helm

```bash
helm repo add datadog https://helm.datadoghq.com
helm repo update

helm install datadog-agent datadog/datadog \
  --set datadog.apiKey=${DD_API_KEY} \
  --set datadog.appKey=${DD_APP_KEY} \
  --set datadog.site="datadoghq.com" \
  --set datadog.apm.portEnabled=true \
  --set datadog.logs.enabled=true \
  -n datadog
```

### Option C: Linux/VM Installation

```bash
DD_API_KEY=${DD_API_KEY} DD_SITE="datadoghq.com" \
  bash -c "$(curl -L https://install.datadoghq.com/scripts/install_script_agent7.sh)"
```

## Step 3: Configure Ballerina Integration

Update `Config.toml`:

```toml
[ballerina.observe]
metricsEnabled = true
metricsReporter = "prometheus"
tracingEnabled = true
tracingProvider = "jaeger"

[ballerinax.prometheus]
port = 9797
host = "0.0.0.0"

[ballerinax.jaeger]
agentHostname = "datadog-agent"  # or localhost for local install
agentPort = 6831
samplerType = "const"
samplerParam = 1.0

[ballerina.log]
level = "INFO"
format = "json"  # JSON for better Datadog parsing
```

## Step 4: Configure Datadog Agent for Metrics

Create `/etc/datadog-agent/conf.d/openmetrics.d/conf.yaml`:

```yaml
global_custom_header: "dd-monitoring-enabled: true"

instances:
  - openmetrics_endpoint: "http://localhost:9797/metrics"
    namespace: "ballerina"
    metrics:
      - http_requests_total
      - http_request_duration_seconds
      - http_response_status_total
      - http_requests_in_flight
      - orders_processed_total
      - orders_active
    tags:
      - "service:wso2-integrator"
      - "environment:production"
```

Restart agent:
```bash
sudo systemctl restart datadog-agent
```

## Step 5: Configure Datadog Agent for Logs

Create `/etc/datadog-agent/conf.d/ballerina.d/conf.yaml`:

```yaml
logs:
  - type: file
    path: /var/log/integrator/*.log
    service: wso2-integrator
    source: ballerina
    tags:
      - "environment:production"
      - "version:1.0"

  # For Docker
  - type: docker
    service: wso2-integrator
    source: ballerina
    tags:
      - "environment:production"
```

## Step 6: Configure Datadog Agent for APM/Traces

Edit `/etc/datadog-agent/datadog.yaml`:

```yaml
apm_config:
  enabled: true
  apm_non_local_traffic: true
  receiver_port: 6831
  
logs_enabled: true
```

## Step 7: Verify Data Collection

### Check Metrics

1. Go to **Metrics** > **Summary** in Datadog
2. Search for `ballerina.*`
3. You should see metrics like `ballerina.http_requests_total`

### Check Traces

1. Go to **APM** > **Traces**
2. You should see traces from your integrator

### Check Logs

1. Go to **Logs** > **Live Tail**
2. Filter by `service:wso2-integrator`
3. You should see logs from your integration

## Creating Datadog Dashboards

### Metrics Dashboard

1. Go to **Dashboards** > **New Dashboard**
2. Add widgets with these queries:

**Request Rate:**
```
avg:ballerina.http_requests_total.count{service:wso2-integrator}.as_rate()
```

**Error Rate:**
```
sum:ballerina.http_response_status_total.count{service:wso2-integrator,status_code:5xx}.as_rate() /
sum:ballerina.http_requests_total.count{service:wso2-integrator}.as_rate()
```

**P95 Latency:**
```
p95:ballerina.http_request_duration_seconds{service:wso2-integrator}
```

**Active Requests:**
```
avg:ballerina.http_requests_in_flight{service:wso2-integrator}
```

### Log Analytics Dashboard

1. Create widget with type **Timeseries**
2. Query:
```
avg:trace.http.request.count{service:wso2-integrator} by {http.status_code}
```

3. Add facet widget:
```
Facets: level, module, service
Data: level=ERROR
```

## Creating Monitors (Alerts)

### Monitor 1: High Error Rate

1. Go to **Monitors** > **New Monitor**
2. Type: **Metric**
3. Query:
```
avg(last_5m): sum:ballerina.http_response_status_total.count{service:wso2-integrator,status_code:5xx}.as_rate() / sum:ballerina.http_requests_total.count{service:wso2-integrator}.as_rate() > 0.05
```
4. Alert message:
```
High error rate on {{service.name}}. Error rate: {{value | humanize}}%
@on-call-team
```

### Monitor 2: High Latency

1. Type: **Metric**
2. Query:
```
avg(last_10m): p95:ballerina.http_request_duration_seconds{service:wso2-integrator} > 2
```
3. Alert message:
```
High P95 latency on {{service.name}}: {{value}}s
@on-call-team
```

### Monitor 3: Log Error Alert

1. Type: **Logs**
2. Query:
```
service:wso2-integrator level:ERROR
```
3. Alert condition:
```
The number of logs is > 10 in the last 5 minutes
```

## Using Service Maps

Service Maps automatically show dependencies between your services:

1. Go to **Service Map** in APM
2. You'll see:
   - Your integrator service
   - Downstream services it calls
   - Error rates and latency

## Custom Dashboards Example

```python
# Python script to create dashboard via Datadog API
from datadog import initialize, api

options = {
    'api_key': 'YOUR_API_KEY',
    'app_key': 'YOUR_APP_KEY'
}

initialize(**options)

dashboard = {
    'title': 'WSO2 Integrator Observability',
    'widgets': [
        {
            'definition': {
                'type': 'timeseries',
                'requests': [
                    {
                        'q': 'avg:ballerina.http_requests_total.count{service:wso2-integrator}.as_rate()'
                    }
                ],
                'title': 'Request Rate'
            }
        },
        {
            'definition': {
                'type': 'query_value',
                'requests': [
                    {
                        'q': 'p95:ballerina.http_request_duration_seconds{service:wso2-integrator}'
                    }
                ],
                'title': 'P95 Latency'
            }
        }
    ]
}

api.Dashboard.create(**dashboard)
```

## Troubleshooting

**Agent not connecting:**
```bash
# Check agent status
sudo systemctl status datadog-agent

# View agent logs
tail -f /var/log/datadog/agent.log
```

**Metrics not appearing:**
```bash
# Check metrics endpoint
curl http://localhost:9797/metrics

# Verify agent config
sudo datadog-agent configcheck

# Verify agent can reach metrics
sudo datadog-agent diagnose openmetrics
```

**Traces not appearing:**
```bash
# Check Jaeger is sending to Datadog
sudo datadog-agent diagnose jaeger

# View APM logs
tail -f /var/log/datadog/trace-agent.log
```

**Logs not appearing:**
```bash
# Check logs are enabled
sudo datadog-agent configcheck | grep logs_enabled

# View logs agent status
tail -f /var/log/datadog/agent.log | grep "logs"
```

## Best Practices

- **Tagging:** Add environment, version, and service tags to all metrics
- **Sampling:** Set trace sampling to 10-20% in production to reduce costs
- **Retention:** Configure appropriate log retention based on compliance needs
- **Dashboards:** Create per-team dashboards for ownership
- **Monitors:** Set up alert monitors for SLA compliance

## Cleanup

To remove Datadog monitoring:

```bash
# Docker
docker-compose down

# Kubernetes
helm uninstall datadog-agent

# Linux
sudo apt remove datadog-agent
```

## What's Next

- **[Datadog Integration Details](datadog-integration.md)** – Advanced configuration
- **[New Relic Setup](recipe-datadog-setup.md)** – Alternative platform
- **[Observability Overview](observability-overview.md)** – Full observability options
- **[Third-Party Tools](third-party-overview.md)** – Platform comparison
