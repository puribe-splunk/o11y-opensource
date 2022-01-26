.. _instrument-nodejs-applications:

***************************************************************
Instrument a Node application for Splunk Observability Cloud
***************************************************************

.. meta:: 
   :description: The Splunk Distribution of OpenTelemetry Node.js can automatically instrument your Node application or service. Follow these steps to get started.

The Splunk Distribution of OpenTelemetry JS can automatically instrument your Node application and many of the popular node.js libraries your application uses.

.. tip:: To generate all the basic install commands for your environment and application, open the Observability Cloud guided setup in :strong:`Data Setup > APM Instrumentation > Node.js > Add Connection`.

.. _install-enable-nodejs-agent:

Install and enable the Node.js instrumentation
===================================================================

To instrument your Node.js application with the Splunk Distribution of OpenTelemetry JS, follow these steps:

#. Install the ``@splunk/otel`` package:

   .. code-block:: bash

      npm install @splunk/otel

#. Install the instrumentation packages for your library or framework:

   .. code-block:: bash

      # Sample command for instrumenting the http library
      npm install @opentelemetry/instrumentation-http

   For a list of supported instrumentation packages, see :new-page:`Default Instrumentation Packages <https://github.com/signalfx/splunk-otel-js#default-instrumentation-packages>` on GitHub.

#. Set the ``OTEL_SERVICE_NAME`` environment variable:
   
   .. tabs::

      .. code-tab:: shell Linux
      
         export OTEL_SERVICE_NAME=<yourServiceName>
      
      .. code-tab:: shell Windows PowerShell
      
         $env:OTEL_SERVICE_NAME=<yourServiceName>

#. To run your Node application, enter the following command:

   .. code-block:: bash

      node -r @splunk/otel/instrument <your-app.js>

By default, tracing uses the Splunk OTel Collector to send trace data to Observability Cloud. If no data appears in :strong:`Observability > APM`, see :ref:`common-nodejs-troubleshooting`.

Instrument your application programmatically
-------------------------------------------------------

To have even finer control over the tracing pipeline, instrument your Node application programmatically.

To instrument your application programmatically, add the following lines at the beginning of your entry point script, before any instrumentation function is called:

.. code-block:: javascript 

   const { startTracing } = require('@splunk/otel');

   startTracing();

   // Rest of your main module

The ``startTracing()`` function accepts :ref:`configuration settings <advanced-java-otel-configuration>` as arguments. For example:

.. code-block:: javascript

   startTracing({
      serviceName: 'my-node-service',
   });

After you add the ``startTracing()`` function to your entry point script, execute your application by passing the instrumented entry point script via the ``-r`` flag:

.. code-block:: bash

   node -r <entry-point.js> <your-app.js>

To add custom or third-party instrumentations that implement the OpenTelemetry JS Instrumentation interface, pass them to ``startTracing()`` using the following code:

.. code-block:: javascript

   const { startTracing } = require('@splunk/otel');
   const { getInstrumentations } = require('@splunk/otel/lib/instrumentations');

   startTracing({
   instrumentations: [
      ...getInstrumentations(), // Adds default instrumentations
      new MyCustomInstrumentation(),
      new AnotherInstrumentation(),
   ]
   });

.. tip:: For an example of entry point script, see the :new-page:`sample tracer.js file <https://github.com/signalfx/splunk-otel-js/blob/main/examples/express/tracer.js>` on GitHub.

.. _kubernetes_nodejs_agent:

Deploy the Node.js distribution in Kubernetes
==========================================================

To deploy the Splunk Distribution of OpenTelemetry JS in Kubernetes, configure the Kubernetes Downward API to expose environment variables to Kubernetes resources.

The following example shows how to update a deployment to inject environment variables by adding the OpenTelemetry configuration under the ``.spec.template.spec.containers.env`` section:

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

.. _configure-nodejs-instrumentation:

Configure the Node.js distribution
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

.. _enable_automatic_metric_collection_nodejs:

- To enable automatic runtime metric collection, enable the metrics feature using the ``SPLUNK_METRICS_ENABLED`` environment variable. See :ref:`metrics-configuration-nodejs` for more information.

   .. tabs::

      .. code-tab:: bash Linux

         export SPLUNK_METRICS_ENABLED='true'

      .. code-tab:: shell Windows PowerShell

         $env:SPLUNK_METRICS_ENABLED='true'

For advanced configuration, like changing trace propagation formats or configuring server trace data, see :ref:`advanced-nodejs-otel-configuration`.

.. _export-directly-to-olly-cloud-nodejs:

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

Instrument Lambda functions
==================================

You can instrument AWS Lambda functions using the Splunk OpenTelemetry Lambda Layer. See :ref:`instrument-aws-lambda-functions` for more information.
