(ntp)=

# NTP receiver
<meta name="Description" content="Documentation on the ntp integration for Splunk Observability Cloud.">

## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the `ntp` monitor type by using the SignalFx Smart Agent Receiver.

Use the NTP receiver to retrieve clock offset from an NTP server. The receiver enforces a minimum interval of 30 minutes.

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
  smartagent/ntp:
    type: ntp
    ... # Additional config
```

To complete the receiver activation, you must also include the receiver in a `metrics` pipeline. To do this, add the receiver to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file. For example:

```
service:
  pipelines:
    metrics:
      receivers: [smartagent/ntp]
```

### Configuration settings

The following table shows the configuration options for the ntp receiver:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `host` | **yes** | `string` | The host or IP address of the NTP server. For example, `pool.ntp.org`. |
| `port` | no | `integer` | The port of the NTP server. Default is `123`. |
| `version` | no | `integer` | NTP protocol version. Default is `4`. |
| `timeout` | no | `int64` | Timeout in seconds for the request. Default is `5s`. |


## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/ntp/metadata.yaml"></div>

### Non-default metrics (version 4.7.0+)

To emit metrics that are not _default_, you can add those metrics in the
generic receiver-level `extraMetrics` config option. You don't need to add
to `extraMetrics` any metric derived from specific options that don't appear 
in the list of metrics.

To see a list of metrics that will be emitted you can run `agent-status
monitors` after configuring this receiver in a running agent instance.

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
