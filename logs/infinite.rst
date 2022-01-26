.. _logs-infinite:

*****************************************************************
Archive your logs with Infinite Logging rules
*****************************************************************

.. meta created 2021-04-28
.. meta DOCS-2247

.. meta::
  :description: Manage the logs pipeline with Infinite Logging rules.

.. note:: Only customers with a Splunk Log Observer entitlement in Splunk Observability Cloud can create Infinite Logging rules. If you do not have a Log Observer entitlement and are using Splunk Log Observer Connect instead, see :ref:`logs-intro-logconnect` to learn what you can do with the Splunk Enterprise integration.

Create Infinite Logging rules to archive all or any subset of logs in Amazon S3 buckets for compliance or possible future use while not paying to index them unless and until you want to analyze them in Splunk Log Observer. Storing all logs in S3 buckets with a retention time that you control helps you meet compliance and audit requirements.

Some logs may not be useful on a day-to-day basis but may still be important in case of a future incident. For example, you might want to exclude logs from a non-production environment or debug logs when indexing. In either case, you can create an Infinite Logging rule to archive those logs in S3 buckets that your team owns in AWS. 

If you want to analyze a sample of your archived logs in Log Observer, you can set the sampling rate in your Infinite Logging rule so that you pay for only the logs that you index and analyze in Log Observer. Archiving logs in S3 buckets without indexing them in Log Observer does not impact your Log Observer billing.

Prerequisites
================================================================================
You must be a Splunk Observability Cloud admin to create new Infinite Logging connections. Non-admins can send data to S3 buckets using an existing Infinite Logging connection, but they cannot create new connections. See AWS documentation for permissions required to create S3 buckets in the AWS Management Console.

Create an Infinite Logging rule
================================================================================

To create an Infinite Logging rule, follow these steps:

1. From the navigation menu, go to :guilabel:`Organization Settings > Logs Pipeline Management`.

2. Click :guilabel:`New Infinite Logging Rule`.

3. Decide where to archive your data. To send your logs to an existing S3 bucket, click the Infinite Logging connection you want, then skip to step 9.

4. If you want to send your data to a new S3 bucket and you are an Observability Cloud admin, click :guilabel:`Create new connection`. The :guilabel:`Establish a New S3 Connection` wizard appears.

5. On the :guilabel:`Choose an AWS Region and Authentication Type` tab, do the following:

   a. Select the AWS region you want to connect to. 
   b. Select whether you want to use the :guilabel:`External ID` or :guilabel:`Security Token` authentication type.
   c. Click :guilabel:`Next`.
   
6. On the :guilabel:`Prepare AWS Account` tab, follow the steps in the wizard to do the following in the AWS Management Console:

   a. Create an AWS policy. The wizard provides the exact policy you must copy and paste into AWS.
   b. Create a role and associate it with the AWS policy.
   c. Create and configure an S3 bucket.

7. On the :guilabel:`Establish Connection` tab, do the following:

   a. Give your new S3 connection a name.
   b. Paste the Role ARN from the AWS Management Console into the :guilabel:`Role ARN` field in the wizard.
   c. Give your S3 bucket a name.
   d. Click :guilabel:`Save`.

8. Choose the Amazon S3 Infinite Logging connection that you created on the first page of the wizard. Your data will go to your S3 bucket in a file that you configure in the following two steps.

9. (Optional) You can add a file prefix, which will be prepended to the front of the file you send to your S3 bucket.

10. (Optional) In :guilabel:`Advanced Configuration Options`, you can select the compression and file formats of the file you will send to your S3 bucket. 

11. Click :guilabel:`Next`.

12. On the :strong:`Filter Data` page, create a filter that matches the log lines you want to archive in your S3 bucket. Only logs matching the filter are archived. If you want to index a sample of the logs being sent to the archive, select a percentage in :guilabel:`Define indexing behavior`. Indexing a small percentage of logs in Log Observer allows you to see trends in logs that are stored in S3 buckets. Click :guilabel:`Next`.

13. Add a name and description for your Infinite Logging rule.

14. Review your configuration choices, then click :guilabel:`Save`.

Your Infinite Logging setup is now complete. Depending on your selections, your logs will be archived, indexed in Observability Cloud for analysis, or both.

