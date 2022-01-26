.. _infrastructure-azure:

**********************************
Monitor Azure
**********************************

.. meta::
   :description: Learn how to monitor Microsoft Azure infrastructure resources with Splunk Observability Cloud.

.. note::
   Before you can start monitoring any Microsoft Azure resources, :ref:`get-started-azure`, and log in with your administrator credentials.

Splunk Observability Cloud can automatically import metrics and metadata about your services from Microsoft Azure. Observability Cloud provides infrastructure monitoring capabilities powered by Azure Monitor. See :new-page:`https://docs.microsoft.com/en-us/azure/azure-monitor/overview <https://docs.microsoft.com/en-us/azure/azure-monitor/overview>` on the Microsoft site for more information.

You can also export and monitor data from sources running in your Azure environment, as described in the following table.

.. list-table::
   :header-rows: 1
   :widths: 30, 20, 50

   * - :strong:`Get data in`
     - :strong:`Monitor`
     - :strong:`Description`

   * - :ref:`get-started-k8s`
     - :ref:`infrastructure-k8s`
     - Collect metrics and logs from Kubernetes clusters running in Azure Kubernetes Service.

   * - - :ref:`get-started-linux`
       - :ref:`get-started-windows`
     - :ref:`infrastructure-hosts`
     - Collect metrics and logs from Linux and Windows hosts running in Virtual Machine instances.

   * - :ref:`get-started-application`
     - :ref:`get-started-apm`
     - Collect application metrics and spans running in hosts or Kubernetes clusters.

.. _monitor-azure-services:

Monitor Azure services and identify problems
=======================================================

View the health of Azure services at a glance from the Infrastructure page. This page provides a key metric for each service. You can also drill down into specific instances of an Azure service. For example, view key metrics for the Virtual Machines service, and filter for a specific ID to analyze a particular virtual machine instance.

Follow these steps to analyze problem Azure services from the Infrastructure page:

1. Select :strong:`Navigation menu > Infrastructure` and view the :strong:`Microsoft Azure` category.
2. Select the specific service you want to analyze. For example, select :strong:`Virtual Machines` to view metrics of a virtual machine. If you see “No Data Found,” you need to first configure an integration.
3. Compare instances of the service along the following metrics with the :strong:`Color by` drop-down list. In the heat map, colors represent the health of instances based on the metrics you select. For example, a heat map that shows green and red, uses green to denote healthy and red to denote unhealthy instances. If your heat map has multiple colors, then the lighter gradient represents less activity, and the darker gradient represents more activity. To apply visually accessible color palettes on custom dashboards and charts and throughout Infrastructure Monitoring, select :strong:`Account Settings > Color Accessibility.`

   You can color by metrics like CPU utilization and filter by dimensions like geographic region.
4. Group instances based on metadata about each instance with the :strong:`Group by` drop-down list.

   You can group instances according to the region or resource group they are running in or the environment tag. Use this to see correlations between different parts of your infrastructure and its performance.
5.  Find outliers for your metrics with the :strong:`Find Outliers` setting. Specify the :strong:`Scope` and :strong:`Strategy`.

    Set the :strong:`Scope` to analyze outliers from across the entire visible population of instances, or only within groups defined by the dimension or property you grouped instances by.

    You can select one of two :strong:`Strategies` to find outliers, as described in the following table.

    .. list-table::
       :header-rows: 1
       :widths: 30, 70

       * - :strong:`Strategy`
         - :strong:`Description`

       * - ``Deviation from Mean``
         - Instances appear as red that exceed the mean value of the metric by at least three standard deviations. Use this setting to find the most extreme outliers.

       * - ``Deviation from Median``
         - Instances appear as red that exceed the median absolute deviation value by at least three absolute deviations. This setting does not weigh extreme outliers as heavily as the standard deviation.

6. Select a specific instance you want to investigate further to view all the metadata and key metrics for the instance. For every instance, Observability Cloud provides a default dashboard.

   Analyze all the available metadata about the cloud service the instance is running in, the instance itself, and any custom tags associated with the instance. The default dashboard provides metric time series (MTS) for key metrics.

Use default dashboards to monitor Azure services
================================================

Observability Cloud provides default dashboards for supported Azure services. Default dashboards are available in dashboard groups based on the Azure service a dashboard represents data for.

To find default dashboards for Azure services, select :strong:`Navigation menu > Dashboards` and search for the Azure service you want to view dashboards for.

Explore built-in content
========================
Observability Cloud collects data from many cloud services. To see all of the navigators provided for data collected in your organization, go to the Infrastructure page. To see all the pre-built dashboards for data collected in your organization, select :strong:`Dashboards > Built-in`.

.. note::

  Azure Virtual Machines instances are powered by their respective public cloud service as well as Splunk OpenTelemetry Collector. You need both for all the charts to display data in the built-in dashboards.

  - If you have only the public cloud service and the Smart Agent configured, some charts in the built-in dashboards for Azure Virtual Machines instances display no data.
  - If you have only the public cloud service configured, you can see all the cards representing the services where data come from, but some charts in the built-in dashboards for Azure Virtual Machines instances display no data.
  - If you have only Smart Agent configured, Azure Virtual Machines instance navigator isn't available.

Identify Azure resources using metadata
================================================================================

Regardless of the mechanism by which you collect and send your metrics,
you can also use the Azure metadata that Observability Cloud imports.
This feature is available for the relevant Azure Services as well as
metrics collected by the collectd agent.

Azure metadata helps you analyze metrics by custom tags, region, host names,
and other dimensions.

The azure_resource_id dimension
--------------------------------------------------------------------------------

The Azure integration adds the ``azure_resource_id`` dimension to metrics received from Azure.
The dimension value is derived from Azure's ``resource_id`` for the resource and has the following syntax:

``<subscription_id>/<resource_group_name>/<resource_provider_namespace>/<resource_name>``

The Azure integration truncates the dimension value to 256 bytes, which is the maximum length of an Observability Cloud dimension value.

If you install collectd on an Azure Compute Virtual Machine instance using the
:new-page:`standard install script <https://github.com/signalfx/signalfx-collectd-installer>`,
the installation automatically adds the ``azure_resource_id``.

Azure integration generic dimensions
--------------------------------------------------------------------------------

The metric time series (MTS) associated with Azure metrics have the following generic dimensions.
These dimensions are common to all services.

.. list-table::
   :header-rows: 1

   * - :strong:`Dimension name`
     - :strong:`Description`

   * - ``azure_resource_id``
     - Unique identifier for the Azure object

   * - ``resource_group_id``
     - ID of the resource group the Azure object belongs to

   * - ``subscription_id``
     - ID of the subscription the resource belongs to

   * - ``resource_type``
     - Type of the Azure object

   * - ``aggregation_type``
     - The Azure aggregation type of the metric

   * - ``primary_aggregation_type``
     - Indicates whether or not the aggregation type is the primary type

   * - ``unit``
     - Unit of the metric value

|br|

``resource_group_id`` is derived from the Azure resource group id with the
following syntax:

``<subscription_id>/<resource_group_name>``

Some Azure services include dimensions that Observability Cloud adds to MTS.
For example, the metrics from :strong:`Azure Storage` provider include the
dimensions ``apiname`` and ``geotype``.

Azure integration resource metadata
--------------------------------------------------------------------------------

The Azure integration queries the Azure API to retrieve metadata for monitored resources.
You can filter and group MTS by this metadata in charts and in the Infrastructure Navigator.

The Azure integration adds the metadata as custom properties of a specific Azure MTS dimension, as follows:

-  Metadata for services in a subscription (:ref:`sub-metadata`)
   is added as custom properties of the ``subscription_id`` dimension.

-  Metadata for services within a resource group (:ref:`resource-metadata`)
   is added as custom properties of the ``resource_group_id`` dimension.

-  Metadata that are service-specific (:ref:`service-metadata`)
   is added as properties of the ``azure_resource_id`` dimension.

-  Tags on all resources are added to the ``azure_resource_id`` dimension. To learn more, see
   :ref:`resource-tags`

.. _sub-metadata:

Subscription metadata
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following table shows the metadata that the Azure integration syncs for services in a subscription:

.. list-table::
   :header-rows: 1

   * - :strong:`Azure name`
     - :strong:`Custom property`
     - :strong:`Description`

   * - ``displayName``
     - ``azure_subscription_display_name``
     - the display name of the subscription (for example,  ``Pay-As-You-Go``)

   * - ``state``
     - ``azure_subscription_state``
     - state of the subscription (for example,  ``Enabled``)


.. _resource-metadata:

Resource-group metadata
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following table shows the metadata that the Azure integration syncs for services in a resource group:

.. list-table::
   :header-rows: 1

   * - :strong:`Azure name`
     - :strong:`Custom property`
     - :strong:`Description`

   * - ``name``
     - ``azure_resource_group_name``
     - name of the resource group

   * - ``provisioningState``
     - ``azure_resource_group_provisioning_state``
     - provisioning state of the resource group (for example,  ``Succeeded``)

   * - ``region``
     - ``azure_resource_group_region``
     - region to which the resource group belongs (for example,  ``eastus``)

   * - Tags
     - ``azure_resource_group_tag<name-of-tag>`` (if resource group has user-defined tags)
     - all resource group wide tags

.. _resource-tags:

Azure tags for resource groups
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Azure tags for resource groups are a list of key:value pairs, and from them the Azure integration creates
Observability Cloud tags that have the syntax ``azure_resource_group_tag<name-of-tag>``.
For example, if Azure has ``[key1:label01, key2:label02]`` as the tags property for a resource group, the Azure integration
creates two tags: ``azure_resource_group_tag_key1`` and ``azure_resource_group_tag_key2``.

.. _service-metadata:

Service-level metadata
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following tables shows the metadata that the Azure integration syncs for individual services:

.. _virtual-machine-service-metadata:

**Virtual Machines**

For Virtual Machines, Observability Cloud retrieves a subset of metadata about the instance,
as well as custom metadata you specify for the instance.

.. list-table::
   :header-rows: 1

   * -   :strong:`Azure name`
     -   :strong:`Custom property`
     -   :strong:`Description`

   * -   ``computerName``
     -   ``azure_computer_name``
     -   name of the virtual machine instance

   * -   ``imageReference.offer``
     -   ``azure_image_reference_offer``
     -   offer of the image reference (for example,  ``UbuntuServer``)

   * -   ``imageReference.publisher``
     -   ``azure_image_reference_publisher``
     -   publisher of the image reference (for example,  ``Canonical``)

   * -   ``imageReference.sku``
     -   ``azure_image_reference_sku``
     -   SKU of the image reference (for example,  ``16.04-LTS``)

   * -   ``imageReference.version``
     -   ``azure_image_reference_version``
     -   version of the image reference (for example,  ``latest``)

   * -   ``osDiskCachingType``
     -   ``azure_os_disk_caching_type``
     -   OS Disk caching type of the instance (for example,  ``ReadWrite``)

   * -   ``osType``
     -   ``azure_os_type``
     -   type of OS on the virtual machine (for example, "LINUX" or "WINDOWS")

   * -   ``osDiskSize``
     -   ``azure_os_disk_size``
     -   disk size in GB

   * -   ``powerState``
     -   ``azure_power_state``
     -   power state of the virtual machine (for example, ``PowerState/running``)

   * -   ``provisioningState``
     -   ``azure_provisioning_state``
     -   provisioning state of the virtual machine (for example, ``Succeeded``)

   * -   ``size``
     -   ``azure_size``
     -   information about the size of the virtual machine (for example, ``Standard_D2s_v3``)

   * -   ``vmId``
     -   ``azure_vm_id``
     -   ID given to the virtual machine instance by Azure

|br|

.. _batch-accounts-service-metadata:

**Batch Accounts**

For Batch Accounts, Observability Cloud syncs the following properties:

.. list-table::
   :header-rows: 1

   * -   :strong:`Azure name`
     -   :strong:`Custom property`
     -   :strong:`Description`

   * -   ``activeJobAndJobScheduleQuota``
     -   ``azure_active_job_and_job_schedule_quota``
     -   active job and job schedule quota for this batch account

   * -   ``coreQuota``
     -   ``azure_core_quota``
     -   core quota for the batch account

   * -   ``poolQuota``
     -   ``azure_pool_quota``
     -   pool quota for the batch account

   * -   ``provisioningState``
     -   ``azure_provisioning_state``
     -   provisioningState of the batch account (for example, ``Succeeded``)

|br| 

.. _storage-account-service-metadata:

**Storage Account**

For Storage Account, Observability Cloud syncs the following properties:

.. list-table::
   :header-rows: 1

   * -   :strong:`Azure name`
     -   :strong:`Custom property`
     -   :strong:`Description`

   * -   ``creationTime``
     -   ``azure_creation_time``
     -   time at which the account was created (for example, ``Thu Jan 19 18:16:25 UTC 2018``)

   * -   ``kind``
     -   ``azure_kind``
     -   kind of storage account  (for example, ``Storage`` or ``BLOB``)

   * -   ``sku``
     -   ``azure_sku``
     -   SKU of the storage account (for example, ``Standard_LRS``)

|br|

.. _redis-cache-service-metadata:

**Redis Cache**

For Redis caches, Observability Cloud syncs the following properties:

.. list-table::
   :header-rows: 1

   * -   :strong:`Azure name`
     -   :strong:`Custom property`
     -   :strong:`Description`

   * -   ``hostName``
     -   ``azure_host_name``
     -   host name of the Redis cache

   * -   ``isPremium``
     -   ``azure_is_premium``
     -   indicates whether or not the service is premium


   * -   ``port``
     -   ``azure_port``
     -   port value for Redis cache (for example, ``6379``)

   * -   ``sslPort``
     -   ``azure_ssl_port``
     -   sslPort value for Redis cache (for example, ``6380``)

   * -   ``nonSslPort``
     -   ``azure_non_ssl_port``
     -   is ``true`` if ``nonSslPort`` is enabled

   * -   ``provisioningState``
     -   ``azure_provisioning_state``
     -   provisioning state of the Redis cache (for example, ``Succeeded``)

   * -   ``redisVersion``
     -   ``azure_redis_version``
     -   version of Redis

   * -   ``shardCount``
     -   ``azure_shard_count``
     -   number of shards

   * -   ``sku``
     -   ``azure_sku``
     -   SKU of the Redis cache (for example, ``Standard_C1``)

|br|

.. _virtual-machine-scale-sets-service-metadata:

**Virtual Machine Scale Sets**

For Virtual Machine Scale Sets, Observability Cloud syncs the following properties:

.. list-table::
   :header-rows: 1

   * -   :strong:`Azure name`
     -   :strong:`Custom property`
     -   :strong:`Description`

   * -   ``computerNamePrefix``
     -   ``azure_computer_name_prefix``
     -   computer name prefix of the instances in the scale set

   * -   ``imageReference.offer``
     -   ``azure_image_reference_offer``
     -   offer of the image reference (for example, ``UbuntuServer``)

   * -   ``imageReference.publisher``
     -   ``azure_image_reference_publisher``
     -   publisher of the image reference (for example, ``Canonical``)

   * -   ``imageReference.sku``
     -   ``azure_image_reference_sku``
     -   SKU of the image reference (for example, ``16.04-LTS``)

   * -   ``imageReference.version``
     -   ``azure_image_reference_version``
     -   version of the image reference (for example, ``latest``)

   * -   ``capacity``
     -   ``azure_capacity``
     -   number of instances in the scale set

   * -   ``osDiskCachingType``
     -   ``azure_os_disk_caching_type``
     -   OS Disk caching type of the instance (for example, ``ReadWrite``)

   * -   ``primaryNetworkId``
     -   ``azure_primary_network_id``
     -   ID of the primary network of the scale set

   * -   ``overProvisionEnabled``
     -   ``azure_over_provision_enabled``
     -   indicates whether or not over provisioning is enabled

   * -   ``upgradeModel``
     -   ``azure_upgrade_model``
     -   upgrade model of the scale set (for example, ``Manual``)

|br|

.. _virtual-machines-in-scale-sets-service-metadata:

**Virtual Machines in Scale Sets**

For Virtual Machines in Scale Sets, Observability Cloud syncs the following properties:

.. list-table::
   :header-rows: 1

   * -   :strong:`Azure name`
     -   :strong:`Custom property`
     -   :strong:`Description`

   * -   ``imageReference.offer``
     -   ``azure_image_reference_offer``
     -   offer of the image reference (for example, ``UbuntuServer``)

   * -   ``imageReference.publisher``
     -   ``azure_image_reference_publisher``
     -   publisher of the image reference (for example, ``Canonical``)

   * -   ``imageReference.sku``
     -   ``azure_image_reference_sku``
     -   SKU of the image reference (for example, ``16.04-LTS``)

   * -   ``imageReference.version``
     -   ``azure_image_reference_version``
     -   version of the image reference (for example, ``latest``)

   * -   ``instanceId``
     -   ``azure_instance_id``
     -   instance id of the VM in the Scaleset

   * -   ``osDiskCachingType``
     -   ``azure_os_disk_caching_type``
     -   OS Disk caching type of the instance (for example, ``ReadWrite``)

   * -   ``osDiskName``
     -   ``azure_os_disk_name``
     -   OS Disk name of the instance

   * -   ``osDiskSize``
     -   ``azure_os_disk_size``
     -   OS Disk size of the instance

   * -   ``osType``
     -   ``azure_os_type``
     -   OS Type (for example, ``Linux``)

   * -   ``powerState``
     -   ``azure_power_state``
     -   Power state of the instance (for example, ``PowerState/running``)

   * -   ``size``
     -   ``azure_size``
     -   Size of the instance (for example, ``Standard_A1``)

   * -   ``sku``
     -   ``azure_sku``
     -   sku of the instance (for example, ``com.microsoft.azure.management.compute.Sku@151e5d8d``)

..
  Supported Azure services
  ========================

  You can monitor these Azure services in Observability Cloud:

  .. hlist::
    :columns: 2

    - API Management
    - App Service
    - Application Gateway
    - Automation
    - Azure Analysis Services
    - Azure Cosmos DB
    - Azure DDoS Protection
    - Azure DNS
    - Azure Data Explorer
    - Azure Database for MySQL
    - Azure Database for PostgreSQL
    - Azure Firewall
    - Azure Front Door
    - Azure Kubernetes Service
    - Azure Location Based Services
    - Azure Machine Learning
    - Azure Maps
    - Batch
    - Cognitive Services
    - Container Instances
    - Container Registry
    - Content Delivery Network (CDN)
    - Customer Insights
    - Data Factory
    - Data Lake Analytics
    - Data Lake Store
    - Event Grid (Event Subscriptions)
    - Event Grid (Extension Topics)
    - Event Grid (System Topics)
    - Event Grid (Topics)
    - Event Grid (domains)
    - Event Hubs
    - ExpressRoute
    - HDInsight
    - Iot Hub
    - Key Vault
    - Load Balancer
    - Logic apps
    - Network Interfaces
    - Notification Hubs
    - Power BI
    - Redis Cache
    - Relays
    - SQL Database
    - SQL Elastic Pools
    - SQL Servers
    - Search Services
    - Service Bus
    - Storage
    - Stream Analytics
    - Traffic Manager
    - VPN Gateway
    - Virtual Machine Scale Sets
    - Virtual Machines
    - Virtual Machines (Classic)
