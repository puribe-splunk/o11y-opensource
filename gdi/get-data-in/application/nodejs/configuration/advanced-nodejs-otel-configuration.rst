.. _advanced-nodejs-otel-configuration:

***************************************************************************
Configure the Splunk Distribution of OTel JS for Splunk Observability Cloud
***************************************************************************

.. meta:: 
   :description: Configure the Splunk Distribution of OpenTelemetry JS to suit your instrumentation needs, like correlating traces with logs, enabling exporters, and more.

You can configure the Splunk Distribution of OpenTelemetry JS to suit your instrumentation needs. In most cases, modifying the basic configuration is enough to get started.

The following sections describe all available settings for configuring OpenTelemetry for Node.js, including options for enabling new features that are unique to the Splunk Distribution of OpenTelemetry JS.

.. _configuration-methods-nodejs:

Configuration methods
===========================================================

To configure the Splunk Distribution of OpenTelemetry JS, you can use a combination of environment variables and arguments passed to the ``startTracing()`` and ``startMetrics()`` functions:

- Environment variables

   For example: ``export OTEL_SERVICE_NAME='test-service'``

- Arguments passed to the ``startTracing()`` and ``startMetrics()`` functions

   For example: ``startTracing({ serviceName: 'my-node-service', });``

.. note:: Function arguments take precedence over the corresponding environment variables.

.. _main-nodejs-agent-settings:

General settings
=========================================================================

The following settings are specific to the Splunk Distribution of OpenTelemetry JS:

.. list-table:: 
   :header-rows: 1

   * - Environment variable
     - Arguments to ``startTracing()``
     - Description
   * - ``SPLUNK_ACCESS_TOKEN``
     - ``accessToken``
     - Splunk authentication token that lets exporters send data directly to Splunk Observability Cloud. Unset by default. Required if you need to send data to the Observability Cloud ingest endpoint. See :ref:`admin-tokens`.
   * - ``SPLUNK_TRACE_RESPONSE_HEADER_ENABLED``
     - ``serverTimingEnabled``
     - Enables the addition of server trace information to HTTP response headers. For more information, see :ref:`server-trace-information-nodejs`. The default value is ``true``.

.. _trace-configuration-nodejs:

Trace configuration
=======================================================

The following settings control tracing limits and attributes:

.. list-table:: 
   :header-rows: 1

   * - Environment variable
     - Arguments to ``startTracing()``
     - Description
   * - ``OTEL_TRACE_ENABLED``
     -  Not applicable
     - Enables tracer creation and autoinstrumentation. Default value is ``true``.
   * - ``OTEL_SERVICE_NAME``
     - ``serviceName``
     - Name of the service or application you're instrumenting. Takes precedence over the service name defined in the ``OTEL_RESOURCE_ATTRIBUTES`` variable.
   * - ``OTEL_RESOURCE_ATTRIBUTES``
     - Not applicable
     - Comma-separated list of resource attributes added to every reported span. For example, ``key1=val1,key2=val2``. 
   * - ``OTEL_SPAN_ATTRIBUTE_COUNT_LIMIT``
     - Not applicable
     - Maximum number of attributes per span. Default value is unlimited.
   * - ``OTEL_SPAN_EVENT_COUNT_LIMIT``
     - Not applicable
     - Maximum number of events per span. Default value is unlimited.
   * - ``OTEL_SPAN_LINK_COUNT_LIMIT``
     - Not applicable
     - Maximum number of links per span. Default value is ``1000``.
   * - ``OTEL_ATTRIBUTE_VALUE_LENGTH_LIMIT``
     - Not applicable
     - Maximum length of strings for attribute values. Values larger than the limit are truncated. Default value is ``1200``. Empty values are treated as infinity.

.. _trace-exporters-settings-nodejs:

Exporters configuration
===============================================================

The following settings control trace exporters and their endpoints:

.. list-table:: 
   :header-rows: 1

   * - Environment variable
     - startTracing argument
     - Description
   * - ``OTEL_TRACES_EXPORTER``
     - ``tracesExporter``
     - Traces exporter to use. The default value is ``otlp``. To select the Jaeger exporter, use ``jaeger-thrift-splunk``.
   * - ``OTEL_EXPORTER_OTLP_ENDPOINT``
     - ``endpoint``
     - OTLP endpoint. The default value is ``http://localhost:4317``.
   * - ``OTEL_EXPORTER_JAEGER_ENDPOINT``
     - ``endpoint``
     - Jaeger endpoint. The default value is ``http://localhost:9080/v1/trace``.

To send data directly to Splunk Observability Cloud, set the access token as an environment variable or as an argument to ``startTracing()``. For example, the following export commands configure the Splunk Distribution of OpenTelemetry JS to export data to the Observability Cloud ingest endpoint:

.. tabs::

   .. code-tab:: shell Linux
      
      export SPLUNK_ACCESS_TOKEN=<access_token>
      export OTEL_TRACES_EXPORTER=jaeger-thrift-splunk
      export OTEL_EXPORTER_JAEGER_ENDPOINT=https://ingest.<realm>.signalfx.com/v2/trace

   .. code-tab:: shell Windows PowerShell

      $env:SPLUNK_ACCESS_TOKEN=<access_token>
      $env:OTEL_TRACES_EXPORTER=jaeger-thrift-splunk
      $env:OTEL_EXPORTER_JAEGER_ENDPOINT=https://ingest.<realm>.signalfx.com/v2/trace

To obtain an access token, see :ref:`admin-api-access-tokens`.

In the ingest endpoint URL, ``realm`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``. 

.. _trace-propagation-configuration-nodejs:

Propagators configuration
=======================================================

The following settings control trace propagation:

.. list-table:: 
   :header-rows: 1

   * - Environment variable
     - Arguments to ``startTracing()``
     - Description
   * - ``OTEL_PROPAGATORS``
     - ``propagators``
     - Comma-separated list of propagators you want to use. The default value is ``tracecontext,baggage``. You can find the list of supported propagators in the OpenTelemetry documentation.

For backward compatibility with the SignalFx Tracing Library for Node.js, use the b3multi trace propagator:

.. tabs::

   .. code-tab:: shell Linux

      export OTEL_PROPAGATORS=b3multi
   
   .. code-tab:: shell Windows PowerShell

      $env:OTEL_PROPAGATORS=b3multi

.. _server-trace-information-nodejs:

Server trace information
==============================================

To connect Real User Monitoring (RUM) requests from mobile and web applications with server trace data, enable Splunk trace response headers by setting the following environment variable:

.. tabs::

   .. code-tab:: shell Linux
   
      export SPLUNK_TRACE_RESPONSE_HEADER_ENABLED=true
   
   .. code-tab:: shell Windows PowerShell

      $env:SPLUNK_TRACE_RESPONSE_HEADER_ENABLED=true

When you set this environment variable, your application instrumentation adds the following response headers to HTTP responses.

.. code-block::

   Access-Control-Expose-Headers: Server-Timing
   Server-Timing: traceparent;desc="00-<serverTraceId>-<serverSpanId>-01"

The ``Server-Timing`` header contains the ``traceId`` and ``spanId`` in ``traceparent`` format. See the following W3C documents for more information about the ``Server-Timing`` header:

-  https://www.w3.org/TR/server-timing
-  https://www.w3.org/TR/trace-context/#traceparent-header

.. _metrics-configuration-nodejs:

Metrics configuration
===============================================================

The following settings enable runtime metrics collection:

.. list-table:: 
   :header-rows: 1

   * - Environment variable
     - startMetrics argument
     - Description
   * - ``SPLUNK_METRICS_ENABLED``
     - ``enabled``
     - Enables runtime metrics collection. The default value is ``false``. For more information on Node metrics, see :ref:`nodejs-otel-metrics`.
   * - ``SPLUNK_METRICS_ENDPOINT``
     - ``endpoint``
     - The metrics endpoint. The default value is ``http://localhost:9943``.
   * - ``SPLUNK_METRICS_EXPORT_INTERVAL``
     - ``exportInterval``
     - The interval, in milliseconds, of metrics collection and exporting. The default value is ``5000``.

.. note:: To pass settings as arguments, use the ``startMetrics()`` function.

Configuring an existing metrics client to send custom metrics
---------------------------------------------------------------------

You can use an existing SignalFx client for sending custom metrics instead of creating and configuring a new one.

To configure an existing client, pass the following data to the ``startMetrics()`` function:

- ``signalfx``: A JavaScript object with optional `client` and `dimensions` fields. The `dimensions` object adds a predefined dimension for each data point. The format for `dimensions` is `{key: value, ...}`.

The following is a list of dimensions added by default:

   - ``service``: See ``serviceName`` in :ref:`trace-configuration-nodejs`.
   - ``metric_source``: ``splunk-otel-js``
   - ``node_version``: ``process.versions.node``, for example ``16.10.0``