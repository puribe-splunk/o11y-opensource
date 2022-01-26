(prometheus-nginx-ingress)=

# Prometheus NGINX Ingress
<meta name="Description" content="Documentation on the prometheus-nginx-ingress integration for Splunk Observability Cloud.">

## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the `prometheus-nginx-ingress` monitor type by using the SignalFx Smart Agent Receiver.

This monitor wraps the {ref}`prometheus-exporter` to scrape Ingress NGINX metrics for Splunk Observability Cloud.

The `prometheus-nginx-ingress` monitor relies on the Prometheus metric implementation that replaces VTS. If you use NGINX 0.15 or lower, use the [Prometheus NGINX VTS](../prometheus-nginx-vts/prometheus-nginx-vts.md) monitor instead.

This receiver is available on Linux and Windows.

### Benefits

```{include} /_includes/benefits.md
```

## Installation

```{include} /_includes/collector-installation.md
```

## Configuration

```{include} /_includes/configuration.md
```

```
receivers:
  smartagent/prometheus-nginx-ingress:
    type: prometheus-nginx-ingress
    ... # Additional config
```

To complete the receiver activation, you must also include the receiver in a `metrics` pipeline. To do this, add the receiver to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file. For example:

```
service:
  pipelines:
    metrics:
      receivers: [smartagent/prometheus-nginx-ingress]
```

### Ingress NGINX configuration

Enable the `controller.stats.enabled=true` and `controller.metrics.enabled=true` 
flags in the NGINX Ingress Controller chart.

### Agent configuration

Use the following configuration for service discovery:

```yaml
monitors:
- type: prometheus/nginx-ingress
  discoveryRule: container_image =~ "nginx-ingress-controller" && port == 10254
  port: 10254
```

### Configuration settings

The following table shows the configuration options for the `prometheus-nginx-ingress` monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `httpTimeout` | no | `int64` | HTTP timeout duration for both reads and writes. Must be a duration string accepted by https://golang.org/pkg/time/#ParseDuration. Default value is `10s`. |
| `username` | no | `string` | User name to use on each request. |
| `password` | no | `string` | Password to use on each request. |
| `useHTTPS` | no | `bool` | If true, the agent connects to the server using HTTPS instead of plain HTTP. Default value is `false`. |
| `httpHeaders` | no | `map of strings` | A map of HTTP header names to values. Comma-separated multiple values for the same message-header are supported. |
| `skipVerify` | no | `bool` | If both `useHTTPS` and `skipVerify` are `true`, the TLS certificate of the exporter is not verified. Default value is `false`. |
| `caCertPath` | no | `string` | Path to the CA certificate that has signed the TLS certificate, unnecessary if `skipVerify` is set to false. |
| `clientCertPath` | no | `string` | Path to the client TLS certificate to use for TLS required connections. |
| `clientKeyPath` | no | `string` | Path to the client TLS key to use for TLS required connections. |
| `host` | **yes** | `string` | Host of the exporter. |
| `port` | **yes** | `integer` | Port of the exporter. |
| `useServiceAccount` | no | `bool` | Use pod service account to authenticate. Default value is `false`. |
| `metricPath` | no | `string` | Path to the metrics endpoint on the exporter server. The default value is `/metrics`. |
| `sendAllMetrics` | no | `bool` | Send all the metrics that come out of the Prometheus exporter without any filtering. This option has no effect when using the Prometheus exporter monitor directly, since there is no built-in filtering. Default value is `false`. |

## Metrics

The following metrics are available for this integration.

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/prometheus/nginxingress/metadata.yaml"></div>

### Non-default metrics (version 4.7.0+)

To emit metrics that are not default, you can add those metrics in the
generic receiver-level `extraMetrics` config option. You don't need to add to
`extraMetrics` any metric derived from configuration options that don't appear
 in the list of metrics.

To see a list of metrics that will be emitted you can run `agent-status
monitors` after configuring the receiver in a running agent instance.

## Troubleshooting

```{include} /_includes/troubleshooting.md
```

