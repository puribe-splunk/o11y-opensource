.. _monitor-imm-billing-usage:

***************************************************************
Monitor Splunk Infrastructure Monitoring billing and usage
***************************************************************

.. meta::
      :description: Splunk Infrastructure Monitoring administrators can view the billing and usage information for the organization. The application provides a summary and detailed reports. In addition to counts for hosts and containers, the reports also contain counts for custom metrics and high-resolution metrics.
      :keywords:  billing usage metrics host container custom metric high-resolution 
   

.. note:: 

      The information in this topic applies to organizations whose subscription plan is based on the number of hosts or metrics that Splunk Infrastructure Monitoring is monitoring for you. If your organization's usage is based on the rate at which you send data points to Infrastructure Monitoring (DPM), see :ref:`dpm-usage`.

Overview
========

Admin users in your Infrastructure Monitoring organization can view the billing and usage information for the organization collectively. The application provides a summary and detailed reports to help you understand and manage the data that Infrastructure Monitoring monitors for you. In addition to counts for hosts and containers, the reports also contain counts for custom, bundled, and high-resolution metrics.

.. _about-custom-high-res:

About custom, bundled, and high-resolution metrics
==================================================

The following sections provide description about different types of metrics collected by Infrastructure Monitoring.

.. _about-custom:

Differences between host, container, bundled, and custom metrics
----------------------------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 20, 80

   * - :strong:`Metrics type`
     - :strong:`Description`
   
   * - Host and container metrics
     - Default metrics sent by the Smart Agent or through Infrastructure Monitoring public cloud integrations for hosts, containers, and the services running on them.

   * - Bundled metrics
     - Additional metrics sent through Infrastructure Monitoring public cloud integrations that are not attributed to specific hosts or containers. They are included as part of a host-based subscription and you are not charged for them.
  
   * - Custom metrics
     - Metrics reported to Infrastructure Monitoring outside of the host, container, or bundled metrics. Custom metrics are often used for application monitoring, such as counting the number of Splunk Infrastructure Monitoring API calls or measuring the duration of the API requests. They might also include system or service metrics that you configure the Smart Agent to send outside of its default set of metrics. Your Infrastructure Monitoring subscription allows you to send a certain number of custom metrics.


**Note:** In this context, the term "metric" actually refers to what is generally called a metric time series (MTS).
 
.. _about-high-res:

Differences between high-resolution metrics and standard-resolution metrics
---------------------------------------------------------------------------

.. list-table::
   :header-rows: 1
   :widths: 20, 80

   * - :strong:`Metrics type`
     - :strong:`Description`

   * - Standard-resolution metrics
     - Metrics processed by Infrastructure Monitoring at the coarser of their native resolution or at 10-second resolution. In other words, they are never displayed at a resolution finer than 10 |nbsp| seconds.
   
   * - High-resolution metrics
     - Metrics processed by Infrastructure Monitoring at their native resolution or at 1-second resolution (whichever is coarser). High-resolution metrics enable exceptionally fine-grained and low-latency visibility and alerting for your infrastructure, applications, and business performance. Your Infrastructure Monitoring subscription allows you to send a certain number of high-resolution metrics.

.. note:: In this context, the term "metric" refers to what is generally called a metric time series (MTS).

.. 
    _how-counted:

.. the following is placeholder text that might be added someday
   It should be moved into an include file  -- brs

   How are high-resolution metrics counted?
   ----------------------------------------------------------------------------------

   If you have a hi res metric that is coming from a container or host (ie in the Host plan) it will still contribute to those host/container counts plus hi res counts (edited) 

   if you have a custom metric that is hi-res it will only be included in the hi res count

..    usage-about:
   
.. 
.. 
.. About usage reports
.. =============================================================================
.. 
.. 
.. -  The :ref:`Monthly Usage report<summary-by-month>`, available on the Billed Usage tab, shows the number of hosts and containers sending in data for each hour within the month, and the number of custom metrics (MTS) and high resolution metrics sent in each hour.
.. 
.. 
   ref:`dimension-report`:
.. 
.. 
.. - The :ref:`dimension-report`, available on the Usage Breakdown tab, shows on a daily basis the number of data points and time series for a given dimension value, as well as its average reporting frequency. It is useful for understanding the nature of the data your organization is sending so you can tune it accordingly. For example, you might notice that there are some metrics that you do not want to send at all, and conversely, you might see that there are some metrics that you want to send at a higher resolution.



.. _using-page:


View and download an Infrastructure Monitoring billing report
=============================================================

Follow these steps to view and download a billing report for your Infrastructure Monitoring subscription.

Prerequisites
-------------

You have to be an admin to view and download billing reports.


View a billing report
---------------------

To view available usage reports, go to :guilabel:`Organization Settings > Billing and Usage` and select the :strong:`Infrastructure Monitoring` tab. You can see a chart showing your current usage numbers for hosts, containers, custom metrics, and high-resolution metrics. Below the chart, you might see additional charts representing usage trends that you can customize to show different data or different time periods.

Download a billing report
-------------------------

To view usage reports available for download, click :guilabel:`View detailed usage reports`. Click on a report on the Billed Usage tab to download it as a tab-delimited text file. In some browsers, you might have to right-click on a report to save the report.

Different reports are available on the Usage Breakdown tab.

.. tip:: If you have switched from a DPM-based subscription plan to a plan based on the number of hosts or metrics that Infrastructure Monitoring monitors for you, older reports on the Billed Usage tab indicate that they represent DPM-based data. Reports on the Usage Breakdown tab are not available for dates before changing your subscription.

.. _summary-by-month:

Monthly usage report
====================

This report is available on the Billed Usage tab. For each hour within the month (or month to date, for the current month), this report shows the number of hosts and containers monitored and the number of custom metrics and high-resolution metrics sent to Infrastructure Monitoring. This report follows your billing period and uses the month when a billing period starts as the label in the report link. For example, if your billing period begins on the 10th of the month, then a link for 'March 2018' covers March 10 through April 9, 2018.

You can use monthly usage report to determine whether your usage is in line with your subscription plan. You can use the data to calculate your average usage, how many hours in the month you have been over or under your plan, and by how much.

The report has six columns:

.. list-table::
   :header-rows: 1
   :widths: 20 80

   * - :strong:`Column`
     - :strong:`Description`

   * - Date
     - Follows the mm/dd/yy format.
   
   * - Hour Ending
     - Follows the 24 hour hh:mm UTC format. For example, 01:00 indicates the hour from midnight to 1:00 AM UTC.
  
   * - # Hosts
     - The number of hosts that sent data during the specified hour.
  
   * - # Containers
     - The number of containers that Infrastructure Monitoring monitored during the specified hour.
   
   * - # Custom Metrics
     - The number of non-high-resolution custom metrics (MTS) that were sent to Infrastructure Monitoring during the specified hour.
  
   * - # High Res Metrics
     - The number of high-resolution metrics (MTS) that were sent to Infrastructure Monitoring during the specified hour.

.. _summary-including-children:

Monthly usage report (multiple organizations)
=============================================

If you have multiple organizations associated with your Infrastructure Monitoring subscription, an option for a summary report that includes information on multiple organizations is also available. Similar to the :ref:`summary-by-month`, this report shows hourly information for hosts, containers, custom metrics, and high-resolution custom metrics. However, this report also includes this data for each organization associated with your subscription.

.. _dimension-report:

Dimension report
================

Available on the :guilabel:`Usage Breakdown` tab, the dimension report shows the MTS information associated with data points sent from hosts or containers and information related to custom, high-resolution, and bundled MTS. It breaks down the totals by dimension so that you can trace the origination of the data.

The dimension report shows the nature of the data your organization is sending so you can adjust the data accordingly. For example, you might see some dimensions (such as ``environment:lab``) that indicate you are sending data for hosts or services that you don't want to monitor using Infrastructure Monitoring.

You can select or type in a date for this report. All values in the report are based on the 24 |hyph| hour period (in UTC) for the date.

The report has 22 columns: two for dimension name and value, and four for each type of usage metric (host, container, custom, high-resolution, or bundled). If you are on a custom metrics subscription plan, you can't see columns for host or container metrics in your report.

The following table explains the different columns in a dimension report:

.. list-table::
   :header-rows: 1
   :widths: 20 80

   * - :strong:`Columns`
     - :strong:`Description`

   * - Dimension Name and Dimension Value
     - Key/value pairs of the dimensions that are sent in with your metrics. Unique combinations of dimensions and metrics are represented as MTS in Infrastructure Monitoring. The values in each row represent counts associated with the MTS for the specified dimension name and value.
   
   * - No. [usage metric type] MTS
     - During the report's 24-hour period (UTC), the number of unique MTS for which at least one data point was received from a host or a container; the number of custom, high-resolution, or bundled MTS. 
  
   * - New [usage metric type] MTS
     - During the report's 24-hour period (UTC), the number of unique MTS for which data was received from a host or a container on that date for the first time; the number of custom metrics, high-resolution, or bundled MTS associated with data that was received on that date for the first time.
  
   * - Avg [usage metric type] MTS Resolution
     - The average reporting frequency (native resolution) of the data points comprising the MTS. This value is averaged across the number of MTS and throughout the 24 |hyph|  hour period represented by the report's date. For example, a value of 10 means the data is being sent every 10 seconds, that is, has a 10s native resolution; a value of 300 means that the data is being sent every 5 minutes, that is, has a 5m native resolution, as is the case with standard AWS CloudWatch metrics. This value is calculated as an average across all of the MTS associated with the relevant dimension value. As a result, it may contain outliers (for example, an MTS reporting more slowly or with more significant jitter or lag) that skew the average. For example, for data sent every 5 minutes (300 seconds), you might see a value of 280 or a value of 315. This value should be treated as an approximate number that guides what you do with your metrics, rather than a way of auditing the precise timing of them.
  
   * - No. [usage metric type] Data points
     - During the report's 24-hour period (UTC), the number of data points received by Infrastructure Monitoring from hosts or containers; the number of data points associated with custom, high-resolution, or bundled MTS.

.. Keeping the following labels (per-dimension and by-dimension) because they may have been used in the past --brs

.. _metrics-per-dimension:

.. _metrics-by-dimension:

.. _custom-metric-report:

.. _custom-metrics-report:

Custom metric report
====================

Available on the :guilabel:`Usage Breakdown` tab, custom metric report shows the information on MTS associated with data points sent from hosts or containers, as well as information related to custom, high-resolution, and bundled MTS, for a specified date. The content of most columns in this report represents the same kinds of values as the :ref:`dimension-report`, except that the information is broken down by metric name instead of by dimension name and value. Therefore, you can see how Infrastructure Monitoring is categorizing data associated with each metric. 

A significant difference about this report is how you can use the No. |nbsp| Custom |nbsp|  MTS column. For example, there is a non-zero value in this column. In that case, the metric is designated as a custom metric, and all MTS for this metric are counted towards the quota associated with your Infrastructure Monitoring plan. Knowing how many custom MTS your organization is sending can help you tune your usage accordingly. For example, you might notice some custom metrics that you no longer want to report to Infrastructure Monitoring. Conversely, you might decide to increase the number of custom metrics in your plan, so that you can avoid overage charges. You can use the No. |nbsp| High |nbsp| Resolution |nbsp| MTS column in the same way.


About older report formats
==========================

The :ref:`dimension-report` is a revised format of the report formerly called the Metrics by Dimension report. If you select a date for the Dimension report earlier than the new format's release, the report you download is formatted like the older Metrics by Dimension report. The old report format provides an aggregate view of the data; that is, it doesn't show different values for different usage metrics (host, container, and so on).