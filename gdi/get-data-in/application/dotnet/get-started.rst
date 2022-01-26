.. _get-started-dotnet:

************************************************************
Instrument a .NET application for Splunk Observability Cloud
************************************************************


.. meta::
   :description: Instrument a .NET application to export metrics and spans to Splunk Observability Cloud.

.. toctree::
   :hidden:

   Requirements <dotnet-requirements>
   Instrument a .NET application <instrumentation/instrument-dotnet-application>
   Manual instrumentation <instrumentation/manual-dotnet-instrumentation>
   Advanced configuration <configuration/advanced-dotnet-configuration>
   Troubleshooting <troubleshooting/common-dotnet-troubleshooting>

The SignalFx Instrumentation for .NET provides automatic instrumentation for popular .NET libraries and frameworks to collect and send telemetry data to Splunk Observability Cloud.

.. raw:: html

  <embed>
    <h2>Features of the SignalFx Instrumentation for .NET<a name="features" class="headerlink" href="#features" title="Permalink to this headline">¶</a></h2>
  </embed>

The SignalFx Instrumentation for .NET provides the following features:

- Collection and reporting of all spans
- B3 and W3C headers for context propagation
- Zipkin trace exporter to send spans as JSON
- Support for existing custom instrumentation through OpenTracing
- Semantic conventions inspired by the OpenTelemetry standards

.. raw:: html

  <embed>
    <h2>Get started<a name="get-started" class="headerlink" href="#get-started" title="Permalink to this headline">¶</a></h2>
  </embed>

To instrument your .NET application, follow these steps:

#. Check compatibility and requirements. See :ref:`dotnet-requirements`.
#. Instrument your .NET application. See :ref:`instrument-dotnet-applications`.
#. Configure your instrumentation. See :ref:`advanced-dotnet-configuration`.