.. _otel-plan-deployment:

*****************************
Plan your deployment
*****************************

.. meta::
      :description: Follow these guidelines when deploying Splunk Distribution of OpenTelemetry Collector in your environment. Use these guidelines to make sure the Splunk OpenTelemetry Collector is properly sized.

You can scale up or out your collector as needed. Use a ratio of 1 CPU to 2 GB of memory. By default, the Splunk OpenTelemetry Collector is configured to use 512 MB of memory.

A single collector is generally capable of receiving, processing, or exporting the following amounts of data:

* Traces: 15,000 spans per second
* Metrics: 20,000 data points per second
* Logs: 10,000 log records per second

.. note::

   The sizing recommendation for logs also accounts for td-agent (Fluentd), which forwards logs to the ``fluentforward`` receiver in the Splunk OpenTelemetry Collector.

If the Splunk OpenTelemetry Collector handles both trace and metrics data, consider both types of data when planning your deployment. For example, 7.5K spans per second, plus 10K data points per second requires 1 CPU core.

For Agent mode, scale up resources as needed. Typically, only a single agent runs per application or host, so properly sizing the agent is important. Multiple independent agents could be deployed on a given application or host depending on the use case. For example, a privileged agent could be deployed alongside an unprivileged agent.

For Gateway mode, allocate at least 1 CPU core per collector. Note that multiple collectors can deployed behind a simple round-robin load balancer for availability and performance reasons. Each collector runs independently, so scale increases linearly with the number of collectors you deploy.

The recommendation is to configure at least N+1 redundancy, which means a load balancer and a minimum of two collector instances should be configured initially.

Balance your data load
===========================

Do the following to evenly distribute the data:

* Install a cluster of collectors behind a load balancer for high availability.
* Define a round-robin DNS name.
