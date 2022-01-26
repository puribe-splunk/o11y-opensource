.. _otel-configuration:

****************************************************************
Configure the Splunk OpenTelemetry Collector
****************************************************************

.. meta::
      :description: Configure Splunk Distribution of OpenTelemetry Collector. There are a variety of default configuration files available, as well additional components that can be configured.

.. toctree::
    :maxdepth: 4
    :titlesonly:
    :hidden:

    optional-configurations.rst
    other-configuration-sources.rst
    configure-the-smart-agent.rst

The collector is configured using the following files:

* :new-page:`agent_config.yaml <https://github.com/signalfx/splunk-otel-collector/blob/main/cmd/otelcol/config/collector/agent_config.yaml>`, which is the recommended starting configuration for most environments. This is the default configuration file for the Linux (Debian/RPM) and Windows Installer collector packages.

* :new-page:`full_config_linux.yaml <https://github.com/signalfx/splunk-otel-collector/blob/main/cmd/otelcol/config/collector/full_config_linux.yaml>`, which is an extended configuration. This configuration requires ``opentelemetry-collector-contrib`` or a distribution based on ``opentelemetry-collector-contrib``.

* :new-page:`Fluentd <https://github.com/signalfx/splunk-otel-collector/tree/main/internal/buildscripts/packaging/fpm/etc/otel/collector/fluentd>`, which is used to collect logs. Fluentd is applicable to Helm or installer script installations only. Common sources including filelog, journald, and Windows Event Viewer are included in the installation. See the Fluentd configuration documentation for more information.

   * The Fluentd directory contains the following files:

     * **fluent.conf** or **td-agent.conf**. These are the main Fluentd configuration files used to forward events to the Splunk OpenTelemetry Collector. The file locations are ``/etc/otel/collector/fluentd/fluent.conf`` on Linux and ``\opt\td-agent\etc\td-agent\td-agent.conf`` on Windows. By default, these files configure Fluentd to include custom Fluentd sources and forward all log events with the ``@SPLUNK`` label to the Splunk OpenTelemetry Collector.  
     * **conf.d**. This directory contains the custom Fluentd configuration files. The location is ``/etc/otel/collector/fluentd/conf.d`` on Linux and ``\opt\td-agent\etc\td-agent\conf.d`` on Windows. All files in this directory ending with the .conf extension are automatically included by Fluentd, including ``\opt\td-agent\etc\td-agent\conf.d\eventlog.conf`` on Windows.
     * **splunk-otel-collector.conf**. This is the drop-in file for the Fluentd service on Linux. Use this file to override the default Fluentd configuration path in favor of the custom Fluentd configuration file for Linux (fluent.conf).

The following is a sample configuration to collect custom logs:

.. code-block:: xml

   <source>
     @type tail
     @label @SPLUNK
     <parse>
       @type none
     </parse>
     path /path/to/my/custom.log
     pos_file /var/log/td-agent/my-custom-logs.pos
     tag my-custom-logs
   </source>

You can also configure the following components:

* :ref:`Configuration sources <otel-other-configuration-sources>`

  * Environment variable (Alpha)

  * etcd (Alpha)

  * Include (Alpha)

  * Vault (Alpha)

  * Zookeeper (Alpha)

* :ref:`SignalFx Smart Agent <otel-smart-agent>`

  * Extension

  * Receiver
