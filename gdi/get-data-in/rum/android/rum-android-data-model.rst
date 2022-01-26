.. _rum-android-data:

****************************************
RUM data model for Android applications
****************************************

.. meta::
   :description: Understand which RUM data you collect from Android applications when using Splunk Real User Monitoring (RUM).

The Splunk Android RUM library collects the following types of data about your mobile Android application: 

.. list-table:: 
   :widths: 20 40 
   :header-rows: 1

   * - :strong:`Data type`
     - :strong:`Description`
   * - App life cycle events
     - App start up, app going to background transitions, and screen transitions, both for activities and fragments 
   * - Network related events
     - Network changes, connection information. For example:
        * HTTP requests
        * Latency
        * Errors
   * - Errors and crash events
     - App crashes and exceptions
   * - Custom events
     - Events not collected by auto instrumentation and any events that need manual work or coding
   * - Application Not Responding (ANR) events
     - ANRs occur when the application does not respond to an input event after a certain amount of time