.. _get-started-linux:

***********************
Collect Linux data
***********************

.. meta::
   :description: Start sending metrics and logs from Linux hosts to Splunk Observability Cloud.

The Splunk OpenTelemetry Collector is a package that provides integrated collection/forwarding for all telemetry types for the Linux platform. Customers will deploy this collector in support of gathering telemetry for Splunk Infrastructure Monitoring, Splunk APM, or Splunk Log Observer use cases.

This component is packaged in several formats/installers as appropriate for each Linux distribution and can be installed to virtual machines or hosts.

Supported versions
=====================

The following Linux distributions and versions are supported:

- Amazon Linux: 2
- CentOS / Red Hat / Oracle: 7, 8
- Debian: 8, 9, 10
- Ubuntu: 16.04, 18.04, 20.04

Getting started
===================

For non-containerized Linux environments, an installation script is available for deploying and configuring the OpenTelemetry Collector and TD Agent (Fluentd). By default, the Splunk OpenTelemetry Collector listens on ``127.0.0.1:8006`` for log events forwarded from Fluentd. See :ref:`otel-configuration` for information on changing the default Fluentd configuration.

Run the following command on your host. Replace ``SPLUNK_REALM``, ``SPLUNK_MEMORY_TOTAL_MIB``, and ``SPLUNK_ACCESS_TOKEN`` for your environment.

.. code-block:: none

   curl -sSL https://dl.signalfx.com/splunk-otel-collector.sh > /tmp/splunk-otel-collector.sh;
   sudo sh /tmp/splunk-otel-collector.sh --realm SPLUNK_REALM --memory SPLUNK_MEMORY_TOTAL_MIB -- SPLUNK_ACCESS_TOKEN

.. note:: By default, ``SPLUNK_MEMORY_TOTAL_MIB`` is set to ``512``. You can change this value to suit your needs. For more information, see :ref:`otel-optional-configurations`.