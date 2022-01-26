.. _profiling-troubleshooting:

*****************************************************************
Troubleshoot AlwaysOn Profiling
*****************************************************************

.. meta:: 
   :description: If you have instrumented an application but are not seeing profiling data in Splunk APM, use the following guidelines to troubleshoot AlwaysOn Profiling.

If you have instrumented an application but are not seeing profiling data in Splunk APM, use the following guidelines to troubleshoot AlwaysOn Profiling:

- :ref:`profiler-no-data-issue`
- :ref:`profiling-ui-not-visible`
- :ref:`profiler-language-specific-troubleshooting`

.. _profiler-no-data-issue:

No profiling data in Splunk Observability Cloud
==================================================

If profiling data does not appear in Observability Cloud, do the following:

Check that you've instrumented your application
----------------------------------------------------

AlwaysOn Profiling data can be extracted only if you've instrumented your application or service for Splunk APM. If the APM instrumentation is not loading or isn't working, the profiler cannot work.

To solve this, check that your application is instrumented and sending trace data to APM. See :ref:`profiling-setup-step-instrument`. 

Check the OpenTelemetry Collector configuration
-------------------------------------------------

If the Splunk OpenTelemetry Collector isn't configured to send logs to Observability Cloud using the Splunk HEC exporter, it drops profiling data.

To solve this issue, check that the configuration for the Splunk OpenTelemetry Collector has an OTLP receiver and a Splunk HEC exporter for the profiling pipeline. See :ref:`profiling-pipeline-setup`.

.. note:: Make sure that you're running Splunk OpenTelemetry Collector version 0.34 or higher.

Check that you've enabled AlwaysOn Profiling
-------------------------------------------------

Depending on the programming language, you can enable AlwaysOn Profiling by setting a system property, a function argument, or an environment variable. System properties and function arguments always take precedence. If the profiler is not enabled, Observability Cloud can’t receive profiling data.

To solve this issue, check that you've enabled the profiler. See :ref:`profiling-setup-enable-profiler`.

.. _profiling-ui-not-visible:

AlwaysOn Profiling is not accessible in Observability Cloud
============================================================

If you're sending profiling data to Observability Cloud but can't see AlwaysOn Profiling in Splunk APM, your organization might be lacking the profiler entitlement. 

To solve this issue, reach out to Splunk Support to request they enable the AlwaysOn Profiling feature.

.. _profiler-language-specific-troubleshooting:

Instrumentation-specific troubleshooting
============================================

Some profiler issues might be specific to the APM instrumentation. See the following instructions to troubleshoot instrumentation-specific issues:

- :ref:`java-profiler-issues`