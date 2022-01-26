(asp-dot-net)=

# ASP.NET

<meta name="description" content="Documentation on the aspdotnet monitor">


## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the `aspdotnet` monitor via the Smart Agent Receiver.

This monitor reports metrics about requests, errors, sessions, and worker processes for ASP.NET applications. This monitor is only available on Windows.

### Windows Performance Counters

The underlying source for these metrics are Windows Performance Counters. Most of the performance counters in this monitor are gauges that represent rates per second and percentages.

This monitor reports the instantaneous values for these Windows Performance Counters. This means that in between a collection interval, spikes might occur on the Performance Counters. The best way to mitigate this limitation is to increase the reporting interval on this monitor to collect more frequently.


## Installation

This monitor is available in the SignalFx Smart Agent Receiver, which is part of the {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>`.

To install this integration:

1. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform.

2. Configure the monitor, as described in the next section.


## Configuration

This OpenTelemetry Collector allows embedding a Smart Agent monitor configuration in an associated Smart Agent Receiver instance.

**Note:** Providing an ASP.NET monitor entry in your Smart Agent or Collector configuration is required for its use. Use the appropriate form for your agent type.

### Smart Agent

To activate this monitor in the Smart Agent, add the following to your agent configuration:  

```
monitors:  # All monitor config goes under this key
 - type: aspdotnet
   ...  # Additional config
```

See <a href="https://docs.splunk.com/Observability/gdi/smart-agent/smart-agent-resources.html#configure-the-smart-agent" target="_blank">Smart Agent example configuration</a> for an autogenerated example of a YAML configuration file, with default values where applicable.

### Splunk Distribution of OpenTelemetry Collector

To activate this monitor in the OpenTelemetry Collector, add the following to your agent configuration:

```
receivers:
  smartagent/aspdotnet:
    type: aspdotnet
    ...  # Additional config
```

To complete the monitor activation, you must also include the `smartagent/aspdotnet` receiver item in a `metrics` pipeline. To do this, add the receiver item to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file.

See <a href="https://github.com/signalfx/splunk-otel-collector/tree/main/examples" target="_blank">configuration examples</a> for specific use cases that show how the Splunk OpenTelemetry Collector can integrate and complement existing environments.

### Configuration settings

The following table shows the configuration options for this monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `counterRefreshInterval` | no | `int64` | Number of seconds that wildcards in counter paths should be expanded and how often to refresh counters from configuration. (**default:** `60s`) |
| `printValid` | no | `bool` | Print out the configurations that match available performance counters. This is used for debugging. (**default:** `false`) |


## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/dotnet/metadata.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```