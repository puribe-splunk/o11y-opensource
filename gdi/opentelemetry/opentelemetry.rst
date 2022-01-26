.. _otel-intro:

*******************************************************************************************
Install and configure Splunk Distribution of OpenTelemetry Collector
*******************************************************************************************

.. meta::
      :description: Install and configure Splunk Distribution of OpenTelemetry Collector to receive, process, and export metric, trace, and log data for Splunk Observability Cloud. Splunk Observability Cloud offers a guided setup to install the Splunk OpenTelemetry Collector. This guide provides information to install the Splunk OpenTelemetry Collector without using the guided setup.

.. toctree::
    :maxdepth: 4
    :titlesonly:
    :hidden:

    install-the-collector.rst
    configure-the-collector.rst
    use-the-collector.rst
    troubleshooting.rst
    Resources <resources.rst>
    
The OpenTelemetry Collector uses pipelines to receive, process, and export trace data with components known as receivers, processors, and exporters. You can also add extensions that provide OpenTelemetry Collector with additional functionality, such as diagnostics and health checks.

The OpenTelemetry Collector has a core version and a contributions version. The core version provides receivers, processors, and exporters for general use. The contributions version provides receivers, processors, and exporters for specific vendors and use cases.

Splunk Distribution of OpenTelemetry Collector is a distribution of the OpenTelemetry Collector. The distribution is a project that bundles components from OpenTelemetry Core, OpenTelemetry Contrib, and other sources to provide data collection for multiple source platforms. The customizations in the Splunk distribution include these features:

* Better defaults for Splunk products
* Fluentd for log capture
* Tools to support migration from SignalFx products

Here is an overview of the process for using the Splunk Distribution of OpenTelemetry Collector:

#. :ref:`otel-install-platform`. Get instructions for installing the Splunk OpenTelemetry Collector on a variety of platforms.
#. :ref:`otel-configuration`. Download default and advanced configuration files.
#. :ref:`otel-using`. Determine your access token, realm, and deployment mode to start using the Splunk OpenTelemetry Collector.  
#. :ref:`otel-troubleshooting`. Try these troubleshooting techniques and learn how to open a support request.

See :new-page:`Use Splunk Distribution of OpenTelemetry Collector <https://docs.splunk.com/Observability/gdi/opentelemetry/resources.html>` for a complete list of resources. Use your browser's search function to quickly find what you’re looking for.

.. note::
   :new-page:`Splunk Observability Cloud <https://docs.splunk.com/Observability/get-started/welcome.html>` offers a guided setup to install the Splunk OpenTelemetry Collector. 

   #. Select **Data Setup** from the Observability Cloud main menu.
   #. Select **Splunk OpenTelemetry Collector**.
   #. Select your platform.
   #. Follow the step-by-step process provided in the guided setup.

   This guide provides information to install the Splunk OpenTelemetry Collector without using the guided setup.
