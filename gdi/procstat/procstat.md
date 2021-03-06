(procstat)=


# procstat

<meta name="description" content="Documentation on the procstat monitor">


## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the procstat monitor via the Smart Agent Receiver.

This monitor reports metrics about processes.


## Installation

This monitor is available in the SignalFx Smart Agent Receiver, which is part of the {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>`.

To install this integration:

1. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform.

2. Configure the monitor, as described in the next section.


## Configuration

This Splunk Distribution of OpenTelemetry Collector allows embedding a Smart Agent monitor configuration in an associated Smart Agent Receiver instance.

**Note:** Providing a `procstat` monitor entry in your Smart Agent or Collector configuration is required for its use. Use the appropriate form for your agent type.

### Smart Agent

To activate this monitor in the Smart Agent, add the following to your agent configuration:

```
monitors:  # All monitor config goes under this key
 - type: telegraf/procstat
   ...  # Additional config
```

See <a href="https://docs.splunk.com/Observability/gdi/smart-agent/smart-agent-resources.html#configure-the-smart-agent" target="_blank">Smart Agent example configuration</a> for an autogenerated example of a YAML configuration file, with default values where applicable.

### Splunk Distribution of OpenTelemetry Collector

To activate this monitor in the Splunk Distribution of OpenTelemetry Collector, add the following to your agent configuration:

```
receivers:
  smartagent/procstat:
    type: telegraf/procstat
    ...  # Additional config
```

To complete the monitor activation, you must also include the `smartagent/procstat` receiver item in a `metrics` pipeline. To do this, add the receiver item to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file.

See <a href="https://github.com/signalfx/splunk-otel-collector/tree/main/examples" target="_blank">configuration examples</a> for specific use cases that show how the Splunk OpenTelemetry Collector can integrate and complement existing environments.

The following is an example??procstat??Smart Agent monitor configuration:

```
procPath: /proc
monitors:
 - type: telegraf/procstat
   exe: "signalfx-agent*"
```   

### Configuration settings

The following table shows the configuration options for the procstat monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `exe` | no | `string` | The name of an executable to monitor. (ie: `exe: "signalfx-agent*"`) |
| `pattern` | no | `string` | Regular expression pattern to match against. |
| `user` | no | `string` | Username to match against |
| `pidFile` | no | `string` | Path to pid file to monitor. (ie: `pidFile: "/var/run/signalfx-agent.pid"`) |
| `processName` | no | `string` | Used to override the process name dimension |
| `prefix` | no | `string` | Prefix to be added to each dimension |
| `pidTag` | no | `bool` | Whether to add PID as a dimension instead of part of the metric name (**default:** `false`) |
| `cmdLineTag` | no | `bool` | When `true` add the full cmdline as a dimension. (**default:** `false`) |
| `cGroup` | no | `string` | The name of the cgroup to monitor. This cgroup name will be appended to the configured `sysPath`. See the agent config schema for more information about the `sysPath` agent configuration. |
| `WinService` | no | `string` | The name of a windows service to report procstat information on. |

Note that the Smart Agent supports the `native` pid finder only and the `cgroup` and `systemd unit` options are not supported at this time.

On Linux hosts, this monitor relies on the `/proc` filesystem. If the underlying host's `/proc` file system is mounted somewhere other than `/proc`, specify the path using the top-level configuration `procPath`.


## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/telegraf/monitors/procstat/metadata.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
