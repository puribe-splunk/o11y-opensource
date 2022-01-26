.. _get-configAPI:


********************************************************
Connect to AWS using the Splunk Observability Cloud API
********************************************************

.. meta::
   :description: Use cURL requests in the API to connect Splunk Observability Cloud to AWS.


To connect Splunk Observability Cloud to your AWS account, complete the following steps:

1. Generate an external ID.

2. Create an AWS policy and an AWS IAM (Identity and Access Management) role with a unique Amazon Resource Name (ARN).

3. Specify whether to collect both metrics and logs, and whether to gather metrics by API polling (which is the default) or through CloudWatch Metric Streams that are delivered to an Amazon Kinesis Data Firehose.

3. Provide the role ARN to the Infrastructure Monitoring component of Splunk Observability Cloud.


Create an AWS connection using POST and PUT requests
=====================================================

To connect Splunk Observability Cloud to AWS through the Observability Cloud API, open your command-line interface and perform the following steps:

1. Use the ``-X`` flag on a POST request to create an AWS connection that generates an external ID:

.. code-block:: none

   curl -X POST 'https://app.<realm>.signalfx.com/v2/integration' \
    -H 'accept: application/json, text/plain, */*' \
    -H 'x-sf-token: <user API access token>' \
    -H 'content-type: application/json' \
    --data-raw '{"name":"AWS-connection-name","type":"AWSCloudWatch","authMethod":"ExternalId","pollRate":300000,"services":[],"regions":[]}'

Your system response looks something like this:

.. code-block:: none

   {
   "authMethod" : "ExternalId",
   "enabled" : false,
   "externalId" : "<externalId>",
   "id" : "<id>",
   "importCloudWatch" : false,
   "name" : "AWS",
   "pollRate" : 300000,
   "regions" : [ ],
   "roleArn" : null,
   "services" : [ ],
   "type" : "AWSCloudWatch"
   }

In the system response, note the following:

- Values are displayed for the ``externalId`` and ``id`` fields.
- The ``importCloudWatch`` value is set to ``false``, because ingest of CloudWatch Metric Streams has not been configured.

2. Use a PUT request to create a new AWS policy and AWS IAM role with the ``externalId`` value generated in the previous step.

   The following example shows a PUT request for collecting data from two regions and three AWS services. The regions involved are ``us-west-1`` and ``us-east-1``. Services are identified by the ``namespace`` tag.

.. code-block:: none

   curl -X PUT 'https://app.<realm>.signalfx.com/v2/integration/E78gbtjBcAA' \
   -H 'accept: application/json, text/plain, */*' \
   -H 'x-sf-token: <user API access token>' \
   -H 'content-type: application/json' \
   --data-raw '{"authMethod": "ExternalId", "created": 1628082281828, "creator": "E73pzL5BUAI", "customCloudWatchNamespaces": null, "enableCheckLargeVolume": false, "enabled": true, "externalId": "<externalId>", "id": "<id>", "importCloudWatch": true, "largeVolume": false, "lastUpdated": 1628090302516, "lastUpdatedBy": "<id>", "name": "AWS", "pollRate": 300000, "regions": ["us-west-1", "us-east-1"], "roleArn": "<your-aws-iam-role-arn>", "services": [], "sfxAwsAccountArn": "arn:aws:iam::134183635603:root", "syncLoadBalancerTargetGroupTags": false, "type": "AWSCloudWatch", "key": null, "token": null, "namedToken": "Default", "namespaceSyncRules": [{"namespace": "AWS/S3"}, {"namespace": "AWS/EC2"}, {"namespace": "AWS/ApplicationELB"}]}'


Review the AWS IAM policy
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The default AWS IAM policy looks like this:

.. code-block:: json

   {
    "Version": "2012-10-17",
    "Statement": [
     {
      "Effect": "Allow",
      "Action": [
       "apigateway:GET",
       "autoscaling:DescribeAutoScalingGroups",
       "cloudfront:GetDistributionConfig",
       "cloudfront:ListDistributions",
       "cloudfront:ListTagsForResource",
       "cloudwatch:DescribeAlarms",
       "cloudwatch:GetMetricData",
       "cloudwatch:GetMetricStatistics",
       "cloudwatch:ListMetrics",
       "dynamodb:DescribeTable",
       "dynamodb:ListTables",
       "dynamodb:ListTagsOfResource",
       "ec2:DescribeInstances",
       "ec2:DescribeInstanceStatus",
       "ec2:DescribeRegions",
       "ec2:DescribeReservedInstances",
       "ec2:DescribeReservedInstancesModifications",
       "ec2:DescribeTags",
       "ec2:DescribeVolumes",
       "ecs:DescribeClusters",
       "ecs:DescribeServices",
       "ecs:DescribeTasks",
       "ecs:ListClusters",
       "ecs:ListServices",
       "ecs:ListTagsForResource",
       "ecs:ListTaskDefinitions",
       "ecs:ListTasks",
       "elasticache:DescribeCacheClusters",
       "elasticloadbalancing:DescribeLoadBalancerAttributes",
       "elasticloadbalancing:DescribeLoadBalancers",
       "elasticloadbalancing:DescribeTags",
       "elasticloadbalancing:DescribeTargetGroups",
       "elasticmapreduce:DescribeCluster",
       "elasticmapreduce:ListClusters",
       "es:DescribeElasticsearchDomain",
       "es:ListDomainNames",
       "kinesis:DescribeStream",
       "kinesis:ListShards",
       "kinesis:ListStreams",
       "kinesis:ListTagsForStream",
       "lambda:GetAlias",
       "lambda:ListFunctions",
       "lambda:ListTags",
       "logs:DeleteSubscriptionFilter",
       "logs:DescribeLogGroups",
       "logs:DescribeSubscriptionFilters",
       "logs:PutSubscriptionFilter",
       "organizations:DescribeOrganization",
       "rds:DescribeDBInstances",
       "rds:DescribeDBClusters",
       "rds:ListTagsForResource",
       "redshift:DescribeClusters",
       "redshift:DescribeLoggingStatus",
       "s3:GetBucketLocation",
       "s3:GetBucketLogging",
       "s3:GetBucketNotification",
       "s3:GetBucketTagging",
       "s3:ListAllMyBuckets",
       "s3:ListBucket",
       "s3:PutBucketNotification",
       "sqs:GetQueueAttributes",
       "sqs:ListQueues",
       "sqs:ListQueueTags",
       "tag:GetResources"
      ],
      "Resource": "*"
     }
    ]
   }


Configure your setup
======================

You enable the configuration you want by modifying the response returned by the Observability Cloud API after you use a POST request to generate an external ID.

You can configure your connection to support any of the following use cases:

 - Collect metrics for selected regions and services using CloudWatch API.
 - Collect metrics for all regions and all services using CloudWatch API.
 - Collect metrics using CloudWatch Metric Streams by itself or together with log collection.

The following example shows how to collect metrics from all regions and services by leaving the regions and services values unspecified.

.. code-block:: none

   curl -X PUT 'https://app.<realm>.signalfx.com/v2/integration/E78gbtjBcAA' \
   -H 'accept: application/json, text/plain, */*' \
   -H 'x-sf-token: <user API access token>' \
   -H 'content-type: application/json' \
   --data-raw '{"authMethod": "ExternalId", "created": 1628082281828, "creator": "E73pzL5BUAI", "customCloudWatchNamespaces": null, "enableCheckLargeVolume": false, "enabled": true, "externalId": "jobcimfczlkhwxlqwbum", "id": "E78gbtjBcAA", "importCloudWatch": true, "largeVolume": false, "lastUpdated": 1628090302516, "lastUpdatedBy": "E73pzL5BUAI", "name": "AWS", "pollRate": 300000, "regions": [], "roleArn": "<your-aws-iam-role-arn>", "services": [], "sfxAwsAccountArn": "arn:aws:iam::134183635603:root", "syncLoadBalancerTargetGroupTags": false, "type": "AWSCloudWatch", "key": null, "token": null, "namedToken": "Default", "namespaceSyncRules": []}'


Enable CloudWatch Metric Streams
===================================

To enable CloudWatch Metric Streams as an alternative to traditional API polling, follow these steps:

1. Submit a GET request to ``https://api.<realm>.signalfx.com/v2/integration/<integration-id>`` to retrieve your current settings. Make sure to substitute your own realm and integration ID in the URL.

2. Set the ``metricStreamsSyncState`` field to ``ENABLED``.

3. Set the ``importCloudWatch`` field to ``true``.

4. Set the ``enabled`` field to ``true``.

5. Submit a PUT request to the ``https://api.<realm>.signalfx.com/v2/integration/<integration-id>`` endpoint to save your updated settings.

.. note:: When you edit an AWS integration through the user interface for Splunk Observability Cloud, the integration ID shows in your browser address bar as an alphanumeric string in quotation marks (") after a colon (:) at the end of the URL.

6. Add the following permissions to the default AWS IAM policy:

.. code-block:: none

   "cloudwatch:ListMetricStreams",
   "cloudwatch:GetMetricStream",
   "cloudwatch:PutMetricStream",
   "cloudwatch:DeleteMetricStream",
   "cloudwatch:StartMetricStreams",
   "cloudwatch:StopMetricStreams",
   "iam:PassRole"

See :new-page:`Create an AWS integration using an external ID and ARN <https://dev.splunk.com/observability/docs/integrations/aws_integration_overview/#Create-an-AWS-integration-using-an-external-ID-and-ARN>` in the Splunk developer documentation for syntax examples.


Collect logs or CloudWatch Metric Streams and logs
===================================================

To collect log data from any CloudWatch log group, perform the following steps:

1. Deploy one of the CloudFormation templates provided by Splunk that supports log collection.

2. Use part of your ``curl -X PUT`` request for the API to set the ``logsSyncState`` field value to ``ENABLED``.

Splunk Observability Cloud synchronizes AWS integration settings with the logging configuration information on your AWS customer account every 5 minutes, adding triggers for newly-added services and removing triggers from regions or services removed from the integration.

To collect CloudWatch Metric Streams and logs from all supported AWS services across all regions, perform the following steps:

1. Select and deploy a CloudFormation template that supports metric streams and logs. Deploying the template creates an AWS Lambda function that is the Splunk AWS log collector. See the :ref:`CloudFormation templates table <aws-wizardconfig>` for more information.

2. Revise your configuration by modifying the response generated by the POST request you used to create an external ID as follows:

.. code-block:: none

   curl -X PUT 'https://app.<realm>.signalfx.com/v2/integration/<id>' \
   -H 'accept: application/json, text/plain, */*' \
   -H 'x-sf-token: <user API access token>' \
   -H 'content-type: application/json' \
   --data-raw '{"authMethod": "ExternalId", "created": 1628082281828, "creator": "<id>", "customCloudWatchNamespaces": null, "enableCheckLargeVolume": false, "enabled": true, "externalId": "<externalId>", "id": "<id>", "importCloudWatch": true, "largeVolume": false, "lastUpdated": 1628090302516, "lastUpdatedBy": "<id>", "logsSyncState": "ENABLED", "metricStreamsSyncState": "ENABLED", "name": "AWS", "pollRate": 300000, "regions": [], "roleArn": "<your-aws-iam-role-arn>", "services": [], "sfxAwsAccountArn": "arn:aws:iam::134183635603:root", "syncLoadBalancerTargetGroupTags": false, "type": "AWSCloudWatch", "key": null, "token": null, "namedToken": "Default", "namespaceSyncRules": []}'


See Splunk developer documentation about :new-page:`POST /integration <https://dev.splunk.com/observability/reference/api/integrations/latest#endpoint-create-integration>` for more examples of request format.

Next step
===========

After you connect Splunk Observability Cloud with AWS, you can use Observability Cloud to track a series of metrics and analyze your AWS data in real time. See :ref:`Leverage data from integration with AWS <aws-post-install>` for more information.
