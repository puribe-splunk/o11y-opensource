(kafka)=

# Apache Kafka

<meta name="description" content="Documentation on the kafka monitor">

## Description

The Splunk Distribution of OpenTelemetry Collector provides this integration as the Apache Kafka monitor type via the SignalFx Smart Agent Receiver.

The Apache Kafka monitor monitors a Kafka instance using the GenericJMX plugin. See [GenericJMX](https://docs.splunk.com/Observability/gdi/genericjmx/genericjmx.html) for more information on how to configure custom MBeans, as well as information on troubleshooting JMX setup.

This monitor has a set of [built-in MBeans
configured](https://github.com/signalfx/signalfx-agent/tree/main/pkg/monitors/collectd/kafka/mbeans.go) for which it pulls metrics from Kafka's JMX endpoint.

**Note:** This monitor supports Kafka v0.8.2.x and above. For Kafka v1.x.x and above, apart from the list of default metrics, `kafka.server:type=ZooKeeperClientMetrics,name=ZooKeeperRequestLatencyMs` is a good metric to monitor since it gives an understanding of how long brokers wait for requests to Zookeeper to be completed. Since Zookeeper is an integral part of a Kafka cluster, monitoring it using the Zookeeper monitor is recommended. It is also a good idea to monitor disk utilization and network metrics of the underlying host.

This monitor is available on Kubernetes, Linux, and Windows.

### Benefits
```{include} /_includes/benefits.md
```

## Installation

```{include} /_includes/collector-installation.md
```

## Configuration

```{include} /_includes/configuration.md
```

To activate this monitor type, add the following to your agent configuration:

```
receivers:
  smartagent/kafka:
    type: collectd/kafka
    ...  # Additional config
```

To complete the monitor activation, you must also include the `smartagent/kafka` monitor type in a `metrics` pipeline. To do this, add the receiver item to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file. For example:

```
service:
  pipelines:
    metrics:
      receivers: [smartagent/kafka]
```

See <a href="https://github.com/signalfx/splunk-otel-collector/tree/main/examples" target="_blank">configuration examples</a> for specific use cases that show how the Splunk OpenTelemetry Collector can integrate and complement existing environments.

### Configuration settings

The following table shows the configuration options for this monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `host` | **yes** | `string` | Host to connect to -- JMX must be configured for remote access and accessible from the agent |
| `port` | **yes** | `integer` | JMX connection port (NOT the RMI port) on the application. This corresponds to the `com.sun.management.jmxremote.port` Java property that should be set on the JVM when running the application. |
| `name` | no | `string` |  |
| `serviceName` | no | `string` | This is how the service type is identified in the Splunk Observability Cloud UI so that you can get built-in content for it.  For custom JMX integrations, it can be set to whatever you like and metrics will get the special property `sf_hostHasService` set to this value. |
| `serviceURL` | no | `string` | The JMX connection string. This is rendered as a Go template and has access to the other values in this config. NOTE: under normal circumstances it is not advised to set this string directly - setting the host and port as specified above is preferred. (**default:** `service:jmx:rmi:///jndi/rmi://{{.Host}}:{{.Port}}/jmxrmi`) |
| `instancePrefix` | no | `string` | Prefixes the generated plugin instance with prefix. If a second `instancePrefix` is specified in a referenced MBean block, the prefix specified in the Connection block will appear at the beginning of the plugin instance, and the prefix specified in the MBean block will be appended to it. |
| `username` | no | `string` | Username to authenticate to the server |
| `password` | no | `string` | User password to authenticate to the server |
| `customDimensions` | no | `map of strings` | Takes in key-value pairs of custom dimensions at the connection level. |
| `mBeansToCollect` | no | `list of strings` | A list of the MBeans defined in `mBeanDefinitions` to actually collect. If not provided, then all defined MBeans will be collected. |
| `mBeansToOmit` | no | `list of strings` | A list of the MBeans to omit. This will come in handy in cases where only a few MBeans need to be omitted from the default list.|
| `mBeanDefinitions` | no | `map of objects (see below)` | Specifies how to map JMX MBean values to metrics.  If using a specific service monitor such as Cassandra, Kafka, or ActiveMQ, they come pre-loaded with a set of mappings, and any that you add in this option will be merged with those.  See [GenericJMX](https://collectd.org/documentation/manpages/collectd-java.5.shtml#genericjmx_plugin) for more details. |
| `clusterName` | **yes** | `string` | Cluster name to which the broker belongs |. 


The **nested** `mBeanDefinitions` configuration object has the following fields:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `objectName` | no | `string` | Sets the pattern used to retrieve MBeans from the MBeanServer. If more than one MBean is returned, you should use the `instanceFrom` option to make the identifiers unique. |
| `instancePrefix` | no | `string` | Prefixes the generated plugin instance with prefix |
| `instanceFrom` | no | `list of strings` | The object names used by JMX to identify MBeans include so-called "properties" which are basically key-value pairs. If the given object name is not unique and multiple MBeans are returned, the values of those properties usually differ. You can use this option to build the plugin instance from the appropriate property values. This option is optional and may be repeated to generate the plugin instance from multiple property values. |
| `values` | no | `list of objects (see below)` | The `value` blocks map one or more attributes of an MBean to a value list. There must be at least one `value` block within each MBean block. |
| `dimensions` | no | `list of strings` |  |


The **nested** `values` configuration object has the following fields:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `type` | no | `string` | Sets the data set used to handle the values of the MBean attribute |
| `table` | no | `bool` | Set this to `true` if the returned attribute is a composite type. If set to `true`, the keys within the composite type are appended to the type instance. (**default:** `false`) |
| `instancePrefix` | no | `string` | Works like the option of the same name directly beneath the MBean block, but sets the type instance instead |
| `instanceFrom` | no | `list of strings` | Works like the option of the same name directly beneath the MBean block, but sets the type instance instead |
| `attribute` | no | `string` | Sets the name of the attribute from which to read the value. You can access the keys of composite types by using a dot to concatenate the key name to the attribute name. For example, “attrib0.key42”. If `table` is set to `true`, path must point to a composite type, otherwise it must point to a numeric type. |
| `attributes` | no | `list of strings` | The plural form of the `attribute` configuration above.  Used to derive multiple metrics from a single MBean. |

## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/integrations/master/kafka/metrics.yaml"></div>

## Troubleshooting
```{include} /_includes/troubleshooting.md
```
