.. _smart-agent:

*******************************************************************
Install and configure the SignalFx Smart Agent 
*******************************************************************

.. meta::
    :description: The SignalFx Smart Agent provides automatic service discovery, configuration, and metrics collection for a variety of environments. The Smart Agent is deprecated as part of the release of Splunk Observability Cloud.

.. note::
   The SignalFx Smart Agent is deprecated. For details, see the :new-page:`Deprecation Notice <https://github.com/signalfx/signalfx-agent/blob/main/docs/smartagent-deprecation-notice.md>`.

The SignalFx Smart Agent gathers host performance, application, and service-level metrics from both containerized and non-container environments. This page provides a complete list of Smart Agent resources. Use your browser’s search function to quickly find what you’re looking for.

.. admonition:: Note about the SignalFx Smart Agent

  If you want to use Splunk Observability Cloud integration for any of the following services, you still need to deploy the SignalFx Smart Agent instead of Splunk Distribution of OpenTelemetry Collector:

    - CloudFoundry and Pivotal CloudFoundry, also known as PCF or VMWare Tanzu Application Service
    - :ref:`Docker <docker>`
 
  Note also that any metrics ingested using the Splunk Distribution of OpenTelemetry Collector will not appear in the classic SignalFx UI. Smart Agent and cloud metrics will continue to appear in either experience, but you can only view OpenTelemetry-sourced metrics in the Splunk Observability Cloud UI.


Components 
============================================

The agent has three main components:

* Observers that discover applications and services running on the host. For a list of supported observers and their configurations, see :new-page:`observer configuration <https://github.com/signalfx/signalfx-agent/blob/main/docs/observer-config.md>`.

* Monitors that collect metrics, events, and dimension properties from the host and applications. For a list of supported monitors and their configurations, see :new-page:`monitor configuration <https://github.com/signalfx/signalfx-agent/blob/main/docs/monitor-config.md>`.

* The writer that sends the metrics, events, and dimension updates collected by monitors to Splunk Observability Cloud. The writer collects metrics emitted by configured monitors and sends them to Observability Cloud on a regular basis. You can configure :new-page:`writer settings <https://github.com/signalfx/signalfx-agent/blob/main/docs/config-schema.md#writer>` in the configuration schema. 

Use cases 
===================================

The Smart Agent gathers metrics using monitors, including Python-based plugins such as Mongo, Redis, and Docker. See :ref:`supported-data-sources` for a list of data source integrations.

Use the Smart Agent to integrate with cloud services, including Amazon Web Services, Microsoft Azure, Google Cloud Platform, and Kubernetes environments. See :ref:`get-started-connect`. Next, log in to Observability Cloud to view the incoming metrics in :ref:`dashboards` and :ref:`data-visualization-charts`.

Check out the health of your network and nodes using :ref:`infrastructure-navigators`. Some features of Observability Cloud such as :ref:`related content <get-started-relatedcontent>` do not work with the Smart Agent.

The Smart Agent also supports receiving and sending trace data. See :ref:`apm-download-traces`.

Check exposed ports 
=====================================================================

Before installing the Smart Agent, check exposed ports to make sure your environment doesn’t have conflicts. You can change the ports in the Smart Agent configuration.

* Port 8095 is the default port that the internal status server listens on. Configure port 8095 using the ``internalStatusPort`` option.
* Port 9080 is the default port that the server listens on. Configure port 9080 using the ``listenAddress`` option. The ``listenAddress`` option is a configurable option for the ``trace-forwarder`` and ``signalfx-forwarder`` monitors. 

Install the Smart Agent
============================================

There are several options available to install the Smart Agent. Select the option that matches your situation or preference. Each of the following links includes prerequisites, configuration instructions, installation instructions, and instructions for verifying your installation.

* To install the Smart Agent application on a single host machine, see :new-page:`quick install <https://github.com/signalfx/signalfx-agent/blob/main/docs/quick-install.md>`. 

* To automate the installation process, see :new-page:`deployment <https://github.com/signalfx/signalfx-agent#deployment>` for the deployment/configuration management tools, such as Chef, Puppet, Salt, and Ansible.

* To install the Smart Agent to a Windows host using a standalone package in a .zip file, see :new-page:`install to Windows using a .zip file <https://github.com/signalfx/signalfx-agent/blob/main/docs/agent-install-standalone-windows.md>`. 

* To install the Smart Agent to a Linux host using a compressed tar file, see :new-page:`install to Linux using a tar.gz file <https://github.com/signalfx/signalfx-agent/blob/main/docs/agent-install-standalone-linux.md>`.

* To deploy the Smart Agent to an AWS ECS instance using a configuration script, see :new-page:`install to AWS ECS <https://github.com/signalfx/signalfx-agent/blob/main/docs/agent-install-awsecs.md>`. 

* To install the Smart Agent using a Debian or RPM package, see :new-page:`install using *nix packages <https://github.com/signalfx/signalfx-agent/blob/main/docs/agent-install-packages.md>`. 

* To switch the APT-RPM package of the Smart Agent to the `splunk.jfrog.io` repository, see :new-page:`DEB/RPM repo migration <https://github.com/signalfx/signalfx-agent/blob/main/docs/deb-rpm-repo-migration.md>`.

* To install the Smart Agent to Kubernetes environments using the Helm package manager, see :new-page:`install using Helm <https://github.com/signalfx/signalfx-agent/blob/main/docs/agent-k8s-install-helm.md>`.

* To install the Smart Agent to Kubernetes environments using kubectl, see :new-page:`install using kubectl <https://github.com/signalfx/signalfx-agent/blob/main/docs/agent-k8s-install-kubectl.md>`.

The Smart Agent is incompatible on Linux systems with SELinux enabled. Check the documentation for your distribution to learn how to disable SELinux.

.. tip:: To uninstall the Smart Agent, see :ref:`uninstall-smart-agent`.

Configure the Smart Agent
==============================

You can configure the Smart Agent by editing the agent.yaml file. By default, the configuration is installed at and looked for at ``/etc/signalfx/agent.yaml`` on Linux and ``\ProgramData\SignalFxAgent\agent.yaml`` on Windows. You can override default locations using the ``-config`` command line flag.

The :new-page:`configuration schema <https://github.com/signalfx/signalfx-agent/blob/main/docs/config-schema.md>` includes the options that you can use in the agent.yaml file to control the behavior of your integrations. :new-page:`Example.yaml <https://github.com/signalfx/signalfx-agent/blob/main/docs/config-schema.md#example-yaml>` provides an autogenerated example of a YAML configuration file, with default values where applicable. :new-page:`Remote configuration <https://github.com/signalfx/signalfx-agent/blob/main/docs/remote-config.md>` describes how to configure the Smart Agent from remote sources, such as other files on the file system, or from key-value stores such as etcd.

Capture logs using the Smart Agent
====================================

The default log level is ``info``, which logs Smart Agent events without spamming the logs. Most of the ``info`` level logs are created upon startup and service discovery changes to record messages about routine operation. The ``debug`` log level creates verbose log output and should only be used when troubleshooting. 

You can change the log level using the ``logging: {level: info}`` YAML configuration option. Other valid log levels include ``warn`` and ``error``. 

The Smart Agent sends logs either as unstructured text (default) or JSON format. You can configure the Smart Agent to send JSON logs using the ``logging: {format: json}`` YAML configuration option.

Linux
-----------

The Smart Agent supports logging to ``stdout/stderr``, which is generally redirected by the init scripts provided to either a file at ``/var/log/signalfx-agent.log`` or to the systemd journal on newer distros.

Windows
------------

On Windows, the Smart Agent logs to the console when executed directly in a shell. If the Smart Agent is configured as a Windows service, log events are logged to the Windows Event Log. Select **Start > Administrative Tools > Event Viewer** to read logs. Select **Windows Logs > Application** to see logged events from the Smart Agent service.

Enable proxy support in the Smart Agent
===========================================

To use an HTTP or HTTPS proxy, set the environment variable ``HTTP_PROXY`` and/or ``HTTPS_PROXY`` in the container configuration to proxy either protocol. The SignalFx ingest and API servers both use HTTPS. If the ``NO_PROXY`` environment variable exists, the Smart Agent automatically appends the local services to the environment variable to not use the proxy.

If the Smart Agent is running as a local service on the host, refer to the host documentation for information on passing environment variables to the Smart Agent service to enable proxy support when the service is started. For example, if the host services are managed by systemd, create the /etc/systemd/system/signalfx-agent.service.d/myproxy.conf file and add the following to the file:

.. code-block:: toml

   [Service]
   Environment="HTTP_PROXY=http://proxy.example.com:8080/"
   Environment="HTTPS_PROXY=https://proxy.example.com:8081/"

After updating the file, run ``systemctl daemon-reload`` and ``systemctl restart signalfx-agent.service`` to restart the service with proxy support.

Sys-V based init.d systems: Debian and RHEL
---------------------------------------------

Create the /etc/default/signalfx-agent.yaml file with the following contents:

.. code-block::

   HTTP_PROXY="http://proxy.example.com:8080/"
   HTTPS_PROXY="https://proxy.example.com:8081/"

Smart Agent diagnostics 
====================================

The Smart Agent serves diagnostic information on an HTTP server at the address configured by the ``internalStatusHost`` and ``internalStatusPort`` options. Use the ``signalfx-agent`` command status to read the server and extract its contents. Use the content to identify and resolve issues with the Smart Agent. The ``signalfx-agent`` command also explains how to get further diagnostic information.

Service discovery using the Smart Agent
===========================================

The Smart Agent includes a comprehensive service discovery feature. This feature allows the Smart Agent to identify each of the services within your environment and automatically configure the appropriate integration plugins from within its bundle. This is particularly valuable in large ephemeral container environments that experience high-churn and dynamic service creation, as new services are automatically discovered, installed, and configured within minutes. However, this capability is extended to non-containerized environments as well. See :new-page:`Service Discovery <https://github.com/signalfx/signalfx-agent/blob/main/docs/auto-discovery.md>` for more information and configuration options.

Filtering data using the Smart Agent
====================================

Filter out certain data points or properties to prevent them from ever leaving the Smart Agent. Filtering can be useful to reduce clutter in charts without having to resort to filtering in the UI. 

If possible, it is preferable to prevent the data points and properties you want to omit from being generated by a monitor in the first place, as this reduces CPU and memory usage of the Smart Agent, but sometimes this is not feasible. 

See :new-page:`Filtering <https://github.com/signalfx/signalfx-agent/blob/main/docs/filtering.md>` for more information and configuration options for Smart Agent 4.7.0+. See :new-page:`Legacy Filtering <https://github.com/signalfx/signalfx-agent/blob/main/docs/legacy-filtering.md>` for more information on the old style of filtering, which is deprecated and removed in Smart Agent 5.0+.

.. _uninstall-smart-agent:

Uninstall the Smart Agent
=====================================================

To remove the Smart Agent from your system, follow the steps for each installation method.

Linux (package manager)
-----------------------------------------------------

- For Debian-based distributions, including Ubuntu, run the following command:

   .. code-block:: bash

      sudo dpkg --remove signalfx-agent

- For Red Hat, CentOS, and other RPM-based installs, run the following command:

   .. code-block:: bash

      sudo rpm -e signalfx-agent

.. note:: Configuration files might persist in ``/etc/signalfx``.

Linux (tar file)
-----------------------------------------------------

To uninstall the Smart Agent, stop the ``signalfx-agent`` process and delete the signalfx-agent directory.

Windows (installer)
-----------------------------------------------------

Uninstall the Smart Agent from :strong:`Programs and Features` in the Control Panel.

.. note:: Configuration files might persist in ``\ProgramData\SignalFxAgent``.

Windows (ZIP file)
-----------------------------------------------------

Run the following PowerShell commands to stop and uninstall the ``signalfx-agent`` service:

.. code-block:: shell
   
   SignalFxAgent\bin\signalfx-agent.exe -service "stop"
   SignalFxAgent\bin\signalfx-agent.exe -service "uninstall"

Then, delete the SignalFxAgent folder.

Deployed using kubectl
-----------------------------------------------------

To delete all Smart Agent resources, run the following command in the directory
containing the YAML configuration files:

.. code-block:: shell
   
   cat *.yaml | kubectl delete -f -

For more details, see the Kubectl Reference Documentation.

Deployed using Helm
----------------------------------------------------

To uninstall the Helm release of the Smart Agent, follow these steps:

- Get the release name from the Helm List at https://helm.sh/docs/helm/helm_list/ 
- Follow the steps in the Helm Uninstall guide.
- To remove the SignalFx Helm repository, see Helm Repo Remove at https://helm.sh/docs/helm/helm_repo_remove/

Deployed in AWS ECS
----------------------------------------------------

To deregister the ``signalfx-agent`` task definitions, see the official AWS documentation at https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deregister-task-definition.html.

To delete the ``signalfx-agent`` service, see Deleting a service at https://docs.aws.amazon.com/AmazonECS/latest/developerguide/delete-service.html.

Frequently asked questions 
=====================================================

See :new-page:`Frequently Asked Questions <https://github.com/signalfx/signalfx-agent/blob/main/docs/faq.md>` to troubleshoot issues with the Smart Agent.

.. _migrate-sa-to-otel-collector:

Migrating from the Smart Agent to Splunk Distribution of OpenTelemetry Collector
======================================================================================

Splunk Distribution of OpenTelemetry Collector is the next-generation agent and gateway for Splunk Observability products. See the following topics for information on migrating to Splunk Distribution of OpenTelemetry Collector:

* :new-page:`Migrating from the SignalFx Smart Agent <https://github.com/signalfx/splunk-otel-collector/blob/main/docs/signalfx-smart-agent-migration.md>`, which describes how to transition to Splunk Distribution of OpenTelemetry Collector without functionality loss.  
* :ref:`get-started-mapping-service`, which describes the Mapping Service. The Mapping Service is a transition tool that defines equivalencies between legacy collectd (Smart Agent) metric naming and semantic conventions to the OpenTelemetry names and formats for metrics and metric metadata.
* :ref:`get-started-mapping-transition-rept`, which describes how to migrate your data and metadata from dashboards, charts, and detectors from the Smart Agent to Splunk Distribution of OpenTelemetry Collector. 
