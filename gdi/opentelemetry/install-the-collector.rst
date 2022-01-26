.. _otel-install-platform:

***********************************************************************************
Install the Splunk OpenTelemetry Collector 
***********************************************************************************

.. meta::
      :description: Describes platform-specific installation information for Splunk Distribution of OpenTelemetry Collector.

.. toctree::
    :maxdepth: 4
    :titlesonly:
    :hidden:

    install-k8s.rst
    install-linux.rst
    install-windows.rst
    uninstall-the-collector.rst

Splunk Distribution of OpenTelemetry Collector is supported on Kubernetes, Linux, and Windows. Deploy one of the following collectors to gather data for Infrastructure Monitoring, APM, and Log Observer:

* Splunk OpenTelemetry Collector for Kubernetes or ``splunk-otel-collector-chart``
* Splunk OpenTelemetry Collector for Linux or ``splunk-otel-collector``
* Splunk OpenTelemetry Collector for Windows or ``splunk-otel-collector``

The Splunk OTel Collector uses the components listed in the following table:

.. list-table::
   :widths: 50 50
   :header-rows: 1

   * - Component
     - Description
   * - Receivers
     - Receivers, which can be push or pull based, is how data gets into the Splunk OpenTelemetry Collector. Receivers support one or more data sources. You must configure one or more receivers. By default, no receivers are configured.
   * - Processors
     - Processors sit between receivers and exporters, reading and sometimes operating on data as it flows through the pipeline. Processors are optional, but you should include processors in your configuration. The order in which processors are listed in the ``processors`` section of the configuration is significant.
   * - Exporters
     - An exporter, which can be push or pull based, is how you send data to one or more destinations. Exporters support one or more data sources.

|br|

Once configured, enable these components using pipelines within the service section of the configuration. The service section contains two subsections: extensions and pipelines. The extensions section is where you optionally enable any extensions you have configured, and the pipelines section is where you define one or more pipelines, each of which consists of receivers, processors (optional), and exporters. The service section's two subsections are described in the following table.


.. list-table::
   :widths: 50 50
   :header-rows: 1

   * - Component
     - Description
   * - Extension
     - Provide capabilities that can be added to the Splunk OpenTelemetry Collector, but which do not require direct access to telemetry data and are not part of pipelines.
   * - Pipeline
     - Pipelines can be traces, metrics, or logs. Pipelines consist of a set of receivers, processors, and exporters. Each receiver, processor, and exporter must be defined in the configuration outside of the service section to be included in a pipeline.

|br|

Next, install the Splunk OTel Collector for your platform. See :ref:`otel-install-k8s`, :ref:`otel-install-linux`, or :ref:`otel-install-windows`.
