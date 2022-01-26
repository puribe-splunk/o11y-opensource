.. _apm-billing-usage-index:

*************************************************
Monitor Splunk APM billing and usage
*************************************************

.. meta::
   :description: View APM billing and usage information and download usage reports to monitor your organization.

   :keywords: Splunk, APM, billing, usage, billing reports



.. toctree::
   :hidden:
   :maxdepth:  3

   /admin/apm-billing-usage/analyze-apm-billing-usage
   /admin/apm-billing-usage/view-apm-billing-reports
   

View Splunk APM billing and usage data to monitor your organization's usage against its subscription plan and entitlements. You have to be an administrator to view the APM Billing and Usage page for your organization. To view your organization's APM billing and usage, go to :strong:`Organization Settings > Billing and Usage` and select the :strong:`APM` tab.

For any questions about billed usage, contact your tech support member or sales representative.

The Billing and Usage page explains the following information about your organization:

- The type of plan

- The entitlement limits for your subscription plan

- The monthly billed value of each entitlement

- The per-minute usage of each entitlement

.. note::

   The APM Billing and Usage page displays a tile for Monitoring MetricSets, but the metric that powers the chart is not currently available.

APM calculates per-minute usage for your subscription plan. There are two types of subscription plans: :strong:`host` and :strong:`traces analyzed per minute (TAPM)`. To learn more about how APM monitors billing and usage for each subscription plan type, see :ref:`analyze-apm-billing-usage`.

For each subscription plan type, download detailed billing and usage reports for recent billing periods. Reports break down usage for each minute in a billing period and provide the following type of information:

- The number of billed hosts and containers, if applicable

- The number of billed TAPM, if applicable

- The number of billed Troubleshooting MetricSets

- The billed trace volume

For more information about detailed billing and usage reports, see :ref:`view-apm-billing-reports`.