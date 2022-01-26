.. _get-started-rum:

************************************************
Introduction to Splunk RUM
************************************************

.. meta::
   :description: With Splunk Real User Monitoring, you can gain insight about the performance and health of the front-end user experience of your application.

What is Splunk RUM?
========================================
With Splunk Real User Monitoring (RUM), you can gain insight about the performance and health of the front-end user experience of your application. Splunk RUM offers two solutions:

.. list-table::
   :header-rows: 1
   :widths: 15, 50

   * - :strong:`Product`
     - :strong:`Description`
   * - Splunk RUM for Browser
     - Splunk RUM for Browser collects performance metrics, web vitals, errors, and other forms of data to enable you to detect and troubleshoot problems in your application. For a complete view of your application from browser to back-end, integrate with Splunk APM.
   * - Splunk RUM for Mobile
     - Splunk Real User Monitoring (RUM) for Mobile provides visibility into your native iOS and Android mobile applications by equipping you with comprehensive performance monitoring, directed troubleshooting, and full-stack observability. 

What can I do with Splunk RUM?
=========================================

.. list-table::
   :header-rows: 1
   :widths: 50, 28

   * - :strong:`Do this`
     - :strong:`Link to documentation`
   * - Learn how to identify errors and other problems like long resource response times in your browser spans. 
     - :ref:`Use case: Investigate errors in your browser spans <rum-identify-span-problems>`
   * - Learn how to start monitoring your iOS and Android data. 
     - :ref:`Use case: Start monitoring your mobile data <use-cases-mobile>` 
   * - Create custom events to capture meaningful metrics about customer journeys and user behavior on your site.
     - :ref:`Create custom events  <rum-custom-event>`  
   * - Experiment with the demo applications for Splunk RUM for Mobile
     - :ref:`Experiment with the demo applications for Splunk RUM for Mobile <rum-sample-app>`  

Integrate with Splunk APM
=============================
If you want to monitor your application from browser to back-end, then integrate Splunk RUM with Splunk APM. When you integrate Splunk RUM for Browser with Splunk APM, you start sending server timing metrics to RUM along with the back-end trace ID that was generated. Splunk RUM for Browser uses the server-timing header response times to associate the RUM span with the corresponding APM trace. For more information on Splunk APM, see :ref:`Monitor applications with Splunk APM <get-started-apm>`.

Get data in 
=============================
Follow these steps: 

.. list-table::
   :header-rows: 1
   :widths: 25, 25 

   * - :strong:`Product`
     - :strong:`Instructions`
   * - Splunk RUM for Browser
     - :ref:`browser-rum-gdi`
   * - iOS
     - :ref:`rum-mobile-ios`
   * - Android
     - :ref:`rum-mobile-android`

