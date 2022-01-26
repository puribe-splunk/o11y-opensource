(kubernetes-cluster)=

# Kubernetes cluster

<meta name="description" content="Documentation on the kubernetes-cluster monitor type">

## Description


The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the `kubernetes-cluster` monitor via the Smart Agent Receiver. This monitor obtains cluster-level resource metrics from the Kubernetes API.

**Note:** If you are using OpenShift, use the `openshift-cluster` monitor instead of this `kubernetes-cluster` monitor. The `openshift-cluster` monitor contains additional OpenShift metrics.

The `kubernetes-cluster` monitor collects cluster-level metrics from the Kubernetes API server. This monitor uses the watch functionality of the Kubernetes API to listen for updates about the cluster and maintains a cache of metrics that get sent on a regular interval.

Since the agent is generally running in multiple places in a Kubernetes cluster, and since it is generally more convenient to share the same configuration across all agent instances, this monitor by default makes use of a leader election process to ensure that it is the only agent sending metrics in a cluster. All of the agents running in the same namespace that have this monitor configured will decide amongst themselves which should send metrics for this monitor, and the rest will stand by ready to activate if the leader agent expires. You can override leader election by setting the configuration option `alwaysClusterReporter` to `true`, which will make the monitor always report metrics.
**Note:** If you are using OpenShift, use the ``openshift-cluster`` monitor type instead of this ``kubernetes-cluster`` monitor type. The ``openshift-cluster`` monitor type contains additional OpenShift metrics.

Since the agent is generally running in multiple places in a Kubernetes cluster, and since it is generally more convenient to share the same configuration across
all agent instances, this monitor type by default makes use of a leader election process to ensure that it is the only agent sending metrics in a cluster. All of the
agents running in the same namespace that have this monitor type configured will decide amongst themselves which should send metrics for this monitor type, and the rest will stand by ready to activate if the leader agent dies. You can override leader election by setting the configuration option ``alwaysClusterReporter`` to ``true``, which will make the monitor always report metrics.


This monitor is similar to `kube-state-metrics` and sends many of the same metrics, but in a way that is less verbose and better fitted for Splunk Infrastructure Monitoring.

## 
**Note**
---
>Larger clusters might encounter instability when setting this configuration across a large number of nodes. **Enable with caution.**
---

This monitor is similar to ``kube-state-metrics`` and sends many of the same metrics, but in a way that is less verbose and better fitted for Splunk Infrastructure Monitoring.


### Benefits

```{include} /_includes/benefits.md
```

##  Installation


```{include} /_includes/collector-installation.md
```

## Configuration

```{include} /_includes/configuration.md
```

```
receivers:
  smartagent/kubernetes-cluster:
    type: kubernetes-cluster
    ... # Additional config
```

To complete the integration, include the monitor in a metrics pipeline. To do this, add the monitor to the `service > pipelines > metrics > receivers` section of your configuration file.

See the [kubernetes.yaml](https://github.com/signalfx/splunk-otel-collector/tree/main/examples/kubernetes-yaml) in GitHub for the Agent and Gateway YAML files.

### Configuration settings

The following tables show the configuration options for this monitor type:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `alwaysClusterReporter` | no | `bool` | If `true`, leader election is skipped and metrics are always reported. **Default is** `false`. |
| `namespace` | no | `string` | If specified, only resources within the given namespace will be monitored. If omitted (blank), all supported resources across all namespaces will be monitored. |
| `kubernetesAPI` | no | `object (see below)` | Configuration for the Kubernetes API client |
| `nodeConditionTypesToReport` | no | `list of strings` | A list of node status condition types to report as metrics. The metrics will be reported as data points of the form `kubernetes.node_<type_snake_cased>` with a value of `0` corresponding to "False", `1` to "True", and `-1` to "Unknown". **Default** is `[Ready]`. |

The **nested** `kubernetesAPI` configuration object has the following fields:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `authType` | no | `string` | To authenticate to the K8s API server: <br> - `none` for no authentication.<br> - `tls` to use manually specified TLS client certs (not recommended). <br> - `serviceAccount` to use the standard service account token provided to the agent pod. <br> - `kubeConfig` to use credentials from `~/.kube/config`. <br> - **Default** is `serviceAccount`. | |
| `skipVerify` | no | `bool` | Whether to skip verifying the TLS cert from the API server. Almost never needed. **Default** is `false`. |
| `clientCertPath` | no | `string` | The path to the TLS client cert on the pod's filesystem, if using `tls` authentication. |
| `clientKeyPath` | no | `string` | The path to the TLS client key on the pod's filesystem, if using `tls` authentication. |
| `caCertPath` | no | `string` | Path to a CA certificate to use when verifying the API server's TLS certificate. This is provided by Kubernetes alongside the service account token, which will be picked up automatically, so this should rarely be necessary to specify. |

## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/kubernetes/cluster/metadata.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
