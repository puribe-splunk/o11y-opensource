(telegraf-win-perf-counters)=
# Windows Performance Counters

<meta name="description" content="Documentation on the telegraf/win_perf_counters receiver">

## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the `telegraf/win_perf_counters` monitor by using the SignalFx Smart Agent Receiver.

This monitor is available on Windows.

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
  smartagent/telegraf/win_perf_counters:
    type: telegraf/win_perf_counters
    ... # Additional config
```

To complete the integration, include the `telegraf/win_perf_counters` receiver in a `metrics` pipeline. To do this, add the receiver to the `service > pipelines > metrics > receivers` section of your configuration file.

Sample YAML configuration:

```yaml
receivers:
 - type: telegraf/win_perf_counters
   printValid: true
   objects:
    - objectName: "Processor"
      instances:
       - "*"
      counters:
       - "% Idle Time"
       - "% Interrupt Time"
       - "% Privileged Time"
       - "% User Time"
       - "% Processor Time"
      includeTotal: true
      measurement: "win_cpu"
```

### Usage

Use this integration to read Windows performance counters.  

See [configuration examples](https://github.com/signalfx/splunk-otel-collector/tree/main/examples) for specific use cases that show how the Splunk OpenTelemetry Collector can integrate and complement existing environments.

### Event-sending functionality

```{include} /_includes/event-sending-functionality.md
```


### Configuration settings


The following table shows the configuration options for the `telegraf/win_perf_counters` receiver:


| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `objects` | no | `list of objects (see below)` |  |
| `counterRefreshInterval` | no | `int64` | The frequency that counter paths should be expanded and how often to refresh counters from configuration. This is expressed as a duration. (**default:** `5s`) |
| `useWildCardExpansion` | no | `bool` | If `true`, instance indexes will be included in instance names, and wildcards will be expanded and localized (if applicable).  If `false`, non partial wildcards will be expanded and instance names will not include instance indexes. (**default:** `false`) |
| `printValid` | no | `bool` | Print out the configurations that match available performance counters (**default:** `false`) |
| `pcrMetricNames` | no | `bool` | If `true`, metric names will be emitted in the format emitted by the SignalFx PerfCounterReporter (**default:** `false`) |


The **nested** `objects` config object has the following fields:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `objectName` | no | `string` | The name of a windows performance counter object |
| `counters` | no | `list of strings` | The name of the counters to collect from the performance counter object |
| `instances` | no | `list of strings` | The windows performance counter instances to fetch for the performance counter object |
| `measurement` | no | `string` | The name of the telegraf measurement that will be used as a metric name |
| `warnOnMissing` | no | `bool` | Log a warning if the perf counter object is missing (**default:** `false`) |
| `failOnMissing` | no | `bool` | Panic if the performance counter object is missing (this will stop the agent) (**default:** `false`) |
| `includeTotal` | no | `bool` | Include the total instance when collecting performance counter metrics (**default:** `false`) |


## Metrics

The Splunk Distribution of OpenTelemetry Collector does not do any built-in filtering of metrics for this receiver.

## Troubleshooting

```{include} /_includes/troubleshooting.md
```