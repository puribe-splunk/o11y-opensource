(logstash)=

# Logstash

<meta name="description" content="Documentation on the Logstash monitor">


## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides these integrations as the `logstash` and `logstash-tcp` monitor types via the Smart Agent Receiver.

### The `logstash` monitor
The `logstash` monitor monitors the health and performance of Logstash deployments through
Logstash's [Monitoring APIs](https://www.elastic.co/guide/en/logstash/current/monitoring-logstash.html).

### The `logstash-tcp` monitor
The `logstash-tcp` monitor fetches events from the [logstash tcp output
plugin](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-tcp.html)
operating in either `server` or `client` mode and converts them to data points. The `logstash-tcp` monitor is meant to be used in conjunction with the Logstash
[Metrics filter
plugin](https://www.elastic.co/guide/en/logstash/current/plugins-filters-metrics.html)
that turns events into metrics.

You can only use autodiscovery when this monitor is in `client` mode.

## Installation

These monitors are available in the SignalFx Smart Agent Receiver, which is part of the {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>`.

To install these integrations:

1. Deploy the Splunk OpenTelemetry Collector to your host or container platform.

2. Configure the monitors, as described in the next section.

## Configuration

The Splunk Distribution of OpenTelemetry Collector allows embedding a Smart Agent monitor configuration in an associated Smart Agent Receiver instance.

### The `logstash` monitor

**Note:** Providing a `logstash` monitor entry in your Smart Agent or Collector configuration is required for its use. Use the appropriate form for your agent type.

### Smart Agent

To activate this monitor in the Smart Agent, add the following to your agent configuration:

```
monitors:  # All monitor config goes under this key
  - type: logstash
    ...  # Additional config
```

See <a href="https://docs.splunk.com/Observability/gdi/smart-agent/smart-agent-resources.html#configure-the-smart-agent" target="_blank">Smart Agent example configuration</a> for an autogenerated example of a YAML configuration file, with default values where applicable.

### Splunk Distribution of OpenTelemetry Collector

To activate this monitor in the Splunk Distribution of OpenTelemetry Collector, add the following to your agent configuration:

```
receivers:
  smartagent/logstash:
    type: logstash
    ...  # Additional config
```

To complete the monitor activation, you must also include the `smartagent/logstash` receiver item in a `metrics` pipeline. To do this, add the receiver item to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file.

See <a href="https://github.com/signalfx/splunk-otel-collector/tree/main/examples" target="_blank">configuration examples</a> for specific use cases that show how the Splunk OpenTelemetry Collector can integrate and complement existing environments.

### Configuration settings

The following table shows the configuration options for this monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `host` | no | `string` | The hostname of Logstash monitoring API (**default:** `127.0.0.1`) |
| `port` | no | `integer` | The port number of Logstash monitoring API (**default:** `9600`) |
| `useHTTPS` | no | `bool` | If `true`, the agent will connect to the host using HTTPS instead of plain HTTP. (**default:** `false`) |
| `timeoutSeconds` | no | `integer` | The maximum amount of time to wait for API requests (**default:** `5`) |

### The `logstash-tcp` monitor

**Note:** Providing a `logstash-tcp` monitor entry in your Smart Agent or Collector configuration is required for its use. Use the appropriate form for your agent type.

To activate this monitor in the Smart Agent, add the following to your agent configuration:

```
monitors:  # All monitor config goes under this key
  - type: logstash-tcp
    ...  # Additional config
```

See <a href="https://docs.splunk.com/Observability/gdi/smart-agent/smart-agent-resources.html#configure-the-smart-agent" target="_blank">Smart Agent example configuration</a> for an autogenerated example of a YAML configuration file, with default values where applicable.

To activate this monitor in the Splunk Distribution of OpenTelemetry Collector, add the following to your agent configuration and include the `smartagent/logstash-tcp` receiver item in the `metrics` pipeline:

```
receivers:
  smartagent/logstash-tcp:
    type: logstash-tcp
    ...  # Additional config
```

The following table shows the configuration options for this monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `host` | **yes** | `string` | If `mode: server`, the local IP address to listen on.  If `mode: client`, the Logstash host/ip to connect to. |
| `port` | no | `integer` | If `mode: server`, the local port to listen on.  If `mode: client`, the port of the Logstash TCP output plugin.  If port is `0`, a random listening port is assigned by the kernel. (**default:** `0`) |
| `mode` | no | `string` | Whether to act as a `server` or `client`.  The corresponding setting in the Logtash `tcp` output plugin should be set to the opposite of this. (**default:** `client`) |
| `desiredTimerFields` | no | `list of strings` |  (**default:** `[mean, max, p99, count]`) |
| `reconnectDelay` | no | `integer` | How long to wait before reconnecting if the TCP connection cannot be made or after it gets broken. (**default:** `5s`) |
| `debugEvents` | no | `bool` | If `true`, events received from Logstash will be dumped to the agent's stdout in deserialized form (**default:** `false`) |

The following is a contrived example configuration that shows the use of both `timer` and
`meter` metrics from the Logstash Metrics filter plugin:

```
input {
  file {
    path => "/var/log/auth.log"
    start_position => "beginning"
    tags => ["auth_log"]
  }

  # A contrived file that contains timing messages
  file {
    path => "/var/log/durations.log"
    tags => ["duration_log"]
    start_position => "beginning"
  }
}

filter {
  if "duration_log" in [tags] {
    dissect {
      mapping => {
        "message" => "Processing took %{duration} seconds"
      }
      convert_datatype => {
        "duration" => "float"
      }
    }
    if "_dissectfailure" not in [tags] { # Filter out bad events
      metrics {
        timer => { "process_time" => "%{duration}" }
        flush_interval => 10
        # This makes the timing stats pertain to only the previous 5 minutes
        # instead of since Logstash last started.
        clear_interval => 300
        add_field => {"type" => "processing"}
        add_tag => "metric"
      }
    }
  }
  # Count the number of logins via SSH from /var/log/auth.log
  if "auth_log" in [tags] and [message] =~ /sshd.*session opened/ {
    metrics {
      # This determines how often metric events will be sent to the agent, and
      # thus how often data points will be emitted.
      flush_interval => 10
      # The name of the meter will be used to construct the name of the metric
      # in Splunk Infrastructure Monitoring. For this example, a data point called `logins.count` would
      # be generated.
      meter => "logins"
      add_tag => "metric"
    }
  }
}

output {
  # This can be helpful to debug
  stdout { codec => rubydebug }

  if "metric" in [tags] {
    tcp {
      port => 8900
      # The agent will connect to Logstash
      mode => "server"
      # Needs to be "0.0.0.0" if running in a container.
      host => "127.0.0.1"
    }
  }
}
```
Once Logstash is configured with the above configuration, the `logstash-tcp` monitor
collects `logins.count` and `process_time.<timer_field>`.

## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/integrations/master/logstash/metrics.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```