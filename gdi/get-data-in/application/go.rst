.. _get-started-go:

*****************************
Instrument a Go application
*****************************

.. meta::
   :description: Instrument a Go application to export metrics and spans to Splunk Observability Cloud.

The SignalFx Tracing Library automatically instruments your Go application to capture and report distributed traces to SignalFx with an OpenTracing-compatible tracer. The tracer has constant sampling and reports every span. Where applicable, context propagation uses B3 headers. The library supports Go 1.12 and higher, and requires Go Modules.

For all available configuration options and their default values, see :new-page:`the README file on GitHub <https://github.com/signalfx/signalfx-go-tracing#readme>`.

Start the integration
========================

To start a Go integration, follow these steps:

1. In the Observability Cloud main menu, select :strong:`Data Setup`.

2. In the :strong:`CATEGORIES` menu, select :strong:`APM Instrumentation`.

3. Click :strong:`Go`.

4. Click :strong:`Add Connection`. The integration guided setup appears.

5. Follow the steps in the guided setup.

6. At the end of the guided setup, go to APM to see a live view of your data flowing into the application.