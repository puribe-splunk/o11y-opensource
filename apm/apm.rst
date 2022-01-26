.. _apm:

*****************
Set up Splunk APM
*****************

.. meta::
   :description: Get started monitoring applications with Splunk APM.

Monitor the availability and performance of your distributed applications by sending traces to Splunk APM. A trace contains one or more spans that log requests or transactions in your system. You can also use trace data as the basis of detector alerts, and you can correlate trace data with logs and other resources.

To set up Splunk APM and begin analyzing application availability and performance, follow these steps: 

1. :ref:`send-traces`
2. :ref:`verify-apm-data`
3. :ref:`apm-orientation`
4. :ref:`customize-apm`

.. _send-traces: 

Get data into Splunk APM
================================

To begin sending spans to Splunk APM, start by choosing the right data collection method for your system.

Choose the right data collection method for your system
-------------------------------------------------------
If you are using multiple components of the Splunk Observability Cloud Suite and want to collect host metrics, logs, or other application data in addition to traces, follow the steps in :ref:`get-started-get-data-in` to get data into Observability Cloud. Then see :ref:`verify-apm-data` in this topic to make sure your data is coming into Splunk APM as you expect. 

If you have already deployed the upstream OpenTelemetry Collector, you can use your existing deployment to send traces to Splunk APM. See :ref:`using-upstream-otel` for more information. However, note that using the Splunk Distribution of OpenTelemetry Collector provides a more supported experience, customized for Splunk APM. 

If you want to start sending traces to Splunk APM with Splunk Distribution of OpenTelemetry Collector using the guided setup wizards in Splunk APM, follow the steps in the sections below. To set it up yourself, see :ref:`otel-intro`. 

    a. :ref:`deploy-connector`
    b. :ref:`instrument-applications`

.. _deploy-connector:

Deploy Splunk OpenTelemetry Collector on your hosts
---------------------------------------------------------------
To send traces to Splunk APM, first deploy an OpenTelemetry Collector on the hosts in which your applications are running. Observability Cloud offers OpenTelemetry Collector distributions for Kubernetes, Linux, and Windows. These distributions integrate the collection of data from hosts and data forwarding to Observability Cloud.

.. note:: 
  Although you're not required to install Splunk OpenTelemetry Collector, when you do you get the following benefits:

  - The collector can add span metadata associated with the infrastructure in which your applications are running.
  - You establish a single configuration point in which you can specify authorization details.
  - You establish a single configuration point in which you add custom tags and custom processing to your spans.
  - You can batch spans together from many sources. By batching spans, you reduce load on the backend.

To deploy Splunk OpenTelemetry Collector on a host, select :guilabel:`Navigation menu > Data setup` and search for the host type you're using. Then follow the steps in the setup wizard. 

See the following table for more information about deploying Splunk OpenTelemetry Collector on Kubernetes, Linux, and Windows hosts:

.. list-table::
   :header-rows: 1
   :widths: 20, 50, 30

   * - :strong:`Host type`
     - :strong:`Collector`
     - :strong:`Documentation`

   * - Kubernetes
     - Splunk OpenTelemetry Collector for Kubernetes 
     - :ref:`get-started-k8s`

   * - Linux
     - Splunk OpenTelemetry Collector for Linux 
     - :ref:`get-started-linux`

   * - Windows
     - Splunk OpenTelemetry Collector for Windows 
     - :ref:`get-started-windows`

.. _instrument-applications:

Instrument your applications and services to get spans into Splunk APM
-------------------------------------------------------------------------------
Use the auto-instrumentation libraries provided by Splunk Observability Cloud to instrument services in supported programming languages. To get the highest level of support, send spans from your applications to the OpenTelemetry Collector you deployed in the previous step.

To instrument a service, send spans from the service to an OpenTelemetry Collector deployed on the host or Kubernetes cluster in which the service is running. How you specify the OpenTelemetry Collector endpoint depends on the language you are instrumenting. 

.. admonition:: Add span tags to your spans

  Span tags, known as "attributes" in OpenTelemetry, add crucial context to your spans to enable troubleshooting and analysis. You can add tags to your spans either during instrumentation or in a processor added to the YAML configuration file of the OpenTelemetry Collector you're using to aggregate data from multiple services. 
  
  The ``deployment.environment`` tag is particularly useful because it enables you to filter your Splunk APM by deployment environment. To learn more about the environment tag, see :ref:`apm-environments`.
  
  See :ref:`apm-add-context-trace-span` to learn how to add span tags to spans.

In the following table, follow the instrumentation steps for the language that each of your applications is running in. 

.. list-table::
   :header-rows: 1
   :widths: 20, 40, 40

   * - :strong:`Language`
     - :strong:`Available instrumentation`
     - :strong:`Documentation`

   * - Java
     - Splunk Distribution of OpenTelemetry Java
     - :ref:`get-started-java`

   * - Python
     - Splunk Distribution of OpenTelemetry Python
     - :ref:`get-started-python`
     
   * - Node.js
     - Splunk Distribution of OpenTelemetry JS
     - :ref:`get-started-nodejs`

   * - .NET
     - SignalFx Instrumentation for .NET 
     - :ref:`get-started-dotnet`

   * - Ruby
     - SignalFx Tracing Library for Ruby 
     - :ref:`get-started-ruby`

   * - PHP
     - SignalFx Tracing Library for PHP 
     - :ref:`get-started-php`
   
   * - Go
     - SignalFx Tracing Library for Go
     - :ref:`get-started-go`

| After you instrument your applications, you're ready to verify that your data is coming in.

.. _verify-apm-data:

Verify that your data is coming into Splunk APM
=========================================================
After you instrument your applications, wait for Splunk Observability Cloud to process incoming spans. After several minutes, select :strong:`Navigation menu > APM` and check that you can see your application data beginning to flow into the APM landing page. 

If your data is not appearing in APM as you expect, see :ref:`Troubleshoot your instrumentation <instr-troubleshooting>`.

.. _apm-orientation:

Learn what you can do with Splunk APM
=================================================
Now that your data is flowing into Splunk APM, it's time to do some exploring. Explore what you can do with the features of APM by following these steps:

    a. :ref:`apm-landing-page`
    b. :ref:`apm-service-map`
    c. :ref:`apm-trace-view`
    d. :ref:`apm-tag-spotlight-overview`

.. _apm-landing-page:

Assess the health of your applications with the APM landing page
------------------------------------------------------------------------------
When you log into Splunk Observability Cloud and select :strong:`Navigation menu > APM`, you arrive on the APM landing page. You can use this dashboard of consolidated and unsampled span metrics to get a real-time snapshot of your services and :ref:`Business Workflows<apm-workflows>` at a glance. 

..  image:: /_images/apm/set-up-apm/set-up-apm-01.png
    :width: 95%
    :alt: This screenshot shows an example of the Splunk APM landing page

Use the alerts and top charts on this page as a guide to what needs your attention first.


.. _apm-service-map: 

View dependencies among your applications in the Explore view
----------------------------------------------------------------
From the landing page, click on a service in a chart legend or a row in the Services table to navigate to the Explore view. This view includes the service map, which presents the dependencies and connections among your instrumented and inferred services in APM. This map is dynamically generated based on your selections in the time range, environment, workflow, service, and tag filters. 

You can use these visual cues to understand dependencies, performance bottlenecks, and error propagation. 

..  image:: /_images/apm/set-up-apm/set-up-apm-02.png
    :width: 95%
    :alt: This screenshot shows an example of Splunk APM Explore view

Click on any service in the service map to see charts for that specific service. You can also use the :guilabel:`Breakdown` selector to break the service down by any indexed span tag. 

Click on any chart in this view to show example traces that match the parameters of the chart.  

.. _apm-trace-view: 

Examine the latency of a particular trace in trace view
--------------------------------------------------------
Click :strong:`Traces` to navigate to trace view, where you can see a complete list of all of your traces from the services you’ve instrumented in Splunk APM. From the list of traces, you can click on a specific trace, search by trace ID or use advanced trace search to view the waterfall chart for a particular trace.

..  image:: /_images/apm/set-up-apm/set-up-apm-03.png
    :width: 95%
    :alt: This screenshot shows an example of Splunk APM Trace view

The waterfall chart provides a visualization of the latency of all of the spans that make up the trace being viewed. Under Performance Summary, you can get a snapshot of the performance of the types of spans comprising the trace.

Under the Span Performance tab, you can view a summary of span duration from each operation within each service involved in the trace and the percentage of overall trace workload that they represent.

Full-fidelity tracing, in which APM receives all traces from each of your services rather than sampling them, helps you find and solve specific problems problems arising in individual traces. With full-fidelity tracing, you never need to wonder whether a trace representative of a particular issue was captured by a sample. 

In addition to searching individual traces, you can get an aggregate view of your traces to see where problems are occuring across your systems using tools such as Tag Spotlight. 

.. _apm-tag-spotlight-overview:

Get a top-down view of your services in Tag Spotlight
--------------------------------------------------------------
Return to the service map and click :guilabel:`Tag Spotlight`. Using Tag Spotlight, you can view the request and error rate or latency by span tag for an individual service or business workflow. This helps you identify which particular attributes of your system might be causing reliability or performance issues. 

Rather than looking for similarities across multiple traces, you can use Tag Spotlight to gain a top-down view of your services. This lets you identify the system-wide source of issues and then drill down to find an individual trace that is representative of a wider issue. 

..  image:: /_images/apm/set-up-apm/set-up-apm-04.png
    :width: 95%
    :alt: This screenshot shows an example of Splunk APM Tag Spotlight view

Splunk APM indexes a set of span tags by default, which are shown as boxes in Tag Spotlight. See :ref:`apm-default-span-tags` for the list of these default tags. By indexing additional span tags, you can have other tags show up in their own boxes on this page. To learn how to index additional span tags, see :ref:`apm-index-span-tags`.

When you navigate to Tag Spotlight from the service map and have a specific service selected, all of the information in trace view and Tag Spotlight preserves the context of that particular service. 

To learn more about Tag Spotlight, see :ref:`apm-tag-spotlight`.

.. _customize-apm: 

Customize your Splunk APM experience
===========================================
Now that you’ve explored what you can do with Splunk APM, you can consider more advanced configurations to tailor Splunk APM to your business needs.

Index additional span tags
---------------------------
As a Splunk APM administrator, you can index additional span tags to generate custom request, error, and duration (RED) metrics for tag values within a service. Indexed span tags become filter options within Tag Spotlight, enable breakdowns in the service map, and generate RED metrics. RED metrics for indexed span tags are known as :ref:`Troubleshooting MetricSets<troubleshooting-metricsets>`. To learn more about adding and indexing span tags, see :ref:`apm-span-tags`.

Set up Business Workflows
---------------------------
Administrators can also set up Business Workflows to correlate, monitor, and troubleshoot related traces that make up end-to-end transactions in your system. This lets you filter Service Level Indicators (SLIs) and visualizations by the transaction types you care about most. To learn more about Business Workflows, see :ref:`apm-workflows`.


Continue learning about Splunk APM
============================================
The following resources provide additional information about Splunk APM:

* For an overview of important terms and concepts in Splunk APM, see :ref:`APM terms and concepts <apm-terms-concepts>`.
* For information about Splunk APM use cases, see :ref:`apm-use-cases-intro`.
* For a set of interactive walkthroughs of Splunk APM, see :new-page:`APM Scenarios <https://quickdraw.splunk.com/redirect/?product=Observability&location=apm-walkthrough&version=current>`. 

.. For an example that shows you how to identify the root cause of issues with APM, see :ref:`Example APM root cause investigation <apm-find-root-cause>`.