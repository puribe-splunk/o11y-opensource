.. _nodejs-otel-requirements:

*************************************************************
Splunk OTel JS compatibility and requirements 
*************************************************************

.. meta::
    :description: This is what you need to instrument any Node.js application using the Splunk Distribution of OpenTelemetry JS.

Meet these requirements to instrument Node.js applications for Splunk Observability Cloud using the Splunk Distribution of OpenTelemetry JS.

.. _nodes-requirements:

Ensure you have supported Node.js and library versions
==============================================================

The Splunk Distribution of OpenTelemetry JS requires Node.js 8.5 or higher. OpenTelemetry Node depends on the ``perf_hooks`` module introduced in version 8.5.0.

The Splunk Distribution of OpenTelemetry JS instruments numerous libraries and packages. For a complete list, see :new-page:`the plugins folder <https://github.com/open-telemetry/opentelemetry-js-contrib/tree/main/plugins/node>` in the OpenTelemetry upstream repository on GitHub. To use any additional instrumentation, install it using npm before running your application.

.. note:: If you're using a Node.js version lower than 8.5, use the :new-page:`SignalFx Tracing Library for Node.js <https://github.com/signalfx/signalfx-nodejs-tracing>`.

.. _nodejs-otel-connector-requirement:

Install and configure the Splunk OpenTelemetry Collector
==============================================================

The Splunk Distribution of OpenTelemetry JS exports application traces and spans to the Splunk OpenTelemetry Collector, which also collects system metric data and logs.

To send application traces and spans to Observability Cloud, install the Splunk OpenTelemetry Collector for your platform. The following distributions are available:

- Splunk OTel Collector for Linux. See :ref:`otel-install-linux`.
- Splunk OTel Collector for Windows. See :ref:`otel-install-windows`.
- Splunk OTel Collector for Kubernetes. See :ref:`otel-install-k8s`.
