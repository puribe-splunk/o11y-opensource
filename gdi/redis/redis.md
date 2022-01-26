(redis)=

# Redis

<meta name="description" content="Documentation on the redis monitor">

## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` deploys this integration as the
Redis monitor via the Smart Agent Receiver.

The Redis monitor uses the [Python Redis plugin](https://github.com/signalfx/redis-collectd-plugin) to
monitor Redis instances. The monitor accepts endpoints and allows multiple instances. The monitor supports Redis 2.8 and later.

Using the Redis monitor, you can capture the following Redis metrics:

 * Memory used
 * Commands processed per second
 * Number of connected clients and followers
 * Number of blocked clients
 * Number of keys stored (per database)
 * Uptime
 * Changes since last save
 * Replication delay (per follower)


## Install the Redis monitor

This monitor is available in the SignalFx Smart Agent Receiver.

To install the integration, perform the following steps:

1. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform.
2. Configure the monitor, as described in the next section.

## Configure the Redis monitor

The Splunk Distribution of OpenTelemetry Collector lets you embed a Smart Agent monitor configuration
in an associated Smart Agent Receiver instance.

**Note:** To use the Redis monitor, you need to provide a Redis monitor entry in your Smart Agent or Collector configuration. Use the form of configuration that's appropriate for your agent type.

### Configure the Redis monitor for Smart Agent

To configure the Redis monitor for the Smart Agent, perform the following steps:

1. Get the agent configuration YAML file from the <a href="https://github.com/signalfx/signalfx-agent/blob/main/packaging/etc/agent.yaml" target="_blank">Git repository for the Smart Agent</a>
2. Add the following lines to the agent configuration in the YAML file:

```
monitors:  # All monitor config goes under this key
 - type: collectd/redis
   ...  # Additional config
```

To see an example Smart Agent configuration that uses an auto-generated YAML configuration file, refer to the **Example YAML** section
in the topic <a href="https://docs.splunk.com/Observability/gdi/smart-agent/smart-agent-resources.html#configure-the-smart-agent" target="_blank">Agent Configuration</a>.

### Configure the Redis monitor for Splunk Distribution of OpenTelemetry Collector

To configure the Redis monitor for the Splunk Distribution of OpenTelemetry Collector, perform the following steps:

1. Get the agent configuration YAML file from the
   <a href="https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/exporter/sapmexporter/examples/signalfx-collector.yaml" target="_blank">Git repository for Splunk Distribution of OpenTelemetry Collector</a>.
2. Add the following lines to the agent configuration in the YAML file:

```
receivers:
  smartagent/redis:
    type: collectd/redis
    ...  # Additional config
```

To complete the monitor activation, you also need to include the `smartagent/redis` receiver item in a `metrics` pipeline. To do this, add the receiver item to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file.

To see examples that show you how the Splunk OpenTelemetry Collector can integrate and complement existing environments, refer to the <a href="https://github.com/signalfx/splunk-otel-collector/tree/main/examples" target="_blank">Examples</a> topic in the documentation for the Git repository for Splunk Distribution of OpenTelemetry Collector.

### Redis monitor configuration settings

The following table shows you the configuration options for the Redis monitor:

| **Option**        | **Required** | **Type**                      | **Description**                                                                                                                                              |
|:------------------|:-------------|:------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `host`            | **yes**      | `string`                      |                                                                                                                                                              |
| `port`            | **yes**      | `integer`                     |                                                                                                                                                              |
| `pythonBinary`    | no           | `string`                      | Path to the Python Redis plugin binary. If you don't provide a name, the monitor uses its built-in runtime.  The string can include arguments to the binary. |
| `name`            | no           | `string`                      | A name for the Python Redis plugin instance. It's limited to 64 characters in length.  (**default**: "{host}:{port}")                                        |
| `auth`            | no           | `string`                      | Authentication password                                                                                                                                      |
| `sendListLengths` | no           | `list of objects (see below)` | List of keys that you want to monitor for length. To learn more, see the section **Monitor the length of Redis lists**.                                      |
| `verbose`         | no           | `bool`                        | Flag that controls verbose logging for the plugin. If `true`, verbose logging is enabled. (**default:** `false`)                                             |


The following table shows you the configuration options for the `sendListLengths` configuration object:

| **Option**      | **Required** | **Type**  | **Description**                                                                                                                                                                                                                                |
|:----------------|:-------------|:----------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `databaseIndex` | **yes**      | `integer` | The database index                                                                                                                                                                                                                             |
| `keyPattern`    | **yes**      | `string`  | A string or pattern to use for selecting keys. A string selects a single key. A pattern that uses `*` as a `glob` style wildcard processes all keys that match the pattern. Surround a pattern with single quotes ('), for example `'mylist*'` |


### Monitor the length of Redis lists

To monitor the length of list keys, you must specify the key and database index in the configuration.

Specify keys using the following syntax:

`sendListLengths: [{databaseIndex: $db_index, keyPattern: "$key_name"}]`.

You can specify `$key_name` as a glob-style pattern, but the only supported wildcard is `*` . When you use a pattern, the configuration processes all keys that match the pattern.
To ensure that the `*` is interpreted correctly, surround the pattern with double quotes (`""`).

When a non-list key matches the pattern, the Redis monitor writes an error to the agent logs.

In Observability Cloud, `gauge.key_llen` is the metric name for Redis list key lengths. Observability Cloud creates a separate MTS
for each Redis list.

**Notes**:

1. The Redis monitor uses the `KEYS` command to match patterns. Because this command isn't optimized, you need to keep your match patterns small. Otherwise, the command can block other commands from executing.
2. To avoid duplicate reporting, choose a single node in which to monitor list lengths. You can use the main node configuration or a follower node configuration.

### Redis monitor configuration example

The following example shows you a YAML configuration file for the Smart Agent:

```yaml
monitors:
- type: collectd/redis
  host: 127.0.0.1
  port: 9100
```

The next example shows you a YAML configuration file that includes list length monitoring:

```yaml
monitors:
- type: collectd/redis
  host: 127.0.0.1
  port: 9100
  sendListLengths:
  - databaseIndex: 0
    keyPattern: 'mylist*'
```

## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/integrations/master/redis/metrics.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
