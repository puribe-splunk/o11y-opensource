.. _configure-browser-instrumentation:

*****************************************************************
Configure the Splunk Browser RUM instrumentation
*****************************************************************

.. meta::
   :description: Configure the Splunk RUM instrumentation for your browser-based web applications.

You can configure the Browser RUM agent from the Splunk Distribution of OpenTelemetry JS for Web to add custom attributes, adapt the instrumentation to your environment and application, customize sampling, and more.

Configuration method
===============================================

To configure the Browser RUM agent, edit the object passed to the ``SplunkRum.init()`` function:

.. code-block:: html
   :emphasize-lines: 5,6,7,8

   <script src="https://cdn.signalfx.com/o11y-gdi-rum/latest/splunk-otel-web.js" crossorigin="anonymous"></script>
   <script>
         SplunkRum.init(
         {
            beaconUrl: 'https://rum-ingest.us0.signalfx.com/v1/rum'
            rumAuth: 'ABC123...789',
            app: 'my-awesome-app',
            // Any additional settings
         });
   </script>

.. _browser-rum-settings:

General settings
======================================================

Use the following settings to configure the Browser RUM agent:

.. list-table:: 
   :header-rows: 1
   :widths: 10 20 70

   * - Property
     - Type
     - Description
   * - ``beaconUrl``
     - String (required)
     - Ingest URL to which the agent sends collected telemetry. The URL must contain your realm in Splunk Observability Cloud. For example, ``https://rum-ingest.us0.signalfx.com/v1/rum`` is the ingest URL for the ``us0`` realm.
   * - ``rumAuth``
     - String (required)
     - RUM token that authorizes the agent to send telemetry data to Splunk Observability Cloud. To generate a RUM access token, see :ref:`rum-access-token`.
   * - ``app``
     - String
     - Name of the application. The default value is ``unknown-browser-app``.
   * - ``environment``
     - String
     - Environment for all the spans produced by the application. For example, ``dev``, ``test``, or ``prod``.
   * - ``globalAttributes``
     - Object
     - Sets additional attributes added to all spans. For example, ``version`` or ``user_id``.
   * - ``instrumentations``
     - Object
     - Enables or disables specific Browser RUM instrumentations. See :ref:`browser-rum-instrumentation-settings`.
   * - ``ignoreUrls``
     - ``(string\|regex)[]``
     - List of URLs that the agent must ignore. Matching URLs don't produce spans. You can provide two different types of rules, strings or regular expressions.
   * - ``cookieDomain``
     - String
     - Domain used for session tracking. For example, if you have sites on both ``foo.example.com`` and ``bar.example.com``, setting ``cookieDomain`` to ``example.com`` allows both sites to use the same session identifier. See :ref:`browser-rum-cookies` for more information.
   * - ``context.async``
     - Boolean
     - Enables the asynchronous context manager. The default value is ``false``. See :ref:`browser-rum-async-traces`.
   * - ``exporter.onAttributesSerializing``
     - ``(a: SpanAttributes, s?: Span) => SpanAttributes``
     - Provides a callback for modifying span attributes before they're exported. The default value is ``(attrs) => attrs``. A sample use case is :ref:`rum-browser-redact-pii`. 
   * - ``tracer``
     - Object
     - Tracing configuration passed to ``WebTracerProvider``. You can use it to configure sampling. See :ref:`browser-rum-sampling-configuration`.
   * - ``debug``
     - Boolean
     - Enables debug logging in the developer console. The default value is ``false``.

.. _browser-rum-instrumentation-settings:

Instrumentations settings
==============================================

To enable or disable specific Browser RUM instrumentations, compose and pass an object to the ``instrumentation`` property. The following example changes the settings of several instrumentations:

.. code-block:: javascript

   SplunkRum.init(
      {
         beaconUrl: 'https://rum-ingest.us0.signalfx.com/v1/rum',
         rumAuth: 'ABC123…789',
         app: 'my-awesome-app',
         instrumentations:
         {
            interactions:
            {
               // Adds``gamepadconneted`` events to the
               // list of events collected by default
               events:
               {
                  gamepadconnected: true,
               },
            },
            longtask: false, // Disables monitoring for longtasks
            websockets: true, // Enables monitoring for websockets
         },
      });

The following table contains all the properties supported by the ``instrumentations`` option:

.. list-table:: 
   :header-rows: 1
   :widths: 20 10 70

   * - Property
     - Default
     - Description
   * - ``connectivity``
     - ``false``
     - Enables the collection of connectivity events. See :ref:`browser-rum-data-connectivity-events`.
   * - ``document``
     - ``true``
     - Enables the collection of spans related to document load events. See :ref:`browser-rum-data-doc-load`.
   * - ``errors``
     - ``true``
     - Enables the collection of JavaScript errors. See :ref:`browser-rum-data-js-errors`.
   * - ``fetch``
     - ``true``
     - Enables the collection of Fetch API requests. See :ref:`browser-rum-data-fetch-requests`.
   * - ``interactions``
     - ``true``
     - Enables the collection of user interactions, such as clicks or key presses. See :ref:`browser-rum-data-user-interactions`.
   * - ``longtask``
     - ``true``
     - Enables the collection of long tasks. See :ref:`browser-rum-data-long-tasks`.
   * - ``postload``
     - ``true``
     - Enables the collection of resources loaded after a load event. See :ref:`browser-rum-data-resources-after-load`.
   * - ``visibility``
     - ``false``
     - Enables the collection of visibility events. See :ref:`browser-rum-data-visibility-events`.
   * - ``websockets``
     - ``false``
     - Enables the collection of websocket lifecycle events. See :ref:`browser-rum-data-websockets`.
   * - ``webvitals``
     - ``true``
     -  Enables the collection of Google Web Vitals metrics. See :ref:`browser-rum-data-webvitals`.
   * - ``xhr``
     - ``true``
     - Enables the collection of XMLHttpRequest events. See :ref:`browser-rum-data-fetch-requests`.

.. _browser-rum-sampling-configuration:

Sampling settings
=============================================

By default, the Browser RUM agent collects all of the data from all of the users. You can adjust sampling by passing a custom sampler to the ``tracer`` property.

The following example shows how to restrict sampling to logged in users:

.. tabs::

   .. code-tab:: html CDN
      :emphasize-lines: 9,10,11

      <script src="https://cdn.signalfx.com/o11y-gdi-rum/latest/splunk-otel-web.js" crossorigin="anonymous"></script>
      <script>
         var shouldTrace = isUserLoggedIn();

         SplunkRum.init({
            beaconUrl: 'https://rum-ingest.<REALM>.signalfx.com/v1/rum',
            rumAuth: '<RUM access token>',
            app: '<application-name>',
            tracer: {
               sampler: shouldTrace ? new AlwaysOnSampler() : new SplunkRum.AlwaysOffSampler(),
            },
         });
      </script>

   .. code-tab:: js npm
      :emphasize-lines: 9,10,11

      // When using npm you can get samplers directly from @opentelemetry/core
      import {AlwaysOnSampler, AlwaysOffSampler} from '@opentelemetry/core';
      import SplunkOtelWeb from '@splunk/otel-web';

      SplunkOtelWeb.init({
         beaconUrl: 'https://rum-ingest..signalfx.com/v1/rum',
         rumAuth: '<RUM access token>', 
         app: '<application-name>', 
         tracer: { 
            sampler: userShouldBeTraced() ? new SplunkRum.AlwaysOnSampler() : new SplunkRum.AlwaysOffSampler(),
         },
      });

The Splunk Distribution of OpenTelemetry JS for Web includes the following samplers:

-  ``AlwaysOnSampler``: Sampling enabled for all requests. This is the default sampler.
-  ``AlwaysOffSampler``: Sampling disabled for all requests.
-  ``ParentBasedSampler``: Inherits the sampler configuration of the parent trace.
-  ``SessionBasedSampler``: Session-based sampling. See :ref:`browser-rum-session-based-sampler`.

.. _browser-rum-session-based-sampler:

Session-based sampler
-----------------------------------------------

The Splunk Distribution of OpenTelemetry JS for Web includes a custom sampler that supports sessions. Session ratios are preferable over trace ratios, as they keep data from each session intact.

You can access the session-based sampler in the following ways:

- ``SplunkRum.SessionBasedSampler`` when using the Splunk CDN build
- ``SessionBasedSampler`` export when using the npm package

The session-based sampler accepts the following settings:

.. list-table:: 
   :header-rows: 1
   :widths: 10 10 20 60

   * - Option
     - Type
     - Default value
     - Description
   * - ``ratio``
     - ``number``
     - ``1.0``
     - Percentage of sessions reported, ranging from ``0.0`` to ``1.0``.
   * - ``sampled``
     - ``Sampler``
     - ``AlwaysOnSampler``
     - Sampler to be used when the session is sampled.
   * - ``notSampled``
     - ``Sampler``
     - ``AlwaysOffSampler``
     - Sampler to be used when the session is not to be sampled.

The following example shows how to collect RUM data from half of the sessions:

.. tabs::

   .. code-tab:: html CDN
      :emphasize-lines: 7,8,9,10

      <script src="https://cdn.signalfx.com/o11y-gdi-rum/latest/splunk-otel-web.js" crossorigin="anonymous"></script>
      <script>
        SplunkRum.init({
          beaconUrl: 'https://rum-ingest.<REALM>.signalfx.com/v1/rum',
          rumAuth: '<RUM access token>',
          app: '<application-name>',
          tracer: {
            sampler: new SplunkRum.SessionBasedSampler({
            ratio: 0.5
            }),
          },
        });
      </script>

   .. code-tab:: javascript npm
      :emphasize-lines: 8,9,10,11

      import SplunkOtelWeb, {SessionBasedSampler} from '@splunk/otel-web';

      SplunkOtelWeb.init({ 
        beaconUrl: 'https://rum-ingest.<REALM>.signalfx.com/v1/rum',
        rumAuth: '<RUM access token>', 
        app: '<application-name>',
        tracer: {
            sampler: new SessionBasedSampler({
              ratio: 0.5 
            }),
        },
      });


.. _browser-rum-async-traces:

Asynchronous traces settings
=======================================

Traces that happen asynchronously, such as user interactions that result in a promise chain, might get disconnected from parent activity. To avoid this problem, the Browser RUM Agent includes a custom context manager that connects parent traces with traces that happen in the following situations:

-  ``setTimeout`` with less than 34ms timeout
-  ``setImmediate``
-  ``requestAnimationFrame``
-  ``Promise.then`` / ``catch`` / ``finally``
-  ``MutationObserver`` on ``textNode``
-  ``MessagePort``
-  Hash-based routers

This allows Splunk RUM to link requests executed when a component is first rendered to the user interaction that caused the application to add the component to the page. ``XMLHttpRequest`` events and Fetch API events through promise methods are patched to preserve the parent context, so subsequent requests link to their parents.

Known issues
---------------------------------------

The following issues apply when using asynchronous tracing:

- ``async/await`` functions aren't supported. You can connect them using Babel as in the following example:

   .. code-block:: javascript

      document.getElementById('save-button').addEventListener('click', async () => {
        const saveRes = await fetch('/api/items', {method: 'POST'});

        const listRes = await fetch('/api/items'); // Can be disconnected from click event when not transpiled
      });

- Only code loaded by promise-based implementations is linked to the parent interaction.

.. _browser-rum-context-propagation:

Context propagation settings
=====================================

The Browser RUM agent doesn't register any context propagators, as it collects ``traceparent`` data from ``Server-Timing`` headers. If needed, you can register context propagators by using the OpenTelemetry API:

.. code-block:: javascript

   import {propagation} from '@opentelemetry/api'; 
   import {B3Propagator} from '@opentelemetry/propagator-b3';

   propagation.setGlobalPropagator(new B3Propagator());

When calling the OpenTelemetry API directly, make sure the API version you're using matches the one used by the Browser RUM agent.

.. _browser-rum-exporters-configuration:

Exporters settings
=====================================

The Browser RUM agent uses the Zipkin exporter to send data to Splunk Observability Cloud. The following example shows how to register a different trace exporter:

.. code-block:: javascript

   import SplunkRum from '@splunk/otel-js-browser';
   import {BatchSpanProcessor} from '@opentelemetry/sdk-trace-base';
   import {CollectorTraceExporter} from '@opentelemetry/exporter-collector';

   const exporter = new CollectorTraceExporter({ url: 'https://collector.example.com' });
   SplunkRum.provider.addSpanProcessor(new BatchSpanProcessor(exporter));

.. _browser-rum-cookies:

Cookies used by the Browser RUM agent
===========================================

The Browser RUM agent uses the following cookies to link traces to sessions:

.. list-table:: 
   :header-rows: 1
   :widths: 10 25 65

   * - Name
     - Purpose
     - Comment
   * - ``__splunk_rum_sid``
     - Stores the session ID.
     - By default, a session lasts until 15 minutes passed from the last user interaction. The maximum session duration is 4 hours.