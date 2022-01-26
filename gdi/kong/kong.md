(kong)=

# Kong Gateway

<meta name="description" content="Documentation for the kong monitor">

## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the Kong Gateway monitor with the SignalFx Smart Agent receiver. This monitor requires version 0.11.2+ of Kong and version 0.0.1+ of kong-plugin-signalfx.

**Note:** This integration is only supported for Kong Gateway Community Edition (CE). 

### Benefits

```{include} /_includes/benefits.md
```

## Installation

This monitor is available in the SignalFx Smart Agent Receiver, which is part of the {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>`.

Follow these steps to deploy the integration:

1. Install the Lua module on all Kong servers.
2. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform.
3. Configure the monitor, as described in the next section.

## Configuration

To activate this monitor in the Splunk Distribution of OpenTelemetry Collector, add the following to your configuration:

```
receivers:
  smartagent/kong:
    type: collectd/kong
    ...  # Additional config
```

To complete the monitor activation, you must also include the `smartagent/kong` receiver item in a `metrics` pipeline. To do this, add the receiver item to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file. For example:

```
service:
  pipelines:
    metrics:
      receivers: [smartagent/kong]
```

### Configuration settings

The following is an example configuration:

```
receivers:
  smartagent/kong:
   type: collectd/kong
   host: 127.0.0.1
   port: 8001
   metrics:
    - metric: request_latency
      report: true
    - metric: connections_accepted
      report: false
```

The following is an example configuration with custom `/signalfx` route and filter lists:

```
receivers:
  smartagent/kong:
    type: collectd/kong
    host: 127.0.0.1
    port: 8443
    url: https://127.0.0.1:8443/routed_signalfx
    authHeader:
      header: Authorization
      value: HeaderValue
    metrics:
      - metric: request_latency
        report: true
    reportStatusCodeGroups: true
    statusCodes:
      - 202
      - 403
      - 405
      - 419
      - "5*"
    serviceNamesBlacklist:
      - "*SomeService*"
```

## Metrics

These metrics are available for this integration.

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/collectd/kong/metadata.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
