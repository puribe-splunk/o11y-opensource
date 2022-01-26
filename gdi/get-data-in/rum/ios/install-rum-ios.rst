.. _ios-rum-install:

**************************************************************
Install the iOS RUM agent for Splunk RUM
**************************************************************

.. meta::
   :description: The iOS RUM agent from the Splunk Distribution of OpenTelemetry iOS provides a Real User Monitoring (RUM) instrumentation framework for your iOS applications. Use it to send RUM data from your mobile apps to Splunk RUM.

You can instrument your iOS applications for Splunk RUM using the iOS RUM agent from the Splunk Distribution of OpenTelemetry iOS.

To instrument your iOS application and get data into Splunk RUM, follow these steps:

Check compatibility and requirements 
===============================================

Splunk RUM for Mobile supports iOS 11 and higher, including iPadOS 13 and higher.

Installation configuration options
---------------------------------------------

The following table describes the installation configuration options:

.. list-table::
   :widths: 20 20 40 10
   :header-rows: 1

   * - :strong:`Configuration option`
     - :strong:`Type`
     - :strong:`Description`
     - :strong:`Default`
   * - :code:`beaconUrl`
     - String (required)
     - The full URL of the RUM ingest endpoint
     - n/a
   * - :code:`rumAuth`
     - String (required)
     - The rumAuth is the RUM Token you generate in the Observability Cloud. RUM access tokens are publicly visible in the client-side code
     - n/a
   * - :code:`debug`
     - Bool
     - Turns the debug logging on or off
     - false
   * - :code:`allowInsecureBeacon`
     - Bool
     - Allows http beacon urls
     - false
   * - :code:`globalAttributes`
     - [String:Any]
     - Extra attributes to add to each reported span. See also setGlobalAttributes
     - [:]


Import and initialize the Splunk RUM iOS library
================================================

Follow these steps to import and initialize the Splunk RUM iOS library.

1. In Xcode, select :strong:`File` > :strong:`Add Packages...` or :strong:`File` > :strong:`Add Packages...` or :strong:`File` -> :strong:`Swift Packages` -> :strong:`Add Package Dependency` and enter the following URL in the search bar:

   ``https://github.com/signalfx/splunk-otel-ios``

2. Initialize the Splunk RUM iOS library with your configuration parameters. ``rumAuth`` is the RUM token that you generated. For more information, see :ref:`Generate your RUM access token in the Observability Cloud<rum-access-token>`.

   .. tabs::

      .. code-tab:: swift

         import SplunkOtel
         //..
         SplunkRum.initialize(beaconUrl: "https://rum-ingest.<realm>.signalfx.com/v1/rum",
                           rumAuth: "<rum-token>",
                           options: SplunkRumOptions(environment:"<environment-name>"))

      .. code-tab:: objective-c

         @import SplunkOtel;
         //...
         [SplunkRum initializeWithBeaconUrl:@"https://rum-ingest.<realm>.signalfx.com/v1/rum" rumAuth: @"<rum-token>" options: nil];

   * The value of ``<realm>`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``. 
   * To generate a RUM access token, see :ref:`rum-access-token`.

3. Deploy the changes to your application.

(Optional) Enable crash reporting
-------------------------------------

Splunk OpenTelemetry iOS agent Crash Reporting module is an add-on to the Splunk RUM iOS agent.

To enable crash reporting, follow these steps:

1. In Xcode, select :strong:`File` > :strong:`Add Packages...` or :strong:`File` -> :strong:`Swift Packages` -> :strong:`Add Package Dependency` and enter the following URL in the search bar:

   ``https://github.com/signalfx/splunk-otel-ios-crashreporting``

2. Initialize the crash reporting module with your configuration parameters:

   .. tabs::

      .. code-tab:: swift

         import SplunkOtel
         import SplunkOtelCrashReporting
         //..
         SplunkRum.initialize(beaconUrl: "https://rum-ingest.<realm>.signalfx.com/v1/rum",
                           rumAuth: "<rum-token>",
                           options: SplunkRumOptions(environment:"<environment-name>"))
         SplunkRumCrashReporting.start()

      .. code-tab:: objective-c

         @import SplunkOtel;
         @import SplunkOtelCrashReporting;
         //...
         [SplunkRum initializeWithBeaconUrl: @"https://rum-ingest.<realm>.signalfx.com/v1/rum" rumAuth: @"<rum-token>" options: nil];
         [SplunkRumCrashReporting start]

.. note:: Symbolication is not supported.

(Optional) Manually instrument your iOS application
=======================================================

You can modify the OpenTelemetry instrumentation to customize names, add global attributes, and more.

Configure error reporting
------------------------------

You can report handled errors, exceptions, and messages with a convenience API.

:strong:`Example`

In this example, :code:`example_error` represents an error. There are  :code:`reportError` overloads for string, error, and NSException.

.. code-block:: Swift

    SplunkRum.reportError(example_error)


Manage global attributes
------------------------------

Global attributes are key-value pairs added to all reported data. Global attributes are useful for reporting app or user-specific values as tags.

:strong:`Example`

Suppose you want to filter spans by account type. In our organization, there are three types of account plans: gold, silver, and bronze.

You add  :code:`accountType={gold,silver,bronze}` to every span reported by the Splunk RUM iOS library. Then, you can specify global attributes  during :code:`SplunkRum.initialize()` as :code:`options.globalAttributes` or use :code:`SplunkRum.setGlobalAttributes` or :code:`SplunkRum.removeGlobalAttribute` at any point during your app's execution.


Manually change screen names
------------------------------

By default, the Splunk RUM iOS library collects the name of :code:`ViewController`. You can customize the screen names for your application. The name you choose persists until your next call to :code:`setScreenName`.

:strong:`Example`

Suppose you want to customize the name of your account settings screen in your application:

.. code-block:: Swift

    SplunkRum.setScreenName("AccountSettingsTab")

After you set the :code:`setScreenName` parameter, it disables automatic screen name instrumentation, to avoid overwriting your chosen name(s). If you instrument your application to :code:`setScreenName`,  apply it to all screen names.

Tracing API
---------------
You can use the OpenTelemetry Swift APIs to report on events in your mobile application.

:strong:`Example`

Suppose you have this calculateTax function you wanted to time:

.. code-block:: swift

    func calculateTax() {
    let tracer = OpenTelemetrySDK.instance.tracerProvider.get(instrumentationName: "MyApp")
    let span = tracer.spanBuilder(spanName: "calculateTax").startSpan()
    span.setAttribute(key: "numClaims", value: claims.count)
    ...
    ...
    span.end() // or use defer for this
    }


Span filtering
---------------

You can modify or reject spans with a spanFilter function. This is supported only in Swift.
This example show how to remove a span:

:strong:`Example`

.. code-block:: swift

  options.spanFilter = { spanData in
    var spanData = spanData
    if spanData.name == "DropThis" {
      return nil // spans with this name will not be sent
    }
    var atts = spanData.attributes
    atts["http.url"] = .string("redacted") // change values for all urls
    return spanData.settingAttributes(atts)
  }

.. _ios-webview-instrumentation:

Instrument iOS WebViews using the Browser RUM agent
==========================================================

Mobile RUM instrumentation and Browser RUM instrumentation can be used simultaneously by sharing the ``splunk.rumSessionId`` between both instrumentations to see RUM data combined in one stream.

The following Swift snippet shows how to integrate iOS RUM with Splunk Browser RUM:

.. code-block:: swift

  import WebKit
  import SplunkOtel

  ...
    /* 
  Make sure that the WebView instance only loads pages under 
  your control and instrumented with Splunk Browser RUM. The 
  integrateWithBrowserRum() method can expose the splunk.rumSessionId
  of your user to every site/page loaded in the WebView instance.
  */
    let webview: WKWebView = ...
    SplunkRum.integrateWithBrowserRum(webview)

How to contribute
=========================================================

The Splunk Distribution of OpenTelemetry iOS is open source software. You can contribute to its improvement by creating pull requests in GitHub. To learn more, see the :new-page:`contribution guidelines <https://github.com/signalfx/splunk-otel-ios/blob/main/CONTRIBUTING.md>` in GitHub.