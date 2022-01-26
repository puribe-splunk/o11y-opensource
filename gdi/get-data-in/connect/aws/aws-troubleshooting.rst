.. _aws-troubleshooting:


************************************
Troubleshoot your AWS connection
************************************

.. meta::
   :description: Resolve AWS policy and permissions conflicts.


If you have a problem connecting Splunk Observability Cloud to your Amazon Web Services (AWS) account, it is most likely caused by conflicts between policies and permissions.


Error validating AWS connection
================================

The automatic attempt to validate a connection that you just configured fails, so there is no connection between Splunk Observability Cloud and your AWS account.

Cause
^^^^^^

The connection might fail due to mismatched Identity Access Management (IAM) policies. To diagnose connection failure, check the permissions or policies you set up and compare them to the permissions that AWS requires.

Verify whether your error message looks similar to this example:

.. code-block:: none

   Error validating AWS / Cloudwatch credentials
   Validation failed for following region(s):
   us-east-1
   [ec2] software.amazon.awssdk.services.ec2.model.Ec2Exception: You are not authorized to perform this operation.

If you receive a similar error message, then the IAM policy that you created to connect AWS to Splunk Observability Cloud does not match the policy already in your AWS account.

Similarly, if your AWS account uses a service control policy (SCP) or administrative features such as ``PermissionsBoundary``, then there might be limits on which calls can be made in your organization, even if those calls are covered by your AWS IAM policy.

Solution
^^^^^^^^^

Splunk Observability Cloud uses the following calls to validate whether it can accept data from the AWS Compute Optimizer tool to support CloudWatch metric streams:

.. code-block:: none

   client.describeInstanceStatus(),
   client.describeTags(),
   client.describeReservedInstances(),
   client.describeReservedInstancesModifications()
   client.describeOrganization()

To ensure that your AWS integration works as expected, revisit your configuration choices in Splunk Observability Cloud to verify that they match the permissions policy in your AWS management console. 

A match ensures that conflicting permissions do not cause your AWS environment to block integrations. See the "Amazon CloudWatch permissions reference" in the Amazon documentation for details about the available permissions.


Splunk Observability Cloud doesn't work as expected
====================================================

Features or tools within Splunk Observability Cloud do not work as expected.

Cause
^^^^^^

When a feature in Splunk Observability Cloud does not work as expected after connection to AWS, then permissions for that feature in the AWS IAM policy are absent or blocking implementation.

For example, ``ec2:DescribeRegions`` is used to detect which AWS regions are active in your account. Without that permission, or if no region is specified, then system settings default to AWS standard regions.

Similarly, metrics collection depends on the following permissions:

.. code-block:: none

   cloudwatch:DescribeAlarms
   cloudwatch:GetMetricData
   cloudwatch:GetMetricStatistics
   cloudwatch:ListMetrics

Solution
^^^^^^^^^

To ensure that Observability Cloud works correctly, look through your AWS IAM policy to verify that it includes the permissions needed for the metrics or other data collection that you intend.

Once integrated with your Amazon Web Services account, Splunk Observability Cloud can gather CloudWatch metrics, CloudWatch logs, CloudWatch Metric Streams, service logs stored in Amazon S3 buckets, and service tag and property information. But leveraging the full power of the integration requires all included permissions.


Metrics for a particular namespace are not displayed
=====================================================

Metrics for a particular namespace are not displayed as expected.

Cause
^^^^^^

If you modified the default IAM policy while setting up an integration between Splunk Observability Cloud and AWS, then your IAM policy does not list namespaces that were removed as not needed for the original integration, and Observability Cloud ignores metrics for those namespaces.

Solution
^^^^^^^^^

To ensure that you can see the metrics you expect to monitor, perform the following steps:

1. Review the default IAM policy shown in :ref:`Connect to AWS using the Splunk Observability Cloud API <get-configAPI>` to find the entry for the namespace you want.

2. Add the missing entry to your AWS IAM file. For more information, search for "Editing IAM policies" in the AWS Identity and Access Management documentation.
