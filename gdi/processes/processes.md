(processes)=

# Host process

<meta name="description" content="Documentation on the processes monitor">


## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the host process monitor via the Smart Agent Receiver.

This monitor gathers information about processes running on the host.


## Installation

This monitor is available in the SignalFx Smart Agent Receiver, which is part of the {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>`.

To install this integration:

1. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform.

2. Configure the monitor, as described in the next section.


## Configuration

The Splunk Distribution of OpenTelemetry Collector allows embedding a Smart Agent monitor configuration in an associated Smart Agent Receiver instance.

**Note:** Providing a host process monitor entry in your Smart Agent or Collector configuration is required for its use. Use the appropriate form for your agent type.

### Smart Agent

To activate this monitor in the Smart Agent, add the following to your agent configuration:

```
monitors:  # All monitor config goes under this key
  - type: collectd/processes
    ...  # Additional config
```

See <a href="https://docs.splunk.com/Observability/gdi/smart-agent/smart-agent-resources.html#configure-the-smart-agent" target="_blank">Smart Agent example configuration</a> for an autogenerated example of a YAML configuration file, with default values where applicable.

### Splunk Distribution of OpenTelemetry Collector

If you are using the Splunk Distribution of OpenTelemetry Collector and want to collect metrics about processes running on a host, use the {ref}`host-metrics-receiver`.

### Configuration settings

The following table shows the configuration options for the host process monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `processes` | no | `list of strings` | A list of process names to match |
| `processMatch` | no | `map of strings` | A map with keys specifying the `plugin_instance` value to send for regex values that match process names. See the example configuration. |
| `collectContextSwitch` | no | `bool` | Collects metrics on the number of context switches made by the process (**default:** `false`) |
| `procFSPath` | no | `string` | (Deprecated) Set the agent configuration `procPath` instead of this monitor configuration option. This option is useful for overriding the path to the `proc` filesystem if the agent is running in a container. |


## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/collectd/processes/metadata.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
