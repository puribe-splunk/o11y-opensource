.. _apm-index-span-tags:

************************************************************
Index span tags to generate Troubleshooting MetricSets
************************************************************

.. meta::
   :description: Index tags to create Troubleshooting MetricSets that help you troubleshoot services with Splunk Observability Cloud.

Generate Troubleshooting MetricSets by choosing to index span tags associated with specific characteristics and attributes of services. Troubleshooting MetricSets display metrics by characteristics and attributes of services you select, giving you deeper insights into service performance.

Thanks to Splunk APM's full-fidelity tracing, which captures every span from every trace, you can use span tags to break down services and inter-service calls along any characteristic or attribute associated with any given trace. This lets you customize data visualizations and metrics for your monitoring and troubleshooting requirements. 

To get additional value from a particular span tag, a Splunk APM administrator can perform an action known as indexing, which enables additional automatic analysis of the indexed span tag. One benefit of this action is to get aggregated metrics, or Troubleshooting MetricSets, across all spans that contain a specified indexed tag.

.. note::
  Note that Troubleshooting MetricSets are distinct from Monitoring MetricSets in Splunk APM. For an overview of the types of MetricSets in APM, see :ref:`apm-metricsets`. 

Prerequisites for indexing new span tags and generating Troubleshooting MetricSets
====================================================================================
* You need to be an administrator in your organization to index tags and create Troubleshooting MetricSets.
* (Optional) Before you start indexing span tags, see :ref:`apm-index-tag-tips` for guidance on choosing span tags to index. 

.. _apm-tms-details:

Overview: Troubleshooting MetricSets
=================================================================
Every Troubleshooting MetricSet generates the following metrics, also known as Request, Error, and Duration (RED) metrics:

- Request rate
- Error rate
- Root cause error rate
- p50, p90, and p99 latency

These metrics appear when you select a service from the :ref:`service map <service-map>` in the :strong:`Troubleshooting` view.

The measurement precision of Troubleshooting MetricSets is 10 seconds. Splunk APM reports quantiles from a distribution of metrics for each 10-second reporting window. 

Automatically indexed span tags
--------------------------------
Splunk APM automatically indexes and generates Troubleshooting MetricSets for the following six span tags:

  - Environment
  - Endpoint
  - Operation
  - HTTP Method
  - Kind
  - Service

For more details about each of these tags, see :ref:`apm-default-span-tags`. You can't modify or stop APM from indexing these span tags, but you can choose to index additional span tags. Scroll to :ref:`index-span-tags-instructions` in this topic to learn how. 

Cardinality contribution of indexed span tags
------------------------------------------------------------
When you index a new span tag to generate Troubleshooting MetricSets, Splunk APM runs a cardinality contribution analysis to calculate the potential total cardinality contribution after indexing the span tag. This gives you control of what you index and helps account for any limits you have to stay within. If you try to index a span tag that could increase the total cardinality contribution beyond your limit, you can change the existing cardinality contribution of indexed tags by modifying or removing indexed span tags.


.. _index-span-tags-instructions:

Index a new span tag
=======================

Follow these steps to index a span tag and create a Troubleshooting MetricSet for it. 

1. In Splunk APM, click :guilabel:`APM Configuration` and select :guilabel:`Troubleshooting MetricSets` from the drop-down menu. The APM Troubleshooting MetricSets Configuration page opens. 

2. Click :strong:`New MetricSet`.

3. Enter the :strong:`Name` of a span tag you want to index.

4. The :strong:`Scope` determines how APM associates the span tag with services in a trace:

   - Select :strong:`Service` to associate the span tag with services. This means the value of the span tag could change across services in a given trace. Specify ``All Services`` to index the span tag for every service. Select specific services to index the span tag for only those services.

   - Select :strong:`Global` to associate the span tag with traces. This means the value of the span tag is the same for all services in a given trace.

   For more information about span tag scope, see :ref:`apm-index-tag-types`.

5. Click :strong:`Start Analysis` to submit the configuration. When you submit a configuration, Splunk APM runs an analysis of the span tag to calculate the potential cardinality contribution of indexing it and determine whether it would generate Troubleshooting MetricSets that exceed your limit.

6. Wait a few moments for the cardinality check to run, and then check under :strong:`Pending MetricSet` to view the status of the span tag you're trying to index. See the table below for possible status options for pending MetricSets and the recommended action for each status. 

   .. list-table::
      :header-rows: 1
      :widths: 15, 40, 45

      * - :strong:`Status`
        - :strong:`Description`
        - :strong:`Next step`

      * - Analyzing
        - The application is currently running the cardinality contribution analysis. When this is the status for a span tag you want to index, you can't create or modify any other span tags.
        - Wait until the cardinality contribution analysis is complete. 

      * - Ready
        - The cardinality contribution analysis is complete: you can index the span tag without any issue.
        - Click the checkmark in the :guilabel:`Actions` column to manually index the span tag and start generating Troubleshooting MetricSets that include the span tag. 

      * - Failed
        - The cardinality contribution analysis is complete, but you can't index the span tag because you reached an entitlement or system limit.
        - Consider pausing or deleting existing Custom MetricSets to open space for another indexed span tag, or reach out to your Splunk Observability Cloud account manage to increase your account limit. See :ref:`apm-limits-metricsets` to learn more about these limits. 

      * - Timeout
        - If more than one hour passes for a pending MetricSet in a ``Ready`` status, the status changes to ``Timeout``. 
        - Rerun the analysis to try indexing the span tag again. 


Manage existing indexed span tags and Troubleshooting MetricSets
=================================================================
After you successfully index a span tag, Splunk APM saves the configuration in the :strong:`MetricSets Configuration` page in your :strong:`Organization Settings`. Visit this page to view the indexing scope of the span tag and its current status.

You can modify the configuration for existing indexed tags, including adding and removing services for specific indexed tags and modifying the scope. You can also pause or stop indexing span tags without deleting their configuration. This is useful when you want to temporarily stop indexing a span tag, but don't want to remove the configuration.

To review or modify existing indexed span tags, do the following:

1. Go to :strong:`Settings > Organization Settings > MetricSets Configuration` 
2. Find the indexed span tag you want to view under the :strong:`Custom MetricSets` section of the configuration table.
3.  Refer to :ref:`tms-status` below to interpret the status of each indexed span tag.
4. Make any desired changes using the buttons in the :guilabel:`Actions` column:

    - Use the pencil button to edit the scope of an indexed span tag.
    - Use the pause button to pause generating MetricSets for a given span tag. 
    - Use the trash button to delete a MetricSet configuration.

.. _tms-status:

Status of configured Troubleshooting MetricSets
-------------------------------------------------
Once configured, custom Troubleshooting MetricSets can have the statuses listed in the following table:

.. list-table::
   :header-rows: 1
   :widths: 15, 85

   * - :strong:`Status`
     - :strong:`Description`

   * - Active
     - The application is indexing the span tag and generating Troubleshooting MetricSets for it.

   * - Paused
     - You or another administrator paused indexing for the span tag. The application isn't generating Troubleshooting MetricSets for the span tag, and you can't view any data you previously indexed for it.
    
   * - Stopped
     - The application stopped indexing the span tag and is no longer generating Troubleshooting MetricSets for the span tag. You can't view any data you previously indexed for this span tag.
