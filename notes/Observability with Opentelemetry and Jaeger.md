![](attachments/Pasted%20image%2020240820165515.png)

Trace represents entire journey of a request within a system.

![](attachments/Pasted%20image%2020240820165704.png)

Trace ID as a header with the request
Span ID is different for each operation

## Standard for Trace Context

![](attachments/Pasted%20image%2020240820165834.png)

Monitoring shows that there is a problem.
Tracing shows where the problem is.
Observability is the ability of the system to provide insights into its internal states of the system based on the outputs. An observable system emits data that lets precisely understand state of the system when an error occurs.

![](attachments/Pasted%20image%2020240820170440.png)

![](attachments/Pasted%20image%2020240820170505.png)

## Opentelemetry Architecture

![](attachments/Pasted%20image%2020240820171050.png)

## Jaeger

![](attachments/Pasted%20image%2020240820175228.png)

## Sampling

[Opentelemetry sampling](https://opentelemetry.io/docs/concepts/sampling/)

If a trace is kept - it is sampled.
If a trace is discarded - it is unsampled.

There are to ways for sampling.
1. Head-based sampling
2. Tail-based sampling

## Collector

[Opentelemetry collector](https://opentelemetry.io/docs/collector/)

![](attachments/Pasted%20image%2020240822085323.png)



![](attachments/Pasted%20image%2020240824141046.png)

## Grafana

Observability open-source solution.

Comparison with Jaeger.
- Jaeger supports only traces
- Grafana supports metrics and logs as well

![](attachments/Pasted%20image%2020240824142101.png)

Sample configuration for the OpenTelemetry Collector to send data to Grafana + debugging logs:

```yaml

extensions:
  basicauth/grafana_cloud:
    # https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/extension/basicauthextension
    client_auth:
      username: "username"
      password: "base64 password"
  health_check:
  pprof:
  zpages:
    endpoint: 0.0.0.0:55679

receivers:
  otlp:
    # https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver/otlpreceiver
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318
  hostmetrics:
    # Optional. Host Metrics Receiver added as an example of Infra Monitoring capabilities of the OpenTelemetry Collector
    # https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/hostmetricsreceiver
    scrapers:
      load:
      memory:

processors:
  batch:
    # https://github.com/open-telemetry/opentelemetry-collector/tree/main/processor/batchprocessor
  resourcedetection:
    # Enriches telemetry data with resource information from the host
    # https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor/resourcedetectionprocessor
    detectors: ["env", "system"]
    override: false
  transform/add_resource_attributes_as_metric_attributes:
    # https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor/transformprocessor
    error_mode: ignore
    metric_statements:
      - context: datapoint
        statements:
          - set(attributes["deployment.environment"], resource.attributes["deployment.environment"])
          - set(attributes["service.version"], resource.attributes["service.version"])

exporters:
  debug:
    verbosity: detailed
    sampling_initial: 1
    sampling_thereafter: 5
  otlphttp/grafana_cloud:
    # https://github.com/open-telemetry/opentelemetry-collector/tree/main/exporter/otlpexporter
    endpoint: "https://otlp-gateway-prod-ap-southeast-1.grafana.net/otlp"
    auth:
      authenticator: basicauth/grafana_cloud


service:
  extensions: [basicauth/grafana_cloud, health_check, pprof, zpages]
  pipelines:
    traces:
      receivers: [otlp]
      processors: [resourcedetection, batch]
      exporters: [otlphttp/grafana_cloud, debug]
    metrics:
      receivers: [otlp, hostmetrics]
      processors: [resourcedetection, transform/add_resource_attributes_as_metric_attributes, batch]
      exporters: [otlphttp/grafana_cloud]
    logs:
      receivers: [otlp]
      processors: [resourcedetection, batch]
      exporters: [otlphttp/grafana_cloud]

```

## Metrics

![](attachments/Pasted%20image%2020240824165026.png)

![](attachments/Pasted%20image%2020240824165727.png)

![](attachments/Pasted%20image%2020240824175122.png)

## Logs

![](attachments/Pasted%20image%2020240824182841.png)





