.. _opentelemetry-resources:

*****************************************************************
Use Splunk Distribution of OpenTelemetry Collector
*****************************************************************

.. meta::
   :description: Resources for using Splunk OpenTelemetry Collector.

This page provides a list of resources for using the Splunk OpenTelemetry Collector. Use your browser’s search function to quickly find what you’re looking for.

.. note::

   This project is currently in **Beta**. See :new-page:`Beta Definition <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/beta-definition.md>` for more information.

Getting started
====================

Refer to the following topics for an overview of the Splunk OpenTelemetry Collector:

- :new-page:`Architecture <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/architecture.md>`, which describes how to deploy the Splunk OpenTelemetry Collector.

- :new-page:`Components <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/components.md>`, which describes what collector supports.

- :new-page:`Monitoring <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/monitoring.md>`, which describes how to ensure that the Splunk OpenTelemetry Collector is healthy.

- :new-page:`Security <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/security.md>`, which describes how to ensure that the Splunk OpenTelemetry Collector is secure.

- :new-page:`Sizing <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/sizing.md>`, which describes how to ensure that the collecor is properly sized.

- :new-page:`Troubleshooting <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/troubleshooting.md>`, which describes how to resolve common issues with the Splunk OpenTelemetry Collector.

You need the following resources to get started using the Splunk OpenTelemetry Collector:

- :new-page:`Splunk Access Token <https://docs.splunk.com/Observability/admin/authentication-tokens/org-tokens.html#admin-org-tokens>`

- :new-page:`Splunk Realm <https://dev.splunk.com/observability/docs/realms_in_endpoints/>`

- :new-page:`Agent or Gateway mode <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/agent-vs-gateway.md>`

- :new-page:`Confirm exposed ports <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/security.md#exposed-endpoints>` to make sure your environment doesn't have conflicts and that firewalls are configured properly. Ports can be changed in the configuration.

This distribution is supported on and packaged for a variety of platforms, including:

- Kubernetes: :new-page:`Helm <https://github.com/signalfx/splunk-otel-collector-chart>` (recommended) and :new-page:`YAML <https://github.com/signalfx/splunk-otel-collector-chart/tree/main/rendered>`

- Linux: :new-page:`installer script <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/getting-started/linux-installer.md>` (recommended), :new-page:`Ansible <https://galaxy.ansible.com/signalfx/splunk_otel_collector>`, :new-page:`Puppet <https://forge.puppet.com/modules/signalfx/splunk_otel_collector>`, :new-page:`Heroku <https://github.com/signalfx/splunk-otel-collector/blob/main/deployments/heroku/README.md>`, and :new-page:`manual <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/getting-started/linux-manual.md>` (including DEB/RPM packages, Docker, and binary).

- Windows: :new-page:`installer script <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/getting-started/windows-installer.md>` (recommended), :new-page:`Puppet <https://forge.puppet.com/modules/signalfx/splunk_otel_collector>`, and :new-page:`manual <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/getting-started/windows-manual.md>` (including MSI with GUI and PowerShell).

See :new-page:`examples <https://github.com/signalfx/splunk-otel-collector/blob/main/examples>` for additional use cases.

Default configuration
============================

The following is a list of default configuration files. These files contain standard specifications and settings.

- :new-page:`signalfx/splunk-otel-collector <https://github.com/signalfx/splunk-otel-collector/tree/main/cmd/otelcol/config/collector>`. *full_config_linux.yaml* includes comments and links to documentation. *agent_config_linux.yaml* is the recommended starting configuration for most environments.

- :new-page:`Fluentd <https://github.com/signalfx/splunk-otel-collector/tree/main/internal/buildscripts/packaging/fpm/etc/otel/collector/fluentd>`, which is only applicable to Helm or installer script installations. See the ``*.conf`` files and the ``conf.d`` directory. Common sources, including filelog, journald, and Windows Event Viewer are included.

Custom configuration
=================================

These components can be customized to change the default behavior of the Splunk OpenTelemetry Collector.

- Configuration sources: Use :new-page:`environment variables <https://github.com/signalfx/splunk-otel-collector/tree/main/internal/configsource/envvarconfigsource>`, :new-page:`etcd2 <https://github.com/signalfx/splunk-otel-collector/tree/main/internal/configsource/etcd2configsource>`, :new-page:`Include <https://github.com/signalfx/splunk-otel-collector/tree/main/internal/configsource/includeconfigsource>`, :new-page:`Vault <https://github.com/signalfx/splunk-otel-collector/tree/main/internal/configsource/vaultconfigsource>`, and :new-page:`Zookeeper <https://github.com/signalfx/splunk-otel-collector/tree/main/internal/configsource/zookeeperconfigsource>` to retrieve data from specific configuration sources. After retrieving the data, you can then insert the data into your configuration.
- SignalFx Smart Agent: :new-page:`Extensions <https://github.com/signalfx/splunk-otel-collector/tree/main/internal/extension/smartagentextension>`, including collectd and Python, are used to implement components that can be added to the configuration, but do not require direct access to data. :new-page:`Receivers <https://github.com/signalfx/splunk-otel-collector/tree/main/internal/receiver/smartagentreceiver>` use the existing Smart Agent monitors as metric receivers to gather data.

.. note::

   SignalFx Smart Agent is deprecated. For details, see the :new-page:`Deprecation Notice <https://github.com/signalfx/signalfx-agent/blob/main/docs/smartagent-deprecation-notice.md>`. See :new-page:`Migrating from the SignalFx Smart Agent <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/signalfx-smart-agent-migration.md>` for resources and best practices to start using the Splunk Distribution of OpenTelemetry Collector, which is the replacement for the Smart Agent.

.. _using-upstream-otel:

Upstream OpenTelemetry Collector
=============================================

It is possible to use the upstream OpenTelemetry Collector instead of this Splunk Distribution of OpenTelemetry Collector. The following features are not available upstream at this time:

- Packaging, including installer scripts for Linux and Windows, and configuration management using Ansible or Puppet
- Configuration sources
- Several Smart Agent capabilities

.. warning::

   Splunk only provides best-effort support for the upstream OpenTelemetry Collector.

Do the following to use the upstream OpenTelemetry Collector:

#. Use the :new-page:`OpenTelemetry Collector contribution <https://github.com/open-telemetry/opentelemetry-collector-contrib>`. This contribution includes receivers/exporters and components that are vendor specific.

#. Configure the upstream OpenTelemetry Collector.

See :new-page:`upstream_agent_config.yaml <https://github.com/signalfx/splunk-otel-collector/blob/main/cmd/otelcol/config/collector/upstream_agent_config.yaml>` for an example configuration for the upstream OpenTelemetry Collector. This configuration includes the recommended settings to ensure :new-page:`infrastructure correlation <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/apm-infra-correlation.md>`.

Troubleshooting
=============================================

See :new-page:`Troubleshooting <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/troubleshooting.md>` to resolve common issues using the OpenTelemetry Collector and Splunk Distribution of OpenTelemetry Collector.
