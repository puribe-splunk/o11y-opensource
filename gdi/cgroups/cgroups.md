(cgroups)=

# cgroups

<meta name="Description" content="Documentation on the cgroups receiver">

## Description

This receiver reports statistics about `cgroups` on Linux. This receiver supports cgroups version 1, not the newer cgroups version 2 unified implementation.

For general information on cgroups, see the Linux control groups documentation at http://man7.org/linux/man-pages/man7/cgroups.7.html.

For detailed information on `cpu` cgroup metrics, see Red Hat's guide to CPU management at https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/sec-cpu. Note that the `cpuacct` cgroup is primarily an informational cgroup that gives detailed information on how long processes in a cgroup used the CPU.

For detailed information on `memory` cgroup metrics, see Red Hat's guide to the Memory cgroup at https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/resource_management_guide/sec-memory. Also refer to the Linux Kernel's memory cgroup document at https://www.kernel.org/doc/Documentation/cgroup-v1/memory.txt.

### Filtering
You can limit the cgroups for which metrics are generated with the `cgroups` config option to the receiver.

For example, the following will only monitor Docker-generated cgroups:

```yaml
monitors:
 - type: cgroups
   cgroups:
    - "/docker/*"
```

### Benefits

```{include} /_includes/benefits.md
```

## Installation

```{include} /_includes/collector-installation-linux.md
```

## Configuration

```{include} /_includes/configuration.md
```

```
receivers:
  smartagent/cgroups: 
    type: cgroups
    ... # Additional config
```

To complete the integration, include the receiver with this monitor type in a `metrics` pipeline. To do this, add the receiver to the `service > pipelines > metrics > receivers` section of your configuration file.

```
service:
  pipelines:
    metrics:
      receivers: [smartagent/cgroups]
```

See the [configuration example](https://github.com/signalfx/splunk-otel-collector/tree/main/examples) for specific use cases that show how the Splunk OpenTelemetry Collector can integrate and complement existing environments.

### Configuration settings

The following table shows the configuration options for this receiver:
  
| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `cgroups` | no | `list of strings` | The cgroup names to include or exclude, based on the full hierarchy path. This set can be overridden. If not provided, `cgroups` defaults to a list of all cgroups. For example, to monitor all Docker container cgroups, you could use a value of `["/docker/*"]`. |

## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/cgroups/metadata.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
