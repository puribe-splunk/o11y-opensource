.. _get-started-relatedcontent:

*****************************************************************
Enable Related Content in Splunk Observability Cloud
*****************************************************************

.. meta created 2021-03-22
.. meta DOCS-2095

.. meta::
   :description: Ensure metadata keys are correct to enable full Related Content functionality.


The Related Content feature automatically correlates data between different views within Splunk Observability Cloud by presenting related data at the bottom of the screen.

You can click tiles in the Related Content bar to seamlessly navigate from Splunk Application Performance Monitoring (APM) to Splunk Log Observer to Splunk Infrastructure Monitoring with the same filters and context automatically applied in the new view.

For example, here is the Related Content bar displaying in APM. With the :strong:`paymentservice` selected in the APM Service Map, the Related Content bar is able to offer easy access to  paymentservice-related Kubernetes cluster data in Infrastructure Monitoring and logs in Log Observer:

.. image:: /_images/get-started/related-content.png
  :width: 80%
  :alt: This screenshot shows a service map in Splunk APM providing access to two Related Content tiles: K8s cluster(s) for paymentservice and Logs for paymentservice.

The following table describes when and where in Observability Cloud you can see Related Content:

.. list-table::
   :header-rows: 1
   :widths: 50, 50

   * - :strong:`Starting Point`
     - :strong:`Possible Destinations`

   * - Specific APM service
     - Kubernetes clusters filtered by service, AWS EC2s, GCP GCEs, Azure VMs, all log lines for the service

   * - Specific trace ID
     - Specific log line

   * - Specific cloud compute instance (AWS EC2, GCP GCE, Azure VM)
     - APM services, log lines for the specific instance

   * - Specific Kubernetes cluster, node, pod, container
     - Log lines for that node

   * - Specific Kubernetes pod or container
     - APM service in that pod/container, log lines for that pod/container

   * - Specific log line
     - Specific APM service, specific trace, specific Kubernetes node/pod/container, specific compute instance (AWS EC2, GCP GCE, Azure VM)

|

Related Content relies on specific metadata keys that allow APM, Infrastructure Monitoring, and Log Observer to pass filters around Observability Cloud. The metadata is how Observability Cloud connects the different data sources to allow you to navigate seamlessly. This page explains how your metadata keys must be named in APM, Infrastructure Monitoring, and Log Observer to enable Related Content.

For more details about how Related Content can help users seamlessly move between key views in Observability Cloud, see :ref:`Use case: Troubleshoot an issue from the browser to the back end <get-started-use-case>`.

Related Content is different from data links, a separate capability, which lets you dynamically transfer contextual information about the property you’re viewing to the resource, helping you get to relevant information faster. To learn more about data links, see :ref:`apm-create-data-links`.


Enable Related Content
=================================================================
Observability Cloud uses OpenTelemetry to correlate telemetry types. To enable this ability, your telemetry field names or metadata key names must exactly match the metadata key names used by OpenTelemetry and Splunk Observability Cloud.

When you deploy Splunk Distribution of Open Telemetry Collector to send your telemetry data to Observability Cloud, your metadata key names are automatically mapped correctly. When you do not use Splunk Distribution of OpenTelemetry Collector, your telemetry data may have metadata key names that are not consistent with those used by Observability Cloud and OpenTelemetry. In that case, you must change your metadata key names.

For example, say Observability Cloud receives the following telemetry data:

- Splunk APM receives a trace with the metadata key ``trace_id: 2b78e7c951497655``

- Splunk Log Observer receives a log with the metadata key ``trace.id:2b78e7c951497655``

Although these refer to the same trace ID value, the log and the trace cannot be correlated in Observability Cloud because the field names, ``trace_id`` and ``trace.id`` do not match. In this case, rename your log metadata key ``trace.id`` to ``trace_id`` using the field copy processor in Logs Pipeline Management. See :ref:`logs-processors` to learn how. Alternatively, you can re-instrument your log collection to make metadata key names align. When the field names in APM and Log Observer match, the trace and the log with the same trace ID value can be correlated in Observability Cloud. Then when you are viewing the trace in APM, you can click directly into the log with the same trace ID value and view the correlated log in Log Observer.

In the following sections, see the metadata key names used in each view within Observability Cloud and ensure that your metadata key names match.


Splunk APM
-----------------------------------------------------------------
To ensure full functionality of Related Content, do not change any of the metadata key names or span tags provided by the Splunk Distribution of OpenTelemetry Collector. To learn more about span tags in Splunk APM, see :ref:`span-tags`. 

The Splunk Distribution of OpenTelemetry Collector provides the following APM span tags that enable Related Content:

- ``service.name``
- ``deployment.environment``: To learn more about deployment environments in Splunk APM, see :ref:`apm-environments`. 

For a Related Content tile in APM to link to data for a specific Kubernetes pod (k8s.pod.name), you must first filter on a specific Kubernetes cluster (k8s.cluster.name). APM cannot guarantee an accurate Related Content Kubernetes pod destination in Infrastructure Monitoring without both values because Kubernetes pod names are not required to be unique across clusters.

For example, consider a scenario in which Related Content needs to return data for a Kubernetes pod named :strong:`Pod-B`. As shown the following diagram, a Kubernetes implementation can have multiple pods with the same name. For Related Content to return the data for the correct :strong:`Pod-B`, you must also provide the name of the Kubernetes cluster the pod resides in. In this case, that name would be either :strong:`Cluster-West` or :strong:`Cluster-East`. This combination of filtering on cluster and pod names creates the unique combination that Related Content needs to link to the correct pod data in Infrastructure Monitoring.

.. image:: /_images/get-started/k8s-clusters-pods.png
  :width: 80%
  :alt: This diagram shows two uniquely named Kubernetes clusters, each containing pods that share names across clusters.


Splunk Infrastructure Monitoring
-----------------------------------------------------------------
To ensure full functionality of Related Content, do not change any of the metadata key names provided by Splunk Distribution of OpenTelemetry Collector.

Splunk Distribution of OpenTelemetry Collector provides the following Infrastructure Monitoring metadata keys that enable Related Content:

- host.name
- k8s.cluster.name
- k8s.node.name
- k8s.pod.name
- container.id
- k8s.namespace.name
- kubernetes.workload.name

Splunk Log Observer
-----------------------------------------------------------------
To ensure full functionality of both Log Observer and Related Content, confirm that your log events fields are correctly mapped.
Correct log field mappings enable built-in log filtering, embed logs in APM and
Infrastructure Monitoring functionality, and enable fast searches as well as the Related Content bar.

If the following keys use different names in your log fields, transform them to these key names:

- service.name
- deployment.environment
- host.name
- trace_id
- span_id

For example, if you do not see values for host.name in the Log Observer UI,
check to see whether your logs use a different field name, such as host_name.
If your logs do not contain the default field names exactly as they appear above,
transform your logs using a Field copy processor. See the :strong:`Field copy processors`
section in :ref:`logs-processors` to learn how.

Methods of transforming log fields
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The following table describes the three methods for remapping log fields to
transform your logs:

.. list-table::
   :header-rows: 1
   :widths: 50 50

   * - :strong:`Remapping Method`
     - :strong:`Instructions`

   * - Observability Cloud Logs Pipeline Management
     - Create and apply a field copy processor. See the
       :strong:`Field copy processors` section in
       :ref:`logs-processors` to learn how.

   * - Client-side
     - Configure your app to remap the necessary fields.

   * - Collector-side
     - Use a Fluentd or FluentBit configuration. See
       :ref:`Configure Fluentd to send logs <fluentd>` to learn how.



Kubernetes log fields
--------------------------------------------------------------------------
Do not change the following fields, which Splunk Distribution of OpenTelemetry Collector injects into your Kubernetes logs:

- k8s.cluster.name
- k8s.node.name
- k8s.pod.name
- container.id
- k8s.namespace.name
- kubernetes.workload.name


Using Observability Collector for Kubernetes
----------------------------------------------------------------------------

For Kubernetes environments, instead of changing existing fluentd configuration, you can install a pre-configured agent provided as a helm chart. It goes with a pre-configured Fluentd agent and OpenTelemetry collector for collecting logs, metrics, and traces with all metadata relevant to Kubernetes.

To view the :new-page:`Observability Collector for Kubernetes helm chart <https://github.com/signalfx/o11y-collector-for-kubernetes>`,
request access from support.
