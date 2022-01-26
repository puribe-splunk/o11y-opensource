.. _infrastructure-aws:

**********************************
Monitor Amazon Web Services
**********************************

.. meta::
   :description: The Splunk Infrastructure Monitoring AWS integration imports AWS metrics, metadata, and logs from AWS CloudWatch. This information helps you monitor your AWS resources and the applications that are using those resources.

.. note:: To monitor AWS resources, start by setting up the Infrastructure Monitoring AWS integration. To learn more see AWS :ref:`get-started-aws`.

The Infrastructure Monitoring Amazon Web Services (AWS) integration imports metrics and metadata from AWS CloudWatch. Metrics are data points
identified by a name; metadata is information that helps you identify aspects of the metrics such as its source.
AWS metrics and metadata help you monitor and troubleshoot the AWS services you're using, such as AWS EC2. The metrics
and metadata also help you monitor applications, such as Kubernetes clusters, that use the AWS services.

Infrastructure Monitoring also receives logs from AWS CloudWatch and other AWS services and applications.

You can also use the Splunk Distribution of OpenTelemetry Collector (OTel) to import AWS metrics and metadata. OTel offers much
more customization than you have available with the AWS integration, so you might want to use OTel
when you want to see metrics at a finer resolution or when you need more control over the metrics you import.

.. note:: You can only use the Smart Agent when you have direct control over the applications installed on an AWS instance. For example, you can use OTel Agent with AWS Elastic Compute Cloud (EC2). Some other AWS services require you to use Infrastructure Monitoring AWS integration and AWS CloudWatch. As a result, you might need to use both the AWS integration and OTel.

To learn how to install OTel, see :ref:`opentelemetry-resources`.

Importing AWS CloudWatch data and metadata
================================================================================

Infrastructure Monitoring queries AWS CloudWatch to import metrics and metadata. During this import, Infrastructure Monitoring gives
the metrics special names so you can identify them as coming from AWS. In Infrastructure Monitoring, AWS metadata becomes
dimensions and custom properties. AWS tags are key-value pairs, so Infrastructure Monitoring converts them to custom properties.

Importing metrics and metadata from AWS into Infrastructure Monitoring is also known as downloading.

You can also view your AWS logs in Infrastructure Monitoring.

* To learn more about the metrics that Infrastructure Monitoring gets from AWS CloudWatch, see :ref:`aws-oc-metrics` and refer to the AWS documentation site.
* To learn more about logs and AWS, see :ref:`get-started-logs`.

Importing data and metadata from applications
--------------------------------------------------------------------------------

Infrastructure Monitoring also imports metrics, metadata, and logs for some of your applications that use AWS services. The
following table lists these applications.

.. list-table::
   :header-rows: 1
   :widths: 30, 20, 50

   * - :strong:`Get data in`
     - :strong:`Monitor`
     - :strong:`Description`

   * - :ref:`get-started-k8s`
     - :ref:`infrastructure-k8s`
     - Import metrics and logs from Kubernetes clusters running in EC2 instances or EKS.

   * - - :ref:`get-started-linux`
       - :ref:`get-started-windows`
     - :ref:`infrastructure-hosts`
     - Import metrics and logs from Linux and Windows hosts running in EC2 instances.

   * - :ref:`get-started-application`
     - :ref:`get-started-apm`
     - Import application metrics and spans running in hosts, Kubernetes clusters, or Lambda functions.

.. _specify-data-metadata:

Specifying data and metadata to import
=============================================================================

The AWS integration imports metrics from a list of supported AWS services in all built-in AWS namespaces.
To limit the amount of AWS data that the integration imports, specify a subset of built-in namespaces
from which you need data. For each namespace, you can then filter the data based on AWS tags or metric names or both.

Refer to the section :ref:`supported-aws-services` to see the list of AWS services from which the AWS integration imports data.

You can also limit the amount of AWS data that the integration imports by changing the rate at which
Infrastructure Monitoring polls AWS CloudWatch.

.. note:: You must be an administrator of your AWS account to choose namespaces and set filters.

* To select the built-in namespaces for which you want data, click :guilabel:`Select namespaces`,
  then choose the namespaces.

* Infrastructure Monitoring also lets you import data from custom namespaces. To specify a custom namespace from
  which you want data, click :guilabel:`Add custom namespaces`, type the name of the custom namespace, then
  press :guilabel:`Enter`. Using this procedure, you can specify multiple custom namespaces.

Specifying filters for AWS data you want to import doesn't affect tag syncing.

Infrastructure Monitoring syncs tags and properties from several AWS services. To see a list of these services,
see :ref:`synced_tags_properties`

Example: Specify namespaces and filters
--------------------------------------------------------------------------------

The following example demonstrates how to specify the following:

* Namespace: Only import data from Amazon ElasticSearch Service and EC2
* Data filters: Only import data from EC2 if it matches a filter
* Tag filters: Exclude data from resources that have the AWS tag ``version:canary``

To create these specifications, perform the following steps:

#. From the list of namespaces, select Amazon ElasticSearch Service and EC2.
#. To limit the data Infrastructure Monitoring imports from EC2, click the drop-down arrow to see the data filters.
#. To select the filters you want from the following options:

   * Use :guilabel:`Import only` if you want to specify a filter for the data to import.
   * Use :guilabel:`Don't import` if you want to specify a filter for the data to exclude.

#. To use AWS tags to limit the data Infrastructure Monitoring imports, filter by tag. For this example, specify a filter
   that excludes data from resources that have the AWS tag ``version:canary``.

Infrastructure Monitoring adds the prefix ``aws_tag_`` to the names of tags importd from AWS, which indicates their origin.
For example, an AWS tag ``version:canary`` appears in Infrastructure Monitoring as
``aws_tag_version:canary``. When you filter an AWS integration by tag, enter the name of the tag as
it appears in AWS.

You can also choose specific metrics to include or exclude. For example, consider the following conditions.

.. image:: /_images/infrastructure/aws-metric-tag.png
   :width: 55%

Only metricA and metricB are included, and only for resources specified by the tags:

-  For a resource that has the tag ``env:prod`` or ``env:beta``, metricA and metricB are included.
-  For a resource that doesn't have the tags ``env:prod`` or ``env:beta``, no metrics are included.
-  No other metrics are included.

Infrastructure Monitoring supports wildcards in filters.
For example, if you want to import data for a resource that has specific tags, regardless of the tag values, specify this
filter:

.. image:: /_images/infrastructure/aws-metric-tag-wildcard.png
   :width: 55%

In this example, metricA and metricB are included for resources that have the ``env`` tag set to any value.
No other metrics are included.

You can use the :guilabel:`Actions` menu next to a namespace name to copy or paste filters from one namespace to another,
clear the filters for the namespace, or remove the namespace from the list of namespaces to include.
When you remove a namespace, Infrastructure Monitoring no longer includes metrics from that namespace.


When you finish specifying the namespaces, metrics, and tags to include or exclude, click :guilabel:`Save`.


.. _api-filters:

.. note:: You can specify more complex filtering options for a namespace by using the Infrastructure Monitoring API.
   In this case, the UI displays a message indicating that the filter is defined programmatically.
   To see which metrics and tags are included or excluded for that namespace, click :guilabel:`View filter code`.

.. _cloudwatch-metric-sync:

Import specific AWS CloudWatch metric sources
=============================================================================

To import some AWS CloudWatch metrics, you need to configure AWS CloudWatch as well as Infrastructure Monitoring.

.. _s3:

Receiving S3 metrics
-------------------------------------------------------------------

For S3, Infrastructure Monitoring defaults to receiving the daily storage metrics listed on the Amazon S3 console page.
Amazon bills you separately for the request metrics shown on that page, so
you must explicitly select to import them. To learn more about selecting them, see the AWS S3 documentation.

Infrastructure Monitoring also imports metadata for AWS S3. To learn more, see :ref:`s3-metadata`.

.. _cloudwatch-agent:

Receiving metrics via the Cloudwatch agent
-------------------------------------------------------------------

AWS provides a CloudWatch agent that lets you import more system-level metrics from Amazon EC2
instances and also lets you collect system-level metrics from on-premises servers. To import these
metrics in Infrastructure Monitoring, add the namespace you use for the AWS CloudWatch agent as a custom namespace
in your AWS integration, as described in the section :ref:`specify-data-metadata`).

To learn more about the AWS CloudWatch agent, see the AWS documentation.

.. _cloudwatch-metadata-sync:


.. _monitor-aws-services:

Monitor AWS services and identify problems
=====================================================

Visit the Infrastructure page to monitor the health of the AWS services you're using.
This page provides a key metric for each service. You can also drill down into specific instances of an AWS service.
For example, start by viewing the key metrics for your EC2 service, and then filter for a specific instance ID to analyze the
EC2 instance with that ID.

Follow these steps to find and troubleshoot AWS services from the Infrastructure page:

#. Select :menuselection:`Navigation menu > Infrastructure`, then click :guilabel:`Amazon AWS` category.
#. Select the specific service you want to analyze. For example, click :guilabel:`EBS` to view
   information about your storage volumes. If you see the message :guilabel:`No Data Found`, you first need to configure the
   integration for the service.
#. Compare instances of the services to investigate their relative health. Select a metric from the :strong:`Color by` drop-down list.
   In the heat map, colors indicate the health of each instance based on the selected metric.
   For example, consider an AWS EBS heat map for the total number of I/O operations in a time period (Total IOPS). The heat map displays
   high Total IOPS in lighter colors, which indicates that the instances are healthy. In comparison, the heat map displays
   low IOPS in a darker color, which indicates that the instances have a I/O-related problem.

   If the heat map only uses green and red, then green indicates a healthy instance and red indicates a problem.

   To apply visually-accessible color palettes to heat maps, select :menuselection:`<USER-ID> > Account Settings`,
   then select your desired color accessibility from the :guilabel:`Color Accessibility` menu.

#. Investigate correlations between instances and their health by grouping the instances
   based on a dimension, custom property, or tag. To group instances, select the metadata name from the :guilabel:`Group by`
   drop-down list.

   .. note:: In the DynamoDB navigator, when you view the heatmap and group the instances by ``aws_account_id``, some entries might report back as "n/a" because properties are omitted when the query is not specific enough. To work around this issue, filter by :strong:`Operation`, then group by ``aws_account_id``.

#. Outliers are another indication of instance health. An outlier is a metric value that is significantly outside the
   mean or median value of all other metric values. To find the outliers in metrics coming from AWS services,
   use the :strong:`Find Outliers` setting and specify the :strong:`Scope` and :strong:`Strategy`:

    You can select one of two :strong:`Strategies` to find outliers, as described in the following table.

    .. list-table::
       :header-rows: 1
       :widths: 30, 70

       * - :strong:`Strategy`
         - :strong:`Description`

       * - ``Deviation from Mean``
         - Instances shown in red are ones that exceed the mean value of the metric by at least three standard deviations.
       * - ``Deviation from Median``
         - Instances shown in red are ones that exceed the median absolute deviation value by at least three absolute deviations.
           Deviation from Median This setting does not weigh extreme outliers as heavily as the standard deviation.

#. To drill down to a specific instance you want to investigate, hover over the heatmap to find the specific instance ID,
   then click the cell to see the information for that ID. For every instance, Infrastructure Monitoring provides a default dashboard.

   The default dashboard helps you analyze all the available metadata about the cloud service the instance is running in,
   the instance itself, and any custom tags associated with the instance. The default dashboard provides
   metric time series (MTS) for key metrics.

Use default dashboards to monitor AWS services
==============================================

Observability Cloud provides default dashboards for supported AWS services. Default dashboards are available in dashboard groups based on the AWS service a dashboard represents data for.

To find default dashboards for AWS services, select :strong:`Navigation menu > Dashboards` and search for the AWS service you want to view dashboards for.

.. _supported-aws-services:

Explore built-in content
========================
Observability Cloud collects data from many cloud services. To see all of the navigators provided for data collected in your organization, go to the Infrastructure page. To see all the pre-built dashboards for data collected in your organization, select :strong:`Dashboards > Built-in`.

.. note::

  Amazon EC2 instances are powered by their respective public cloud service as well as Splunk OpenTelemetry Collector. You need both for all the charts to display data in the built-in dashboards.

  - If you have only the public cloud service and the Smart Agent configured, some charts in the built-in dashboards for Amazon EC2 instances display no data.
  - If you have only the public cloud service configured, you can see all the cards representing the services where data come from, but some charts in the built-in dashboards for Amazon EC2 instances display no data.
  - If you have only Smart Agent configured, Amazon EC2 instance navigator isn't available.

..

Supported AWS services
============================================================================

Infrastructure Monitoring imports data and metadata for these AWS services:

.. hlist::
   :columns: 2

   - Amazon API Gateway
   - AppStream 2.0
   - Amazon Athena
   - Auto Scaling
   - AWS Billing
   - ACM Private CA
   - Amazon CloudFront
   - AWS CloudHSM
   - Amazon CloudSearch
   - Amazon CloudWatch Events
   - Amazon CloudWatch Logs
   - AWS CodeBuild
   - Amazon Cognito
   - Amazon Connect
   - AWS Database Migration Service
   - AWS Direct Connect
   - Amazon DocumentDB
   - Amazon DynamoDB
   - Amazon EC2
   - Amazon EC2 (Spot Instances)
   - Amazon EC2 Container Service (ECS)
   - AWS Elastic Beanstalk
   - Amazon Elastic Interface
   - Amazon Elastic Block Store
   - Amazon Elastic File System
   - Elastic Load Balancing (ELB): Classic Load Balancers
   - Elastic Load Balancing (ELB): Application Load Balancers
   - Elastic Load Balancing (ELB): Network Load Balancer
   - Amazon Elastic Transcoder
   - Amazon ElastiCache
   - Amazon Elasticsearch Service
   - Amazon Elastic MapReduce (EMR)
   - Amazon FSx for Lustre or Windows File Server
   - Amazon GameLift
   - AWS Glue
   - Amazon Inspector
   - AWS IoT
   - AWS IoT Analytics
   - Amazon Managed Streaming for Kafka
   - AWS Key Management Service
   - Amazon Kinesis Analytics
   - Amazon Kinesis Firehose
   - Amazon Kinesis Streams
   - Amazon Kinesis Video Streams
   - AWS Lambda
   - Amazon Lex
   - AWS Elemental MediaConnect
   - AWS Elemental MediaConvert
   - AWS Elemental MediaPackage
   - AWS Elemental MediaTailor
   - Amazon Machine Learning
   - Amazon Managed Message Broker (MQ)
   - AWS OpsWorks
   - Amazon Polly
   - Amazon Redshift
   - Amazon Relational Database Service
   - AWS RoboMaker
   - Amazon Route 53
   - Amazon SageMaker
   - Amazon SageMaker Training Jobs
   - Amazon SageMaker Transform Jobs
   - Amazon SageMaker Endpoints
   - AWS SDK Metrics for Enterprise Support
   - AWS Shield Advanced
   - Amazon Simple Email Service
   - Amazon Simple Notification Service
   - Amazon Simple Queue Service
   - Amazon Simple Storage Service
   - Amazon Simple Workflow Service
   - AWS Step Functions
   - AWS Storage Gateway
   - Amazon Textract
   - AWS IoT Things Graph
   - Amazon Translate
   - AWS Trusted Advisor
   - Amazon VPC (NAT gateway)
   - Amazon VPC VPN
   - AWS Web Application Firewall (WAF)
   - Amazon WorkMail
   - Amazon WorkSpaces
   - Amazon Neptune
   - Amazon MediaLive
   - Amazon CloudWatch agent

.. _synced_tags_properties:

Synced tags and properties
============================================================================

Infrastructure Monitoring syncs tags and properties for the following AWS services:

.. hlist::
   :columns: 2

   - Amazon Api Gateway
   - AWS Elastic Load Balancing (ELB): Application Load Balancers
   - AWS Auto Scaling
   - Amazon CloudFront
   - Amazon DynamoDB
   - Amazon Elastic Block Store (EBS)
   - Amazon EC2
   - Amazon EC2 Container Service (ECS)
   - AWS Elastic Load Balancing ELB: Classic Load Balancers
   - Amazon Elasticsearch Service
   - Amazon ElastiCache
   - AWS Elastic Beanstalk
   - Amazon Elastic MapReduce
   - Amazon Kinesis Analytics
   - AWS Lambda
   - AWS Elastic Load Balancing (ELB): Network Load Balancer
   - Amazon Relational Database Service (RDS)
   - Amazon Redshift
   - Amazon Route 53
   - Amazon Simple Storage Service (S3)
   - Amazon Simple Queue Service (SQS)
   - Amazon VPC VPN (VPN)

.. _aws-oc-metrics:

Import AWS CloudWatch metadata
=============================================================================

Infrastructure Monitoring automatically imports AWS metadata for imported AWS CloudWatch metrics.
This metadata might take up to 15 minutes to arrive.

You can filter AWS data using AWS tags, but only with namespaces for which Infrastructure Monitoring syncs tags.
For more information, see :ref:`aws-namespaces`.

For example, if you use Detailed Monitoring for EC2 instances in AWS, Infrastructure Monitoring imports the following
dimensions:

* ``AutoScalingGroupName``
* ``ImageId``
* ``InstanceId``
* ``InstanceType``.

.. note:: Unsupported characters within a dimension key are converted to underscores.

Filtering using AWS CloudWatch metadata
--------------------------------------------------------------------------------

You can use the following AWS metadata to filter metrics:

.. list-table::
   :header-rows: 1
   :widths: 25 25 50

   * - :strong:`Custom Property`
     - :strong:`Form`
     - :strong:`Description`

   * - aws_account_id
     - key-value pair
     - AWS account ID for the instance, volume or load balancer. Use this property
       to differentiate between metrics you import.

   * - aws_tag_<TAGNAME>
     - key and optional value
     - AWS custom tag name for the instance, volume or load balancer. A metric may have
       more than one associated custom tag name.

Use aws_account_id to differentiate between metrics you import from multiple AWS accounts.
Infrastructure Monitoring adds aws_account_id as a dimension of the MTS for the metric.

For supported AWS services, Infrastructure Monitoring imports AWS tags and adds them as
custom properties to the MTS for the metric. For example, if AWS tag has the value named Production,
it will be shown in Infrastructure Monitoring as aws_tag_Production.

Metadata available from AWS CloudWatch
--------------------------------------------------------------------------------


Infrastructure Monitoring imports metadata for supported AWS services. Click a link
to see more information.


-  :ref:`apigateway-metadata`
-  :ref:`autoscaling-metadata`
-  :ref:`cloudfront-metadata`
-  :ref:`dynamodb-metadata`
-  :ref:`cloudwatch-ebs-metadata`
-  :ref:`cloudwatch-ec2-metadata`
-  :ref:`cloudwatch-ec2-optimization-data`
-  :ref:`cloudwatch-ecs-metadata`
-  :ref:`cloudwatch-elb-metadata`
-  :ref:`elasticache-metadata`
-  :ref:`elasticsearch-metadata`
-  :ref:`emr-metadata`
-  :ref:`kinesis-metadata`
-  :ref:`lambda-metadata`
-  :ref:`cloudwatch-rds-metadata`
-  :ref:`redshift-metadata`
-  :ref:`sqs-metadata`
-  :ref:`s3-metadata`


.. _apigateway-metadata:

API Gateway metadata
-------------------------------------------------------------------

For API Gateway, Infrastructure Monitoring imports the names and tags of every REST API and stage.
For more information, see the AWS documentation for API Gateway.

..  list-table::
    :header-rows: 1
    :widths: 30 30 60

    *  - :strong:`API Gateway Name`
       - :strong:`Custom Property`
       - :strong:`Description`

    *  - ApiName
       - aws_rest_api_name
       - The API's name

    *  - Stage
       - aws_stage_name
       - The first path segment in the Uniform Resource Identifier (URI) of a call to API Gateway


.. _autoscaling-metadata:

Auto Scaling metadata
-------------------------------------------------------------------

For Auto Scaling, Infrastructure Monitoring imports properties of every group as well as all the tags set on the group.
For more information, see the AWS documentation for Auto Scaling.

.. list-table::
    :header-rows: 1
    :widths: 30 30 60

    *  - :strong:`Auto Scaling Name`
       - :strong:`Custom Property`
       - :strong:`Description`

    *  - CreatedTime
       - aws_created_time
       - Time the resource was created at (e.g. ``Thu Apr 13 15:59:25 UTC 2017``)

    *  - DefaultCoolDown
       - aws_default_cool_down
       - Amount of time, in seconds, after a scaling activity completes before another scaling activity can start

    *  - HealthCheckGracePeriod
       - aws_health_check_grace_period
       - Amount of time, in seconds, that Auto Scaling waits before checking the health status of an EC2 instance that has come into service

    *  - HealthCheckType
       - aws_health_check_type
       - Service to use for the health checks

    *  - LaunchConfigurationName
       - aws_launch_configuration_name
       - Name of the associated launch configuration

    *  - NewInstancesProtectedFromScaleIn
       - aws_new_instances_protected_from_scale_in
       - Indicates whether newly launched instances are protected from termination by Auto Scaling when scaling in

    *  - PlacementGroup
       - aws_placement_group
       - The name of the placement group into which you'll launch your instances, if any

    *  - ServiceLinkedRoleARN
       - aws_service_linked_role_arn
       - ARN of the service-linked role that the Auto Scaling group uses to call other Amazon services on your behalf

    *  - Stats
       - aws_status
       - Current state of the group when DeleteAuto ScalingGroup is in progress

    *  - VPCZoneIdentifier
       - aws_vpc_zone_identifier
       - One or more subnet IDs, if applicable, separated by commas

    *  - Region
       - aws_region
       - AWS Region to which the Auto Scaling group belongs


.. _cloudfront-metadata:

CloudFront metadata
-------------------------------------------------------------------

For CloudFront, Infrastructure Monitoring scans every distribution for your AWS account and imports the properties of each
distribution and all the tags set on the distribution.
For more information on these properties, including acceptable values and constraints,
see the AWS documentation for AWS CloudFront.

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   *  -  :strong:`CloudFront Name`
      -  :strong:`Custom Property`
      -  :strong:`Description`

   *  -  Id
      -  aws_distribution_id
      -  The identifier for the distribution, for example ``EDFDVBD632BHDS5``.

   *  -  DomainName
      -  aws_domain_name
      -  The domain name corresponding to the distribution, for example ``d111111abcdef8.cloudfront.net``.

.. _dynamodb-metadata:

DynamoDB metadata
-------------------------------------------------------------------

For DynamoDB, Infrastructure Monitoring scans every table in your AWS account and imports properties of the table
and any tags set for the table. For more information on these properties, including acceptable values and constraints,
see the AWS documentation for DynamoDB.

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   *  -  :strong:`DynamoDB Name`
      -  :strong:`Custom Property`
      -  :strong:`Description`

   *  -  ProvisionedThroughputDescription.ReadCapacityUnits
      -  aws_read_capacity_units
      -  Maximum number of strongly consistent reads consumed per second before DynamoDB returns a ThrottlingException

   *  -  ProvisionedThroughputDescription.WriteCapacityUnits
      -  aws_write_capacity_units
      -  Maximum number of writes consumed per second before DynamoDB returns a ThrottlingException

   *  -  TableName
      -  aws_table_name
      -  Name of the DynamoDB table

   *  -  TableStatus
      -  aws_table_status
      -  Current state of the table



.. _elasticsearch-metadata:

Elasticsearch metadata
-------------------------------------------------------------------

For Elasticsearch, Infrastructure Monitoring scans every domain from your AWS account and imports
the version and any tags set on the domain.
For more information, see the documentation for AWS Elasticsearch

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   *  -  :strong:`Elasticsearch Name`
      -  :strong:`Custom Property`
      -  :strong:`Description`

   *  -  ElasticsearchVersion
      -  aws_es_version
      -  The Elasticsearch version, for example ``7.1``.



.. _cloudwatch-ebs-metadata:

EBS metadata
-------------------------------------------------------------------
For EBS, Infrastructure Monitoring scans every volume ID from your AWS account and imports
properties of the volume and any tags set on the volume.
For more information on these properties, including acceptable values and constraints, see
the AWS documentation for EBS.

.. list-table::
   :header-rows: 1
   :widths: 20 20 60

   * - :strong:`EBS Name`
     - :strong:`Custom Property`
     - :strong:`Description`

   * - attachment_state
     - aws_attachment_state
     - The attachment state of the volume

   * - availability-zone
     - aws_availability_zone
     - The Availability Zone in which the volume was created

   * - create-time
     - aws_create_time
     - The time stamp when the volume was created

   * - delete_on_termination
     - aws_delete_on_termination
     - Whether or not a volume will be deleted if the instance it is attached to is terminated

   * - encrypted
     - aws_encrypted
     - The encryption status of the volume

   * - instance_id
     - aws_instance_id
     - ID of the instance to which the volume is attached. This property will be propagated only if the volume is attached to an instance

   * - iops
     - aws_iops
     - The number of I/O operations per second (IOPS) that the volume supports

   * - kms_key_id
     - aws_kms_key_id
     - The full ARN of the AWS customer master key used to protect the volume encryption key for the volume

   * - size
     - aws_size
     - The size of the volume, in GiB

   * - snapshot_id
     - aws_snapshot_id
     - The snapshot from which the volume was created

   * - state
     - aws_state
     - The status of the volume

   * - volume_id
     - aws_volume_id
     - The volume ID

   * - volume_type
     - aws_volume_type
     - The Amazon EBS volume type

.. _cloudwatch-ec2-metadata:

EC2 metadata
-------------------------------------------------------------------
For EC2, Infrastructure Monitoring scans every instance ID in your AWS account and imports properties of the
instance and any tags set on the instance. Any property named "Host" or "InstanceId" in Infrastructure Monitoring
that has the value of the instance ID, private DNS name, or private IP address
now gets the same tags and properties of the instance ID.
Each instance property is prefixed with aws\_. For more information on these properties,
including acceptable values and constraints, see the Amazon documentation for EC2 metadata

.. list-table::
   :header-rows: 1
   :widths: 25 25 50

   *  -  :strong:`EC2 Name`
      -  :strong:`Custom Property`
      -  :strong:`Description`

   *  - architecture
      - aws_architecture
      - Instance architecture (i386 or x86_64)

   *  - availability-zone
      - aws_availability_zone
      - The availability zone of the instance

   *  - dns-name
      - aws_public_dns_name
      - Public DNS name of the instance

   *  - hypervisor
      - aws_hypervisor
      - Hypervisor type of the instance (ovm or xen)

   *  - image-id
      - aws_image_id
      - ID of the image used to launch the instance

   *  - instance-id
      - aws_instance_id
      - ID of the instance

   *  - instance-state-name
      - aws_state
      - An object defining the state code and name of the instance

   *  - instance-type
      - aws_instance_type
      - Type of the instance

   *  - ip-address
      - aws_public_ip_address
      - The address of the Elastic IP address bound to the network interface

   *  - kernel-id
      - aws_kernel_id
      - Kernel ID

   *  - launch-time
      - aws_launch_time
      - The time when the instance was launched

   *  - private-dns-name
      - aws_private_dns_name
      - Private DNS name of the instance

   *  - reason
      - aws_state_reason
      - The state reason for the instance (if provided)

   *  - region
      - aws_region
      - The region in which the instance is running

   *  - reservation-id
      - aws_reservation_id
      - ID of the instance's reservation

   *  - root-device-type
      - aws_root_device_type
      - Type of root device that the instance uses


.. _cloudwatch-ec2-optimization-data:

EC2 data for AWS Optimizer
-------------------------------------------------------------------

Infrastructure Monitoring AWS Optimizer helps you find cost-saving opportunities and underutilized investments in EC2.
AWS Optimizer shows you usage patterns and cost attribution by InstanceType, AWS Region, and AWS Availability Zone.
AWS Optimizer also shows you categories specific to your setup, such as Service, Team, and all other dimensions that
come from EC2 instance tags.

AWS Optimizer generates metrics from usage and cost data imported by calls to the AWS API.
These generated metrics let you visualize and analyze EC2 usage and costs, as shown in built-in dashboards.
You can also create detectors based on AWS Optimizer metrics. These detectors send real-time alerts for
unexpected changes in cost or usage patterns.

* To learn more about visualizing and analyzing the metrics, see :new-page-ref:`built-in dashboards <built-in>`.
* To learn more about creating detectors, see :new-page:`Set Up Detectors to Trigger Alerts <https://quickdraw.splunk.com/redirect/?product=Observability&location=userdocs.infrastructure.aws.detectors.create&version=current>`.

To import the usage and cost data to be imported, make sure the following lines are in your AWS Policy Document.
To learn how to view and modify your AWS Policy Document, see :ref:`get-started-aws`):

.. code-block:: none

   "ec2:DescribeInstances",
   "ec2:DescribeInstanceStatus",
   "ec2:DescribeTags",
   "ec2:DescribeReservedInstances",
   "ec2:DescribeReservedInstancesModifications",
   "organizations:DescribeOrganization",

.. admonition:: Notes on using AWS Optimizer

  -   AWS Optimizer is only available in the Splunk Observability Cloud Enterprise Edition.
  -   The imported data does not include AWS billing data.
  -   Infrastructure Monitoring doesn't import data or generate metrics for EC2 Spot Instances.
  -   If you have multiple AWS accounts, you need to add a Infrastructure Monitoring AWS integration for each account,
      and each integration must have "Import data for AWS Optimizer" selected.
      If you don't set this option, your generated metrics may not contain accurate values.

.. _cloudwatch-ecs-metadata:

Elastic Container Service (ECS) metadata
-------------------------------------------------------------------

For ECS, Infrastructure Monitoring scans every cluster and service for your AWS account
and imports their properties as well as any tags set on the cluster or service.
For more information, see the AWS documentation for ECS

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   * - :strong:`ECS Name`
     - :strong:`Custom Property`
     - :strong:`Description`

   * - ClusterName
     - aws_cluster_name
     - A user-generated string that you use to identify your cluster.

   * - ServiceName
     - aws_service_name
     - The name of your service.


.. _cloudwatch-elb-metadata:

Classic, Application, and Network ELB metadata
-------------------------------------------------------------------
For ELB, Infrastructure Monitoring scans every load balancer name for your AWS account and
imports properties of the load balancer and any tags set on the load balancer.
For more information on these properties, including acceptable values and constraints,
see the AWS Documentation for ELB.

.. list-table::
   :header-rows: 1
   :widths: 20 20 60

   * - :strong:`ELB Name`
     - :strong:`Custom Property`
     - :strong:`Description`

   * - create-time
     - aws_create_time
     - The time stamp when the load balancer was created


.. _elasticache-metadata:


ElastiCache metadata
-------------------------------------------------------------------

For ElastiCache, Infrastructure Monitoring scans every cluster and node for your AWS account and
imports their properties as well as any tags set on the cluster or node.
For more information about these properties, including acceptable values and constraints,
see the following AWS documentation:

* AWS CacheCluster documentation
* AWS CacheNode documentation

.. list-table::
   :header-rows: 1

   *  -  :strong:`ElastiCache Name`
      -  :strong:`Custom Property`
      -  :strong:`Description`
      -  :strong:`Applies to`

   *  -  ReplicationGroupId
      -  aws_replication_group_id
      -  The replication group to which this cluster belongs. If this field is empty, the cluster is not associated with any
         replication group.
      -  Cluster metrics that are part of a replication group

   *  -  CacheClusterCreateTime
      -  aws_cache_cluster_create_time
      -  The date and time when the cluster was created
      -  Cluster and node

   *  -  Engine
      -  aws_engine
      -  The name of the cache engine used by this cluster
      -  Cluster and node

   *  -  EngineVersion
      -  aws_engine_version
      -  The version of the cache engine by this cluster
      -  Cluster and node

   *  -  CustomerAvailabilityZone
      -  aws_availability_zone
      -  The AWS Availability Zone where this node was created and now resides
      -  Node only

   *  -  CacheNodeCreateTime
      -  aws_cache_node_create_time
      -  The date and time when the cache node was created
      -  Node only

   *  -  n/a
      -  aws_cache_cluster_name
      -  Either the value of ``aws_replication_group_id`` (if applicable) or the value of the dimension ``CacheClusterId``
      -  Cluster and node


CacheClusterId is a dimension that is already in ElastiCache MTS that Infrastructure Monitoring imports from AWS Cloudwatch.

.. _emr-metadata:

EMR metadata
-------------------------------------------------------------------

For EMR, Infrastructure Monitoring scans the properties of every cluster as well as any tags set on each cluster.
For more information on these properties, including acceptable values and constraints,
see the AWS documentation for the DescribeCluster API.

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   * - :strong:`EMR Name`
     - :strong:`Custom Property`
     - :strong:`Description`

   *  - Id
      - aws_cluster_id
      - AWS identifier of the cluster

   *  - Name
      - aws_cluster_name
      - The name you gave the cluster

   *  - AutoScalingRole
      - aws_auto_scaling_role
      - An IAM role for automatic scaling policies

   *  - CustomAmiId
      - aws_custom_ami_id
      - The ID of a custom Amazon EBS-backed Linux AMI if the cluster uses a custom AMI

   *  - InstanceCollectionType
      - aws_instance_collection_type
      - The instance group configuration of the cluster

   *  - LogUri
      - aws_log_uri
      - The path to the Amazon S3 location where logs for this cluster are stored

   *  - MasterPublicDnsName
      - aws_master_public_dns_name
      - The DNS name of the master node

   *  - ReleaseLabel
      - aws_release_label
      - The Amazon EMR release label, which determines the version of open-source application packages installed on the cluster

   *  - RepoUpgradeOnBoot
      - aws_repo_upgrade_on_boot
      - Applies only when CustomAmiID is used

   *  - RequestedAmiVersion
      - aws_requested_ami_version
      - The AMI version requested for this cluster

   *  - RunningAmiVersion
      - aws_running_ami_version
      - The AMI version running on this cluster

   *  - ScaleDownBehavior
      - aws_scale_down_behavior
      - The way that individual Amazon EC2 instances terminate when an automatic scale-in
        activity occurs or an instance group is resized

   *  - SecurityConfiguration
      - aws_security_configuration
      - The name of the security configuration applied to the cluster

   *  - ServiceRole
      - aws_service_role
      - The IAM role that the Amazon EMR service uses to access AWS resources on your behalf

   *  - Status
      - aws_status
      - The current status details about the cluster

   *  - AutoTerminate
      - aws_auto_terminate
      - Specifies whether the cluster terminates after completing all steps

   *  - TerminationProtected
      - aws_termination_protected
      - Indicates whether Amazon EMR locks the cluster to prevent the EC2 instances from being terminated by an API call or
        user intervention, or in the event of a cluster error

   *  - VisibleToAllUsers
      - aws_visible_to_all_users
      - Indicates whether the cluster is visible to all IAM users of the AWS account associated with the cluster

   *  - NormalizedInstanceHours
      - aws_normalized_instance_hours
      - An approximation of the cost of the cluster, represented in m1.small/hours


.. _kinesis-metadata:

Kinesis Streams metadata
-------------------------------------------------------------------

For Kinesis Streams, Infrastructure Monitoring scans the properties of every stream as well as any tags set on each stream.
If shard-level metrics are enabled in AWS, properties and tags are also applied to Kinesis shards for their
respective parent streams. For more information, see the AWS documentation
for the StreamDescription API.

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   * - :strong:`Kinesis Name`
     - :strong:`Custom Property`
     - :strong:`Description`

   * - StreamName
     - aws_stream_name
     - The name of the stream

   * - StreamStatus
     - aws_stream_status
     - The server-side encryption type used on the stream

   * - RetentionPeriodHours
     - aws_retention_period_hours
     - The current retention period, in hours


.. _cloudwatch-rds-metadata:

RDS metadata
-------------------------------------------------------------------

For RDS, Infrastructure Monitoring scans every database instance for your AWS account and imports properties of
each instance and any tags set on each instance. For more information, including acceptable values and constraints,
see  the AWS documentation for the DBCluster API.

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   * - :strong:`RDS Name`
     - :Strong:`Custom Property`
     - :Strong:`Description`

   * - AvailabilityZone
     - aws_availability_zone
     - Name of the DB instance Availability Zone

   * - DBClusterIdentifier
     - aws_db_cluster_identifier
     - If the DB instance is a member of a DB cluster, contains the name of the DB cluster

   * - DBInstanceClass
     - aws_db_instance_class
     - Name of the compute and memory capacity class of the DB instance

   * - DBInstanceStatus
     - aws_db_instance_status
     - Current state of the DB instance

   * - Engine
     - aws_engine
     - Name of the database engine this DB instance uses

   * - EngineVersion
     - aws_engine_version
     - Database engine version.

   * - InstanceCreateTime
     - aws_instance_create_time
     - DB instance creation date and time

   * - Iops
     - aws_iops
     - New Provisioned IOPS value for the DB instance. AWS might apply this value in the future, or might
       be applying it at the moment.

   * - MultiAZ
     - aws_multi_az
     - Indicates if the DB instance is a Multi-AZ deployment

   * - PubliclyAccessible
     - aws_publicly_accessible
     - Accessibility options for the DB instance.
       `"true"` indicates an Internet-facing instance with a publicly resolvable DNS name
       that resolves to a public IP address. `"false"` indicates an internal instance with a
       DNS name that resolves to a private IP address.

   * - ReadReplicaSourceDBInstanceIdentifier
     - aws_read_replica_source_db_instance_identifier
     - If the DB instance is a Read Replica, this value is the identifier of the source DB instance.

   * - SecondaryAvailabilityZone
     - aws_second_availability_zone
     - If this property is present, and the DB instance has multi-AZ support, this value
       specifies the name of the secondary Availability Zone.

   * - StorageType
     - aws_storage_type
     - Storage type associated with the DB instance


.. _lambda-metadata:

AWS Lambda metadata
-------------------------------------------------------------------

For AWS Lambda, Infrastructure Monitoring scans every version of every function associated with your AWS
account and imports properties of the function version and any tags set on the function.
Infrastructure Monitoring also imports the ``lambda_arn`` dimension, which is the qualified ARN for an AWS Lambda function.
For more information on these properties, including acceptable values and constraints,
see the AWS Lambda documentation for API function configuration.

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   * - :strong:`AWS Lambda Filter Name`
     - :strong:`Custom Property`
     - :strong:`Description`


   * - CodeSha256
     - aws_function_code_sha256
     - SHA256 hash of your function deployment package

   * - CodeSize
     - aws_function_code_size
     - The size of the .zip file you uploaded for the function, in bytes

   * - FunctionName
     - aws_function_name
     - Function name

   * - MemorySize
     - aws_function_memory_size
     - Memory size you configured for the function, in MB

   * - Runtime
     - aws_function_runtime
     - Runtime environment for the function

   * - Timeout
     - aws_function_timeout
     - The function execution time at which AWS Lambda needs to terminate the function

   * - Version
     - aws_function_version
     - The function version

   * - VpcConfig.vpcId
     - aws_function_vpc_id
     - The VPC ID associated with your function




.. _redshift-metadata:

Redshift metadata
-------------------------------------------------------------------

For RedShift, Infrastructure Monitoring scans every cluster for your AWS account and
imports properties of the cluster and any tags set on the cluster.
For more information, including acceptable values and constraints, see
the AWS documentation for the RedShift Cluster API.

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   * - :strong:`Redshift Name`
     - :strong:`Custom Property`
     - :strong:`Description`

   * - ClusterIdentifier
     - aws_cluster_identifier
     - The unique identifier of the cluster

   * - AvailabilityZone
     - aws_availability_zone
     - Name of the Availability Zone in which the cluster is located

   * - ClusterCreateTime
     - aws_cluster_create_time
     - Creation date and time for the cluster

   * - ClusterStatus
     - aws_cluster_status
     - The current state of the cluster

   * - ClusterRevisionNumber
     - aws_cluster_revision_number
     - Revision number of the database in the cluster.

   * - ClusterVersion
     - aws_cluster_version
     - Version ID of the Amazon Redshift engine that is running in the cluster

   * - NodeType
     - aws_cluster_node_type
     - The node type for the nodes in the cluster

   * - DBName
     - aws_cluster_db_name
     - Name of the initial database created when the cluster was created

   * - Encrypted
     - aws_cluster_encrypted
     - Boolean. If ``true``, indicates that data in the cluster is encrypted at rest.

   * - MasterUsername
     - aws_cluster_master_username
     - Master user name for the cluster. This is the name used to connect to the database specified in the DBName parameter.

   * - PubliclyAccessible
     - aws_cluster_publicly_accessible
     - Boolean. If ``true``, indicates that the cluster can be accessed from a public network.


.. _sqs-metadata:

SQS metadata
-------------------------------------------------------------------

For SQS, Infrastructure Monitoring imports properties of every queue as well as any tags set on the queue.
For more information on these properties, including acceptable values and constraints,
see the AWS developer documentation for SQS.

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   * - :strong:`SQS Name`
     - :strong:`Custom Property`
     - :strong:`Description`

   *  - QueueArn
      - aws_queue_arn
      - AWS resource name of the SQS queue

   *  - QueueURL
      - aws_queue_url
      - URL for the SQS queue

   *  - MaximumMessageSize
      - aws_maximum_message_size
      - Maximum size of a message that SQS accepts, in bytes. SQS rejects a message that is larger than this value.

   *  - CreateTimestamp
      - aws_created_timestamp
      - Creation timestamp for the SQS queue

   *  - VisibilityTimeout
      - aws_visibility_timeout
      - Visibility timeout for the queue

   *  - FifoQueue
      - aws_fifo_queue
      - Indicates whether the queue is a fifo queue

   *  - Region
      - aws_region
      - The region in which the SQS resides


.. _s3-metadata:

S3 metadata
-------------------------------------------------------------------

For S3, Infrastructure Monitoring imports the region in which the bucket resides,
as well as any tags set on buckets. Infrastructure Monitoring only imports metadata for non-empty buckets.
For more information on S3 bucket tags, see
the documentation for AWS S3 Cost Allocation tagging.

.. list-table::
   :header-rows: 1
   :widths: 30 30 60

   * - :strong:`S3 Name`
     - :strong:`Custom Property`
     - :strong:`Description`

   *  - Region
      - aws_region
      - The region in which the S3 bucket resides



.. _using-cloudwatch-metrics:

CloudWatch rollups and Infrastructure Monitoring MTS
=============================================================================

AWS CloudWatch uses rollups to summarize metrics, and it refers to them as "statistics". To learn more about rollups, see :ref:`rollups` in Data resolution and rollups in charts.


Because AWS CloudWatch rollups don't map directly to Infrastructure Monitoring rollups, you can't
directly access AWS CloudWatch rollups using the rollup selection menu in the Chart Builder.
Instead, Infrastructure Monitoring captures the rollups as individual MTS that have the dimension ``stat``.

.. list-table::
   :header-rows: 1
   :widths: 25 25 50

   * - :strong:`AWS statistic`
     - :strong:`IM dimension`
     - :strong:`Definition`

   * - Average
     - stat:mean
     - Mean value of metric over the sampling period

   * - Maximum
     - stat:upper
     - Maximum value of metric over the sampling period

   * - Minimum
     - stat:lower
     - Minimum value of metric over the sampling period

   * - Data Samples
     - stat:count
     - Number of samples over the sampling period

   * - Sum
     - stat:sum
     - Sum of all values that occurred over the sampling period


To use an AWS CloudWatch metric in a plot, always specify the following:

* AWS Cloudwatch metric name
* Filter for the ``stat`` dimension value that's appropriate for the metric you've chosen.

For example, if you are using the metric ``NetworkPacketsIn`` for EC2 metrics,
the only meaningful AWS statistics are ``Minimum``, ``Maximum`` and ``Average``. To plot ``NetworkPacketsIn`` metric with
the rollup you want, filter for the ``stat`` dimension with a value that corresponds to the AWS statistic (rollup) value:

* ``lower``: Rollup that corresponds to the AWS rollup ``Minimum``
* ``upper``: Rollup that corresponds to the AWS rollup ``Maximum``
* ``mean``: Rollup that corresponds to the AWS rollup ``Average``

.. note:: The "Rollup: Multiple" label in a plot for a CloudWatch metric indicates that you haven't specified the rollup you want. To avoid confusion, specify the rollup as soon as possible.

Infrastructure Monitoring uses a sixty-second sampling period for metrics it imports from AWS.

To learn more, see the AWS developer documentation for AWS CloudWatch.


.. _aws-namespaces:

AWS namespaces
-------------------------------------------------------------------

Infrastructure Monitoring imports AWS namespace metadata in the using the dimension ``namespace``.
For most AWS services, the namespace name has the form ``"AWS/<NAME_OF_SERVICE>"``, such as "AWS/EC2" or "AWS/ELB".
To select an MTS for an AWS metric when the metric has the same name for more than one service,
such as ``CPUUtilization``, use the ``namespace`` dimension as a filter.

To control the amount of data you import, specify the namespaces you want to import as well as the data you want to import or
exclude from each namespace. For more information, see :ref:`specify-data-metadata`.


.. _sfx-aws-metrics:

Organization metrics related to AWS
-------------------------------------------------------------------

Infrastructure Monitoring also sends a set of metrics for AWS related to errors and service calls for your organization.
The names of these metrics all start with ``sf.org.num.aws``. For more information, see
:new-page:`Usage metrics for Splunk Observability Cloud <https://quickdraw.splunk.com/redirect/?product=Observability&location=userdocs.infrastructure.aws.organization.metrics&version=current>`.

.. _aws-unique-id:

Uniquely identifying AWS instances
=============================================================================

The AWS instance ID is not a unique identifier. To uniquely identify an AWS instance,
you need to concatenate the ``instanceId``, ``region``, and ``accountID`` dimension values, separated by underscores "\_", as shown
in the following example:

``instanceId_region_accountID``

To construct the identifier manually, first get the specified values for each of your instances. For example, you can
use the following ``cURL`` command:

.. code-block:: none

   curl http://<INSTANCE_URL>/latest/dynamic/instance-identity/document

Here's an example JSON response from the ``cURL`` command:

.. code-block:: json

   {
     "devpayProductCodes" : null,
     "privateIp" : "10.1.15.204",
     "availabilityZone" : "us-east-1a",
     "version" : "2010-08-31",
     "accountId" : "134183635603",
     "instanceId" : "i-a99f9802",
     "billingProducts" : null,
     "instanceType" : "c3.2xlarge",
     "pendingTime" : "2015-09-02T16:45:40Z",
     "imageId" : "ami-2ef44746",
     "kernelId" : null,
     "ramdiskId" : null,
     "architecture" : "x86_64",
     "region" : "us-east-1"
   }

From the response, copy the values for ``instanceId``, ``region``, and ``accountId``, then concatenate them with
underscores as separators.

Use the resulting string identifier as the value for the ``sfxdim\_AWSUniqueId`` dimension.
