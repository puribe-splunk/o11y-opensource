(nagios)=

# Nagios

<meta name="Description" content="Documentation on nagios monitor">

## Description

The Splunk Distribution of OpenTelemetry Collector provides this integration as a wrapper to run existing Nagios status check scripts through the Splunk Distribution of OpenTelemetry Collector, which acts as the Nagios Remote Plugin Executor (NRPE) or the Simple Network Management Protocol (SNMP) exec directive.

Use this integration to run the script set in the command parameter and send the [state](https://nagios-plugins.org/doc/guidelines.html#AEN78) of the check, depending on the exit code of the command. This integration is similar to the [telegraf/exec monitor configured with dataFormat:nagios integration](https://docs.splunk.com/Observability/gdi/exec/telegraf-exec.html), with the following exceptions:

- Does not retrieve perfdata metrics. This integration only retrieves the state of the script for alerting purposes.
- Overrides the state if the exit code `== 0`, but the output string starts with `warn`, `crit`, or `unkn` (not case-sensitive).

This monitor is available on Kubernetes, Linux, and Windows.

### Benefits

This integration adds more context to the status check state by using [events](https://docs.splunk.com/Observability/alerts-detectors-notifications/view-data-events.html#events-intro). In addition to the `state` metric, this integration sends an event that includes the output and the error caught from the command execution.

Using this integration should make troubleshooting more efficient and let you to remain in Observability Cloud without connecting to your Linux or Windows machine in case of an abnormal state to understand what is happening. Using this integration also lets you create a dashboard that is familiar to Nagios users.

**Note:** The last sent event is cached into memory and compared to new events to avoid repeatedly sending the same event for each collection interval. Restarting the OTel Collector erases its cache, so the  most recently sent event is sent again upon restart. If your check always “normally” produces a different output for each run, for example, the uptime check, you can use the `FilterStdOut: true` parameter to ignore it in comparison.
{: .note}

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
  smartagent/nagios:
    type: nagios
    ... # Additional config
```

To complete the integration, include the monitor in a metrics pipeline. To do this, add the monitor to the `service > pipelines > metrics > receivers` section of your configuration file. For example:

```
service:
  pipelines:
    metrics:
      receivers: [smartagent/nagios]
```

### Event-sending functionality

```{include} /_includes/event-sending-functionality.md
```

### Configuration settings

The following table shows the configuration options for this monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `command` | **yes** | `string` | The command to exec with any arguments like: `"LC_ALL=\"en_US.utf8\" /usr/lib/nagios/plugins/check_ntp_time -H pool.ntp.typhon.net -w 0.5 -c 1"` |
| `service` | **yes** | `string` | Corresponds to the nagios `service` column and allows to aggregate all instances of the same service (when calling the same check script with different arguments) |
| `timeout` | no | `integer` | The max execution time allowed in seconds before sending SIGKILL (**default:** `9`) |
| `ignoreStdOut` | no | `bool` | If `false` and change is detected on `stdout` compared to the last event it will send a new one (**default:** `false`) |
| `ignoreStdErr` | no | `bool` | If `false` and change is detected on `stderr` compared to the last event it will send a new one (**default:** `false`) |

## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/nagios/metadata.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
