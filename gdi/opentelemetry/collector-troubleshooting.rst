.. _otel-collector-tshoot:

*********************************************
Troubleshoot the OpenTelemetry Collector
*********************************************

.. meta::
      :description: Describes known issues using OpenTelemetry Collector.

These issues are specific to the OpenTelemetry Collector.

Collector exits or restarts
================================

The collector might exit or restart for the following reasons:

* Memory pressure due to a missing or misconfigured ``memory_limiter`` processor
* Improperly sized for load
* Improperly configured. For example, a queue size configured higher than available memory.
* Infrastructure resource limits. For example, Kubernetes.

Restart the Splunk OpenTelemetry Collector and check the configuration.

Collector is dropping data
===================================
Data might drop for a variety of reasons, but most commonly for the following reasons:

* The collector is improperly sized, resulting in the Splunk OpenTelemetry Collector being unable to process and export the data as fast as it is received. See :ref:`otel-plan-deployment` for sizing guidelines.
* The exporter destination is unavailable or accepting the data too slowly. To mitigate drops, configure the ``batch`` processor. In addition, you might also need to configure the queued retry options on enabled exporters.

Collector isn't receiving data
===================================

The collector might not receive data for the following reasons:

* Network configuration issues
* Receiver configuration issues
* The receiver is defined in the receivers section, but not enabled in any pipelines.
* The client configuration is incorrect

Check the logs and :new-page:`Troubleshooting zPages <https://github.com/open-telemetry/opentelemetry-collector/blob/main/docs/troubleshooting.md#zpages>` in GitHub for more information.

Collector can't process data
===================================

The collector might not process data for the following reasons:

* The attributes processors work only for "tags" on spans. The span name is handled by the span processor.
* Processors for trace data (except tail sampling) only work on individual spans. Make sure your collector is configured properly.

Collector can't export data
===================================

The collector might be unable to export data for the following reasons:

* Network configuration issues, such as firewall, DNS, or proxy support. The collector does have proxy support for exporters. If configured at collector start time, then exporters, regardless of protocol, do or do not proxy traffic as defined by these environment variables.
* Incorrect exporter configuration
* Incorrect credentials
* The destination is unavailable

Check the logs and :new-page:`Troubleshooting zPages <https://github.com/open-telemetry/opentelemetry-collector/blob/main/docs/troubleshooting.md#zpages>` in GitHub for more information.

Collector doesn’t start in Windows Docker containers
======================================================================

The process might fail to start in a custom built, Windows-based Docker container, resulting in a "The service process could not connect to the service controller." error message.

In this case, the ``NO_WINDOWS_SERVICE=1`` environment variable must be set to force the Splunk OpenTelemetry Collector to start as if it were running in an interactive terminal, without attempting to run as a Windows service.
