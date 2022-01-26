.. _rum-ios-data:

***********************************
RUM data model for iOS applications
***********************************

.. meta::
   :description: Understand which RUM data you collect from iOS applications when using Splunk Real User Monitoring (RUM).

The Splunk RUM iOS library includes a Swift package that collects the following types of data about your iOS application: 

.. list-table:: 
   :widths: 20 40 
   :header-rows: 1

   * - :strong:`Data type`
     - :strong:`Description`
   * - App life cycle events
     - App start up and app going to background
   * - UI level events
     - Screen transitions and clicks on components
   * - Network related events
     - Network changes, connection information. For example:
        * HTTP requests
        * Latency
        * Errors
   * - Errors
     - Crashes and exceptions
   * - Custom events 
     - Events not collected by auto-instrumentation and any events that need manual work or coding.