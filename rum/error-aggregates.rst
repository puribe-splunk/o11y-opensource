.. _error-aggregates:

*********************************************
Error monitoring in Splunk RUM
*********************************************

Splunk RUM for Browser creates an aggregate view called "Frontend Errors by ErrorID" that shows the JavaScript errors that occur most frequently in your applications. When you drill into an error, you can see the error type, the error message, associated stack trace, and the trend of the error frequency. 


How errors are aggregated 
========================================
Errors are aggregated based on the stack trace. The error stack trace contains the error type and error message in the body of the stack trace. The following table outlines the different ways errors are grouped together depending on the situation. 

.. list-table::
   :widths: 20 20 
   :header-rows: 1

   * - :strong:`Situation`
     - :strong:`How errors are grouped together`
   * - Errors have a stack trace
     - Errors are grouped by the hash ID of the stack trace
   * - Errors don't have a stack trace 
     - Errors are grouped by the hash ID of the message and error type
   * - Errors don't have an error message
     - Errors are grouped by the hash ID of the error type

The error ID can represent: 

* hash ID of a stack trace
* hash ID of a message 
* hash ID of the error type 


Use case: Quickly find the top JavaScript errors across your application
==========================================================================
In Splunk RUM, the front-end errors view shows the JavaScript errors by page. With "Frontend Errors by ErrorID", you can see the top ten JavaScript errors across your entire application. In the "Frontend Errors by ErrorID" modal the information is displayed by error type, error ID, then error message. 

Select :strong:`See All` to explore all of the front-end errors in Tag Spotlight. For more examples on how you can use Tag Spotlight, see :ref:`Find the root cause of an error using Tag Spotlight <troubleshoot-tag-spotlight>`.



