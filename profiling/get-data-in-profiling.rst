.. _get-data-in-profiling:

***************************************************
Get AlwaysOn Profiling data into Splunk APM
***************************************************

.. meta:: 
   :description: Follow these instructions to get profiling data into Splunk APM using AlwaysOn Profiling.

Follow these instructions to get profiling data into Splunk APM using AlwaysOn Profiling:

1. :ref:`profiling-setup-step-collector`.
2. :ref:`profiling-setup-step-instrument`.
3. :ref:`profiling-setup-enable-profiler`.
4. :ref:`profiling-check-data-coming-in`.

.. _profiling-setup-step-collector:

Set up the Splunk OpenTelemetry Collector
=============================================================

To get data into Splunk AlwaysOn Profiling, you must have a Splunk OpenTelemetry (OTel) collector set up and working. See :ref:`otel-intro`.

.. _profiling-pipeline-setup:

Configure a pipeline for profiling data
-------------------------------------------------------------

AlwaysOn Profiling requires the Splunk HTTP Event Collector (HEC) exporter to send profiling data to Splunk Observability Cloud. In the Splunk OpenTelemetry Collector configuration file, make sure that a profiling pipeline exists with an OTLP gRPC receiver and a Splunk HEC exporter. For more information, see :ref:`otel-configuration`.

The following example shows you how to configure a pipeline in the ``agent-config.yaml`` file. Set the ``SPLUNK_ACCESS_TOKEN`` environment variable to a valid access token. See :ref:`admin-org-tokens` for more information.

.. code-block:: yaml

   receivers:
     otlp:
       protocols:
         grpc:

   exporters:
     splunk_hec:
       token: "${SPLUNK_ACCESS_TOKEN}"
       endpoint: "https://ingest.${SPLUNK_REALM}.signalfx.com/v1/log"
     logging/info:
       loglevel: info

   service:
     pipelines:
       logs/profiling:
         receivers: [otlp]
         processors: [memory_limiter, batch]
         exporters: [logging/info, splunk_hec]

.. _profiling-setup-step-instrument:

Instrument your application or service
=============================================================

AlwaysOn Profiling requires APM tracing data to correlate stack traces to your application requests. To instrument your application for Splunk APM, follow the steps for the appropriate programming language: 

.. list-table::
   :header-rows: 1
   :widths: 20, 40, 40

   * - :strong:`Language`
     - :strong:`Available instrumentation`
     - :strong:`Documentation`

   * - Java
     - Splunk Distribution of OpenTelemetry Java
     - :ref:`instrument-java-applications`

.. note:: See :ref:`apm-data-retention` for information on Profiling data retention periods.

.. _profiling-setup-enable-profiler:

Enable AlwaysOn Profiling
=============================================================

After you've instrumented your service for Observability Cloud and checked that APM data is getting into Splunk APM, enable the profiler.

To enable AlwaysOn Profiling, follow the steps for the appropriate programming language: 

.. tabs::

   .. group-tab:: Java

      - Enable the profiler by setting the ``splunk.profiler.enabled`` system property or the ``SPLUNK_PROFILER_ENABLED`` environment variable to ``true``.
      - Make sure that the ``splunk.profiler.logs-endpoint`` system property or the ``SPLUNK_PROFILER_LOGS_ENDPOINT`` environment variable point to ``http://localhost:4317``.
      
      The following example shows how to enable the profiler using the system property:

      .. code-block:: bash
         :emphasize-lines: 2,3

         java -javaagent:./splunk-otel-javaagent.jar \
         -Dsplunk.profiler.enabled=true \
         -Dsplunk.profiler.logs-endpoint=https://localhost:4317 \
         -jar <your_application>.jar

      For more configuration options, see :ref:`profiling-configuration-java`.

.. _profiling-check-data-coming-in:

Check that Observability Cloud is receiving profiling data
=============================================================

After you set up and enable AlwaysOn Profiling, check that profiling data is coming in:

1. Open Splunk Observability Cloud.
2. Go to :guilabel:`APM` and select :guilabel:`AlwaysOn Profiling`.
3. Check that your spans have call stacks available. See :ref:`spans-stack-traces` to learn how to locate and browse call stacks. 

You can also browse all stack traces coming from your application in the flame graph. See :ref:`flamegraph-howto` for more information about the flame graph.
