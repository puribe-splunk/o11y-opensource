(couchbase)=

# Couchbase server

<meta name="description" content="Documentation for the couchbase monitor">

## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the Couchbase server monitor with the SignalFx Smart Agent receiver. The integration collects metrics from Couchbase servers.

## Installation

Follow these steps to deploy the integration:

1. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform.
2. Complete the [Configuration](#splunk-opentelemetry-collector-configuration).

## Configuration

The Splunk Distribution of OpenTelemetry Collector allows embedding a Smart Agent monitor configuration in an associated Smart Agent Receiver instance.

**Note:** Providing a Couchbase server monitor entry in your Smart Agent or Collector configuration is required for its use. Use the appropriate form for your agent type.

### Smart Agent

To activate this monitor in the Smart Agent, add the following to your agent configuration:

```
monitors:  # All monitor config goes under this key
 - type: collectd/couchbase
   ...  # Additional config
```

See <a href="https://docs.splunk.com/Observability/gdi/smart-agent/smart-agent-resources.html#configure-the-smart-agent" target="_blank">Smart Agent example configuration</a> for an autogenerated example of a YAML configuration file, with default values where applicable.

### Splunk Distribution of OpenTelemetry Collector

To activate this monitor in the OpenTelemetry Collector, add the following to your agent configuration:

```
receivers:
    smartagent/couchbase:
        type: collectd/couchbase
        ...  # Additional config
```

To complete the monitor activation, you must also include the `smartagent/couchbase` receiver item in a `metrics` pipeline. To do this, add the receiver item to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file.

See <a href="https://github.com/signalfx/splunk-otel-collector/tree/main/examples" target="_blank">configuration examples</a> for specific use cases that show how the Splunk OpenTelemetry Collector can integrate and complement existing environments.

## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/collectd/couchbase/metadata.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```