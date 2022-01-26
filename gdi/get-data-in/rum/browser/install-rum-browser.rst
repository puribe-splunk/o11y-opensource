.. _browser-rum-install:

*******************************************************************************
Install the Browser RUM agent for Splunk RUM
*******************************************************************************

.. meta::
   :description: The Browser RUM agent from the Splunk Distribution of OpenTelemetry JavaScript for Web provides a Real User Monitoring (RUM) instrumentation framework for your browser-based web applications. Use it to send RUM data from your front end to Splunk RUM.

You can instrument the front end of your web applications for Splunk RUM using the Browser RUM agent from the Splunk Distribution of OpenTelemetry JavaScript for Web.

To instrument your browser application and get data into Splunk RUM, follow these steps:

.. _rum-browser-requirements:

Check compatibility and requirements
==============================================

The Browser RUM agent supports the following browser versions:

- Chrome and Chrome Android 51 and higher
- Edge 12 and higher
- Firefox 36 and higher
- Safari and Safari iOS 10.1 and higher
- Internet Explorer 11 requires the ``splunk-otel-web-legacy.js`` version of the Browser RUM agent.

All your pages, assets, and requests must be securely loaded over the HTTPS protocol.

.. _rum-browser-install:

Instrument your web application for Splunk RUM
====================================================================

Choose one of the following methods to instrument your web application:

* :ref:`rum-browser-install-cdn`
* :ref:`rum-browser-install-self-hosted`
* :ref:`rum-browser-install-npm`

.. tip:: To generate all the install commands for your environment and application, open the Observability Cloud guided setup in :strong:`Data Setup > RUM Instrumentation > Browser Instrumentation > Add Connection`.

.. _rum-browser-install-cdn:

Splunk CDN
----------------------------------------------------------------------

You can use the Splunk Content Delivery Network (CDN) to load the Browser RUM agent synchronously. The CDN link ensures that your application always uses the latest version.

Follow these steps to instrument your application with the CDN:

#. Customize the following snippet:

   .. code-block:: html

      <script src="https://cdn.signalfx.com/o11y-gdi-rum/latest/splunk-otel-web.js" crossorigin="anonymous"></script>
      <script>
         SplunkRum.init({
            beaconUrl: 'https://rum-ingest.<REALM>.signalfx.com/v1/rum',
            rumAuth: '<RUM Token>',
            app: '<YOUR-APPLICATION-NAME>'
         });
      </script>

   * In the beacon URL, ``REALM`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``. 
   * To generate a RUM access token, see :ref:`rum-access-token`.

#. Add the snippet to the head section of every page you want to monitor in your application.

#. Deploy the changes to your application.

.. _rum-browser-install-self-hosted:

Self-hosted script 
------------------------------------------------------

To use your own CDN or comply with your own deployment requirements, instrument your application using a self-hosted script. When you host the script, you need to update to newer versions of the agent manually.

Follow these steps to instrument your application using a self-hosted script: 

#. Go to :new-page:`splunk-otel-js-web <https://github.com/signalfx/splunk-otel-js-web/releases>` in GitHub and download the ``splunk-otel-web.js`` file for the release you want to use.

#. Deploy the files in a location accessible by the users of your application.

#. Customize the following snippet:

   .. code-block:: html

      <script src="http://example.domain/path/splunk-otel-web.js"></script>
      <script>
         SplunkRum.init({
            beaconUrl: 'https://rum-ingest.<REALM>.signalfx.com/v1/rum',
            rumAuth: '<RUM Token>',
            app: '<YOUR-APPLICATION-NAME>'
         });
      </script>

   * In the beacon URL, ``REALM`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``.
   * To generate a RUM access token, see :ref:`rum-access-token`.

#. Add the snippet to the head section of every page you want to monitor in your application.

#. Deploy the changes to your application.

.. _rum-browser-install-npm:

npm package
------------------------------------------------

To bundle the Browser RUM agent directly with your application, use the ``@splunk/otel-web`` npm package.

Follow these steps to instrument and configure Splunk RUM using npm:

#. Enter the following command to install the Browser RUM agent and add it to your ``package.json`` file: 

   .. code-block:: shell

      npm install @splunk/otel-web --save

#. Create the ``splunk-instrumentation.js`` initialization file next to your bundle root file. The following snippet contains sample content for the initialization file:

   .. code-block:: javascript

      import SplunkOtelWeb from '@splunk/otel-web';
      SplunkOtelWeb.init({
         beaconUrl: 'https://rum-ingest.<REALM>.signalfx.com/v1/rum',
         rumAuth: '<RUM Token>',
         app: 'your-application-name'
      });

   * In the beacon URL, ``REALM`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``. 
   * To generate a RUM access token, see :ref:`rum-access-token`.

#. Import or require the ``splunk-instrumentation.js`` file above other files to ensure that the instrumentation runs before the application code.

#. Deploy the changes to your application.

.. note:: Make sure the Splunk RUM agent doesn't run in Node.js. To instrument Node.js services for Splunk APM, see :ref:`get-started-nodejs`.

.. _loading-initializing_browser-rum:

Loading and initializing the Browser RUM agent
========================================================

To avoid gaps in your data, load and initialize the Browser RUM agent synchronously and as early as possible. Delayed loading might result in missing data, as the instrumentation cannot collect data before it's initialized.

Use the following methods to load an initialize the Browser RUM agent, ordered by decreasing quality:

* Synchronously loaded as the first resource in the head section. This ensures that the instrumentation collects all user interactions, resources, and errors.
* Bundled with other application scripts. Place the Browser RUM agent at the top of the bundle and make sure the bundle loads synchronously.
* Synchronously loaded as the first JS resource in the head section.

If you defer the loading of the Browser RUM agent, make sure other scripts are also deferred to preserve the initialization order. Note that asynchronously loaded scripts are not supported.

.. _modify-spans:

Customize your RUM data intake
=================================================

Before you instrument and configure Splunk RUM for your web application, understand which data RUM collects about your application and determine the scope of what you want to monitor. See :ref:`rum-data-collected`.

Opt out of error.message collection
------------------------------------------------

To avoid collecting ``error.message`` responses, disable the errors instrumentation as in the following example:

.. code-block:: html
   :emphasize-lines: 7

   <script src="https://cdn.signalfx.com/o11y-gdi-rum/latest/splunk-otel-web.js" crossorigin="anonymous"></script>
   <script>
      SplunkRum.init({
         beaconUrl: 'https://rum-ingest.<REALM>.signalfx.com/v1/rum',
         rumAuth: '<RUM Token>',
         app: '<YOUR-APPLICATION-NAME>',
         instrumentations: { errors: false }
      });
   </script>

Change attributes before they're collected
----------------------------------------------------------------

To remove or change attributes in your spans, see :ref:`rum-browser-redact-pii`.

.. _rum-apm-connection:

Link RUM with Splunk APM
==================================

Splunk RUM uses server timings to calculate the response time between the front end and back end of your application, and to join the front-end and back-end traces for end-to-end visibility.

By default, the Splunk Distributions of OpenTelemetry already send the ``Server-Timing`` header. The header links spans from the browser with back-end spans and traces.

The APM environment variable for controlling the ``Server-Timing`` header  is ``SPLUNK_TRACE_RESPONSE_HEADER_ENABLED``. To create a header manually, see :ref:`browser-server-trace-context`.

.. note:: Safari doesn't support the :code:`Server-Timing` header.

Instrument WebViews in Mobile applications
=============================================

You can instrument WebViews in your iOS and Android applications by sharing the `splunk.rumSessionId` between the mobile instrumentation and the web instrumentation. This lets you to see data from both your native app and your web app in a single stream.

To instrument WebViews, follow these instructions:

* :ref:`Android WebViews <android-webview-instrumentation>`
* :ref:`iOS WebViews <ios-webview-instrumentation>`

Considerations for content security policy (CSP)
=================================================

If your application uses Content Security Policy (CSP) to mitigate potential impact from Cross-site Scripting (XSS) and other attacks, make sure the policy allows Splunk RUM:

- When using the CDN version of the agent, allow ``script-src cdn.signalfx.com``.
- When self-hosting or using the npm package, configure your site accordingly.
- Add the host from ``beaconUrl`` to ``connect-src``. For example: ``connect-src app.us1.signalfx.com``.

How to contribute
=========================================================

The Splunk Distribution of OpenTelemetry JavaScript for Web is open source software. You can contribute to its improvement by creating pull requests in GitHub. To learn more, see the :new-page:`contribution guidelines <https://github.com/signalfx/splunk-otel-js-web/blob/main/CONTRIBUTING.md>` in GitHub.

Versioning policy
---------------------------------------------------------

The versioning of the Splunk RUM Agent follows semantic versioning rules. To have more control over the version you load, see the following versioning policy:

* Use the ``LATEST`` version to use the latest version of the Browser RUM agent. This might not be suitable for manual instrumentation, as breaking API changes might occur between major version changes.
* Use ``MAJOR`` versions, for example ``v1``, if you want to receive new features automatically while keeping backward compatibility with the API. This is the default for all production deployments, as well as for npm installations.
* Use ``MINOR`` versions, for example ``v1.1``, to receive bug fixes while not receiving new features automatically.
* Use ``PATCH`` versions, for example, ``v1.2.1``, to pin a specific version of the agent for your application.

The versions of the agent are included in URLs as a designated token:

``https://cdn.signalfx.com/o11y-gdi-rum/v<MAJOR.MINOR.PATCH>/splunk-otel-web.js``