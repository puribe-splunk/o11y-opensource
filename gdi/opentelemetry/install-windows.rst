.. _otel-install-windows:

****************************
Install on Windows
****************************

.. meta::
      :description: Describes how to install Splunk Distribution of OpenTelemetry Collector on Windows.

Install the Splunk OpenTelemetry Collector on Windows using one of these methods:

* :ref:`Installer script <windows-script>`
* :ref:`Windows Installer <windows-installer>`
* :ref:`Puppet <windows-puppet>`
* :ref:`PowerShell terminal <windows-powershell>`
* :ref:`Docker <windows-docker>`

.. _windows-script:

Installer script
==========================

The installer script is available for Windows 64-bit environments. The script deploys and configures these things:

* Splunk OpenTelemetry Collector for Windows
* Fluentd (via the TD Agent)

The following Windows versions are supported and require PowerShell 3.0 or newer:

* Windows Server 2012 64-bit
* Windows Server 2016 64-bit
* Windows Server 2019 64-bit

Do the following to install the Splunk OpenTelemetry Collector using the installer script:

#. Ensure that you have Administrator access on your host.
#. Run the following PowerShell command on your host, replacing the following variables for your environment:

   * ``SPLUNK_REALM``: This is the realm to send data to. The default is ``us0``. See :new-page:`realms <https://dev.splunk.com/observability/docs/realms_in_endpoints/>`.
   * ``SPLUNK_ACCESS_TOKEN``: This is the Splunk access token to authenticate requests. See :ref:`admin-org-tokens`.

.. code-block:: none

   & {Set-ExecutionPolicy Bypass -Scope Process -Force; $script = ((New-Object System.Net.WebClient).DownloadString('https://dl.signalfx.com/splunk-otel-collector.ps1')); $params = @{access_token = "SPLUNK_ACCESS_TOKEN"; realm = "SPLUNK_REALM"}; Invoke-Command -ScriptBlock ([scriptblock]::Create(". {$script} $(&{$args} @params)"))}

.. _windows-installer:

Windows Installer
=======================

Do the following to install the Splunk OpenTelemetry Collector using the Windows Installer:

#. Download the Windows MSI package (64-bit only) from :new-page:`GitHub releases <https://github.com/signalfx/splunk-otel-collector/releases>`.
#. Double click the downloaded package and follow the instructions in the wizard.

The collector is installed to ``\Program Files\Splunk\OpenTelemetry Collector``, and the ``splunk-otel-collector`` service is created, but not started. A default configuration file is copied to ``\ProgramData\Splunk\OpenTelemetry Collector\agent_config.yaml``, if it does not already exist. This file is required to start the ``splunk-otel-collector`` service.

.. _windows-puppet:

Puppet
================
Splunk provides a Puppet module to install and configure the Splunk OpenTelemetry Collector. A module is a collection of resources, classes, files, definition, and templates. See :new-page:`Splunk OpenTelemetry Collector Puppet Module <https://forge.puppet.com/modules/signalfx/splunk_otel_collector>` to download the module.

.. _windows-powershell:

PowerShell terminal
=========================
Do the following to install the Splunk OpenTelemetry Collector from a PowerShell terminal:

#. Download the Windows MSI package (64-bit only) from :new-page:`GitHub releases <https://github.com/signalfx/splunk-otel-collector/releases>`.
#. Run the following command in a PowerShell terminal. Replace ``PATH_TO_MSI`` with the full path to the downloaded package. For example, ``C:\your\download\folder\splunk-otel-collector-0.4.0-amd64.msi``::

    PS> Start-Process -Wait msiexec "/i PATH_TO_MSI /qn"
#. Update all variables in the configuration file as appropriate. See the next section for the steps to do this.
#. Start the ``splunk-otel-collector`` service by rebooting the system or running the following command in a PowerShell terminal::

    PS> Start-Service splunk-otel-collector

The collector is installed to ``\Program Files\Splunk\OpenTelemetry Collector``, and the ``splunk-otel-collector`` service is created, but not started. A default configuration file is copied to ``\ProgramData\Splunk\OpenTelemetry Collector\agent_config.yaml``, if it does not already exist. This file is required to start the ``splunk-otel-collector`` service.

Change the default configuration file
==========================================

Before starting the ``splunk-otel-collector`` service, change the variables in the default configuration file to the appropriate values for your environment. The following table provides a description of each variable.

.. list-table::
   :widths: 50 50
   :header-rows: 1

   * - Variable
     - Description
   * - ``${SPLUNK_ACCESS_TOKEN}``
     - The Splunk access token to authenticate requests
   * - ``${SPLUNK_API_URL}``
     - The Splunk API URL. For example, ``https://api.us0.signalfx.com``.
   * - ``${SPLUNK_HEC_TOKEN}``
     - The Splunk HEC authentication token
   * - ``${SPLUNK_HEC_URL}``
     - The Splunk HEC endpoint URL. For example, ``https://ingest.us0.signalfx.com/v1/log``.
   * - ``${SPLUNK_INGEST_URL}``
     - The Splunk ingest URL. For example, ``https://ingest.us0.signalfx.com``.
   * - ``${SPLUNK_TRACE_URL}``
     - The Splunk trace endpoint URL. For example, ``https://ingest.us0.signalfx.com/v2/trace``.
   * - ``${SPLUNK_BUNDLE_DIR}``
     - The location of your Smart Agent bundle for monitor functionality. For example, ``C:\Program Files\Splunk\OpenTelemetry Collector\agent-bundle``.

|br|

Do the following after updating the variables in the default configuration file:

#. Start the service by rebooting the system or by running the following command in a PowerShell terminal::

    PS> Start-Service splunk-otel-collector
#. To modify the default path to the configuration file for the service, run ``regdit`` and modify the ``SPLUNK_CONFIG`` value in the ``HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment`` registry key, or run the following PowerShell command (replace PATH with the full path to the new configuration file)::

    Set-ItemProperty -path "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" -name "SPLUNK_CONFIG" -value "PATH"
#. After modifying the configuration file or registry key, apply the changes by restarting the system or by running the following PowerShell commands::

    Stop-Service splunk-otel-collector
    Start-Service splunk-otel-collector

.. _windows-docker:

Docker
============

To deploy the latest Docker image:

.. code-block:: none

   $ docker run --rm -e SPLUNK_ACCESS_TOKEN=12345 -e SPLUNK_REALM=us0  `
	          -p 13133:13133 -p 14250:14250 -p 14268:14268 -p 4317:4317 -p 6060:6060  `
	          -p 8888:8888 -p 9080:9080 -p 9411:9411 -p 9943:9943 `
	          --name=otelcol quay.io/signalfx/splunk-otel-collector-windows:latest

More options
==================================
Once you have installed the Splunk OpenTelemetry Collector, you can perform these actions:

* :ref:`use-navigators`
* View logs and errors in the Windows Event Viewer
* :ref:`apm`
