.. _otel-components:

******************************************
Build your configuration file
******************************************

.. meta::
    :description: Learn about the components that make up the OpenTelemetry Collector.

The OpenTelemetry Collector is made up of the following components:

* Receivers: How to get data in. Receivers can be push or pull based.
* Processors: What to do with received data.
* Exporters: Where to send received data. Exporters can be push or pull based.
* Extensions: Provide capabilities on top of the primary functionality of the Splunk OpenTelemetry Collector.

You can enable these components through :ref:`pipelines <otel-data-processing>`. You can also define multiple instances of components as well as pipelines with a YAML configuration.

Splunk Distribution of OpenTelemetry Collector offers support for the components described in the following tables.

Receivers
==================

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Name
     - Description
     - Supported pipeline type
   * - ``fluentforward``
     - Runs a TCP server that accepts events through the Fluentd Forward protocol.
     - Logs
   * - ``hostmetrics``
     - Generates metrics about the host system scraped from various sources. This receiver is intended to be used when the Splunk OpenTelemetry Collector is deployed as an agent. 
     - Metrics
   * - ``jaeger``
     - Receives trace data in Jaeger format.
     - Traces
   * - ``k8s_cluster``
     - Collects cluster-level metrics from the Kubernetes API server. It uses the Kubernetes API to listen for updates. You can use a single instance of this receiver to monitor a cluster.
     - Metrics
   * - ``kubeletstats``
     - Pulls pod metrics from the API server on a kubelet.
     - Metrics
   * - ``otlp``
     - Receives data through gRPC or HTTP using OTLP format.
     - Metrics, logs, traces
   * - ``receiver_creator``
     - Instantiates other receivers at runtime based on whether observed endpoints match a configured rule. To use the receiver creator, you must first configure one or more observer extensions to discover networked endpoints that you might be interested in.
     - N/A
   * - ``sapm``
     - Builds on the Jaeger proto. This allows the Splunk OpenTelemetry Collector to receive traces from other collectors.
     - Traces
   * - ``signalfx``
     - Accepts metrics and logs in the proto format.
     - Metrics, logs
   * - ``prometheus_simple``
     - Provides a simple configuration interface to configure the Prometheus receiver to scrape metrics from a single target.
     - Metrics
   * - ``smartagent``
     - Utilizes the existing Smart Agent monitors as OpenTelemetry Collector metric receivers.
     - Metrics
   * - ``splunk_hec``
     - Accepts metrics in the Splunk HEC format.
     - Metrics, logs, traces
   * - ``zipkin``
     - Receives spans from Zipkin versions 1 and 2.
     - Traces

Processors
=================================

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Name
     - Description
     - Supported pipeline type
   * - ``attributes``
     - Modifies attributes of a span.
     - Logs, traces
   * - ``batch``
     - Accepts spans, metrics, or logs and places them into batches. Batching helps better compress the data and reduce the number of outgoing connections required to transmit the data. This processor supports both size-based and time-based batching.
     - Metrics, logs, traces
   * - ``filter``
     - Can be configured to include or exclude metrics based on metric name in the case of the ``strict`` or ``regexp`` match types, or based on other metric attributes in the case of the ``expr`` match type.
     - Metrics
   * - ``k8s_tagger``
     - Allows automatic tagging of spans, metrics, and logs with Kubernetes metadata.
     - Metrics, logs, traces
   * - ``memorylimiter``
     - Prevents out of memory situations on the Splunk OpenTelemetry Collector.
     - Metrics, logs, traces
   * - ``metricstransform``
     - Renames metrics, and adds, renames, or deletes label keys and values.
     - Metrics
   * - ``probabilisticsampler``
     - Provides samples based on hash values determined by trace IDs.
     - Traces
   * - ``resource``
     - Applies changes to resource attributes. Attributes represent actions that can be applied on resources. OpenCensus has defined  standard attributes for resources, including services, environments, and hosts. Do not change the host name, as this can cause issues with infrastructure correlation. 
     - Metrics, logs, traces
   * - ``resourcedetection``
     - Detects resource information from the host, in a format that conforms to the OpenTelemetry resource semantic conventions, and appends or overrides the resource value in telemetry data with this information.
     - Metrics, logs, traces
   * - ``span``
     - Modifies either the span name or attributes of a span based on the span name.
     - Traces

Exporters
=================================

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Name
     - Description
     - Supported pipeline type
   * - ``file``
     - Writes pipeline data to a JSON file. The data is written in Protobuf JSON encoding using OpenTelemetry protocol. 
     - Metrics, logs, traces
   * - ``logging``
     - Exports data to the console through zap.Logger. This exporter does not send its output to Windows Event Viewer by default. Run the Splunk OpenTelemetry Collector directly from the PowerShell terminal to send output to Windows Event Viewer.
     - Metrics, logs, traces
   * - ``otlp``
     - Exports data through gRPC using OTLP format. By default, this exporter requires TLS and offers queued retry capabilities. 
     - Metrics, traces
   * - ``otlphttp``
     - Exports traces and/or metrics via HTTP using OTLP format. 
     - Metrics, traces
   * - ``sapm``
     - Builds on the Jaeger proto and adds additional batching on top, which allows the Splunk OpenTelemetry Collector to export traces from multiple nodes or services in a single batch. 
     - Traces
   * - ``signalfx``
     - Sends metrics, events, and trace correlation to Infrastructure Monitoring. 
     - Logs (events), metrics, traces (trace to metric correlation only)
   * - ``splunk_hec``
     - Sends metrics to a Splunk HEC endpoint. 
     - Metrics, logs, traces

Extensions
=================================

.. list-table::
   :widths: 50 50
   :header-rows: 1

   * - Name
     - Description
   * - ``healthcheck``
     - Enables an HTTP URL that can be probed to check the status of the OpenTelemetry Collector. You can use this extension as a liveness or readiness probe on Kubernetes.
   * - ``httpforwarder``
     - Accepts HTTP requests and optionally adds headers to them and forwards them. The RequestURIs of the original requests are preserved by the extension. 
   * - ``host_observer``
     - Looks at the current host for listening network endpoints. Uses the /proc filesystem and requires the SYS_PTRACE and DAC_READ_SEARCH capabilities so that it can determine what processes own the listening sockets. 
   * - ``k8s_observer``
     - Uses the Kubernetes API to discover pods running on the local node. This extension assumes the Splunk OpenTelemetry Collector is deployed in Agent mode where it is running on each individual node or host instance. 
   * - ``pprof``
     - Enables the golang ``net/http/pprof`` endpoint, which is typically used by developers to collect performance profiles and investigate issues with the service.
   * - ``smartagent``
     - Provides a mechanism to specify configuration options that are not specific only to a single instance of the Smart Agent Receiver but are applicable to all instances. This component provides a means of migrating your existing Smart Agent configuration to the Splunk Distribution of OpenTelemetry Collector. 
   * - ``zpages``
     - Enables an extension that serves zPages, an HTTP endpoint that provides live data for debugging different components that were properly instrumented for such. 
