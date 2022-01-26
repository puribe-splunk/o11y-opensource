.. _get-started-nodejs:

************************************************************
Instrument a Node application for Splunk Observability Cloud
************************************************************

.. meta::
   :description: Instrument a Node.js application to export metrics and spans to Splunk Observability Cloud.

.. toctree::
   :hidden:

   Requirements <nodejs-otel-requirements>
   Instrument a Node application <instrumentation/instrument-nodejs-application>
   Connect trace data with logs <instrumentation/connect-traces-logs>
   Runtime and custom metrics <configuration/nodejs-otel-metrics>
   Advanced configuration <configuration/advanced-nodejs-otel-configuration>
   Troubleshooting <troubleshooting/common-nodejs-troubleshooting>
   About Splunk OTel JS <splunk-nodejs-otel-distribution>
   Migrate from the SFx Tracing Library <troubleshooting/migrate-signalfx-nodejs-agent-to-otel>

The Splunk Distribution of OpenTelemetry JS provides a Node.js SDK that automatically adds APM instrumentation to your Node application. The instrumentation captures distributed traces and sends them to Splunk Observability Cloud.

To instrument your Node.js application, follow these steps:

#. Check compatibility and requirements. See :ref:`nodes-requirements`.
#. Instrument your Node application. See :ref:`instrument-nodejs-applications`.
#. Configure your instrumentation. See :ref:`configure-nodejs-instrumentation`.

For more information, see :ref:`splunk-nodejs-otel-dist`.

.. note:: The SignalFx Tracing Library for NodeJS is deprecated. To learn how to migrate to the Splunk Distribution of OpenTelemetry JS, see :ref:`migrate-signalfx-nodejs-agent-to-otel`.
