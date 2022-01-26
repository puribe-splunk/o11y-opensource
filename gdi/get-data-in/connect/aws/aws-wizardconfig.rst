.. _aws-wizardconfig:


*********************************************************************
Connect to AWS using the guided setup in Splunk Observability Cloud
*********************************************************************

.. meta::
   :description: Use guided setup to connect Splunk Observability Cloud to AWS through CloudWatch.

If you have administrator privileges for Splunk Observability Cloud and your Amazon Web Services (AWS) account, you can use guided setup to do the following:

- Connect AWS to Observability Cloud.

- Configure metrics and logs collection.

For other ways to connect Observability Cloud to AWS, see :ref:`Connect to AWS and send data to Splunk Observability Cloud <get-started-aws>`.


Access the guided setup for AWS integration
============================================

To access the guided setup for AWS integration, perform the following steps:

1. Log in to Splunk Observability Cloud.

2. Select :strong:`Data Setup` from the navigation menu.

3. In the Connect Your Data page, select the tile for :strong:`Amazon Web Services`.

4. Follow the steps provided in the guided setup.

5. For the authentication step of establishing a connection between your AWS account and Splunk Observability Cloud, do one of the following:

   - In most AWS regions, use the Identity and Access Management (IAM) policy created through the guided setup.

   - For the GovCloud or China regions, select the option to authenticate using a secure token.

.. note:: While choosing data sources, you might encounter an option to import all data from built-in CloudWatch namespaces. In such a case, select that option to ensure that built-in dashboards display automatically.

If you run into a problem, guided setup displays an error message in context at the step with the problem. The error message summarizes and suggests a fix for that problem. If more error detail is available, you can click on the error summary to expand and display additional details.

See also :new-page:`Getting data in for AWS CloudWatch <https://youtu.be/MFs5Dge8t3A>` on the Splunk YouTube channel for an instructional video that describes how to get AWS CloudWatch data into Splunk Observability Cloud.


Create an AWS IAM policy
-------------------------

The AWS IAM policy is a JSON object to which Observability Cloud refers for permission to collect data from every supported AWS service.

If this is the first time you have connected Observability Cloud to Amazon CloudWatch, or if you want to create a new AWS IAM policy, follow these steps. If you have already installed at least one AWS integration and want to reuse the same IAM policy, skip to the "Create an AWS IAM role" section.

1. Log into your Amazon Web Services account.

2. From the Services list, select :strong:`IAM` to open Identity and Access Management.

3. Click :strong:`Policies > Create Policy`.

4. Click the :strong:`JSON` tab.

5. Delete the text shown under the JSON tab so that you can replace it with the code stanza shown below and also in the "Prepare AWS Account" step of the guided setup.

6. Select :strong:`Review policy`.

7. Give the policy a name and, optionally, a description.

8. Select :strong:`Create policy`.


While preparing your AWS account, guided setup prompts you to copy the default IAM policy to connect your AWS account to Splunk Observability Cloud.

.. note:: The default IAM policy supports metrics and logs collection. To add support for CloudWatch Metric Streams, add the permissions shown in the "Enable CloudWatch Metric Streams" section.


Create an AWS IAM role
-------------------------

Your AWS account includes IAM in its list of services. After creating an AWS IAM policy, you assign that policy to a particular role by performing the following steps at the Amazon Web Services console:

1. Select :strong:`Roles > Create Role`.

2. Select :strong:`Another AWS account` as the type of trusted entity.

3. Copy and paste the Account ID displayed in guided setup into the :strong:`Account ID` field.

4. Select :strong:`Require external ID`.

5. Copy and paste the External ID displayed in guided setup into the :strong:`External ID` field.

6. Click :strong:`Next: Permissions`.

7. Under :strong:`Policy name`, select the policy you made in the previous step.

8. Click through :strong:`Next: Tags` and :strong:`Next: Review`.

9. Name your new AWS IAM role. You also have the option of adding a short description for it.

10. Click :strong:`Create role`.

Creating the AWS IAM role generates the Role ARN used to establish connection with AWS, after you attach the permissions policy to the role.


Optionally revise default AWS integration settings
==================================================

After creating an AWS IAM policy and assigning it to a particular role through the guided setup, you can modify your configuration as follows:

- Limit the scope of data collection in either of the following ways:

  - Use checkbox options in the guided setup to limit the scope of your data collection.

  - Use the AWS console to revise the contents of the ``Action`` and ``Resource`` fields.

- Add permissions for CloudWatch Metric Streams to your IAM policy.

- Select a CloudFormation template to collect logs for each AWS region that you want to operate in.


Enable CloudWatch Metric Streams
----------------------------------

You can enable CloudWatch Metric Streams rather than metrics gathered through API polling if you connect to AWS through the Splunk Observability API. See :new-page:`Enable CloudWatch Metric Streams through the API <https://docs.splunk.com/Observability/gdi/get-data-in/connect/aws/aws-apiconfig.html#enable-cloudwatch-metric-streams-through-the-api>` for details.

CloudWatch settings gather metrics at the polling interval you specify, with one minute as the minimum unit. The API metric polling rate is expressed in seconds. For example, a value of 300 polls metrics once every 5 minutes.



Choose a CloudFormation template
-----------------------------------

You choose a CloudFormation template depending on your deployment method (for example, per AWS region or per AWS account) and integration type (for example, logs only, metric streams only, or both).

From the "CloudFormation templates" table, select the QuickLink for a template with support for metric streams or logs. The QuickLink automatically opens the AWS Management Console in the last region that you used, but you can optionally select another region in the AWS Management Console.

If the prepopulated CloudFormation template does not meet your needs, create required resources using CloudFormation manually by following these steps:

1. Select the :strong:`Hosted template link` to download and modify the template you choose.

2. In the :strong:`Quick Create stack` dialog box for the selected template, enter the access token for your organization.

3. Select :strong:`Create stack`.

4. Use an API call to enable CloudWatch Metric Streams. See :new-page:`Enable CloudWatch Metric Streams through the API <https://docs.splunk.com/Observability/gdi/get-data-in/connect/aws/aws-apiconfig.html#enable-cloudwatch-metric-streams-through-the-api>.`

You can optionally use AWS CloudFormation StackSets to work simultaneously across multiple AWS regions after configuring the StackSet prerequisites for self-managed permissions.

See Amazon Web Services documentation for configuring StackSet prerequisites at :new-page:`https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-prereqs-self-managed.html <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-prereqs-self-managed.html>`

Even if you don't intend to use both logs and metrics, you can safely deploy a CloudFormation template, because unused infrastructure does not generate costs.

:strong:`CloudFormation templates`

Select the QuickLink for a template with support for metric streams or logs. The QuickLink opens the AWS Management Console in the last region that you used.

.. list-table::
   :header-rows: 1
   :widths: 16, 16, 16, 16, 36

   * - Supports Log collection
     - Supports Metric Streams
     - Deployment type
     - QuickLink
     - Hosted template link

   * - yes
     - yes
     - once per account (using StackSets)
     - deploy this :new-page:`https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_all_features.yaml <https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_all_features.yaml>`
     - :new-page:`https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_all_features.yaml`

   * - yes
     - yes
     - in each region
     - deploy this in every region :new-page:`https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_all_features_regional.yaml <https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_all_features_regional.yaml>`
     - :new-page:`https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_all_features_regional.yaml`

   * - yes
     - no
     - once per account (using StackSets)
     - deploy this :new-page:`https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_logs.yaml <https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_logs.yaml>`
     - :new-page:`https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_logs.yaml`

   * - yes
     - no
     - in each region
     - deploy this in every region :new-page:`https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_logs_regional.yaml <https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_logs_regional.yaml>`
     - :new-page:`https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_logs_regional.yaml`

   * - no
     - yes
     - once per account (using StackSets)
     - deploy this :new-page:`https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_metric_streams.yaml <https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_metric_streams.yaml>`
     - :new-page:`https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_metric_streams.yaml`

   * - no
     - yes
     - in each region
     - deploy this in every region :new-page:`https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_metric_streams_regional.yaml <https://console.aws.amazon.com/cloudformation/home#/stacks/create/review?templateURL=https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_metric_streams_regional.yaml>`
     - :new-page:`https://o11y-public.s3.amazonaws.com/aws-cloudformation-templates/release/template_metric_streams_regional.yaml`

After you connect Splunk Observability Cloud with AWS, you can use Observability Cloud to track metrics and analyze your AWS data in real time. See :ref:`AWS metrics <aws-metrics>` for a list of the available AWS resources.
