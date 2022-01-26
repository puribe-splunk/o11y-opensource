.. _instrument-java-applications:

***************************************************************************
Instrument a Java application for Splunk Observability Cloud
***************************************************************************

.. meta:: 
   :description: The Splunk OpenTelemetry Java agent can automatically instrument your Java application or service. Follow these steps to get started.

The Java agent from the Splunk Distribution of OpenTelemetry Java can automatically instrument your Java application by injecting instrumentation to Java classes.

.. tip:: To generate all the basic install commands for your environment and application, open the Observability Cloud guided setup in :strong:`Data Setup > APM Instrumentation > Java > Add Connection`.

.. _install-enable-jvm-agent:

Install and enable the Java agent
===================================================================

Follow these steps to automatically instrument your application using the Java agent:

#. Check that you meet the requirements. See :ref:`java-otel-requirements`.

#. Download the JAR file for the latest version of the agent:

   .. tabs::

      .. code-tab:: shell Linux

         curl -L https://github.com/signalfx/splunk-otel-java/releases/latest/download/splunk-otel-javaagent.jar \
         -o splunk-otel-javaagent.jar

      .. code-tab:: shell Windows PowerShell

         Invoke-WebRequest -Uri https://github.com/signalfx/splunk-otel-java/releases/latest/download/splunk-otel-javaagent.jar -OutFile splunk-otel-javaagent.jar

#. Set the ``OTEL_SERVICE_NAME`` environment variable:

   .. tabs::

      .. code-tab:: shell Linux
      
         export OTEL_SERVICE_NAME=<yourServiceName>
      
      .. code-tab:: shell Windows PowerShell
      
         $env:OTEL_SERVICE_NAME=<yourServiceName>

#. Set the ``-javaagent`` argument to the path of the Java agent:

   .. code-block:: bash
      :emphasize-lines: 1

      java -javaagent:./splunk-otel-javaagent.jar \
      -jar <myapp>.jar

   .. note:: If your application runs on a supported Java server, see :ref:`java-servers-instructions`. 

#. Configure the Java agent. See :ref:`configure-java-instrumentation`.

By default, tracing uses the Splunk OTel collector to send trace data to Observability Cloud. If no data appears in :strong:`Observability > APM`, see :ref:`common-java-troubleshooting`.

.. note:: If you need to add custom attributes to spans or want to manually generate spans, instrument your Java application or service manually. See :ref:`java-manual-instrumentation`.

.. _kubernetes_java_agent:

Deploy the Java agent in Kubernetes
==========================================================

To deploy the Java agent in Kubernetes, configure the Kubernetes Downward API to expose environment variables to Kubernetes resources.

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

.. _configure-java-instrumentation:

Configure the Java agent
===========================================================

You can configure the agent using environment variables or by setting system properties as runtime arguments. For more details about both methods, see :ref:`configuration-methods-java`.

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

.. _enable_automatic_metric_collection:

- To enable automatic metric collection, enable the metrics feature using a system property argument. You can also use the ``SPLUNK_METRICS_ENABLED`` environment variable.

   ``-Dsplunk.metrics.enabled=true``

.. _enable_profiling_java:

- To enable AlwaysOn Profiling, use the following system property argument. You can also use the ``SPLUNK_PROFILER_ENABLED`` environment variable. For more information, see :ref:`profiling-intro`.

   ``-Dsplunk.profiler.enabled=true``

For advanced configuration of the JVM agent, like changing trace propagation formats, correlating traces and logs, or enabling custom sampling, see :ref:`advanced-java-otel-configuration`.

.. _export-directly-to-olly-cloud-java:

Send data directly to Observability Cloud
==============================================================

If you need to bypass the OTel Collector and send data directly to Observability Cloud, set the following environment variables:

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

In the ingest endpoint URL, ``realm`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``.

.. note:: This procedure applies to spans and traces. To send AlwaysOn Profiling data, you must use the OTel Collector.

.. _instrument_aws_lambda_functions:

Instrument Lambda functions
=========================================================

You can instrument AWS Lambda functions using the Splunk OpenTelemetry Lambda Layer. See :ref:`instrument-aws-lambda-functions` for more information.