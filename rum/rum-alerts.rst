

.. _rum-alerts:


************************************************
Alert on Splunk RUM data 
************************************************

Configure detectors to alert on your Splunk RUM metrics so that you can monitor and take timely action on alerts associated with your application. In Splunk RUM for Browser, alerts are triggered on aggregate metrics for the entire application. If you want to create an alert for a page level metric, first create a custom event for the metric, then create an alert for the custom event. To learn more, see :ref:`Create custom events <rum-custom-event>`. 


If you are new to alerts and detectors, start with :ref:`Introduction to alerts and detectors in Splunk Observability Cloud <get-started-detectoralert>`. 


How alerts work in RUM
========================
Splunk RUM leverages the Infrastructure Monitoring platform to create detectors and alerts.


What metrics can I trigger alerts on?
------------------------------------------------------------
You can trigger alerts on the following kind of metrics:

.. list-table:: 
   :widths: 30 60 
   :header-rows: 1

   * - :strong:`Category`
     - :strong:`Metrics`
   * - Web vitals
     -  * LCP (Largest Contentful Paint) 
        * FID (First Input Delay)
        * CLS (Cumulative Layout Shift) 
   * - Custom events  
     -  * :ref:`Create custom events<rum-custom-event>`
   * - Page metrics 
     -  * Front-end requests 
        * Front-end errors 
        * Long task length 
        * Long task count 
   * - Endpoint metrics 
     -  * Endpoint requests 
        * Endpoint latency 
        * TTFB (Time to First Byte)


If you are new to web vitals, see :new-page:`https://web.dev/vitals/` in the Google developer documentation.


Alert trigger conditions
------------------------------
RUM alert conditions are designed to reduce noise and provide clear, actionable insights on your data. When 50% of the data points in a five minute window cross the static threshold you defined then an alert is triggered. 

Integrations for RUM alerts
------------------------------

You can use the following methods and integrations to receive alerts from Splunk RUM:

* Email notifications
* Jira
* PagerDuty
* ServiceNow
* Slack 
* VictorOps
* XMatters 

You can also add a link in your message such as a link to a run book or other troubleshooting information in your organization.  

Data retention
------------------------------

Alerts are triggered based on Infrastructure Monitoring metrics. Metrics are stored for 13 months. For more, see :ref:`Data retention in Splunk Observability Cloud <data-o11y>`.


Use case for RUM alerts
==============================

Web vitals have a standard range that denotes good performance. For example, a Largest Contentful Paint (LCP) metric of more than 2.5 seconds might lead to bad user experience on your application. With Splunk RUM, you can create an alert to notify you when your aggregated LCP is more than 2.5 seconds, send a Slack notification to your team, and link to the runbook with the steps on how to remedy the slow LCP.


Create a detector 
==================

You can create a detector from either the RUM overview page or from Tag Spotlight.

Follow these steps to create a detector in RUM: 

1. In Splunk RUM, select a metric that is of interest to you to open Tag Spotlight.  

2. Select :strong:`Create New Detector`.

3. Configure your detector:

    * Name your detector 
    * Select the metric that is of interest to you and the type of data 
    * Set the static threshold for your alert 
    * Select the scope of your alert
    * Select the severity of the alert 

4. Share your alert with others by integrating with the tool your team uses to communicate and adding a link to your runbook.  

5. Select :strong:`Activate`.


Create dashboards for your RUM alerts 
================================================
You can create dashboards for both web and mobile metrics.

The following table lists the name for each metric in RUM. 

.. list-table:: 
   :widths: 25 25 
   :header-rows: 1

   * - :strong:`Metric`
     - :strong:`Name`
   * - LCP 
     - :code:`rum.webvitals_lcp.time.ns.p75`
   * - CLS
     - :code:`rum.webvitals_cls.score.p75`
   * - FID
     - :code:`rum.webvitals_fid.time.ns.p75`   
   * - Mobile crash 
     - :code:`rum.crash.count`
   * - App error 
     - :code:`rum.app_error.count`
   * - Cold start
     - :code:`rum.cold_start.time.ns.p75`
   * - Cold start count  
     - :code:`rum.cold_start.count`
   * - Warm start count
     - :code:`rum.warm_start.count`
   * - Warm start time 
     - :code:`rum.warm_start.time.ns.p75`
   * - Hot start count 
     - :code:`rum.hot_start.count`
   * - Hot start time 
     - :code:`rum.hot_start.time.ns.p75`
   * - Event Requests/Errors
     - :code:`rum.workflow.count`
   * - Front-end requests 
     - :code:`rum.page_view.count`      
   * - Document load latency 
     - :code:`rum.page_view.time.ns.p75`
   * - Front-end errors 
     - :code:`rum.client_error.count`
   * - Long Task Count
     - :code:`rum.long_task.count`
   * - Long Task Length
     - :code:`rum.long_task.time.ns.p75`
   * - Endpoint Requests/Errors
     - :code:`rum.resource_request.count`
   * - Endpoint Latency 
     - :code:`rum.resource_request.time.ns.p75`
   * - TTFB 
     - :code:`rum.resource_request.ttfb.time.ns.p75`
  
  







To create charts and dashboard for your RUM alerts and detectors, see:   

* :ref:`Link detectors to charts <linking-detectors>` in Alerts and Detectors.    

* :ref:`Dashboards in Splunk Observability Cloud <dashboards>` in Dashboards and Charts. 



View detectors and alerts  
==========================================

For instructions, see:

* :ref:`Edit detectors through the SignalFlow tab <v2-detector-signalflow>`

* :ref:`View alerts in Splunk Observability Cloud <view-alerts>` 

* :ref:`View detectors in Splunk Observability Cloud <view-detectors>`