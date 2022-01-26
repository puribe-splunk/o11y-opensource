.. _advanced-java-otel-configuration:

********************************************************************
Configure the Java agent for Splunk Observability Cloud
********************************************************************

.. meta:: 
   :description: Configure the agent of the Splunk Distribution of OpenTelemetry Java to suit most of your instrumentation needs, like correlating traces with logs, enabling custom sampling, and more.

You can configure the Java agent from the Splunk Distribution of OpenTelemetry Java to suit most of your instrumentation needs. In most cases, modifying the basic configuration is enough to get started. More advanced settings are also available. 

The following sections describe all available settings for configuring the Java Virtual Machine (JVM) agent, including options for enabling new features that are unique to the Splunk Distribution of OpenTelemetry Java.

.. _configuration-methods-java:

Configuration methods
===========================================================

You can change the agent settings in two ways:

- Set an environment variable. For example:

   .. tabs::

      .. code-tab:: shell Linux

         export OTEL_SERVICE_NAME=my-java-app

      .. code-tab:: shell Windows PowerShell

         $env:OTEL_SERVICE_NAME=my-java-app

- Add a system property as runtime parameter. For example:

   .. code-block:: shell
         :emphasize-lines: 2

         java -javaagent:./splunk-otel-javaagent.jar \
         -Dotel.service.name=<my-java-app> \
         -jar <myapp>.jar

Environment variables are the preferred way of configuring OpenTelemetry agents. System properties, if specified, override existing environment variables.

.. _main-java-agent-settings:

General settings
=========================================================================

The following settings are specific to the Splunk Distribution of OpenTelemetry Java:

.. list-table:: 
   :header-rows: 1

   * - System property
     - Environment variable
     - Description
   * - ``splunk.access.token``
     - ``SPLUNK_ACCESS_TOKEN``
     - Splunk authentication token that lets exporters send data directly to Splunk Observability Cloud. Unset by default. Not required unless you need to send data to the Observability Cloud ingest endpoint. See :ref:`admin-tokens`.
   * - ``splunk.trace-response-header.enabled``
     - ``SPLUNK_TRACE_RESPONSE_HEADER_ENABLED``
     - Enables the addition of server trace information to HTTP response headers. For more information, see :ref:`server-trace-information-java`. The default value is ``true``.

.. _trace-configuration-java:

Trace configuration
=======================================================

The following settings control tracing limits and attributes:

.. list-table:: 
   :header-rows: 1

   * - System property
     - Environment variable
     - Description
   * - ``otel.service.name``
     - ``OTEL_SERVICE_NAME``
     - Name of the service or application you're instrumenting. Takes precedence over the service name defined in the ``OTEL_RESOURCE_ATTRIBUTES`` variable.
   * - ``otel.resource.attributes``
     - ``OTEL_RESOURCE_ATTRIBUTES``
     - Comma-separated list of resource attributes added to every reported span. For example, ``key1=val1,key2=val2``. 
   * - ``otel.instrumentation.common.peer-service-mapping``
     - ``OTEL_INSTRUMENTATION_COMMON_PEER_SERVICE_MAPPING``
     - Used to add a ``peer.service`` attribute by specifying a comma-separated list of mapping from hostnames or IP addresses. For example, if set to ``1.2.3.4=cats-service,dogs-service.serverlessapis.com=dogs-api``, requests to ``1.2.3.4`` have a ``peer.service`` attribute of ``cats-service`` and requests to ``dogs-service.serverlessapis.com`` have one of ``dogs-api``.
   * - ``otel.instrumentation.methods.include``
     - ``OTEL_INSTRUMENTATION_METHODS_INCLUDE``
     -  Adds ``@WithSpan`` annotation functionality for the target method string. Format is ``my.package.MyClass1[method1,method2];my.package.MyClass2[method3]``.
   * - ``otel.instrumentation.opentelemetry-annotations.exclude-methods``
     - ``OTEL_INSTRUMENTATION_OPENTELEMETRY_ANNOTATIONS_EXCLUDE_METHODS``
     - Suppresses ``@WithSpan`` instrumentation for specific methods. Format is ``my.package.MyClass1[method1,method2];my.package.MyClass2[method3]``.
   * - ``otel.span.attribute.count.limit``
     - ``OTEL_SPAN_ATTRIBUTE_COUNT_LIMIT``
     - Maximum number of attributes per span. The default value is unlimited.
   * - ``otel.span.event.count.limit``
     - ``OTEL_SPAN_EVENT_COUNT_LIMIT``
     - Maximum number of events per span. The default value is unlimited.
   * - ``otel.span.link.count.limit``
     - ``OTEL_SPAN_LINK_COUNT_LIMIT``
     - Maximum number of links per span. The default value is ``1000``.

.. _profiling-configuration-java:

Java settings for AlwaysOn Profiling
===============================================

The following settings control the AlwaysOn Profiling feature for the Java agent:

.. list-table:: 
   :header-rows: 1

   * - System property
     - Environment variable
     - Description
   * - ``splunk.profiler.enabled``
     - ``SPLUNK_PROFILER_ENABLED``
     - Enables AlwaysOn Profiling. The default value is ``false``.
   * - ``splunk.profiler.logs-endpoint``
     - ``SPLUNK_PROFILER_LOGS_ENDPOINT``
     - The collector endpoint for profiler logs. By default, it takes the value of ``otel.exporter.otlp.endpoint``, which has a default value of ``localhost:4317``.
   * - ``splunk.profiler.directory``
     - ``SPLUNK_PROFILER_DIRECTORY``
     -  The location of the JDK Flight Recorder files. The default value is the local directory ``.``.
   * - ``splunk.profiler.recording.duration``
     - ``SPLUNK_PROFILER_RECORDING_DURATION``
     - The duration of the recording unit, in seconds. You can define duration in the form ``<number><unit>``, where the unit can be ``ms``, ``s``, ``m``, ``h``, or ``d``. The default interval is ``20s``. If you enter a number but not a unit, the default unit is assumed to be ``ms``.
   * - ``splunk.profiler.keep-files``
     - ``SPLUNK_PROFILER_KEEP_FILES``
     -  Whether to preserve JDK Flight Recorder (JFR) files or not. The default value is ``false``, which means that JFR files are deleted after processing.
   * - ``splunk.profiler.call.stack.interval``
     - ``SPLUNK_PROFILER_CALL_STACK_INTERVAL``
     - Frequency with which call stacks are sampled. The default value is 10 seconds.
   * - ``splunk.profiler.memory.enabled``
     - ``SPLUNK_PROFILER_MEMORY_ENABLED``
     - Enables memory profiling with all the options. The default value is ``false``. To enable or disable specific memory profiling options, set their values explicitly.
   * - ``splunk.profiler.memory.sampler.interval``
     - ``SPLUNK_PROFILER_MEMORY_SAMPLER_INTERVAL``
     - Defines the sampling interval. The default value is 1. Set the value to 2 or higher to sample data every nth allocation event.
   * - ``splunk.profiler.tlab.enabled``
     - ``SPLUNK_PROFILER_TLAB_ENABLED``
     - Whether to enable TLAB memory events. The default value is the value assigned to the ``splunk.profiler.memory.enabled`` property.
   * - ``splunk.profiler.include.agent.internals``
     - ``SPLUNK_PROFILER_INCLUDE_AGENT_INTERNALS``
     - Whether to include internal Java agent call stacks. The default value is ``false``.

.. note:: For more information on AlwaysOn Profiling, see :ref:`profiling-intro`.

.. _metrics-configuration-java:

Metrics settings
===============================================

The following settings control metrics collection for the Java agent:

.. list-table:: 
   :header-rows: 1

   * - System property
     - Environment variable
     - Description
   * - ``splunk.metrics.enabled``
     - ``SPLUNK_METRICS_ENABLED``
     - Enables exporting metrics. See :ref:`java-otel-metrics-attributes` for more information. Default is ``false``.
   * - ``splunk.metrics.endpoint``
     - ``SPLUNK_METRICS_ENDPOINT``
     - The OTel collector metrics endpoint. Default is ``http://localhost:9943``.
   * - ``splunk.metrics.export.interval``
     - ``SPLUNK_METRICS_EXPORT_INTERVAL``
     - Interval between pushing metrics. You can define duration in the form ``<number><unit>``, where the unit can be ``ms``, ``s``, ``m``, ``h``, or ``d``. The default interval is ``30s``. If you enter a number but not a unit, the default unit is assumed to be ``ms``.

To configure the export to send metrics directly to Splunk ingest API, set the ``splunk.access.token`` and ``splunk.metrics.endpoint`` properties. For example:

.. tabs::

   .. code-tab:: shell Linux

      export SPLUNK_ACCESS_TOKEN=:<admin-api-access-tokens>
      export SPLUNK_METRICS_ENDPOINT=https://ingest.<realm>.signalfx.com

   .. code-tab:: shell Windows

      $env:SPLUNK_ACCESS_TOKEN=:<admin-api-access-tokens>
      $env:SPLUNK_METRICS_ENDPOINT=https://ingest.<realm>.signalfx.com

To obtain an access token, see :ref:`admin-api-access-tokens`.

In the ingest endpoint URL, ``realm`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``. 

.. note:: Metric support is experimental.

.. _trace-exporters-settings-java:

Exporters configuration
===============================================================

The following settings control trace exporters and their endpoints:

.. list-table:: 
   :header-rows: 1

   * - System property
     - Environment variable
     - Description
   * - ``otel.traces.exporter``
     - ``OTEL_TRACES_EXPORTER``
     - Traces exporter to use. You can set multiple comma-separated values. For example, ``otlp,console_span``. The default value is ``otlp``. To select the Jaeger exporter, use ``jaeger-thrift-splunk``.
   * - ``otel.exporter.otlp.endpoint``
     - ``OTEL_EXPORTER_OTLP_ENDPOINT``
     - OTLP gRPC endpoint. The default value is ``http://localhost:4317``.
   * - ``otel.exporter.jaeger.endpoint``
     - ``OTEL_EXPORTER_JAEGER_ENDPOINT``
     - Jaeger endpoint. The default value is ``http://localhost:9080/v1/trace``.

The Splunk Distribution of OpenTelemetry Java uses the OTLP gRPC span exporter by default. If you're still using the Smart Agent, or need to send traces directly to the Splunk ingest API endpoint, use the Jaeger exporter.

For example, the following export statements configure the agent to export data to the Splunk ingest API endpoint through Jaeger:

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

.. _trace-propagation-configuration-java:

Propagators configuration
=======================================================

The following settings control trace propagation:

.. list-table:: 
   :header-rows: 1

   * - System property
     - Environment variable
     - Description
   * - ``otel.propagators``
     - ``OTEL_PROPAGATORS``
     - Comma-separated list of propagators you want to use. The default value is ``tracecontext,baggage``. You can find the list of supported propagators in the OpenTelemetry documentation.

For backward compatibility with older versions of the Splunk Distribution of OpenTelemetry Java or the SignalFx Java Agent, use the b3multi trace propagator:

.. tabs::

   .. code-tab:: shell Linux

      export OTEL_PROPAGATORS=b3multi
   
   .. code-tab:: shell Windows PowerShell

      $env:OTEL_PROPAGATORS=b3multi

.. _server-trace-information-java:

Server trace information
==============================================

To connect Real User Monitoring (RUM) requests from mobile and web applications with server trace data, enable Splunk trace response headers by setting the following environment variable:

.. tabs::

   .. code-tab:: shell Linux
   
      export SPLUNK_TRACE_RESPONSE_HEADER_ENABLED=true
   
   .. code-tab:: shell Windows PowerShell

      $env:SPLUNK_TRACE_RESPONSE_HEADER_ENABLED=true

When you set this environment variable, your application instrumentation adds the following response headers to HTTP responses:

.. code-block::

   Access-Control-Expose-Headers: Server-Timing 
   Server-Timing: traceparent;desc="00-<serverTraceId>-<serverSpanId>-01"

The ``Server-Timing`` header contains the ``traceId`` and ``spanId`` parameters in ``traceparent`` format. See the following W3C documents for more information about the ``Server-Timing`` header:

-  https://www.w3.org/TR/server-timing
-  https://www.w3.org/TR/trace-context/#traceparent-header

The following server frameworks and libraries add ``Server-Timing`` information:

- Servlet API versions 2.2 to 4.X.
- Netty versions 3.8 to 4.0.

.. _other-java-settings:

Other settings
================================================

.. list-table:: 
   :header-rows: 1

   * - System property
     - Environment variable
     - Description
   * - ``otel.javaagent.enabled``
     - ``OTEL_JAVAAGENT_ENABLED``
     - Globally enables the Java agent automatic instrumentation. The default value is ``true``. Useful for disabling auto instrumentation in testing scenarios or pipelines.