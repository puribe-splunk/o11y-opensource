.. _rum-browser-data:

**************************************************
RUM data model for browser-based web applications
**************************************************

.. meta::
   :description: Understand which data you collect from browser-based web applications when using Splunk Real User Monitoring (RUM).

The Browser RUM agent collects general RUM telemetry data about your front-end web application, as well as data from several instrumentations. The following sections describe which data is collected by Splunk RUM for Browser.

Common data types
==============================================

.. include:: /_includes/rum-data-model.rst

.. _browser-rum-basic-properties:

Basic properties
==============================================

The following properties are common to all web applications instrumented for Splunk RUM:

.. list-table:: 
   :widths: 10 10 80
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - String
     - Unique 64bit identifier generated for the span within the trace.
   * - ``parentId``
     - String
     - Parent span ID. Absent if the span is the root span in a trace.
   * - ``name``
     - String
     - Logical operation the span represents. For example, ``/pay``, ``/customers/{1}/details``.
   * - ``duration``
     - Number
     - Duration in microseconds.
   * - ``traceId``
     - String
     - Unique 128-bit identifier, set on all spans belonging to the trace.
   * - ``timestamp``
     - Number
     - Epoch microseconds of the start of the span. Can be absent if incomplete.
   * - ``tags``
     - Object
     - Additional context, allowing to search and analyse the spans based on specific tags.
   * - ``annotations``
     - Array
     - Associates events that explain latency with the time they happened.

.. _browser-rum-default-tags:

Default tags
==============================================

By default, the Browser RUM agent adds the following tags to all spans:

.. list-table:: 
   :widths: 20 10 70
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``app``
     - String
     - Application name, as set in the configuration.
   * - ``component``
     - String
     - Instrumentation name that produced the span. For example, ``document-load``.
   * - ``location.href``
     - String
     - Value of ``location.href`` at the moment of creating the span.
   * - ``splunk.rumSessionId``
     - String
     - Session ID, collected from the ``_splunk_rum_sid`` cookie. See :ref:`browser-rum-cookies`.
   * - ``splunk.rumVersion``
     - String
     - Version of the Splunk RUM SDK instrumenting the application.
   * - ``splunk.scriptInstance``
     - String
     - 64-bit ID. Every instance of ``splunk-otel-web.js`` gets its own ID. This is useful, for example, when distinguishing between different open tabs within the same browser window which share the same session. The ID is not persisted: every time the page is reloaded the ID is renewed.
   * - ``ot.status_code``
     - String
     - Always ``UNSET``.
   * - ``telemetry.sdk.language``
     - String
     - Always ``webjs``.
   * - ``telemetry.sdk.name``
     - String
     - Always ``@splunk/otel-web``.

.. _browser-rum-timing-annotations:

Request timing annotations
===============================================

All spans produced by the Browser RUM agent are annotated with performance timings, as specified by the W3C specification for the ``PerformanceNavigationTiming`` interface:

.. list-table:: 
   :widths: 10 90
   :header-rows: 1

   * - Name
     - Timestamp
   * - ``fetchStart``
     - Immediately before the browser starts to fetch the resource.
   * - ``domainLookupStart``
     - Immediately before the browser starts the domain name lookup for the resource.
   * - ``domainLookupEnd``
     - Immediately after the browser finishes the domain name lookup for the resource.
   * - ``connectStart``
     - Immediately before the browser starts to establish the connection to the server to retrieve the resource.
   * - ``secureConnectionStart``
     - Immediately before the browser starts the handshake process to secure the current connection.
   * - ``connectEnd``
     - Immediately after the browser finishes establishing the connection to the server to retrieve the resource.
   * - ``requestStart``
     - Immediately before the browser starts requesting the resource from the server.
   * - ``responseStart``
     - Immediately after the browser receives the first byte of the response from the server.
   * - ``responseEnd``
     - Immediately after the browser receives the last byte of the resource or immediately before the transport connection is closed, whichever comes first.

.. _browser-rum-data-sending:

Data forwarding limits
===============================================

The Browser RUM agent has the following built-in limits:

- The forwarding frequency for data batches is 5 seconds.
- Up to 100 spans in 30 seconds per component. The agent drops all spans produced beyond the limit.
- Tag values can be up to 4,096 characters long. The agent truncates longer values.
- Batch size, as determined by the number of spans, is 20 spans.

The browser agents sends the IP addresses of all beacon connections to Splunk Observability Cloud, which uses them to map the geographical location of the user, such as country, city, and so on.

.. note:: Observability Cloud calculates only geographical metadata from the IPs, and drops IP addresses within 6 hours.

.. _browser-rum-instrumentation-data:

Instrumentation-specific data
==============================================

Splunk RUM for Browser collects the following data through automatic instrumentations. To enable or disable instrumentations, see :ref:`browser-rum-instrumentation-settings`.

.. _browser-rum-data-doc-load:

Document load
------------------------------

The ``document`` instrumentation produces spans about resources that have loaded by the time the ``Window:load`` event fires. The root span generated is ``documentLoad``. The ``documentFetch`` and ``resourceFetch`` spans have their
``parentId`` set as ``documentLoad.id``.

If the page load request has a ``Server-Timing`` header, the data is used to link the ``documentFetch`` span to the corresponding back-end span. Resources such as ``script``, ``link``, ``css - font``, ``iframe``, ``XHR/fetch``, ``img``, ``favicon`` and ``manifest.json`` are also collected and linked to APM traces if the ``Server-Timing`` header is present.

.. _browser-rum-documentload:

documentLoad
~~~~~~~~~~~~~~~~~~~~~~~~

The Browser RUM agent collects the following data using the ``documentLoad`` instrumentation:

.. list-table:: 
   :widths: 10 10 80
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``document.referrer``	 
     - String
     - URI of the referral page. For example, ``https://subdomain.example.com``.
   * - ``screen.xy``
     - String
     - Width and height of the display. For example, ``2560x1440``.

The following annotations are collected from the navigation timings, as specified by the W3C specification for the ``PerformanceNavigationTiming`` interface:

.. list-table:: 
   :widths: 20 80
   :header-rows: 1

   * - Name
     - Timestamp
   * - ``fetchStart``
     - Immediately before the browser starts fetching the resource.
   * - ``unloadEventStart``
     - Immediately before the user agent starts the unload event of the previous document.
   * - ``unloadEventEnd``
     - Immediately after the user agent finishes the unload event of the previous document.
   * - ``domInteractive``
     - Immediately before the user agent sets the readiness of the current document to `Interactive`.
   * - ``domContentLoadedEventStart``	
     - Immediately before the user agent fires the ``DOMContentLoaded`` event at the current document.
   * - ``domContentLoadedEventEnd``	
     - Immediately after the ``DOMContentLoaded`` event of the currend document completes.
   * - ``domComplete``
     - Immediately before the browser sets the readiness of the current document to `Complete`.
   * - ``loadEventStart``
     - Immediately before the load event of the current document is fired.
   * - ``loadEventEnd``
     - When the load event of the current document is completed.

.. _browser-rum-documentfetch:

documentFetch
~~~~~~~~~~~~~~~~~~~~~~~~~~

The Browser RUM agent collects the following data using the ``documentFetch`` instrumentation:

.. list-table:: 
   :widths: 30 10 60
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``http.response_content_length``
     - Number
     - The size of the document received from the payload body.
   * - ``link.traceId``
     - String
     - Trace identifier, collected from the ``Server-Timing`` response header set by the APM agent.
   * - ``link.spanId``	
     - String
     - Span identifier, collected from the ``Server-Timing`` response header set by the APM agent.

.. _browser-rum-resourcefetch:

resourceFetch
~~~~~~~~~~~~~~~~~~~~~~~~~~

The Browser RUM agent collects the following data using the ``resourceFetch`` instrumentation:

.. list-table:: 
   :widths: 30 10 60
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``http.response_content_length``
     - Number
     - The size of the document received from the payload body.
   * - ``http.url``
     - String
     - URL of the requested resource.
   * - ``link.traceId``
     - String
     - Trace identifier, collected from ``Server-Timing`` response header set by the APM agent.
   * - ``link.spanId``
     - String
     - Span identifier, collected from ``Server-Timing`` response header set by the APM agent.

.. note:: Safari 10.1 doesn't support :code:`resourceFetch` spans.

.. _browser-rum-data-fetch-requests:

XHR/Fetch
--------------------------

The ``xhr`` and ``fetch`` instrumentations collect XMLHttpRequest events and Fetch API events. Spans differ in the value of the ``component`` tag, which differentiates between ``xml-http-request`` and ``fetch``.

This instrumentation prepends the HTTP method name to the name of the span. If the instrumentation maps to a back end
providing a ``Server-Timing`` header in the response, the link with the back-end trace is also generated.

The Browser RUM agent collects the following data using the XHR/Fetch instrumentation:

- ``http.method``
- ``http.response_content_length``
- ``http.host``
- ``http.scheme``
- ``http.status_code``
- ``http.status_text``
- ``http.user_agent``
- ``http.url``

The XHR/Fetch instrumentation annotates the span with timestamps representing when the following events fired:

.. list-table:: 
   :widths: 10 10 80
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``open``
     - Number
     - Time in UNIX epoch, measured in microseconds when XHR ``open`` event fires.
   * - ``send``
     - Number
     - Time when the XHR ``send`` event fires.
   * - ``load``
     - Number
     - Time when the XHR ``load`` event fires.
   * - ``"error"``
     - Number
     - Time when the XHR ``"error"`` event fires.
   * - ``timeout``
     - Number
     - Time when the XHR ``timeout`` event fires.
   * - ``abort``
     - Number
     - Time when the XHR ``abort`` event fires.

Annotations collected by the XHR/Fetch instrumentation are described in :ref:`browser-rum-timing-annotations`.

.. _browser-rum-data-webvitals:

Web Vitals
---------------------------

The ``webvitals`` instrumentation collects data about Google Web Vitals metrics. The Browser RUM agent collects Web Vitals metrics as spans with zero duration. Every span has a designated ``traceId`` and no parent span.

The Browser RUM agent collects the following data using the ``webvitals`` instrumentation:

.. list-table:: 
   :widths: 10 30 60
   :header-rows: 1

   * - Name
     - Web Vital
     - Description
   * - ``lcp``
     - Largest Contentful Paint
     - Measures loading performance by capturing the render time of the largest image or text block visible within the viewport.
   * - ``fid``
     - First Input Delay
     - Measures interactivity by capturing the timestamp between user interactions to time when the browser is  able to begin processing event handlers in response to that interaction.
   * - ``cls``
     - Cumulative Layout Shift
     - Measures visual stability by capturing the sum of all individual layout shift scores for every unexpected layout shift that occurs during the entire lifespan of the page. A layout shift occurs any time a visible element changes its position from one rendered frame to the next.

.. _browser-rum-data-resources-after-load:

Resources after load
-----------------------------------

The ``postload`` instrumentation collects data about resources that load after a page ``load`` event. By default, the instrumentation enables instrumenting ``<script>`` and ``<img>`` resources. A common use case for the Postload instrumentation is to collect telemetry when loading images on ``scroll`` events. 

Spans collected by the ``postload`` instrumentation match the data model described in :ref:`browser-rum-resourcefetch`.

.. _browser-rum-data-user-interactions:

User Interactions
----------------------------------

The ``interactions`` instrumentation collects telemetry data on interactions on elements that have a registered event listener of the type ``Element.addEventListener``. Events collected by the listener generate a span with a name matching the DOM event name.

The Browser RUM agent collects the following data using the ``interactions`` instrumentation:

.. list-table:: 
   :widths: 10 10 80
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``event_type``
     - String
     - Name of the event. For example, ``click``.
   * - ``target_element``
     - String
     - Name of the target element. For example, ``BUTTON``.
   * - ``target_xpath``
     - String
     - XPath of the target element.

.. _browser-rum-data-visibility-events:

Visibility
---------------------------

The ``visibility`` instrumentation collects ``visibilitychange`` events. Visibility changes that happen when a page refreshes aren't recorded, as the browser tab might never go visible.

The Browser RUM agent collects the following data using the ``visibility`` instrumentation:

.. list-table:: 
   :widths: 10 10 80
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``hidden``
     - Boolean
     - Whether the page is hidden or not.

.. _browser-rum-data-connectivity-events:

Connectivity
-----------------------------

The ``connectivity`` instrumentation collects ``offline`` and ``online`` events. The browser records offline events when the browser goes offline and is cached in memory until the browser goes online. Offline and online events are sent at the same time.

The Browser RUM agent collects the following data using the ``connectivity`` instrumentation:

.. list-table:: 
   :widths: 10 10 80
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``online``
     - Boolean
     - Whether the browser went online or offline.
 
History API
~~~~~~~~~~~~~~~~~~~~~~

The Browser RUM agent also instruments the History API to provide visibility into the session history of the browser. The History API tracks URL changes that don't reload the page, and is used in single-page applications. 

The instrumentation also tracks URL changes executed by changing the ``location.hash`` by listening to ``hashchange`` events. Route changes have no duration. The ``routeChange`` span contains the following tags:

.. list-table:: 
   :widths: 10 10 80
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``component``
     - String
     - The value is always ``"user-interaction"``.
   * - ``prev.href``
     - String
     - Page URL prior to the route change.
   * - ``location.href``
     - String
     - Page URL after the route change.

.. _browser-rum-data-long-tasks:

Long tasks
---------------------------

The ``longtask`` instrumentation collects information about long tasks. The Browser RUM agent creates a span for every long task detected.

Span attributes include the containers where that the task occurred. For tasks that don't occur within the top level page, the ``containerId``, ``containerName``, and ``containerSrc`` fields provide information about the source of the task.

The Browser RUM agent collects the following data using the ``longtask`` instrumentation:

.. list-table:: 
   :widths: 20 80
   :header-rows: 1

   * - Name
     - Type
   * - ``longtask.name``
     - String
   * - ``longtask.entry_type``	
     - Number
   * - ``longtask.duration``	
     - Number
   * - ``attribution.name``	
     - String
   * - ``attribution.entry_type``	
     - String
   * - ``attribution.start_time``	
     - Number
   * - ``attribution.duration``	
     - Number
   * - ``attribution.container_type``	
     - String
   * - ``attribution.container_src``	
     - String
   * - ``attribution.container_id``	
     - String
   * - ``attribution.container_name``
     - String

.. _browser-rum-data-websockets:

Websockets
---------------------------

The ``websockets`` instrumentation collects websocket lifecycle events and use it to populate spans. The instrumentation collects spans from websocket ``connect``, ``send``, and ``onmessage`` events.

connect
~~~~~~~~~~~~~~

The ``websockets`` instrumentation collects the following data from ``connect`` events:

.. list-table:: 
   :widths: 10 10 80
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``http.url``
     - String
     - Websocket URL.
   * - ``duration``
     - Number
     - Time lapsed between a websocket constructor call and the ``ws.open`` event firing.
   * - ``protocols``
     - String or array
     - Protocols passed to the websocket constructor.
   * - ``error``
     - String
     - The value can be ``true`` or ``false`` depending on whether an error occurred. Errors are collected during websocket construction or when an ``ws.error`` event fires.
   * - ``error``
     - String
     - Websocket error event message.

send and onmessage
~~~~~~~~~~~~~~~~~~~~~~~~~

The ``websockets`` instrumentation collects the following data from ``send`` and ``onmessage`` events:

.. list-table:: 
   :widths: 10 10 80
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``http.url``
     - String
     - Websocket URL
   * - ``response_content_length``
     - Number
     - Payload size in bytes.