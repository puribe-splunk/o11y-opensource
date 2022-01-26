.. _manual-rum-browser-instrumentation:

*******************************************************************************
Manually instrument browser-based web applications
*******************************************************************************

.. meta::
   :description: Manually instrument front-end applications for Splunk RUM to collect additional telemetry, sanitize Personal Identifiable Information (PII), identify users, and much more.

The agent from the Splunk Distribution of OpenTelemetry JS for Web collects telemetry using several automatic instrumentations. See :ref:`rum-browser-data` for more information.

You can manually instrument front-end applications to collect additional telemetry, sanitize Personal Identifiable Information (PII), identify users, and much more.

.. tip:: To avoid load problems due to content blockers when using the CDN version of the Browser RUM agent, add ``if (window.SplunkRum)`` checks around ``SplunkRum`` API calls. 

Instrument your application using the OpenTelemetry API
=============================================================

To instrument your front end manually, use the OpenTelemetry API. The Browser RUM agent automatically registers its TraceProvider using ``@opentelemetry/api``, so that your own instrumentations can access it.

The following examples show several manual instrumentations for Splunk RUM:

.. tabs::

   .. code-tab:: javascript Create a span

      import {trace} from '@opentelemetry/api'

      const span = trace.getTracer('searchbox').startSpan('search');
      span.setAttribute('searchLength', searchString.length);
      // Time passes
      span.end();

   .. code-tab:: javascript Set the user ID on all spans

      SplunkRum.setGlobalAttributes({
         'enduser.id': 'Test User'
      });

   .. code-tab:: javascript Create a custom event

      import {trace} from '@opentelemetry/api'

      const tracer = trace.getTracer('appModuleLoader');
      const span = tracer.startSpan('test.module.load', {
      attributes: {
         'workflow.name': 'test.module.load'
      }
      });
      // time passes
      span.end();

To migrate manual instrumentation created for another vendor, see :ref:`browser-rum-migrate-instrumentation`.

.. note:: The version of ``@opentelemetry/api`` you use must match the same major version of ``@opentelemetry/api`` used by the Browser RUM agent.

.. _rum-browser-redact-pii:

Sanitize Personally Identifiable Information (PII)
=========================================================

The metadata collected by the Browser RUM agent might include Personally Identifiable Information (PII) if your front-end application injects such data in its code. For example, UI components might include PII in their IDs.

To redact PII in the data collected for Splunk RUM, pass a sanitizer using the ``exporter.onAttributesSerializing`` setting when initializing the Browser RUM instrumentation, as in the following example:

.. code-block:: javascript
   :emphasize-lines: 5,6,7,8

   SplunkRum.init({
   // ...
   exporter: {
   // You can use the entire span as an optional second argument of the sanitizer if needed
      onAttributesSerializing: (attributes) => ({
         ...attributes,
         'http.url': /secret\=/.test(attributes['http.url']) ? '[redacted]' : attributes['http.url'],
      }),
   },
   });

.. note:: The Browser RUM automatic instrumentations do not collect or report any data from request payloads or POST bodies other than their size.

.. _browser-rum-identify-users:

Add user metadata using global attributes
=============================================

By default, the Browser RUM agent doesn't automatically link traces to users of your site. However, you might need to collect user metadata to filter or debug traces.

The OpenTelemetry specification provides a number of attributes for identifying users, such as ``enduser.id`` and ``enduser.role``, which you can add to your spans using global attributes.

The following examples show how to add identification metadata as global attributes when initializing the agent or after you've initialized it, depending on whether user data is accessible at initialization:

.. tabs::

   .. code-tab:: html During initialization
      :emphasize-lines: 7,8,9,10

      <script src="https://cdn.signalfx.com/o11y-gdi-rum/latest/splunk-otel-web.js" crossorigin="anonymous"></script>
      <script>
      SplunkRum.init({
         beaconUrl: 'https://rum-ingest.<REALM>.signalfx.com/v1/rum',
         rumAuth: '<RUM access token>',
         app: '<application-name>',
         globalAttributes: {
            // The following data is already available
            'enduser.id': 42,
            'enduser.role': 'admin',
         },
      });
      </script>

   .. code-tab:: javascript After initialization
      :emphasize-lines: 5,6,7,8

      import SplunkRum from '@splunk/otel-js-browser';

      const user = await (await fetch('/api/user')).json();
      // Spans generated prior to this call don't have user metadata
      SplunkRum.setGlobalAttributes({
         'enduser.id': user ? user.id : undefined,
         'enduser.role': user ? user.role : undefined,
      });

.. _browser-server-trace-context:

Add server trace context from APM
==========================================

The Browser RUM agent collects server trace context using back-end data provided by APM instrumentation through the ``Server-Timing`` header. In some cases, you might want to generate the header manually.

To create the ``Server-Timing`` header manually, provide a ``Server-Timing`` header with the name ``traceparent``, where the ``desc`` field holds the version, the trace ID, the parent ID, and the trace flag. 

Consider the following HTTP header:

.. code-block:: shell
   
   Server-Timing: traceparent;desc="00-4bf92f3577b34da6a3ce929d0e0e4736-00f067aa0ba902b7-01"

The example resolves to a context containing the following data:

.. code-block:: shell

   version=00 trace-id=4bf92f3577b34da6a3ce929d0e0e4736
   parent-id=00f067aa0ba902b7 trace-flags=01

When generating a value for ``traceparent``, make sure that it matches the following regular expression:

.. code-block:: shell
   
   00-([0-9a-f]{32})-([0-9a-f]{16})-01

Server timing headers with values that don't match the pattern are automatically discarded. For more information, see the ``Server-Timing`` and ``traceparent`` documentation on the W3C website.

.. note:: If you're using cross-origin resource sharing (CORS) headers, such as ``Access-Control-*``, you might need to grant permission to read the ``Server-Timing`` header. For example:

   - ``Access-Control-Expose-Headers: Server-Timing``

.. _browser-rum-workflows:

Create workflow spans
===================================================

Workflow spans are custom spans with the following attributes:

.. list-table:: 
   :widths: 10 10 80
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - String
     - Unique ID for the workflow instance.
   * - ``name``
     - String
     - Semantic name for the current workflow.

The following snippet shows how to create a workflow span:

.. code-block:: javascript

   import {trace} from '@opentelemetry/api'

   const tracer = trace.getTracer('appModuleLoader');
   const span = tracer.startSpan('test.module.load', {
   attributes: {
      'workflow.id': 1,
      'workflow.name': 'test.module.load'
   }
   });

   // Time passes
   span.end();

To enable error collection for workflow spans, add the ``error`` and ``error.message`` attributes:

.. code-block:: javascript
   :emphasize-lines: 8,9

   import {trace} from '@opentelemetry/api'

   const tracer = trace.getTracer('appModuleLoader');
   const span = tracer.startSpan('test.module.load', {
   attributes: {
      'workflow.id': 1,
      'workflow.name': 'test.module.load',
      'error': true,
      'error.message': 'Custom workflow error message'
   }
   });

   span.end();

.. _browser-rum-migrate-instrumentation:

Migrate existing manual instrumentation
====================================================

You can migrate manual instrumentation that you added for another vendor to send telemetry data to Splunk RUM. To migrate the instrumentation, you must edit the instrumentation code to use OpenTelemetry conventions.

The following examples shows how you can instrument different data sources for Splunk RUM: 

.. tabs::

   .. tab:: Actions or Events

      You might have instrumentation that collects custom timestamps or time ranges for activity within your app. For example, you might have manually instrumented a CPU-intensive ``calculateEstateTax`` function to know how its performance is affecting users. 
      
      When instrumenting the same function using OpenTelemetry, in addition to capturing start and end time in a span, you can include additional details using attributes:

      .. code-block:: javascript
         :emphasize-lines: 5,8

         import {trace} from '@opentelemetry/api'

         function calculateEstateTax(estate) {
            const span = trace.getTracer('estate').startSpan('calculateEstateTax');
            span.setAttribute('estate.jurisdictionCount', estate.jurisdictions.length);
            var taxOwed = 0;
            // ...
            span.setAttribute('isTaxOwed', taxOwed > 0);
            span.end();
            return taxOwed;
         }

   .. tab:: Custom properties, Tags, or Attributes

      You might have a feature where you collect additional tags or properties about the page and include that information in your RUM data stream. For example, you might be capturing details about A/B tests, account
      categorization, the relase version of the app, or UI modes. 
      
      OpenTelemetry attributes are key-value pairs designed to store this kind of information. If the relevant properties are known at the time the page is loaded, use ``globalAttributes``:

      .. code-block:: javascript

         SplunkRum.init( {
            beaconUrl: '...',
            rumAuth: '...',
            globalAttributes: {
               'account.type': goldStatus,
               'app.release': getReleaseNumber(),
            },
         });

      If the properties are not known until later or can change over the lifetime of the page, update or add them dynamically like in the following example:

      .. code-block:: javascript

         SplunkRum.setGlobalAttributes({
            'account.type': goldStatus,
            'app.release': getReleaseNumber(),
            'dark_mode.enabled': darkModeToggle.status,
         });

   .. tab:: Errors

      You might have manual instrumentation that reports errors that are collected or handled in your code. To collect and report errors to Splunk RUM, use the ``SplunkRum.error`` function:

      .. code-block:: javascript
         :emphasize-lines: 4

         try {
            doSomething();
         } catch (e) {
            SplunkRum.error(e);
         }

      ``SplunkRum.error`` accept strings and arrays of strings, as well as ``Error`` and ``ErrorEvent`` objects.