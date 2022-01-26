.. _otel-uninstall-integration:

********************************************
Uninstall the Splunk OpenTelemetry Collector
********************************************

.. meta::
      :description: Describes how to uninstall Splunk Distribution of OpenTelemetry Collector.

Use the platform-specific information provided in this topic if you need to uninstall the Splunk OpenTelemetry Collector.

Uninstall on Kubernetes
==========================

Run the ``helm uninstall`` command to uninstall the Splunk OpenTelemetry Collector. For example, to uninstall ``my-splunk-otel-collector``, run this command:

.. code-block:: bash

   helm uninstall my-splunk-otel-collector

Running this command does the following things:

* Removes all Kubernetes components associated with the chart
* Deletes the release
* Removes the release history

Uninstall on Linux
========================
To uninstall the Splunk OpenTelemetry Collector and Fluentd, run the following command:

.. code-block:: bash

   sudo sh /tmp/splunk-otel-collector.sh --uninstall

Note that configuration files might be left on the file system:

* On RPM-based systems, modified configuration files are renamed with the .rpmsave extension and can be manually deleted if they are no longer needed.
* On Debian-based systems, modified configuration files persist and need to be manually deleted before re-running the installer script if you do not intend on re-using these configuration files.

Uninstall on Windows
=======================
If installed with the installer script, the Splunk OpenTelemetry Collector and td-agent (Fluentd) can be uninstalled from **Programs and Features** in the Windows Control Panel. The configuration files may persist in \ProgramData\Splunk\OpenTelemetry Collector and \opt\td-agent after uninstall.
