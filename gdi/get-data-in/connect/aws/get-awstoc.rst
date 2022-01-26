.. _get-started-aws:


************************************************************
Connect to AWS and send data to Splunk Observability Cloud
************************************************************

.. meta::
   :description: Connection planning information and links to the different ways to connect AWS to Splunk Observability Cloud

.. toctree::
   :hidden:

   aws-wizardconfig
   aws-apiconfig
   aws-terraformconfig
   aws-troubleshooting
   aws-metrics
   aws-post-install


As a system administrator who wants to leverage the power of full-fidelity data monitoring across your infrastructure, you can establish a connection with AWS and then set the following configuration options to complete the integration:

- Select Amazon Web Services (AWS) regions to collect data from
- Enable the ingestion of metrics through polling or streaming
- Decide whether to process information about application logs

Following configuration, you can use Amazon CloudWatch to import metrics and logs from supported AWS services into Splunk Observability Cloud, and analyze your data using Observability Cloud tools.

To connect Splunk Observability Cloud to AWS, follow these steps:

1. Plan the integration

2. Choose and use a connection option

3. Configure your connection

.. note:: To collect traces and metrics of your AWS Lambda functions for Splunk APM, see :ref:`splunk-otel-lambda-layer`.

.. raw:: html

  <embed>
    <h2>AWS integration prerequisites<a name="aws-integration-prereqs" class="headerlink" href="#aws-integration-prereqs" title="Permalink to this headline">¶</a></h2>
  </embed>

To connect AWS to Observability Cloud and integrate those platforms, you must meet the following prerequisites:

- Administrator privileges in Observability Cloud and your AWS accounts

- One of the following authentication methods:

  - An AWS IAM role and an external ID from Observability Cloud

  - A secure token, which combines an access key ID and a secret access key

The AWS GovCloud and China regions require a secure token for access. See :new-page:`Create and manage organization access tokens <https://docs.splunk.com/Observability/admin/authentication-tokens/org-tokens.html>` for more information.


.. raw:: html

  <embed>
    <h2>Prepare for AWS integration<a name="prep-for-aws-integration" class="headerlink" href="#prep-for-aws-integration" title="Permalink to this headline">¶</a></h2>
  </embed>

Regardless of the connection option you choose, you can configure your system more efficiently if you decide beforehand what data types and sources you want.

To choose the best connection method and configuration settings, answer the following questions before you connect AWS to Splunk Observability Cloud:

- Do I want to collect metrics through API polling at specified intervals, or through CloudWatch Metric Streams? (You configure through the Observability API to support CloudWatch Metric Streams)
- Do I want to collect logs in addition to metrics? (If yes, then include logs while configuring through the API or when given that option while performing a guided setup)


.. raw:: html

  <embed>
    <h2>AWS connection options<a name="connection-options-aws" class="headerlink" href="#aws-connection-options-aws" title="Permalink to this headline">¶</a></h2>
  </embed>

You can connect Observability Cloud to AWS in several different ways. Choose the connection method that best matches your needs:

.. list-table::
   :header-rows: 1
   :widths: 50, 50

   * - :strong:`Connection option`
     - :strong:`Reason for using this method`

   * - Connect to AWS using the :new-page:`guided setup <https://docs.splunk.com/Observability/gdi/get-data-in/connect/aws/aws-wizardconfig.html>` in Splunk Observability Cloud
     - Guides you step-by-step to set up an AWS connection and default configuration in Observability Cloud. Guided setup includes links to Amazon CloudFormation templates that you can select to create needed AWS IAM roles.

   * - Connect to AWS using the :new-page:`Splunk Observability Cloud API <https://docs.splunk.com/Observability/gdi/get-data-in/connect/aws/aws-apiconfig.html>`
     - Requires knowledge of POST and PUT call syntax, but includes options and automation that are not part of the guided setup. Choose this method if you want to configure many integrations at once, or enable CloudWatch Metric Streams rather than polling for metrics data.

   * - Connect to AWS using :new-page:`Splunk Terraform <https://docs.splunk.com/Observability/gdi/get-data-in/connect/aws/aws-terraformconfig.html>`
     - Can be used if you already manage your infrastructure as code by deploying through Terraform.

.. note:: If you can't connect AWS to Splunk Observability Cloud, see :new-page:`Troubleshoot your AWS connection <https://docs.splunk.com/Observability/gdi/get-data-in/connect/aws/aws-troubleshooting.html#nav-Troubleshoot-your-AWS-connection>`.


.. raw:: html

  <embed>
    <h2>How Observability Cloud impacts your Amazon CloudWatch usage<a name="plan-to-use-cloudwatch-metric-streams" class="headerlink" href="#plan-to-use-cloudwatch-metric-streams" title="Permalink to this headline">¶</a></h3>
  </embed>

CloudWatch Metric Streams send metrics to a Kinesis Data Firehose stream, so you can see and act on them faster than is possible when metrics are collected by polling the API at specified intervals, because stream functionality greatly reduces latency.

Each metric stream generates its own set of metrics. See :new-page:`Low Latency Observability Into AWS Services With Splunk <https://www.splunk.com/en_us/blog/devops/real-time-observability-splunk-cloudwatch-metric-streams.html>` in the DevOps blog for more information.

Although metric streams are more efficient than API polling, CloudWatch metric streams also have constraints you should consider.

.. raw:: html

  <embed>
    <h3>Collection interval<a name="collection-interval-aws" class="headerlink" href="#aws-collection-interval-aws" title="Permalink to this headline">¶</a></h3>
  </embed>

CloudWatch Metric Streams continually stream Amazon CloudWatch metrics as soon as they are published. In most cases, the metrics are published once per minute.

For customers currently collecting Amazon CloudWatch metrics at the default polling rate of 300 seconds (5 minutes), this difference in intervals results in more data being collected from Amazon CloudWatch. This increase in data rate typically increases Amazon CloudWatch usage costs.

Customers already polling at 1-minute intervals generally see a slight decrease in Amazon CloudWatch usage costs.

.. raw:: html

  <embed>
    <h3>Tag filtering<a name="tag-filtering-aws" class="headerlink" href="#aws-tag-filtering-aws" title="Permalink to this headline">¶</a></h3>
  </embed>

CloudWatch Metric Streams do not support filtering based on resource tags. Configuration applies to individual services, and all resources that report metrics from a configured service stream those metrics.

If you filter data based on tags, your costs for Amazon CloudWatch and Splunk Infrastructure Monitoring might increase.


.. raw:: html

  <embed>
    <h2>After AWS integration<a name="after-aws-integration" class="headerlink" href="#after-aws-integration" title="Permalink to this headline">¶</a></h2>
  </embed>

See :new-page:`Leverage data from integration with AWS <https://docs.splunk.com/Observability/gdi/get-data-in/connect/aws/aws-post-install.html>` for an overview of what you can do after you connect Observability Cloud to AWS.

See :ref:`AWS metrics <aws-metrics>` for a list of the available AWS resources.

For instructions on how to import AWS metrics and metadata or AWS tag and log information using namespaces and filters, see :new-page:`Monitor Amazon Web Services <https://docs.splunk.com/Observability/infrastructure/navigators/aws.html#nav-Monitor-Amazon-Web-Services>`.
