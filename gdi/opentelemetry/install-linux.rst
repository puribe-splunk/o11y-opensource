.. _otel-install-linux:

*************************
Install on Linux
*************************

.. meta::
      :description: Describes how to install Splunk Distribution of OpenTelemetry Collector on Linux.

Install the Splunk OpenTelemetry Collector on Linux using one of these methods:

* :ref:`Installer script <linux-script>`
* :ref:`Ansible <linux-ansible>`
* :ref:`Puppet <linux-puppet>`
* :ref:`Heroku <linux-heroku>`
* :ref:`Debian or RPM <linux-debian-rpm>`
* :ref:`Docker <linux-docker>`
* :ref:`Amazon Fargate <linux-amazon-fargate>`
* :ref:`Amazon ECS EC2 <linux-amazon-ecs-ec2>`
* :ref:`Binary file <linux-binary-file>`

.. note::

   The SignalFx Smart Agent and collectd bundle is only supported and installed on x86_64 and AMD64 platforms. The SignalFx Smart Agent Receiver is subject to the same limitation.

.. _linux-script:

Installer script
=================================

The following Linux distributions and versions are supported:

* Amazon Linux: 2
* CentOS, Red Hat, or Oracle: 7, 8
* Debian: 8, 9, 10
* SUSE: 12, 15 for collector versions v0.34.0 or higher. Log collection with Fluentd is not currently supported.
* Ubuntu: 16.04, 18.04, 20.04

You must have systemd installed to use this script. The installer script deploys and configures these things:

* Splunk OpenTelemetry Collector for Linux
* :new-page:`SignalFx Smart Agent and collectd bundle <https://github.com/signalfx/signalfx-agent/releases>` 
* Fluentd (via the TD Agent)

Do the following to install the Splunk OpenTelemetry Collector using the installer script:

#. Ensure that you have ``curl`` and ``sudo`` installed.
#. Download and execute the installer script.
#. Replace the following variables for your environment:

* ``SPLUNK_REALM``: This is the realm to send data to. The default is ``us0``. See :new-page:`realms <https://dev.splunk.com/observability/docs/realms_in_endpoints/>`.
* ``SPLUNK_MEMORY_TOTAL_MIB``: This is the total memory in MiB allocated to the Splunk OpenTelemetry Collector. For example, ``512``.
* ``SPLUNK_ACCESS_TOKEN``: This is the Splunk access token to authenticate requests. See :ref:`admin-org-tokens`.

.. code-block:: bash

   curl -sSL https://dl.signalfx.com/splunk-otel-collector.sh > /tmp/splunk-otel-collector.sh;
   sudo sh /tmp/splunk-otel-collector.sh --realm SPLUNK_REALM -- SPLUNK_ACCESS_TOKEN

.. _linux-ansible:

Ansible
==================

Splunk provides an Ansible collection to install and configure the Splunk OpenTelemetry Collector. Collections are a distribution format for Ansible content that can include playbooks, roles, modules, and plugins. See :new-page:`Ansible Collection for Splunk OpenTelemetry Collector <https://galaxy.ansible.com/signalfx/splunk_otel_collector>` to download the collection.

.. _linux-puppet:

Puppet
================

Splunk provides a Puppet module to install and configure the Splunk OpenTelemetry Collector. A module is a collection of resources, classes, files, definition, and templates. See :new-page:`Splunk OpenTelemetry Collector Puppet Module <https://forge.puppet.com/modules/signalfx/splunk_otel_collector>` to download the module.

.. _linux-heroku:

Heroku
===================

Splunk OpenTelemetry Collector is available as a Heroku buildpack that collects metrics and can receive spans from apps and reports them to Splunk Observability Cloud.

From the Observability Cloud home page, select :menuselection:`Navigation menu > Data Setup > Applications > Heroku` and click ``Add Connection``. See :new-page:`Splunk OpenTelemetry Collector for Heroku <https://github.com/signalfx/splunk-otel-collector/blob/main/deployments/heroku/README.md>` in GitHub if you need more information.

.. _linux-debian-rpm:

Debian or RPM packages
==========================

All Intel, AMD, and ARM systemd-based operating systems are supported, including CentOS, Debian, Oracle, Red Hat, and Ubuntu. Manually installing an integration is useful for containerized environments, or if you want to use other common deployment options.

All installation methods offer default configurations that you can configure by using environment variables. The configuration of these variables depends on the installation method used.

Do the following to install the Splunk OpenTelemetry Collector using a Debian or RPM package:

#. Set up the package repository and install the package, as shown in the following examples. The first example shows the Debian package and the subsequent examples show the RPM package. A default configuration is installed to /etc/otel/collector/agent_config.yaml, if it does not already exist::

    # Debian 
    curl -sSL https://splunk.jfrog.io/splunk/otel-collector-deb/splunk-B3CD4420.gpg > /etc/apt/trusted.gpg.d/splunk.gpg
    echo 'deb https://splunk.jfrog.io/splunk/otel-collector-deb release main' > /etc/apt/sources.list.d/splunk-otel-collector.list
    apt-get update
    apt-get install -y splunk-otel-collector

    # RPM with yum
    yum install -y libcap  
    # Required for enabling cap_dac_read_search and cap_sys_ptrace 
    # capabilities on the Splunk OpenTelemetry Collector

    cat <<EOH > /etc/yum.repos.d/splunk-otel-collector.repo
    [splunk-otel-collector]
    name=Splunk OpenTelemetry Collector Repository
    baseurl=https://splunk.jfrog.io/splunk/otel-collector-rpm/release/\$basearch
    gpgcheck=1
    gpgkey=https://splunk.jfrog.io/splunk/otel-collector-rpm/splunk-B3CD4420.pub
    enabled=1
    EOH

    yum install -y splunk-otel-collector

    # RPM with dnf
    dnf install -y libcap  
    # Required for enabling cap_dac_read_search and cap_sys_ptrace 
    # capabilities on the Splunk OpenTelemetry Collector

    cat <<EOH > /etc/yum.repos.d/splunk-otel-collector.repo
    [splunk-otel-collector]
    name=Splunk OpenTelemetry Collector Repository
    baseurl=https://splunk.jfrog.io/splunk/otel-collector-rpm/release/\$basearch
    gpgcheck=1
    gpgkey=https://splunk.jfrog.io/splunk/otel-collector-rpm/splunk-B3CD4420.pub
    enabled=1
    EOH

    dnf install -y splunk-otel-collector

    # RPM with zypper
    zypper install -y libcap-progs  
    # Required for enabling cap_dac_read_search and cap_sys_ptrace 
    # capabilities on the Splunk OpenTelemetry Collector

    cat <<EOH > /etc/zypp/repos.d/splunk-otel-collector.repo
    [splunk-otel-collector]
    name=Splunk OpenTelemetry Collector Repository
    baseurl=https://splunk.jfrog.io/splunk/otel-collector-rpm/release/\$basearch
    gpgcheck=1
    gpgkey=https://splunk.jfrog.io/splunk/otel-collector-rpm/splunk-B3CD4420.pub
    enabled=1
    EOH

    zypper install -y splunk-otel-collector
#. Configure the splunk-otel-collector.conf environment file with the appropriate variables. You need this environment file to start the ``splunk-otel-collector`` systemd service. When you install the package in step 1, a sample environment file is installed to /etc/otel/collector/splunk-otel-collector.conf.example. This file includes the required environment variables for the default configuration.
#. Run ``sudo systemctl restart splunk-otel-collector.service`` to start or restart the service.

.. _linux-docker:

Docker
===========

Run the following command to install the Splunk OpenTelemetry Collector using Docker:

.. code-block:: bash

   docker run --rm -e SPLUNK_ACCESS_TOKEN=12345 -e SPLUNK_REALM=us0 \
       -p 13133:13133 -p 14250:14250 -p 14268:14268 -p 4317:4317 -p 6060:6060 \
       -p 7276:7276 -p 8888:8888 -p 9080:9080 -p 9411:9411 -p 9943:9943 \
       --name otelcol quay.io/signalfx/splunk-otel-collector:latest

Use a semantic versioning (semver) tag instead of the ``latest`` tag.

See :new-page:`docker-compose.yml <https://github.com/signalfx/splunk-otel-collector/blob/main/examples/docker-compose/docker-compose.yml>` in GitHub to download a ``docker-compose`` example.

.. _linux-amazon-fargate:

Amazon Fargate 
====================
.. note::

   Available for Prometheus only. Not yet available for Amazon EKS.

Splunk provides an integration wizard to deploy the Splunk OpenTelemetry Collector on Amazon Fargate as a sidecar (additional container) to Amazon ECS tasks. 

From the Observability Cloud home page, select :menuselection:`Navigation menu > Data Setup > AWS Services > Amazon Fargate` and click ``Add Connection``. 

See the :new-page:`AWS Fargate Deployment <https://github.com/signalfx/splunk-otel-collector/tree/main/deployments/fargate>` README in GitHub if you need more information.

.. _linux-amazon-ecs-ec2:

Amazon ECS EC2 
=======================================

.. note::

   Available for Prometheus only. 

Splunk provides a task definition to deploy the Splunk OpenTelemetry Collector to ECS EC2. The task definition is a text file, in JSON format, that describes one or more containers that form your application. 

From the Observability Cloud home page, select :menuselection:`Navigation menu > Data Setup > AWS Services > Amazon ECS EC2` and click ``Add Connection``. 

See the :new-page:`AWS ECS EC2 <https://github.com/signalfx/splunk-otel-collector/tree/main/deployments/ecs/ec2>` README in GitHub if you need more information.

.. _linux-binary-file:

Binary file
==================
Download pre-built binaries (``otelcol_linux_amd64`` or ``otelcol_linux_arm64``) from :new-page:`GitHub releases <https://github.com/signalfx/splunk-otel-collector/releases>`.

More options
==================================
Once you have installed the Splunk OpenTelemetry Collector, you can perform these actions:

* :new-page:`Get started using Log Observer <https://quickdraw.splunk.com/redirect/?product=Observability&location=log.observer.setup&version=current>`
* :ref:`apm`
