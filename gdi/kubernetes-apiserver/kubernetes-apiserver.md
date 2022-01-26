(kubernetes-apiserver)=

# Kubernetes API server
<meta name="Description" content="Documentation on kubernetes-apiserver monitor">

## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the `kubernetes-apiserver` monitor type by using the SignalFx Smart Agent Receiver.

Use this integration to retrieve metrics from the API server's Prometheus metric endpoint.

This monitor is available on Kubernetes, Linux, and Windows.

## Benefits

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
  smartagent/kubernetes-apiserver:
    type: kubernetes-apiserver
    ... # Additional config
```

To complete the monitor activation, you must also include the monitor in a `metrics` pipeline. To do this, add the monitor to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file. For example:

```
service:
  pipelines:
    metrics:
      receivers: [smartagent/kubernetes-apiserver]
```

See the [kubernetes-yaml examples](https://github.com/signalfx/splunk-otel-collector/tree/main/examples/kubernetes-yaml) in GitHub for the Agent and Gateway YAML files.

### Configuration settings

The following table shows the configuration options for this monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `httpTimeout` | no | `int64` | HTTP timeout duration for both read and writes. This should be a duration string that is accepted by https://golang.org/pkg/time/#ParseDuration (**default:** `10s`) |
| `username` | no | `string` | Basic Auth username to use on each request, if any. |
| `password` | no | `string` | Basic Auth password to use on each request, if any. |
| `useHTTPS` | no | `bool` | If `true`, the agent will connect to the server using HTTPS instead of plain HTTP. (**default:** `false`) |
| `httpHeaders` | no | `map of strings` | A map of HTTP header names to values. Comma separated multiple values for the same message-header is supported. |
| `skipVerify` | no | `bool` | If useHTTPS is `true` and this option is also `true`, the exporter's TLS cert will not be verified. (**default:** `false`) |
| `caCertPath` | no | `string` | Path to the CA cert that has signed the TLS cert, unnecessary if `skipVerify` is set to `false`. |
| `clientCertPath` | no | `string` | Path to the client TLS cert to use for TLS required connections |
| `clientKeyPath` | no | `string` | Path to the client TLS key to use for TLS required connections |
| `host` | **yes** | `string` | Host of the exporter |
| `port` | **yes** | `integer` | Port of the exporter |
| `useServiceAccount` | no | `bool` | Use pod service account to authenticate. (**default:** `false`) |
| `metricPath` | no | `string` | Path to the metrics endpoint on the exporter server, usually `/metrics` (the default). (**default:** `/metrics`) |
| `sendAllMetrics` | no | `bool` | Send all the metrics that come out of the Prometheus exporter without any filtering.  This option has no effect when using the prometheus exporter monitor directly since there is no built-in filtering, only when embedding it in other monitors. (**default:** `false`) |

### Example configuration

The following is an example YAML configuration:

```yaml
receivers:
  smartagent/kubernetes-apiserver:
    type: kubernetes-apiserver
    host: localhost
    port: 443
    extraDimensions:
      metric_source: kubernetes-apiserver
```

The OpenTelemetry Collector has a Kubernetes observer (`k8sobserver`) that can be implemented as an extension to discover networked endpoints, such as a Kubernetes pod. Using this observer assumes that the OpenTelemetry Collector is deployed in Agent mode, where it is running on each individual node or host instance.

To use the observer, you must create a receiver creator instance with an associated rule. For example:

```
extensions:
  # Configures the Kubernetes observer to watch for pod start and stop events.
  k8s_observer:
  host_observer:

receivers:
  receiver_creator/1:
    # Name of the extensions to watch for endpoints to start and stop.
    watch_observers: [k8s_observer]
    receivers:
      smartagent/kubernetes-apiserver:
        rule: type == "pod" && labels["k8s-app"] == "kube-apiserver"
        type: kubernetes-apiserver
        port: 443
        extraDimensions:
          metric_source: kubernetes-apiserver

      prometheus_simple:
        # Configure prometheus scraping if standard prometheus annotations are set on the pod.
        rule: type == "pod" && annotations["prometheus.io/scrape"] == "true"
        config:
          metrics_path: '`"prometheus.io/path" in annotations ? annotations["prometheus.io/path"] : "/metrics"`'
          endpoint: '`endpoint`:`"prometheus.io/port" in annotations ? annotations["prometheus.io/port"] : 9090`'

      redis/1:
        # If this rule matches an instance of this receiver will be started.
        rule: type == "port" && port == 6379
        config:
          # Static receiver-specific config.
          password: secret
          # Dynamic configuration value.
          collection_interval: `pod.annotations["collection_interval"]`
      resource_attributes:
          # Dynamic configuration value.
          service.name: `pod.labels["service_name"]`

      redis/2:
        # Set a resource attribute based on endpoint value.
        rule: type == "port" && port == 6379
        resource_attributes:
          # Dynamic value.
          app: `pod.labels["app"]`
          # Static value.
          source: redis

  receiver_creator/2:
    # Name of the extensions to watch for endpoints to start and stop.
    watch_observers: [host_observer]
    receivers:
      redis/on_host:
        # If this rule matches, an instance of this receiver is started.
        rule: type == "port" && port == 6379 && is_ipv6 == true
        resource_attributes:
          service.name: redis_on_host

processors:
  exampleprocessor:

exporters:
  exampleexporter:

service:
  pipelines:
    metrics:
      receivers: [receiver_creator/1, receiver_creator/2]
      processors: [exampleprocessor]
      exporters: [exampleexporter]
  extensions: [k8s_observer, host_observer]
```

## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/kubernetes/apiserver/metadata.yaml"></div>

## Troubleshooting

```{include} /_includes/bind_address_error_msg.md
```

```{include} /_includes/missing_pipeline_configuration.md
```

```{include} /_includes/out_of_memory_error.md
```

```{include} /_includes/troubleshooting.md
```

