(processlist)=

# Host process list

<meta name="description" content="Documentation on the processlist monitor">

## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the `processlist` monitor type by using the SignalFx Smart Agent Receiver.

This monitor reports the running processes for a host, analogous to how the `top` or `ps` command on Unix/Linux systems works. The output format is a special base64-encoded event that gets processed by our backend and displayed under the Infrastructure view for a specific host. Historical process information is not retained on the backend, only the most recent version.

This monitor is available on Linux and Windows.

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
  smartagent/processlist:
    type: processlist
    ...  # Additional config
```

To complete the monitor activation, include this monitor in a `logs` pipeline with the following options:

* `signalfx` exporter. The `signalfx` exporter can be used to send metrics, events, and trace correlation to Infrastructure Monitoring.
* `resourcedetection` processor. The `resourcedetection` processor can be used to detect resource information from the host, in a format that conforms to the OpenTelemetry resource semantic conventions, and append or override the resource value in telemetry data with this information. 

For example:

```
service:
  pipelines:
    logs:
      receivers: [smartagent/processlist]
      exporters: [signalfx]
      processors: [resourcedetection]
```

## Metrics

The Splunk Distribution of OpenTelemetry Collector does not do any built-in filtering of metrics for this monitor.

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
