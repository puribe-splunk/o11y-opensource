.. _data-o11y:

******************************************************
Data retention in Splunk Observability Cloud
******************************************************

.. meta::
   :description: Data retention for Splunk Observability Cloud.

The following sections list the data retention for each product in Splunk Observability Cloud.

.. _im-data-retention:

Infrastructure Monitoring
==========================
The following table shows the retention time period for each data type in Infrastructure Monitoring. 

.. list-table:: 
   :widths: 25 25
   :header-rows: 1

   * - :strong:`Data type`
     - :strong:`Retention`
   * - One-minute roll-ups 
     - 13 months
   * - One-second native resolution data 
     - 
       * Standard contract: 8 days
       * Enterprise contract: 3 months 

.. _rum-data-retention:

Real User Monitoring (RUM)
===========================

The following table shows the retention time period for each data type in RUM. 

.. list-table:: 
   :widths: 25 25
   :header-rows: 1

   * - :strong:`Data type`
     - :strong:`Retention`
   * - Spans 
     - 8 days
   * - Metrics 
     - 8 days
   * - :ref:`Monitoring MetricSets <monitoring-metricsets>`
     - 13 months 

.. _apm-data-retention:

Application Performance Monitoring (APM)
==================================================
The following table shows the retention time period for each data type in APM. 
To increase extended traces, see :ref:`Extended trace retention <apm-extended-trace-retention>`. 

.. list-table:: 
   :widths: 25 25
   :header-rows: 1

   * - :strong:`Data type`
     - :strong:`Retention`
   * - Traces
     - 8 days to 13 months
   * - :ref:`Troubleshooting MetricSets <troubleshooting-metricsets>`
     - 8 days   
   * - :ref:`Monitoring MetricSets <monitoring-metricsets>`
     - 13 months 
   * - :ref:`Profiling data <profiling-intro>`
     - 8 days

.. _log-observer-data-retention:

Log Observer 
=========================

The retention period for indexed logs in Splunk Log Observer is 30 days. If you send logs to S3 through the infinite logging feature, then the data retention period depends on the policy you purchased for your Amazon S3 bucket.  

.. _oncall-data-retention:

Splunk On-Call
=========================

Data collected by Splunk On-Call is retained unless you request that your data be deleted. For more information, see :new-page:`Splunk On-Call Security FAQ <https://www.splunk.com/en_us/support-and-services/on-call-security-faq.html>`.