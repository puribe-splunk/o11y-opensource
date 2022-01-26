.. _correlate-traces-with-logs-java:

****************************************************************
Connect Java trace data with logs for Splunk Observability Cloud
****************************************************************

.. meta:: 
   :description: You can configure Java logging libraries to include tracing attributes provided automatically by the Splunk OTel Java agent. Use the trace metadata to correlate traces with log events and explore logs in Observability Cloud.

You can configure Java logging libraries to include tracing attributes provided automatically by the Splunk OTel Java agent. Use the trace metadata to correlate traces with log events and explore logs in Observability Cloud.

.. _java-traces-logs-requirements:

Check compatibility and requirements
====================================================

The Splunk OTel Java agent supports the following logging libraries:

- Log4j 2 2.7 or higher
- Log4j 1 1.2 or higher
- Logback 1.0 or higher

.. _java-include-trace-data:

Include trace metadata in log statements
===================================================

The Splunk OTel Java agent provides the following attributes for logging libraries:

- Trace information: ``trace_id`` and ``span_id``
- Resource attributes: ``service.name`` and ``deployment.environment``

The following Logback example shows how to include trace data in log statements produced by the logging library:

.. code-block::

   logging.pattern.console = %d{yyyy-MM-dd HH:mm:ss} - %logger{36} - %msg trace_id=%X{trace_id} span_id=%X{span_id} trace_flags=%X{trace_flags} %n

Add resource attributes to your application logs
---------------------------------------------------

The Splunk Distribution of OpenTelemetry Java exposes resource attributes as system properties prefixed with ``otel.resource.``, which you can use to configure logger libraries.

The following examples show how to add Splunk OTel Java metadata to the logger configuration:

.. tabs::

   .. code-tab:: xml Log4j

      <PatternLayout>
      <pattern>service.name=${sys:otel.resource.service.name}, deployment.environment=${sys:otel.resource.deployment.environment} %m%n</pattern>
      </PatternLayout>

   .. code-tab:: xml Logback

      <pattern>service: %property{otel.resource.service.name}, env: %property{otel.resource.deployment.environment}: %m%n</pattern>

.. _explore-log-observer-java:

Explore application logs in Log Observer
==================================================

You can send Java application logs to Splunk Observability Cloud in the same way you send any other type of log data. To learn more about logs in Observability Cloud, see :ref:`logs-logs`.