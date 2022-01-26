.. _profiling-intro:

***************************************************
Introduction to AlwaysOn Profiling for Splunk APM
***************************************************

.. meta:: 
   :description: AlwaysOn Profiling continuously collects stack traces so that you can discover which lines in your code are slowing down or crashing your systems as a way to reduce mean time to resolution (MTTR). As part of Splunk APM, AlwaysOn Profiling provides context to spans produced by instrumented applications.

AlwaysOn Profiling continuously collects stack traces so that you can discover which lines in your code are slowing down or crashing your systems as a way to reduce mean time to resolution (MTTR). As part of Splunk APM, AlwaysOn Profiling provides context to spans produced by instrumented applications.

After you get profiling data into Observability Cloud, you can explore stack traces directly from APM and visualize the performance of each component using the flame graph. By identifying bottlenecks and inefficiencies visually, AlwaysOn Profiling can help you understand the behavior of your application's code.

What you can do with AlwaysOn Profiling
=============================================

Here are some of the things you can do with AlwaysOn Profiling for Splunk APM:

- Perform continuous profiling of your applications. The profiler is always on once you enable it.
- Collect code performance context and link it to trace data.
- Analyze code bottlenecks that impact service performance.
- Identify inefficiencies that increase the need for scaling up cloud resources.

Here are some of the typical issues that AlwaysOn Profiling can identify:

- Slow or inefficient database queries
- Thread locks
- Thread pool starvation
- File system bottlenecks
- Slow calls to external services

For a sample use case, see :ref:`profiling-use-case`.

Get AlwaysOn Profiling data into Splunk APM
==============================================================

To get started, instrument your application or service for Splunk APM and enable AlwaysOn Profiling.

See :ref:`get-data-in-profiling` for instructions.