(mongodb-atlas)=


# MongoDB Atlas cluster

<meta name="description" content="Documentation on the mongodb-atlas monitor">


## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the `mongodb-atlas` monitor via the Smart Agent Receiver.

MongoDB Atlas provides MongoDB as an on-demand fully managed service. Atlas exposes MongoDB cluster monitoring and logging data through its [monitoring and logs](https://docs.atlas.mongodb.com/reference/api/monitoring-and-logs/) REST API endpoints. These Atlas monitoring API resources are grouped into measurements for MongoDB processes, host disks, and MongoDB databases.

This monitor repeatedly scrapes MongoDB monitoring data from Atlas at the configured time interval. It scrapes the process and disk measurements into metric groups called mongodb and hardware. The original measurement names are included in the metric descriptions.

A set of data points are fetched at the configured granularity and period for each measurement. Metric values are set to the latest non-empty data point value in the set. The finest granularity supported by Atlas is 1 minute. The configured period for the monitor needs to be wider than the interval at which Atlas provides values for measurements, otherwise some of the sets of fetched data points will contain only empty values. The default configured period is 20 minutes, which works across all measurements and gives a reasonable response payload size.

## Installation

This monitor is available in the SignalFx Smart Agent Receiver, which is part of the {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>`.

To install this integration:

1. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform.

2. Configure the monitor, as described in the next section.


## Configuration

The Splunk Distribution of OpenTelemetry Collector allows embedding a Smart Agent monitor configuration in an associated Smart Agent Receiver instance.

**Note:** Providing a `mongodb-atlas` monitor entry in your Smart Agent or Collector configuration is required for its use. Use the appropriate form for your agent type.

### Smart Agent

To activate this monitor in the Smart Agent, add the following to your agent configuration:

```
monitors:  # All monitor config goes under this key
  - type: mongodb-atlas
    ...  # Additional config
```

See <a href="https://docs.splunk.com/Observability/gdi/smart-agent/smart-agent-resources.html#configure-the-smart-agent" target="_blank">Smart Agent example configuration</a> for an autogenerated example of a YAML configuration file, with default values where applicable.

### Splunk Distribution of OpenTelemetry Collector

To activate this monitor in the Splunk Distribution of OpenTelemetry Collector, add the following to your agent configuration:

```
receivers:
  smartagent/mongodb-atlas:
    type: mongodb-atlas
    ...  # Additional config
```

To complete the monitor activation, you must also include the `smartagent/mongodb-atlas` receiver item in a `metrics` pipeline. To do this, add the receiver item to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file.

See <a href="https://github.com/signalfx/splunk-otel-collector/tree/main/examples" target="_blank">configuration examples</a> for specific use cases that show how the Splunk OpenTelemetry Collector can integrate and complement existing environments.

### Configuration settings

The following table shows the configuration options for the `mongodb-atlas` monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `projectID` | **yes** | `string` | ProjectID is the Atlas project ID |
| `publicKey` | **yes** | `string` | PublicKey is the Atlas public API key |
| `privateKey` | **yes** | `string` | PrivateKey is the Atlas private API key |
| `timeout` | no | `integer` | Timeout for HTTP requests to get MongoDB process measurements from Atlas. This should be a duration string that is accepted by [`func ParseDuration`](https://golang.org/pkg/time/#ParseDuration) (**default:** `5s`) |
| `enableCache` | no | `bool` | EnableCache enables locally cached Atlas metric measurements to be used when true. The metric measurements that were supposed to be fetched are in fact always fetched asynchronously and cached. (**default:** `true`) |
| `granularity` | no | `string` | Granularity is the duration in ISO 8601 notation that specifies the interval between measurement data points from Atlas over the configured period. The default is shortest duration supported by Atlas of 1 minute. (**default:** `PT1M`) |
| `period` | no | `string` | Period the duration in ISO 8601 notation that specifies how far back in the past to retrieve measurements from Atlas. (**default:** `PT20M`) |


The following is an example `mongodb-atlas` Smart Agent monitor configuration. This excerpt shows the minimum required fields. Note that `disableHostDimensions` is set to `true` so that the host name in which the agent/monitor is running is not used for the `host` metric dimension value. The names of the MongoDB cluster hosts from which metrics emanate are used instead.

```
monitors:
- type: mongodb-atlas
  projectID:  <Project ID>
  publicKey:  <Public API Key>
  privateKey: <Private API Key>
  disableHostDimensions: true
```


## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/integrations/master/mongodb-atlas/metrics.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```