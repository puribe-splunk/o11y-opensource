(docker)=

# Docker Containers

<meta name="description" content="Documentation on the docker monitor">

## Description

This integration primarily consists of the Smart Agent monitor `docker-container-stats`. This monitor reads container stats from a Docker API server. The monitor does not currently support CPU share/quota metrics.

To collect data for this service, you need to deploy the Smart Agent instead of Splunk Distribution of OpenTelemetry Collector. Using the Splunk Distribution of OpenTelemetry Collector may result in metrics collected with different names.

If you are running the agent directly on a host (outside of a container itself) and you are using the default Docker UNIX socket URL, add the `signalfx-agent` user to the `docker` group to have permission to access the Docker API via the socket.

This integration is available for Kubernetes, Linux, and Windows.

## Benefits

```{include} /_includes/benefits.md
```

## Installation

To install this integration:

1. Install the Smart Agent using the [installation instructions](https://github.com/signalfx/signalfx-agent#installation) in GitHub.
2. Configure the monitor, as described in the next section.
3. Restart the Smart Agent.

## Configuration

To activate this monitor in the Smart Agent, add the following to your agent configuration:

```
monitors:  # All monitor config goes under this key
 - type: docker-container-stats
   ...  # Additional config
```

See <a href="https://github.com/signalfx/signalfx-agent/blob/main/deployments/docker/agent.yaml" target="_blank">Docker agent.yaml</a> for an example configuration file, with default values where applicable. See [Docker Deployment](https://github.com/signalfx/signalfx-agent/blob/main/deployments/docker) for a link to the Docker image.

### Configuration settings

The following table shows the configuration options for this monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `enableExtraBlockIOMetrics` | no | `bool` | Sends all extra block IO metrics. (**default:** `false`) |
| `enableExtraCPUMetrics` | no | `bool` | Sends all extra CPU metrics. (**default:** `false`) |
| `enableExtraMemoryMetrics` | no | `bool` | Sends all extra memory metrics. (**default:** `false`) |
| `enableExtraNetworkMetrics` | no | `bool` | Sends all extra network metrics. (**default:** `false`) |
| `dockerURL` | no | `string` | The URL of the docker server. (**default:** `unix:///var/run/docker.sock`) |
| `timeoutSeconds` | no | `integer` | The maximum amount of time to wait for docker API requests. (**default:** `5`) |
| `cacheSyncInterval` | no | `integer` | The time to wait before resyncing the list of containers the monitor maintains through the docker event listener. An example is `cacheSyncInterval: "20m"` (**default:** `60m`) |
| `labelsToDimensions` | no | `map of strings` | A mapping of container label names to dimension names. The corresponding label values become the dimension value for the mapped name.  For example, `io.kubernetes.container.name: container_spec_name` results in a dimension called `container_spec_name` that has the value of the `io.kubernetes.container.name` container label. |
| `envToDimensions` | no | `map of strings` | A mapping of container environment variable names to dimension names.  The corresponding env var values become the dimension values on the emitted metrics.  For example, `APP_VERSION: version` results in data points having a dimension called `version` whose value is the value of the `APP_VERSION` envvar configured for that particular container, if present. |
| `excludedImages` | no | `list of strings` | A list of filters of images to exclude.  Supports literals, globs, and regex. |


## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/integrations/master/docker/metrics.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
