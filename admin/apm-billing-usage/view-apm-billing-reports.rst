.. _view-apm-billing-reports:

********************************************************************
View Splunk APM billing reports for your subscription plan
********************************************************************

.. meta::
   :description: View detailed APM billing information and download usage reports to monitor your organization.

   :keywords: Splunk, APM, billing, usage, billing reports



Get detailed APM billing reports for recent billing periods to analyze billed usage values and per-minute usage. You have to be an administrator to view the :guilabel:`Billing and Usage` page for your organization.

Download an APM billing report
==============================

APM billing reports are available as tab-delimited text files. They include metrics and billed usage for the entire billing period and break down usage for each minute in the billing period. Follow these steps to view and download a billing report:

1. Go to :guilabel:`Organization Settings > Billing and Usage` and select the :strong:`APM` tab.

2. Click :guilabel:`View Detailed Usage Reports`.

3. Select the billing report for the billing period you want to analyze. The billing report opens in a new tab.

4. To download the report, right-click the billing report and save it as a ``.txt`` file.


Metrics for host subscription plans
-----------------------------------

In addition to per-minute usage metrics, billing reports for host subscription plans include the following information about your organization's usage:

- The number of billed hosts

- The number of billed containers

- The number of billed Troubleshooting MetricSets

- The billed trace volume

- 50% of the peak number of hosts

- 50% of the peak number of containers

- 50% of the peak number of Troubleshooting MetricSets

- 50% of the peak trace volume

- The average number of hosts

- The average number of containers

- The average number of Troubleshooting MetricSets

- The average trace volume in bytes

Metrics for TAPM subscription plans
-----------------------------------

In addition to per-minute usage metrics, billing reports for TAPM subscription plans include the following information about your organization's usage:

- The number of billed TAPM

- The number of billed Troubleshooting MetricSets

- The billed trace volume

- 50% of the peak number of TAPM

- 50% of the peak number of Troubleshooting MetricSets

- 50% of the peak trace volume

- The average number of TAPM

- The average number of Troubleshooting MetricSets

- The average trace volume in bytes
