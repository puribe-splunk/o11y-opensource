.. _logs-intro-logconnect:

*****************************************************************
Introduction to Splunk Log Observer Connect
*****************************************************************

.. meta created 2021-12-03

.. meta::
   :description: Integrate Log Observer with Splunk Enterprise and use Log Observer Connect.


What is Splunk Log Observer Connect?
==============================================================
Splunk Log Observer Connect is an integration that allows you to query your Splunk Enterprise logs using the capabilities of Splunk Log Observer. With Log Observer Connect, you can troubleshoot your application and infrastructure behavior using high-context logs. Perform codeless queries on your Splunk Enterprise logs to detect the source of problems in your systems, then jump to related content throughout Splunk Observability Cloud in one click.

Region and version availability
==============================================================
Splunk Log Observer Connect is available in the AWS regions us0, us1, and eu0. Splunk Log Observer Connect is compatible with Splunk Enterprise versions 8.2 and higher. 

What can I do with Log Observer Connect?
==============================================================
The following table lists features available to customers who have integrated Splunk Enterprise with Log Observer, allowing them to use Log Observer Connect. If you have a Log Observer entitlement in Observability Cloud, see :ref:`get-started-logs` for a complete list of Log Observer features.

.. list-table::
   :header-rows: 1
   :widths: 40, 30, 30

   * - :strong:`Do this`
     - :strong:`With this tool`
     - :strong:`Link to documentation`

   * - View your incoming logs grouped by severity over time and zoom in or out to the time period of your choice.
     - Timeline
     - :ref:`logs-timeline`

   * - Scan logs.
     - Logs Table
     - :ref:`logs-raw-logs-display`

   * - Find out which path in your API has the slowest response time.
     - Log aggregations
     - :ref:`logs-aggregations`

   * - Search logs by keyword or field.
     - Filter bar
     - :ref:`logs-keyword`

   * - Filter your logs to see only logs that contain a field of your choice with the value :guilabel:`error`.
     - Logs Table
     - :ref:`logs-filter-logs-by-field`

   * - View the JSON of an individual log.
     - Log Details
     - :ref:`logs-individual-log`

   * - See the metrics, traces, and infrastructure related to a specific log.
     - Related Content
     - :ref:`get-started-use-case`

   * - Save and share Log Observer queries.
     - Saved Queries
     - :ref:`logs-save-share`


Get started with Log Observer Connect
==============================================================
See :ref:`logs-set-up-logconnect` to learn how to set up Log Observer Connect and begin querying your Splunk Enterprise logs.