.. _common-nodejs-troubleshooting:

*******************************************************************
Troubleshoot Node.js instrumentation for Splunk Observability Cloud
*******************************************************************

.. meta::
   :description: If your instrumented Node.js application is not sending data to Splunk Observability Cloud, or data is missing, follow these steps to identify the issue and solve it.

When you instrument a Node.js application using the Splunk Distribution of OpenTelemetry JS and you don't see your data in Observability Cloud, review these solutions.

.. _basic-nodejs-troubleshooting:

Steps for troubleshooting Node.js OpenTelemetry issues
=======================================================

The following steps can help you troubleshoot Node.js instrumentation issues:

#. :ref:`enable-nodejs-debug-logging`

.. include:: /_includes/troubleshooting-steps.rst

.. _enable-nodejs-debug-logging:

Enable diagnostic logging
-------------------------------------------------------

Diagnostic logs can help you troubleshoot instrumentation issues.

To output instrumentation logs to the console, add ``DiagConsoleLogger`` and ``DiagLogLevel``:

.. code-block:: javascript

   const { diag, DiagConsoleLogger, DiagLogLevel } = require("@opentelemetry/api");

   diag.setLogger(new DiagConsoleLogger(), DiagLogLevel.ALL);

Logging level is controlled by the ``OTEL_LOG_LEVEL`` environment variable. The following list shows you the possible values, ordered from least to most detailed:

- ``ERROR``: Identifies errors.
- ``WARN``: Identifies warnings.
- ``INFO``: Informational messages. This is the default level.
- ``DEBUG``: Debug log messages.
- ``VERBOSE``: Detailed trace level logging.

.. note:: Enable debug logging only when needed. Debug mode requires more resources.

.. _nodejs-trace-exporter-issues:

Trace exporter issues
=====================================================

By default, the :ref:`Splunk Distribution of OpenTelemetry JS <splunk-nodejs-otel-dist>` uses the OTLP exporter. Any issue affecting the export of traces produces an error in the debug logs.

OTLP can't export spans
-----------------------------------------------------

The following error in the logs means that the instrumentation can't send trace data to the OpenTelemetry Collector:

.. code-block::

   @opentelemetry/instrumentation-http http.ClientRequest return request
   {"stack":"Error: connect ECONNREFUSED 127.0.0.1:55681\n    at TCPConnectWrap.afterConnect [as oncomplete] (net.js:1148:16)\n    at TCPConnectWrap.callbackTrampoline (internal/async_hooks.js:131:17)","message":"connect ECONNREFUSED 127.0.0.1:55681","errno":"-111","code":"ECONNREFUSED","syscall":"connect","address":"127.0.0.1","port":"55681","name":"Error"}

To troubleshoot the lack of connectivity between the OTLP exporter and the OTel Collector, try the following:

#. Make sure that ``OTEL_EXPORTER_OTLP_ENDPOINT`` points to the correct OpenTelemetry Collector instance host.
#. Check that your collector instance is configured and running. See :ref:`otel-collector-tshoot`.
#. Check that the OTLP receiver is enabled in the OTel Collector and plugged into the traces pipeline.
#. Check that the OTel Collector points to the following address: ``http://<host>:4317``. Verify that your URL is correct.

401 error when sending spans
--------------------------------------------------------

If you send traces directly to Observability Cloud and receive a 401 error code, the authentication token specified in ``SPLUNK_ACCESS_TOKEN`` is invalid. The following are possible reasons:

- The value is null.
- The value is not a well-formed token.
- The token is not an access token that has ``authScope`` set to ingest.

Make sure that you're using a valid Splunk access token when sending data directly to your Splunk platform instance. See :ref:`admin-api-access-tokens`.
