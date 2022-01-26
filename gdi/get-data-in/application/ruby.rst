.. _get-started-ruby:

********************************
Instrument a Ruby application
********************************

.. meta::
   :description: Instrument a Ruby application to export metrics and spans to Splunk Observability Cloud.


The SignalFx Tracing Library for Ruby helps you instrument Ruby applications with the OpenTracing API to capture and report distributed traces to Splunk Observability Cloud.

This library consists of an auto-instrumenter that works with OpenTracing community-provided instrumentations and provides a bootstrap utility to help install instrumentations. The library also configures and uses a :new-page:`Jaeger tracer <https://github.com/salemove/jaeger-client-ruby>` to send trace data to Splunk Observability Cloud.

For all available configuration options and their default values, see :new-page:`the README file <https://github.com/signalfx/signalfx-ruby-tracing#readme>`.

Start the integration
========================

To start a Ruby integration, follow these steps:

1. In the Observability Cloud main menu, select :strong:`Data Setup`.

2. In the :strong:`CATEGORIES` menu, select :strong:`APM Instrumentation`.

3. Click :strong:`Ruby Tracing`.

4. Click :strong:`Add Connection`. The integration guided setup appears.

5. Follow the steps in the guided setup.

6. At the end of the guided setup, go to APM to see a live view of your data flowing into the application.


Lambda wrappers
==================

You can also instrument AWS Lambda functions with Ruby using SignalFx Lambda wrappers or instrumentation libraries. See :new-page:`SignalFx Ruby Lambda Wrapper <https://github.com/signalfx/lambda-ruby>` for more information.
