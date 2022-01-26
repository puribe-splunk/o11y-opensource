.. _android-rum-install:

*******************************************************************************
Install the Android RUM agent for Splunk RUM
*******************************************************************************

.. meta::
   :description: The Android RUM agent from the Splunk Distribution of OpenTelemetry Android provides a Real User Monitoring (RUM) instrumentation framework for your Android applications. Use it to send RUM data from your mobile apps to Splunk RUM.

You can instrument your Android applications for Splunk RUM using the Android RUM agent from the Splunk Distribution of OpenTelemetry Android.

To instrument your Android application and get data into Splunk RUM, follow these steps:

Check compatibility and requirements 
===============================================

Splunk RUM for Mobile supports API Level 21 and higher, with core library desugaring enabled. 

To enable desugaring follow these steps: 

1. Go to :new-page:`Java 8+ API desugaring support (Android Gradle Plugin 4.0.0+) <https://developer.android.com/studio/write/java8-support#library-desugaring>` on the Android developer website.

2. Update the Android plugin to the version 4.1.x. For instructions on how to upgrade, see :new-page:`Update the Android Gradle plugin <https://developer.android.com/studio/releases/gradle-plugin#updating-plugin>`.

3. Add the code snippet from the documentation :new-page:`Java 8+ API desugaring support (Android Gradle Plugin 4.1.x) <https://developer.android.com/studio/write/java8-support#library-desugaring>` on the Android developer website for either Groovy or Kotlin to your app module's :code:`build.gradle` file.

4. Depending on the configuration of your app, you might also need to add the code snippet you selected in step 2 to the  :code:`build.gradle` file of your library module. For more information, see the documentation :new-page:`Java 8+ API desugaring support (Android Gradle Plugin 4.1.x) <https://developer.android.com/studio/write/java8-support#library-desugaring>` on the Android developer website. 

.. _install-android-rum-agent:

Install the Android RUM agent 
======================================================

The Android RUM agent must be installed as a code-level dependency in your Android application. You can install the dependency either by loading it from the Maven Central repository or by using a local repository.

Maven Central
------------------------------------------------------

Follow these steps to install the Android RUM agent using Maven Central:

1.  Add Maven Central as a Maven repository to the repositories section of your main :code:`build.gradle` file:

.. code-block:: java

   allprojects {
      repositories {
         google()
      ...
         mavenCentral()
      }
   }

2. Add the latest Android RUM agent release as a dependency in the :code:`build.gradle` file of your application:

.. code-block:: java

   dependencies {
   ...
   // To find the latest release, see https://github.com/signalfx/splunk-otel-android/releases 
      implementation ("com.splunk:splunk-otel-android:<latest-release>")
   ...
   }

3. Configure and initialize the Android RUM agent by passing a configuration object to the initialization call in ``Application.onCreate()``:

   .. code-block:: java 

      class MyApplication extends Application {

         private final Config config = SplunkRum.newConfigBuilder()
            .realm("<realm>") // Splunk Realm. For example, us0
            .rumAccessToken("<RUM-access-token>") // Your RUM token
            .applicationName("<your-application-name>") // Name of your app
            .deploymentEnvironment("<environment-name>") // Environment name
            .build();
         
      // ...

         @Override
         public void onCreate() {
            super.onCreate();
            SplunkRum.initialize(config, this);
         }
      }

   * The value of ``.realm()`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``. 
   * To generate a RUM access token, see :ref:`rum-access-token`.

4. Release the changes to the Android application and make sure that the application is being used.

5. Verify that the data is appearing in the RUM dashboard.

Local repository
----------------------------------------------------

To use a local Maven repository, follow these steps:

1. Clone the Android RUM agent repository locally:

   .. code-block:: bash
      
      git clone https://github.com/signalfx/splunk-otel-android.git

2. Create a local build and publish to your local Maven repository:

   .. code-block:: bash

      ./gradlew publishToMavenLocal

3. Make sure that ``mavenLocal()`` is set as a repository in the main ``build.gradle`` file of your application:

   .. code-block:: java

      allprojects {
         repositories {
            google()
               ...
            mavenLocal()
         }
      }

4. Add the library you've built locally as a dependency in your ``build.gradle`` file. Use the version you've built:

   .. code-block:: java

      dependencies{
         ...
         implementation ("com.splunk:splunk-otel-android:0.13.0-SNAPSHOT")
         ...
      }

5. Configure and initialize the Android RUM agent by passing a configuration object to the initialization call in ``Application.onCreate()``:

   .. code-block:: java 

      class MyApplication extends Application {

         private final Config config = SplunkRum.newConfigBuilder()
            .realm("<realm>") // Splunk Realm. For example, us0
            .rumAccessToken("<RUM-access-token>") // Your RUM token
            .applicationName("<your-application-name>") // Name of your app
            .deploymentEnvironment("<environment-name>") // Environment name
            .build();
         
      // ...

         @Override
         public void onCreate() {
            super.onCreate();
            SplunkRum.initialize(config, this);
         }
      }

   * The value of ``.realm()`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``. 
   * To generate a RUM access token, see :ref:`rum-access-token`.

6. Release the changes to the Android application and make sure that the application is being used.

7. Verify that the data is appearing in the RUM dashboard.


Additional settings
=============================================

You can customize your instrumentation by adding the following settings to the configuration object:

.. list-table:: 
   :widths: 25 25 50
   :header-rows: 1

   * - :strong:`Configuration option`
     - :strong:`Type`
     - :strong:`Description`
   * - :code:`deploymentEnvironment`
     - String
     - Sets the Splunk Environment attribute on the spans that are generated by the instrumentation. 
   * - :code:`beaconEndpoint`
     - String
     - Use this method instead of :code:`realm(String)` to provide the full URL of the RUM ingest endpoint. For example, :code:`https://rum-ingest.<realm>.signalfx.com/v1/rum`. 
   * - :code:`debugEnabled`
     - Boolean
        - Default: false
     - Enable debug mode to turn on the OTel logging span exporter. 
   * - :code:`crashReportingEnabled`
     - Boolean
        - Default: true
     - Ability to disable crash reporting
   * - :code:`anrDetectionEnabled`
     - Boolean
        - Default: true
     - Ability to disable ANR (application not responding) detection and reporting.  
   * - :code:`networkMonitorEnabled`
     - Boolean
        - Default: true
     - Ability to disable network monitoring.
   * - :code:`globalAttributes`
     - Attributes
     - Ability to append every span collected by with a set of OTel attributes. 
   * - :code:`filterSpans`
     - Consumer<SpanFilterBuilder>
     - Ability to customize and remove spans emitted by the splunk-otel-android library. 

.. _android-webview-instrumentation:

Instrument Android WebViews using the Browser RUM agent
==========================================================

Mobile RUM instrumentation and Browser RUM instrumentation can be used simultaneously by sharing the ``splunk.rumSessionId`` between both instrumentations to see RUM data combined in one stream.

The following Android snippet shows how to integrate Android RUM with Splunk Browser RUM:

.. code-block:: java

   import android.webkit.WebView;
   import com.splunk.rum.SplunkRum;

   //...
   /* 
   Make sure that the WebView instance only loads pages under 
   your control and instrumented with Splunk Browser RUM. The 
   integrateWithBrowserRum() method can expose the splunk.rumSessionId
   of your user to every site/page loaded in the WebView instance.
   */
   @Override
   public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
      super.onViewCreated(view, savedInstanceState);
      binding.webView.setWebViewClient(new LocalContentWebViewClient(assetLoader));
      binding.webView.loadUrl("https://subdomain.example.com/instrumented-page.html");

      binding.webView.getSettings().setJavaScriptEnabled(true);
      binding.webView.addJavascriptInterface(new WebAppInterface(getContext()), "Android");
      SplunkRum.getInstance().integrateWithBrowserRum(binding.webView);
   }

How to contribute
=========================================================

The Splunk Distribution of OpenTelemetry Android is open source software. You can contribute to its improvement by creating pull requests in GitHub. To learn more, see the :new-page:`contribution guidelines <https://github.com/signalfx/splunk-otel-android/blob/main/CONTRIBUTING.md>` in GitHub.

Versioning policy
---------------------------------------------------------

The versioning of the Android RUM agent follows semantic versioning rules. To have more control over the version you load, see the following versioning policy:

* Use the ``LATEST`` version to use the latest version of the Android RUM agent. This might not be suitable for manual instrumentation, as breaking API changes might occur between major version changes.
* Use ``MAJOR`` versions, for example ``v1``, if you want to receive new features automatically while keeping backward compatibility with the API. This is the default for all production deployments, as well as for npm installations.
* Use ``MINOR`` versions, for example ``v1.1``, to receive bug fixes while not receiving new features automatically.
* Use ``PATCH`` versions, for example, ``v1.2.1``, to pin a specific version of the agent for your application.