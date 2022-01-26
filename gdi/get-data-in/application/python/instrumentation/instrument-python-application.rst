.. _instrument-python-applications:

***************************************************************
Instrument a Python application for Splunk Observability Cloud
***************************************************************

.. meta:: 
   :description: The Splunk OpenTelemetry Python agent can automatically instrument your Python application or service. Follow these steps to get started.

The Python agent from the Splunk Distribution of OpenTelemetry Python can automatically instrument your Python application by dynamically patching supported libraries.

.. tip:: To generate all the basic install commands for your environment and application, open the Observability Cloud guided setup in :strong:`Data Setup > APM Instrumentation > Python > Add Connection`.

.. _install-enable-python-agent:

Install and enable the Python agent
===================================================================

Follow these steps to automatically instrument your application using the Python agent:

#. Check that you meet the requirements. See :ref:`python-otel-requirements`.

#. Install the ``splunk-opentelemetry[all]`` package:
   
   .. code-block:: bash

      pip3 install splunk-opentelemetry[all]

   If you're using a ``requirements.txt`` or ``pyproject.toml`` file, add ``splunk-opentelemetry[all]`` to it. 

#. Run the bootstrap script to install instrumentation for every supported package in your environment:

   .. code-block:: bash

      splunk-py-trace-bootstrap

   To print the instrumentation packages to the console instead of installing them, run ``splunk-py-trace-bootstrap --action=requirements``. You can then add the output to your requirements or Pipfile.

#. Set the ``OTEL_SERVICE_NAME`` environment variable:

   .. tabs::

      .. code-tab:: shell Linux
      
         export OTEL_SERVICE_NAME=<yourServiceName>
      
      .. code-tab:: shell Windows PowerShell
      
         $env:OTEL_SERVICE_NAME=<yourServiceName>

#. Enable the Splunk OTel Python agent by editing your Python service command.

   For example, if you open your Python application as follows:

      .. code-block:: bash

         python3 main.py --port=8000

   prefix the command with ``splunk-py-trace``:

      .. code-block:: bash

         splunk-py-trace python3 main.py --port=8000
         
   .. note:: To instrument uWSGI applications, see :ref:`python-manual-instrumentation`.

#. (Optional) Perform these additional steps if you're using the Django framework:
   
   - :ref:`django-instrumentation`

By default, tracing uses the Splunk OTel Collector to send trace data to Observability Cloud. If no data appears in :strong:`Observability > APM`, see :ref:`common-python-troubleshooting`.

.. _kubernetes_python_agent:

Deploy the Python agent in Kubernetes
==========================================================

To deploy the Python agent in Kubernetes, configure the Kubernetes Downward API to expose environment variables to Kubernetes resources.

The following example shows how to update a deployment to inject environment variables by adding the agent configuration under the ``.spec.template.spec.containers.env`` section:

.. code-block:: yaml

   apiVersion: apps/v1
   kind: Deployment
   spec:
     selector:
       matchLabels:
         app: your-application
     template:
       spec:
         containers:
           - name: myapp
             env:
               - name: SPLUNK_OTEL_AGENT
                 valueFrom:
                   fieldRef:
                     fieldPath: status.hostIP
               - name: OTEL_EXPORTER_OTLP_ENDPOINT
                 value: "http://$(SPLUNK_OTEL_AGENT):4317"
               - name: OTEL_SERVICE_NAME
                 value: "<serviceName>"
               - name: OTEL_RESOURCE_ATTRIBUTES
                 value: "deployment.environment=<environmentName>"

.. _configure-python-instrumentation:

Configure the Python agent
===========================================================

In most cases, the only configuration setting you need to enter is the service name. You can also define other basic settings, like the deployment environment, the service version, and the endpoint, among others.

- To set the deployment environment, provide a value for the ``deployment.environment`` attribute by entering the following command:

   .. tabs::

      .. code-tab:: bash Linux

         export OTEL_RESOURCE_ATTRIBUTES='deployment.environment=<envtype>'

      .. code-tab:: shell Windows PowerShell

         $env:OTEL_RESOURCE_ATTRIBUTES='deployment.environment=<envtype>'

- To set the service version, provide a value for the ``service.version`` attribute by entering the following command:

   .. tabs::

      .. code-tab:: bash Linux

         export OTEL_RESOURCE_ATTRIBUTES='deployment.environment=<envtype>,service.version=<version>'

      .. code-tab:: shell Windows PowerShell

         $env:OTEL_RESOURCE_ATTRIBUTES='deployment.environment=<envtype>,service.version=<version>'

- To use an exporter endpoint different than the default value, set the endpoint environment variable for the exporter:

   .. tabs::

      .. code-tab:: bash Linux

         export OTEL_EXPORTER_OTLP_ENDPOINT='http://<host>:<port>'

      .. code-tab:: shell Windows PowerShell

         $env:OTEL_EXPORTER_OTLP_ENDPOINT='http://<host>:<port>'

For advanced configuration of the Python agent, like changing trace propagation formats, correlating traces and logs, or configuring server trace data, see :ref:`advanced-python-otel-configuration`.

.. _export-directly-to-olly-cloud-python:

Send data directly to Observability Cloud
==============================================================

If you need to send data directly to Observability Cloud, set the following environment variables:

.. tabs::

   .. code-tab:: bash Linux

      export SPLUNK_ACCESS_TOKEN=<access_token>
      export OTEL_TRACES_EXPORTER=jaeger-thrift-splunk
      export OTEL_EXPORTER_JAEGER_ENDPOINT=https://ingest.<realm>.signalfx.com/v2/trace

   .. code-tab:: shell Windows PowerShell

      $env:SPLUNK_ACCESS_TOKEN=<access_token>
      $env:OTEL_TRACES_EXPORTER=jaeger-thrift-splunk
      $env:OTEL_EXPORTER_JAEGER_ENDPOINT=https://ingest.<realm>.signalfx.com/v2/trace

To obtain an access token, see :ref:`admin-api-access-tokens`.

In the ingest endpoint URL, ``realm`` is the :new-page:`Observability realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``. 

.. _instrument_aws_python_functions:

Instrument Lambda functions
=========================================================

You can instrument AWS Lambda functions using the Splunk OpenTelemetry Lambda Layer. See :ref:`instrument-aws-lambda-functions` for more information.