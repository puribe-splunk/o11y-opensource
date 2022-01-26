.. _common-java-troubleshooting:

****************************************************************
Troubleshoot Java instrumentation for Splunk Observability Cloud
****************************************************************

.. meta::
   :description: If your instrumented Java application is not sending data to Splunk Observability Cloud, or data is missing, follow these steps to identify the issue and solve it.

When you instrument a Java application using the Splunk Distribution of OpenTelemetry Java and you don't see your data in Observability Cloud, review these solutions.

.. _basic-java-troubleshooting:

Steps for troubleshooting Java OpenTelemetry issues
=======================================================

The following steps can help you troubleshoot Java agent issues:

#. :ref:`enable-java-debug-logging`
#. :ref:`verify-runtime-status`

.. include:: /_includes/troubleshooting-steps.rst

.. _enable-java-debug-logging:

Enable debug logging
-------------------------------------------------------

Debug logging is a special execution mode that outputs more information about the Java agent of the Splunk Distribution of OpenTelemetry Java. This can help you troubleshoot Java instrumentation issues.

To turn on the debug logging for the agent, pass the following argument when running your application:

``-Dotel.javaagent.debug=true``

When you run the agent with debug logging enabled, debug information is sent to the console as ``stderr``. Debug log entries look like the following example:

.. code-block:: bash

   ...
   [opentelemetry.auto.trace 2021-10-10 10:57:05:814 +0200] [main] DEBUG io.opencensus.tags.Tags - <Could not load lite implementation for TagsComponent, now using default implementation for TagsComponent.3>
   [opentelemetry.auto.trace 2021-10-10 10:57:05:722 +0200] [main] DEBUG io.grpc.netty.shaded.io.netty.util.internal.PlatformDependent0 - direct buffer constructor: unavailable
   ...

While not all debug entries are relevant to the issue affecting your Java instrumentation, the root cause is likely to appear in your debug log.

.. note:: Enable debug logging only when needed. Debug mode requires more resources.

.. _verify-runtime-status:

Check the status of the runtime
----------------------------------------------

Run the ``jps -lvm`` command to verify that the Java runtime has started. The output is a list of all the Java Virtual Machines (JVM) currently running. Make sure the JVM you instrumented appears among them.

In the following example, the first entry shows a JVM running the agent with ``-javaagent``:

.. code-block:: bash

   37602 target/spring-petclinic-2.4.5.jar -javaagent:./splunk-otel-javaagent.jar -Dotel.resource.attributes=service.name=pet-store-demo,deployment.environment=prod,service.version=1.2.0 -Dotel,javaagent.debug=true
   38262 jdk.jcmd/sun.tools.jps.Jps -lvm -Dapplication.home=/usr/lib/jvm/java-16-openjdk-amd64 -Xms8m -Djdk.module.main=jdk.jcmd

If the instrumented JVM doesn't appear in the list, check the JVM or application logs to find the cause of the problem. Also check that the additional startup parameters are correctly passed to the runtime. See :ref:`instrument-java-applications` to learn more about startup parameters.

.. _java-instrumentation-issues:

Library instrumentation issues
==============================================================

If you find an issue with a specific instrumentation of a library, or suspect there might be an issue affecting that instrumentation, disabling it can help you troubleshoot the Java agent.

To disable a specific library instrumentation, add the following argument:

``-Dotel.instrumentation.<name>.enabled=false``

Replace ``<name>`` with the corresponding instrumentation from the OpenTelemetry Java instrumentation on GitHub at https://github.com/open-telemetry/opentelemetry-java-instrumentation/blob/main/docs/suppressing-instrumentation.md#suppressing-specific-agent-instrumentation.

.. _java-class-instrumentation-issues:

Class instrumentation issues
-------------------------------------------------

You can prevent specific classes from being instrumented. Excluded classes don't send spans, which is useful for muting specific classes or packages.

To disable instrumentation for a class, set the ``otel.javaagent.exclude-classes`` system property or the ``OTEL_JAVAAGENT_EXCLUDE_CLASSES`` environment variable to the name of the class or classes.

You can enter multiple classes. For example, ``my.package.MyClass,my.package2.*``.

.. warning:: Disabling instrumentation for specific classes can have unintended side effects. Use this feature with caution.

.. _java-trace-exporter-issues:

Trace exporter issues
=====================================================

By default, the Splunk Distribution of OpenTelemetry Java uses the OTLP exporter. Any issue affecting the export of traces produces an error in the debug logs.

OTLP can't export spans
-----------------------------------------------------

The following error in the logs means that the agent can't send trace data to the OpenTelemetry Collector:

.. code-block:: bash

   [BatchSpanProcessor_WorkerThread-1] ERROR io.opentelemetry.exporter.otlp.trace.OtlpGrpcSpanExporter - Failed to export spans. Server is UNAVAILABLE. Make sure your collector is running and reachable from this network. Full error message:UNAVAILABLE: io exception

To troubleshoot the lack of connectivity between the OTLP exporter and the OTel Collector, try the following steps:

#. Make sure that ``otel.exporter.otlp.endpoint`` points to the correct OpenTelemetry Collector instance host.
#. Check that your OTel Collector instance is configured and running. See :ref:`otel-collector-tshoot`.
#. Check that the OTLP gRPC receiver is enabled in the OTel Collector and plugged into the traces pipeline.
#. Check that the OTel Collector points to the following address: ``http://<host>:4317``. Verify that your URL is correct.

Channel pipeline error
-------------------------------------------------------------------

If you're seeing the following error in your logs, it might mean that the Java agent is trying to send trace data to the Splunk ingest API endpoint, which is not yet supported by OTLP:

.. code-block:: bash

   [grpc-default-executor-1] ERROR io.opentelemetry.exporter.otlp.trace.OtlpGrpcSpanExporter - Failed to export spans. Server is UNAVAILABLE. Make sure your collector is running and reachable from this network. Full error message:UNAVAILABLE: io exception
   Channel Pipeline: [SslHandler#0, ProtocolNegotiators$ClientTlsHandler#0, WriteBufferingAndExceptionHandler#0, DefaultChannelPipeline$TailContext#0]

To solve this issue, use the Jaeger exporter instead. See :ref:`trace-exporters-settings-java`.

Jaeger can't export spans
------------------------------------------------------

The following warnings in your logs mean that the Java agent can't send trace data to the Smart Agent, the OTel Collector, or Splunk Cloud Platform using the Jaeger exporter:

.. code-block:: bash

   [BatchSpanProcessor_WorkerThread-1] WARN io.opentelemetry.exporter.jaeger.thrift.JaegerThriftSpanExporter - Failed to export spans
   io.jaegertracing.internal.exceptions.SenderException: Could not send 8 spans
      at io.jaegertracing.thrift.internal.senders.HttpSender.send(HttpSender.java:69)
      ...
   Caused by: java.net.ConnectException: Failed to connect to localhost/0:0:0:0:0:0:0:1:9080
      at okhttp3.internal.connection.RealConnection.connectSocket(RealConnection.java:265)
      ...
   Caused by: java.net.ConnectException: Connection refused (Connection refused)
      ...

To troubleshoot the lack of connectivity between Jaeger and Splunk Observability Cloud, try the following steps:

1. Make sure that ``otel.exporter.jaeger.endpoint`` points to a Smart Agent or OpenTelemetry Collector instance, or to the Splunk Ingest URL. See :new-page:`Send data measurements <https://dev.splunk.com/observability/docs/apibasics/send_data_basics#Send-data-measurements>` in the Splunk Developer documentation.
2. Check that the Smart Agent or the OTel Collector instance is configured and running.
3. Check that the Jaeger Thrift HTTP receiver is enabled and plugged into the traces pipeline. See :ref:`otel-exposed-endpoints`.
4. Check that the endpoint is correct. The Smart Agent and OpenTelemetry Collector use different ports and paths by default. For the Jaeger receiver, the Smart Agent uses ``http://<host>:9080/v1/trace``, while the OTel Collector uses ``http://<host>:14268/api/traces``.

401 error when sending spans
--------------------------------------------------------

If you send traces directly to Observability Cloud and receive a 401 error code, the authentication token specified in ``SPLUNK_ACCESS_TOKEN`` is invalid. The following are possible reasons:

- The value is null.
- The value is not a well-formed token.
- The token is not an access token that has ``authScope`` set to ingest.

Make sure that you're using a valid Splunk access token when sending data directly to your Splunk platform instance. See :ref:`admin-api-access-tokens`.

.. _java-metrics-exporter-issues:

Metrics exporter issues
===============================================================

If you see the following warning in your logs, it means that the Java agent can't send metrics to your OTel Collector, Smart Agent, or to the Splunk platform endpoints:

.. code-block:: bash

   [signalfx-metrics-publisher] WARN com.splunk.javaagent.shaded.io.micrometer.signalfx.SignalFxMeterRegistry - failed to send metrics: Unable to send data points

To troubleshoot connectivity issues affecting application metrics, try the following steps:

1. Make sure that ``splunk.metrics.endpoint`` points to the correct host.
2. Check that the Smart Agent or the OTel Collector instance is configured and running.
3. Check that the Smart Agent and the OpenTelemetry Collector are using the correct ports for the SignalFx receiver. The Collector uses ``http://<host>:9943``, and the Agent uses ``http://<host>:9080/v2/datapoint``.
4. Make sure that you're using a valid Splunk access token when sending data directly to your Splunk platform instance. See :ref:`admin-api-access-tokens`.

.. note:: Metric collection for Java using OpenTelemetry instrumentation is still experimental.

.. _java-profiler-issues:

Troubleshoot AlwaysOn Profiling for Java
===============================================================

Follow these steps to troubleshoot issues with AlwaysOn Profiling:

Check that AlwaysOn Profiling is enabled
----------------------------------------------------------------

The Java agent logs the string ``JFR profiler is active`` at startup using an ``INFO`` message. To check whether AlwaysOn Profiling is enabled, search your logs for the following string:

.. code-block:: bash 

   [otel.javaagent 2021-09-28 18:17:04:246 +0000] [main] INFO com.splunk.opentelemetry.profiler.JfrActivator - JFR profiler is active.

If the string does not appear, make sure that you've enabled the profiler by setting the ``splunk.profiler.enabled`` system property or the ``SPLUNK_PROFILER_ENABLED`` environment variable. See :ref:`profiling-configuration-java`.

Check the AlwaysOn Profiling configuration
----------------------------------------------------------------

If AlwaysOn Profiling is not working as intended, check the configuration settings. The Java agent logs AlwaysOn Profiling's settings using INFO messages at startup. Search for the string ``com.splunk.opentelemetry.profiler.ConfigurationLogger`` to see entries like the following:

.. code-block:: shell 
      
   [otel.javaagent 2021-09-28 18:17:04:237 +0000] [main] INFO <snip> - -----------------------
   [otel.javaagent 2021-09-28 18:17:04:237 +0000] [main] INFO <snip> - Profiler configuration:
   [otel.javaagent 2021-09-28 18:17:04:238 +0000] [main] INFO <snip> -                 splunk.profiler.enabled : true
   [otel.javaagent 2021-09-28 18:17:04:239 +0000] [main] INFO <snip> -               splunk.profiler.directory : .
   [otel.javaagent 2021-09-28 18:17:04:244 +0000] [main] INFO <snip> -      splunk.profiler.recording.duration : 20s
   [otel.javaagent 2021-09-28 18:17:04:244 +0000] [main] INFO <snip> -              splunk.profiler.keep-files : false
   [otel.javaagent 2021-09-28 18:17:04:245 +0000] [main] INFO <snip> -           splunk.profiler.logs-endpoint : null
   [otel.javaagent 2021-09-28 18:17:04:245 +0000] [main] INFO <snip> -             otel.exporter.otlp.endpoint : http://collector:4317
   [otel.javaagent 2021-09-28 18:17:04:245 +0000] [main] INFO <snip> -            splunk.profiler.tlab.enabled : false
   [otel.javaagent 2021-09-28 18:17:04:246 +0000] [main] INFO <snip> -   splunk.profiler.period.jdk.threaddump : null
   [otel.javaagent 2021-09-28 18:17:04:246 +0000] [main] INFO <snip> - -----------------------

JFR is not available error
-----------------------------------------------

If your Java Virtual Machine does not support Java Flight Recording (JFR), the profiler logs a warning at startup with the message ``Java Flight Recorder (JFR) is not available in this JVM. Profiling is disabled.``. 

To use the profiler, upgrade your JVM version to 8u262 or higher. See :ref:`java-otel-requirements`.

AlwaysOn Profiling data and logs don't appear in Observability Cloud
--------------------------------------------------------------------

Collector configuration issues might prevent AlwaysOn Profiling data and logs from appearing in Splunk Observability Cloud.

To solve this issue, do the following:

- Find the value of ``splunk.profiler.logs-endpoint`` and ``otel.exporter.otlp.endpoint`` in the startup log messages. Check that a collector is running using that endpoint and that the application host or container can resolve any host names and connect to the OTLP port.
- Make sure that you're running the Splunk OpenTelemetry Collector and that the version is 0.34 or higher. Other collector distributions might not be able to route the log data that contains profiling data.
- A custom configuration might override settings that let the collector handle profiling data. Make sure to configure an ``otlp`` receiver and a ``splunk_hec`` exporter with correct token and endpoint fields. The ``profiling`` pipeline must use the OTLP receiver and Splunk HEC exporter you've configured.

The following snippet contains a sample ``profiling`` pipeline:

.. code-block:: yaml

   receivers:
     otlp:
        protocols:
           grpc:

   exporters:
     splunk_hec:
        token: "${SFX_TOKEN}"
        endpoint: "https://ingest.${SFX_REALM}.signalfx.com/v1/log"
     logging/info:
        loglevel: info

   processors:
     batch:

   service:
     pipelines:
        profiling:
           receivers: [otlp]
           processors: [batch]
           exporters: [logging/info, splunk_hec]

Loss of profiling data or gaps in profiling data
-------------------------------------------------------------

If there are less than 100 megabytes of space available for the Java Virtual Machine, AlwaysOn Profiling enables the recording escape hatch, which appears in the logs as ``com.splunk.opentelemetry.profiler.RecordingEscapeHatch``. The escape hatch drops all logs with profiling data until more space is available.

To avoid the loss of profiling data due to the escape hatch, provide enough resources to the JVM.

.. _disable-java-agent-logs:

Disable all Java agent logs
============================================================

By default, the Splunk Java agent outputs logs to the console. In certain situations you might want to silence the output of the agent so as not to clutter the system logs. 

To run the Java agent in silent mode, add the following argument:

.. code-block:: bash
   
   -Dio.opentelemetry.javaagent.slf4j.simpleLogger.defaultLogLevel=off

