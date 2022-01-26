.. _analyze-apm-billing-usage:

*********************************************************************
Analyze Splunk APM billing and usage data for your subscription plan
*********************************************************************

.. meta::
   :description: View APM billing and usage information and download usage reports to monitor your organization.

   :keywords: Splunk, APM, billing, usage, billing reports



Use the APM Billing and Usage page to view charts that track your organization's APM usage against its entitlements. You have to be an administrator to view the Billing and Usage page for your organization.

You can also use the Billing and Usage page to get detailed reports of each billing period. For more information, see :ref:`view-apm-billing-reports`.

To view your organization's APM billing and usage, go to :guilabel:`Organization Settings > Billing and Usage` and select the :guilabel:`APM` tab.


How APM calculates usage
========================

APM uses Splunk Observability Cloud metrics to calculate usage for traces analyzed per minute (TAPM) and host subscription plans. Entitlements for host subscription plans are based on the number of hosts and containers sending data to APM. Entitlements for TAPM subscription plans are based on the number of traces you send to APM per minute.

As a result, the metrics for calculating usage depend on the subscription plan type. See the following sections for more information about how APM calculates usage for each subscription plan type. To confirm the plan for your organization, view the :guilabel:`Subscription` panel on the Billing and Usage page.

To see all of the organization metrics for APM, see :ref:`Usage metrics for Splunk Observability Cloud <org-metrics>`.

To see the usage charts and metrics for your subscription plan, go to :guilabel:`Organization Settings > Billing and Usage` and select the :strong:`APM` tab. The following sections detail the metrics for TAPM and host subscription plans respectively.

.. _tapm_subscription_plans:

Metrics for TAPM subscription plans
-----------------------------------

The following metrics power the charts in your APM Billing and Usage page with a TAPM subscription plan:

.. list-table::
   :header-rows: 1
   :widths: 25, 25, 50

   * - :strong:`Metric`
     - :strong:`Chart`
     - :strong:`Description`

   * - ``sf.org.apm.numTracesReceived``
     - TAPM
     - The number of traces Splunk APM receives and processes.

   * - ``sf.org.apm.numSpanBytesReceived``
     - Trace Volume
     - The number of bytes Splunk APM accepts from ingested span data after decompression, filtering and throttling.

   * - ``sf.org.apm.numTroubleshootingMetricSets``
     - Troubleshooting MetricSets
     - The cardinality of Troubleshooting MetricSets for each 1-minute window.

.. _host_subscription_plans:

Metrics for host subscription plans
-----------------------------------

The following metrics power the charts in your APM Billing and Usage page with a host subscription plan:

.. list-table::
   :header-rows: 1
   :widths: 25, 25, 50

   * - :strong:`Metric`
     - :strong:`Chart`
     - :strong:`Description`

   * - ``sf.org.apm.numHosts``
     - Hosts
     - The number of hosts that are actively sending data to Splunk APM.

   * - ``sf.org.apm.numContainers``
     - Containers
     - The number of containers actively sending data to Splunk APM.

   * - ``sf.org.apm.numSpanBytesReceived``
     - Trace Volume
     - The number of bytes Splunk APM accepts from ingested span data after decompression following filtering and throttling.

   * - ``sf.org.apm.numTroubleshootingMetricSets``
     - Troubleshooting MetricSets
     - The cardinality of Troubleshooting MetricSets for each 1-minute window.

How APM calculates billing
==========================

APM provides a billed value for each usage metric the system collects for each billing period. The billed value is the higher of these metric values:

- The average per-minute usage throughout the billing period

- 50% of the peak usage for the billing period.

Every chart on the APM Billing and Usage page plots these metrics so you can monitor the billed value for each metric.

The detailed billing report for each billing period provides the billed value for each usage metric. The following example illustrates how the billed value is based on the higher value of the usage metrics for a billing period:

.. code-block:: none

   # The billed TAPM value for this month is: 47064

   # The average TAPM value for this month is: 31516

   # The halfpeak TAPM value for this month is: 47064

For more information about APM billing reports, see :ref:`view-apm-billing-reports`.
