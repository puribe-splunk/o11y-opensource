.. _otel-security:

******************************************
Secure the Splunk OpenTelemetry Collector
******************************************

.. meta::
      :description: Describes how to ensure that Splunk Distribution of OpenTelemetry Collector is secure.

The OpenTelemetry Collector defaults to operating in a secure manner, but is configuration driven. See :new-page:`Security <https://github.com/open-telemetry/opentelemetry-collector/blob/main/docs/security.md>` for important security aspects and considerations for the OpenTelemetry Collector. This document is intended for both end users and component developers, and assumes at least a basic understanding of OpenTelemetry Collector architecture and functionality.

See :ref:`otel-exposed-endpoints` for information on exposed ports. Check to make sure your environment doesn't have port conflicts and that firewalls are configured properly.
