.. _otel-deployment-mode:

**********************************
Determine your deployment mode
**********************************

.. meta::
      :description: Splunk Distribution of OpenTelemetry Collector provides a single binary and two deployment methods. Both deployment methods can be configured using a default configuration.

The collector provides a single binary and two deployment modes, Agent mode or Gateway mode.

Agent mode
===============

In Agent mode, the Splunk OpenTelemetry Collector runs with the application or on the same host as the application. Use Agent mode when you want to do these things:

* Configure instrumentation. Agent mode offloads responsibilities from the application including batching, queuing, and retrying.
* Collect host and application metrics, as well as host and application metadata enrichment for metrics, spans, and logs.

In Agent mode, the Splunk OpenTelemetry Collector is configured to send data directly to Splunk Observability Cloud. To copy the default Agent mode configuration YAML for the Splunk OpenTelemetry Collector, see :new-page:`Agent mode configuration <https://github.com/signalfx/splunk-otel-collector/blob/main/cmd/otelcol/config/collector/agent_config.yaml>`.

Gateway mode
==================

In Gateway mode, one or more collectors run a standalone service, such as, a container or deployment. This mode is deployed per cluster, datacenter, or region. You can configure Splunk OpenTelemetry Collector running in Agent mode or serverless instrumentation to send data in Gateway mode. Use Gateway mode when you want to do these things:

* Configure increased buffer and retrying
* Configure egress and token management control

To copy the default Gateway mode configuration YAML for the Splunk OpenTelemetry Collector, see :new-page:`Gateway mode configuration <https://github.com/signalfx/splunk-otel-collector/blob/main/cmd/otelcol/config/collector/gateway_config.yaml>`.
