.. _migrate-signalfx-nodejs-agent-to-otel: 

**************************************************************
Migrate from the SignalFx Tracing Library for NodeJS
**************************************************************

.. meta:: 
   :description: The Splunk Distribution of OpenTelemetry JS replaces the deprecated SignalFx Tracing Library for Node.js. To migrate to the Splunk Distribution of OTel JS, follow these instructions.

The SignalFx Tracing Library for Node.js is deprecated. Replace it with the Splunk Distribution of OpenTelemetry JS. Read the following instructions to learn how to migrate.

The Splunk Distribution of OpenTelemetry JS is based on the OpenTelemetry Instrumentation for Node.js, an open-source project that uses the OpenTelemetry API.

.. _requirements-splunk-nodejs-otel-migration:

Compatibility and requirements
==========================================================

The Splunk Distribution of OpenTelemetry JS requires Node.js 8.5 or higher. See :ref:`nodejs-otel-requirements`.

See :ref:`considerations-nodejs-migration` for considerations about migrating from the SignalFx Tracing Library for Node.js to the Splunk Distribution of OpenTelemetry JS.

.. _migrate-to-splunk-nodejs-otel-agent:

Migrate to the Splunk Distribution of OpenTelemetry JS
==========================================================

To migrate from the SignalFx Tracing Library for Node.js to the Splunk Distribution of OpenTelemetry JS, follow these steps:

#. Remove the tracing library packages. See :ref:`remove-nodejs-tracing-library`.
#. Deploy the Splunk Distribution of OpenTelemetry JS. See :ref:`install-splunk-otel-nodejs-distribution`.
#. Update the instrumentation entry point. See :ref:`update-instrumentation-entry-point-nodejs`
#. Migrate your existing configuration. See :ref:`migrate-settings-nodejs-agent`.

.. note:: Semantic conventions for span names and attributes change when you migrate. For more information, see :ref:`migrate-sa-to-otel-collector`.

.. _remove-nodejs-tracing-library:

Remove the SignalFx Tracing Library for Node.js
-----------------------------------------------------------------

Follow these steps to remove the tracing library and its dependencies:

#. Uninstall ``signalfx-tracing``:

   .. code-block:: bash

      npm uninstall --save signalfx-tracing  

#. If npm didn't remove every dependency for ``signalfx-tracing``, remove them manually.

#. Remove any additional instrumentation packages related to the SignalFx Tracing Library for Node.js.

.. _install-splunk-otel-nodejs-distribution:

Deploy the Splunk Distribution of OpenTelemetry JS
---------------------------------------------------

To install the Splunk Distribution of OpenTelemetry JS, see :ref:`instrument-nodejs-applications`.

.. _update-instrumentation-entry-point-nodejs:

Update the instrumentation entry point
-----------------------------------------------

In your code, the instrumentation entry point for SignalFx tracing is similar to the following:

.. code-block:: javascript

   const tracer = require('signalfx-tracing').init({
   // your options here
   })

Update the entry point to use ``@splunk/otel`` and ``startTracing()``, as shown in the following code:

.. code-block:: javascript

   const { startTracing } = require('@splunk/otel');

   startTracing({
   // your new options here
   });

To automatically update your application to use Splunk Distribution of OpenTelemetry JS instead of SignalFx Tracing Library, launch Node using the following command:

.. code-block:: bash

   node -r @splunk/otel/instrument <your-app.js>

.. _migrate-settings-nodejs-agent:

Migrate settings
-----------------------------------------------------------------

To migrate settings from the SignalFx tracing library to the Splunk Distribution of OpenTelemetry JS, rename the following environment variables:

.. list-table:: 
   :header-rows: 1
   
   * - SignalFx environment variable
     - OpenTelemetry environment variable
   * - ``SIGNALFX_ACCESS_TOKEN``
     - ``SPLUNK_ACCESS_TOKEN``
   * - ``SIGNALFX_SERVICE_NAME``
     - ``OTEL_SERVICE_NAME``
   * - ``SIGNALFX_ENDPOINT_URL``
     - See :ref:`migrate-endpoint-url-nodejs`.
   * - ``SIGNALFX_RECORDED_VALUE_MAX_LENGTH``
     - ``OTEL_ATTRIBUTE_VALUE_LENGTH_LIMIT``
   * - ``SIGNALFX_TRACING_DEBUG``
     - See :ref:`migrate-logging-nodejs`.
   * - ``SIGNALFX_SPAN_TAGS``
     - ``OTEL_RESOURCE_ATTRIBUTES`` as comma-separated key-value pairs. See :ref:`advanced-nodejs-otel-configuration`.
   * - ``SIGNALFX_LOGS_INJECTION``
     - Not applicable. Log injection is always enabled. See :ref:`correlate-traces-with-logs-nodejs`.
   * - ``SIGNALFX_LOGS_INJECTION_TAGS``
     - Not applicable See :ref:`correlate-traces-with-logs-nodejs`.
   * - ``SIGNALFX_ENABLED_PLUGINS``
     - Not applicable. To install instrumentation packages, see :ref:`instrument-nodejs-applications`.
   * - ``SIGNALFX_SERVER_TIMING_CONTEXT``
     - ``SPLUNK_TRACE_RESPONSE_HEADER_ENABLED``
   * - ``SIGNALFX_TRACING_ENABLED``
     - ``OTEL_TRACE_ENABLED``

If you're using the passing configuration options as arguments to ``startTracing()``, update them as well:

.. list-table:: 
   :header-rows: 1
   
   * - SignalFx property
     - OpenTelemetry property
   * - ``service``
     - ``serviceName``
   * - ``url``
     - ``endpoint``
   * - ``accessToken``
     - ``accessToken``
   * - ``enabled``
     - No equivalent. Use the ``OTEL_TRACE_ENABLED`` environment variable.
   * - ``debug``
     - No equivalent. See :ref:`migrate-logging-nodejs`.
   * - ``tags`` 
     - ``tracerConfig.resource``
   * - ``logInjection``
     - ``logInjectionEnabled``
   * - ``logInjectionTags``
     - No equivalent. Use ``tracerConfig.resource`` instead.
   * - ``flushInterval``
     - No equivalent. Contact Splunk support if you customized this value.
   * - ``plugins``
     - Not applicable. To install instrumentation packages, see :ref:`instrument-nodejs-applications`.
   * - ``recordedValueMaxLength``
     - Not applicable. Set the ``OTEL_ATTRIBUTE_VALUE_LENGTH_LIMIT`` environment variable instead.
   * - ``enableServerTiming``
     - ``serverTimingEnabled`` 

.. _migrate-logging-nodejs:

Migrate debug log settings
-----------------------------------------------

To configure the debug logs produced by the instrumentation, the closest equivalent to ``SIGNALFX_TRACING_DEBUG`` is ``OTEL_LOG_LEVEL``. See :ref:`enable-nodejs-debug-logging`.

.. _migrate-endpoint-url-nodejs:

Update the endpoint URL
-----------------------------------------------

By default, the Splunk Distribution of OpenTelemetry JS uses the OTLP exporter instead of Jaeger.

- If the receiver endpoint you were using with the SignalFx Tracing Library supports OTLP, set ``OTEL_EXPORTER_OTLP_ENDPOINT`` instead of ``SIGNALFX_ENDPOINT_URL``. The OTel Collector supports OTLP. 
- If you have the exporter set to Jaeger, use ``OTEL_EXPORTER_JAEGER_ENDPOINT`` instead of ``SIGNALFX_ENDPOINT_URL`` and configure the Jaeger Exporter (see :ref:`trace-exporters-settings-nodejs`).

.. _considerations-nodejs-migration:

Considerations for migrating to Splunk Distribution of OpenTelemetry JS
=======================================================================

The following limitations apply when migrating from the SignalFx Tracing Library for Node.js:

- The set of Node.js versions that Splunk Distribution of OpenTelemetry JS supports is different from the set that SignalFx Tracing Library supports. See :ref:``nodejs-otel-requirements``.
- The default flush interval, which defines how frequently captured telemetry data is sent to the backend, is now 30 seconds instead of 2 seconds, and can't be modified.
- Autoinstrumentation is not available for the following libraries:
   - ``AdonisJS``
   - ``amqp10``
   - ``mongodb-core``
   - ``sails``
- Only limited instrumentation for:
   - ``nest`` (only manual instrumentation helpers are available)
- Some instrumentations have specific requirements:
   - ``express``, ``koa``, and ``hapi`` instrumentations require active ``http`` or ``https`` instrumentation to produce spans.
   - ``bluebird``, ``q``, and ``when`` are supported through ``AsyncLocalStorageContextManager`` (or ``AsyncHooksContextManager`` when the Node version is lower than 14.8).

Use the :new-page:`OpenTelemetry Registry <https://opentelemetry.io/registry>` to find autoinstrumentation packages for libraries supported by the Splunk Distribution of OpenTelemetry JS.