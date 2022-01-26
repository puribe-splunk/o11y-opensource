.. _otel-optional-configurations:

*********************************************************************************
Change the default behavior of the Splunk OpenTelemetry Collector
*********************************************************************************

.. meta::
      :description: Optional configurations for Splunk Distribution of OpenTelemetry Collector.

Configure optional configurations for the Splunk OpenTelemetry Collector to change the default behavior.

Linux
=============

Additional configuration options supported by the script can be found by running the script with the ``-h`` flag.

.. code-block:: bash

   sh /tmp/splunk-otel-collector.sh -h

To configure memory allocation, you might need to change the ``--memory`` parameter. By default, this parameter is set to 512. If you have allocated more memory to the Splunk OpenTelemetry Collector, you must increase this setting, as shown in the following example.

.. code-block:: bash

   curl -sSL https://dl.signalfx.com/splunk-otel-collector.sh > /tmp/splunk-otel-collector.sh;
   sudo sh /tmp/splunk-otel-collector.sh --realm SPLUNK_REALM --memory SPLUNK_MEMORY_TOTAL_MIB \
       -- SPLUNK_ACCESS_TOKEN

Fluentd configuration
----------------------------

.. note::

   If log collection is not required, run the installer script with the ``--without-fluentd`` option to skip installation of Fluentd and the plugins and dependencies described in this section.

By default, the Fluentd service is installed and configured to forward log events with the ``@SPLUNK`` label to the Splunk OpenTelemetry Collector, and the Splunk OpenTelemetry Collector sends these events to the HEC ingest endpoint determined by the ``--realm SPLUNK_REALM option``. For example, ``https://ingest.SPLUNK_REALM.signalfx.com/v1/log``.

The following Fluentd plugins are also installed:

* capng_c for enabling Linux capabilities.
* fluent-plugin-systemd for systemd journal log collection.

Additionally, the following dependencies are installed as prerequisites for the Fluentd plugins:

Debian-based systems:

* build-essential
* libcap-ng0
* libcap-ng-dev
* pkg-config

RPM-based systems:

* Development Tools
* libcap-ng
* libcap-ng-devel
* pkgconfig

You can specify the following parameters for the installer script to configure the Splunk OpenTelemetry Collector to send log events to a custom Splunk HTTP Event Collector (HEC) endpoint URL. HEC lets you send data and application events to a Splunk deployment over the HTTP and Secure HTTP (HTTPS) protocols. See :new-page:`Set up and use HTTP Event Collector in Splunk Web <https://docs.splunk.com/Documentation/Splunk/8.2.1/Data/UsetheHTTPEventCollector>.`

* ``--hec-url URL``
* ``--hec-token TOKEN``

The main Fluentd configuration is installed to /etc/otel/collector/fluentd/fluent.conf. Custom Fluentd source configuration files can be added to the /etc/otel/collector/fluentd/conf.d directory after installation.

Note the following:

* All files in this directory with the .conf extension are automatically included by Fluentd.
* The td-agent user must have permissions to access the configuration files and the paths defined within.
* By default, Fluentd is configured to collect systemd journal log events from /var/log/journal.

After any configuration modification, run ``sudo systemctl restart td-agent`` to restart the td-agent service.

.. note::

   If the td-agent package is upgraded after initial installation, you might need to set the Linux capabilities for the new version by performing the following steps for td-agent versions 4.1 or later:

#. Check for the enabled capabilities:

   .. code-block:: bash

      sudo /opt/td-agent/bin/fluent-cap-ctl --get -f /opt/td-agent/bin/ruby
      Capabilities in '/opt/td-agent/bin/ruby',
      Effective:   dac_override, dac_read_search
      Inheritable: dac_override, dac_read_search
      Permitted:   dac_override, dac_read_search
#. If the output from the previous command does not include ``dac_override`` and ``dac_read_search`` as shown above, run the following commands:

   .. code-block:: bash

      sudo td-agent-gem install capng_c
      sudo /opt/td-agent/bin/fluent-cap-ctl --add "dac_override,dac_read_search" -f /opt/td-agent/bin/ruby
      sudo systemctl daemon-reload
      sudo systemctl restart td-agent

Kubernetes
================

The :new-page:`values.yaml <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/helm-charts/splunk-otel-collector/values.yaml>` lists all supported configurable parameters for the Helm chart, along with a detailed explanation of each parameter. Review values.yaml to understand how to configure this chart.

The Helm chart can also be configured to support different use cases, such as trace sampling and sending data through a proxy server. See :new-page:`Examples of chart configuration <https://github.com/signalfx/splunk-otel-collector-chart/blob/main/examples/README.md>` for more information.

Windows
==========================

To configure memory allocation, you might need to change the ``--memory`` parameter. By default, this parameter is set to 512. If you have allocated more memory to the Splunk OpenTelemetry Collector, you must increase this setting, as shown in the following example.

.. code-block:: none

   & {Set-ExecutionPolicy Bypass -Scope Process -Force; $script = ((New-Object System.Net.WebClient).DownloadString('https://dl.signalfx.com/splunk-otel-collector.ps1')); $params = @{access_token = "SPLUNK_ACCESS_TOKEN"; realm = "SPLUNK_REALM"; memory = "SPLUNK_MEMORY_TOTAL_MIB"}; Invoke-Command -ScriptBlock ([scriptblock]::Create(". {$script} $(&{$args} @params)"))}

Replace ``SPLUNK_MEMORY_TOTAL_MIB`` with the desired value.

Fluentd configuration
----------------------------
By default, the Fluentd service is installed and configured to forward log events with the ``@SPLUNK`` label to the Splunk OpenTelemetry Collector, and the Splunk OpenTelemetry Collector sends these events to the HEC ingest endpoint determined by the ``--realm SPLUNK_REALM option``. For example, ``https://ingest.SPLUNK_REALM.signalfx.com/v1/log``.

To configure the Splunk OpenTelemetry Collector to send log events to a custom HEC endpoint URL, you can specify the following parameters for the installer script:

* ``--hec-url URL``
* ``--hec-token TOKEN``

The main Fluentd configuration file is installed to \opt\td-agent\etc\td-agent\td-agent.conf. Custom Fluentd source configuration files can be added to the \opt\td-agent\etc\td-agent\conf.d directory after installation.

Note the following:

* All files in this directory with the .conf extension are automatically included by Fluentd.
* By default, Fluentd is configured to collect from the Windows Event Log. See \opt\td-agent\etc\td-agent\conf.d\eventlog.conf for the default configuration.

After any configuration modification, apply the changes by restarting the system or running the following PowerShell commands:

.. code-block:: bash

   Stop-Service fluentdwinsvc
   Start-Service fluentdwinsvc
