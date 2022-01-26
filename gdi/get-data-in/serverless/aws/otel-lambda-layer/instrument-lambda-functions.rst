.. _instrument-aws-lambda-functions:

******************************************************************
Instrument your AWS Lambda function for Splunk Observability Cloud
******************************************************************

.. meta::
   :description: The Splunk OpenTelemetry Lambda Layer automatically instruments your AWS Lambda functions for many programming languages. Follow these steps to get started.

Use the Splunk OpenTelemetry Lambda Layer automatically instruments your AWS Lambda functions for many programming languages.

.. tip:: To generate a template that instruments your Lambda function using the Splunk OpenTelemetry Lambda Layer, open the Observability Cloud guided setup in :strong:`Data Setup > AWS Services > AWS Lambda`.

.. _otel-lambda-layer-requirements:

Check compatibility and requirements
====================================

The Splunk OpenTelemetry Lambda Layer supports the following runtimes in AWS Lambda:

- Java. See :ref:`java-requirements`.
- Python. See :ref:`python-requirements`.
- Node.js. See :ref:`nodes-requirements`.

.. _install-otel-lambda-layer:

Install the AWS Lambda layer
====================================

Follow these steps to instrument your function using the Splunk OpenTelemetry Lambda Layer:

#. In the AWS Lambda console, select the function that you want to instrument.

#. In the :guilabel:`Layers` section, click :guilabel:`Add a layer`, then select :guilabel:`Specify an ARN`.

#. Copy the Amazon Resource Name (ARN) that matches the region of your Lambda function from the following list: https://github.com/signalfx/lambda-layer-versions/blob/master/splunk-apm/splunk-apm.md 

#. Paste the selected ARN in the :guilabel:`Specify an ARN` field and click :guilabel:`Add`.

#. Check that the Splunk layer appears in the :guilabel:`Layers` table.

.. _set-env-vars-otel-lambda:

Configure the Splunk OpenTelemetry Lambda Layer
===============================================

Follow these steps to add the required configuration for the Splunk OpenTelemetry Lambda Layer:

1. In the AWS Lambda console, open the function that you are instrumenting.

2. Navigate to :guilabel:`Configuration` > :guilabel:`Environment variables`, then click :guilabel:`Edit`.

3. Add each of the following environment variables by clicking :guilabel:`Add environment variable`:

   .. list-table:: 
      :header-rows: 1
      :widths: 40 60

      * - Environment variable
        - Description

      * - ``SPLUNK_REALM``
        - To find the realm of your Splunk Observability Cloud account, open the main menu in Observability Cloud, hover over your username, and then select :menuselection:`Account Settings`.

      * - ``SPLUNK_ACCESS_TOKEN``
        - Splunk authentication token that lets exporters send data directly to Splunk Observability Cloud. See :ref:`Authentication token <admin-tokens>`.

      * - ``OTEL_TRACES_EXPORTER`` and |br| ``OTEL_EXPORTER_OTLP_TRACES_PROTOCOL``
        - Depending on the language of your Lambda function, set the following environment variables:  
          
            .. tabs::

               .. group-tab:: Java

                  Set the ``OTEL_EXPORTER_OTLP_TRACES_PROTOCOL`` environment variable to ``http/protobuf``.

               .. group-tab:: Python

                  Set the ``OTEL_EXPORTER_OTLP_TRACES_PROTOCOL`` environment variable to ``http/protobuf``.

               .. group-tab:: NodeJS

                  Set the ``OTEL_TRACES_EXPORTER`` environment variable to ``jaeger-thrift-splunk``.

      * - ``AWS_LAMBDA_EXEC_WRAPPER``
        - Set the most appropriate value for the ``AWS_LAMBDA_EXEC_WRAPPER`` environment variable:  
          
            .. tabs::

               .. group-tab:: Java

                  Set the ``AWS_LAMBDA_EXEC_WRAPPER`` environment variable to one of the following values:

                  - ``/opt/otel-handler``: Wraps regular handlers that implement ``RequestHandler``.
                  - ``/opt/otel-proxy-handler``: Same as ``otel-handler``, but proxied through API Gateway, with HTTP context propagation enabled.
                  - ``/opt/otel-stream-handler``: Wraps streaming handlers that implement ``RequestStreamHandler``.

               .. group-tab:: Python

                  Set the ``AWS_LAMBDA_EXEC_WRAPPER`` environment variable to ``/opt/otel-instrument``.

               .. group-tab:: NodeJS

                  Set the ``AWS_LAMBDA_EXEC_WRAPPER`` environment variable to ``/opt/nodejs-otel-handler``.

      * - (Optional) ``OTEL_SERVICE_NAME``
        - The name of your service. If you don't provide a value, the value is the name of your function.

4. Click :guilabel:`Save` and check that the environment variables appear in the table.

For advanced configuration of the layer, like changing trace propagation formats or enabling debug logging, see :ref:`advanced-lambda-layer-configuration`.

.. _check-otel-lambda-data:

Check that data appears in Splunk Observability Cloud
=====================================================

Each time the AWS Lambda function runs, trace and metric data appears in Splunk Observability Cloud. If no data appears, see :ref:`troubleshooting-lambda-layer`.
