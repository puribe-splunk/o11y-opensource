.. _logs-pipeline:

*****************************************************************
Manage the logs pipeline
*****************************************************************

.. meta::
   :description: Manage the logs pipeline with log processing rules, log metricization rules, and infinite logging rules.

.. note:: Only customers with a Splunk Log Observer entitlement in Splunk Observability Cloud can manage the logs pipeline. If you do not have a Log Observer entitlement and are using Splunk Log Observer Connect instead, see :ref:`logs-intro-logconnect` to learn what you can do with the Splunk Enterprise integration.

Add value to your raw logs by customizing your pipeline. The pipeline is a set of rules that execute sequentially starting with log processing rules and ending with infinite logging rules. Adjust the order of custom rules by dragging and dropping their placement within their rule category. 

Observability Cloud lets you create three types of pipeline rules:

* :ref:`Log processing rules <logs-processors>` transform your data or a subset of your data as it arrives in Observability Cloud.
* :ref:`Log metricization rules <logs-metricization>` let you create charts to see trends in your logs.
* :ref:`Infinite logging rules <logs-infinite>` archive unindexed logs in Amazon S3 buckets for potential future use.
