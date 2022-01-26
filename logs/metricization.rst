.. _logs-metricization:

*****************************************************************
Create metrics from your logs with log metricization rules
*****************************************************************

.. meta created 2021-04-28
.. meta DOCS-2247

.. meta::
  :description: Manage the logs pipeline with log metricization rules.

.. note:: Only customers with a Splunk Log Observer entitlement in Splunk Observability Cloud can create log metricization rules. If you do not have a Log Observer entitlement and are using Splunk Log Observer Connect instead, see :ref:`logs-intro-logconnect` to learn what you can do with the Splunk Enterprise integration..

Log metricization rules allow you to create a log-derived metric that shows aggregate counts. While Log Observer visual analysis allows you to dynamically view aggregate metrics in the context of your query, log metricization rules allow you to embed metrics from log data in charts, dashboards, and detectors. Like any other metric, a metric derived from log metricization rules counts toward your metrics quota per your contract.

There are two ways to create log metricization rules:

* :ref:`Create a log metricization rule from the logs pipeline <metricization-rule-from-pipeline>`
* :ref:`Create a log metricization rule in the context of a Log Observer query <metricization-rule-from-Log-Observer-query>`

.. _metricization-rule-from-pipeline:

Create a log metricization rule from the logs pipeline
================================================================================

To create a new log metricization rule from scratch in the logs pipeline, follow these steps:

1. From the navigation menu, go to :guilabel:`Organization Settings > Logs Pipeline Management`.

2. Click :guilabel:`New Metricization Rule`.

3. Define a matching condition. Only matching logs will be included in the chart resulting from your metricization rule.

4. To configure a metric, perform a Log Observer aggregation query. Select a function, an aggregate, and a dimension for this query. You can choose from the following functions: :guilabel:`Count`, :guilabel:`AVG`, :guilabel:`MAX`, :guilabel:`MIN`, and :guilabel:`SUM`. The default function is :guilabel:`Count`. The default aggregate is :guilabel:`All(*)`, and the default dimension is :guilabel:`severity`. See :ref:`logs-aggregations` for a thorough explanation of Log Observer aggregation queries.

5. Next, select a target field by which you want to aggregate logs. For example, you can choose :guilabel:`services` as your target field, then group logs by :guilabel:`status`. Fields with "#", such as :guilabel:`amount`, require a numerical value to aggregate logs. 

6. Click :guilabel:`Next`.

7. Review your MTS (Metric Time Series) summary to see how your metricization will impact billing. You can optionally select an ingest token to limit the MTS count.

8. Click :guilabel:`Next`.

9. Give your metric a name. The name defaults to the function and target fields.

10. You can optionally change the Metric Type to :guilabel:`Gauge`, :guilabel:`Counter`, or :guilabel:`Cumulative counter`.

11. Give your rule a name and description.

12. Review your configuration, then click :guilabel:`Save`. Your rule appears in the list of Metricization Rules on the Logs Pipeline Management page. Click the name of your rule to view a summary of the rule. To view the output of your rule, click :guilabel:`view your new metric in a chart`. This takes you to chart builder populated with your new metric. In less than 60 seconds, you will see metrics reported within the chart.

13. While still in chart builder, click :guilabel:`Save As` to save your new metric as a chart. You can then embed it on a new or existing dashboard.

.. _metricization-rule-from-Log-Observer-query:

Create a log metricization rule in the context of a Log Observer query
================================================================================

Often, you might notice the potential value of an existing query and decide to create a log metricization rule based on that query. You can quickly launch the creation of a new metricization rule from a Log Observer query. 
  
To create a new log metricization rule in the context of an existing search query, follow these steps:

1. In the navigation menu, go to :guilabel:`Log Observer`.

2. Create a query that aggregates logs. See :ref:`logs-aggregations` to learn how.

3. In the upper right corner, click the three-dot :guilabel:`More` menu and select :guilabel:`Save as Metric`. This takes you to the Configure Metric page in Logs Pipeline Management.

4. Go to step 3 in :ref:`Create a log metricization rule from the logs pipeline <metricization-rule-from-pipeline>` and complete the instructions.