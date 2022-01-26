(openshift-cluster)=

# OpenShift cluster

<meta name="description" content="Documentation for the openshift-cluster monitor">

## Description

The Splunk Distribution of OpenTelemetry Collector provides this integration as the `openshift-cluster` monitor type by using the SignalFx Smart Agent Receiver.

Use this integration to collect cluster-level metrics from the Kubernetes API server, which includes all metrics from the [kubernetes-cluster monitor](https://docs.splunk.com/Observability/gdi/kubernetes-cluster/kubernetes-cluster.html#nav-Kubernetes-cluster) with additional OpenShift-specific metrics. You only need to use the `openshift-cluster` monitor for OpenShift deployments, as it incorporates the `kubernetes-cluster` monitor automatically.

Since the agent is generally running in multiple places in a Kubernetes cluster, and since it is generally more convenient to share the same configuration across all agent instances, this monitor by default makes use of a leader election process to ensure that it is the only agent sending metrics in a cluster.

All of the agents running in the same namespace that have this monitor configured decide amongst themselves which agent should send metrics for this monitor. This agent becomes the leader agent. The remaining agents stand by, ready to activate if the leader agent dies. You can override leader agent election by setting the `alwaysClusterReporter` option to `true`, which makes the monitor always report metrics.

This monitor is similar to the `kube-state-metrics` monitor, and sends many of the same metrics, but in a way that is less verbose and a better fit for the Splunk Observability Cloud backend.

This monitor is available on Kubernetes, Linux, and Windows.

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
receivers:  # All configuration goes under this key
 - smartagent/openshift-cluster:
    type: openshift-cluster

   ...  # Additional config
```

To complete the monitor activation, you must also include the `smartagent/openshift-cluster` monitor in a `metrics` pipeline. To do this, add the monitor to the `service` > `pipelines` > `metrics` > `receivers` section of your configuration file. For example:

```
service:
  pipelines:
    metrics:
      receivers: [smartagent/openshift-cluster]
```

### Configuration settings

The following table shows the configuration options for this monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `alwaysClusterReporter` | no | `bool` | If `true`, leader election is skipped and metrics are always reported. (**default:** `false`) |
| `namespace` | no | `string` | If specified, only resources within the given namespace are monitored. If omitted (blank), all supported resources across all namespaces are monitored. |
| `kubernetesAPI` | no | `object (see below)` | Config for the K8s API client |
| `nodeConditionTypesToReport` | no | `list of strings` | A list of node status condition types to report as metrics. The metrics are reported as data points of the form `kubernetes.node_<type_snake_cased>` with a value of `0` corresponding to "False", `1` to "True", and `-1` to "Unknown". (**default:** `[Ready]`) |

The **nested** `kubernetesAPI` configuration object has the following fields:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `authType` | no | `string` | How to authenticate to the K8s API server. This can be one of `none` (for no auth), `tls` (to use manually specified TLS client certs, not recommended), `serviceAccount` (to use the standard service account token provided to the agent pod), or `kubeConfig` to use credentials from `~/.kube/config`. (**default:** `serviceAccount`) |
| `skipVerify` | no | `bool` | Whether to skip verifying the TLS cert from the API server. Almost never needed. (**default:** `false`) |
| `clientCertPath` | no | `string` | The path to the TLS client cert on the pod's filesystem, if using `tls` auth. |
| `clientKeyPath` | no | `string` | The path to the TLS client key on the pod's filesystem, if using `tls` auth. |
| `caCertPath` | no | `string` | Path to a CA certificate to use when verifying the API server's TLS cert.  Generally, this is provided by Kubernetes alongside the service account token, which is picked up automatically, so this should rarely be necessary to specify. |

## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/kubernetes/cluster/metadata.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
