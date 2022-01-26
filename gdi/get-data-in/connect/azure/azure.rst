.. _get-started-azure:

**************************************************************
Connect to Azure and send data to Splunk Observability Cloud
**************************************************************

.. meta::
   :description: Connect your Microsoft Azure account to Splunk Observability Cloud.

.. toctree::
   :hidden:

   azure-metrics

Splunk Observability Cloud provides an integration with Microsoft Azure, lets you travel through Azure entities, and includes built-in dashboards to help you monitor Azure services.

After you connect your Azure account to Observability Cloud, you can do the following:

- Import Azure metadata

- Use Observability Cloud tools to monitor your Azure services

- Filter Azure monitoring results using tags or dimensions such as ``region`` and ``host name``


.. raw:: html

  <embed>
    <h2>Azure integration prerequisites<a name="azure-integration-prereqs" class="headerlink" href="#azure-integration-prereqs" title="Permalink to this headline">¶</a></h2>
  </embed>


Successful integration requires administrator privileges for the following:

- Your organization in Splunk Observability Cloud.

- Creating a new Azure Active Directory application.

To learn more about these privileges, refer to the Azure documentation for registering a new app.


.. raw:: html

  <embed>
    <h2>Prepare for Azure integration<a name="prep-azure-integration" class="headerlink" href="#prep-azure-integration" title="Permalink to this headline">¶</a></h2>
  </embed>

To prepare Microsoft Azure for connection to Splunk Observability Cloud, do the following:

#. Create an Azure Active Directory application by following these steps:

   #. Open a new tab in your web browser.
   #. Login to your Azure portal.
   #. Navigate to :menuselection:`Azure Active Directory` and select :menuselection:`App registrations`. Then click :guilabel:`New registration` at the top of the page.
   #. Enter the name, indicate access type, select :menuselection:`Web`, enter your sign-on URL, and then click :guilabel:`Register`.

      Observability Cloud does not use this information, but you need to provide it in order to create an app on Azure.
   #. The Azure portal displays summary information about the application. Save the following information to use when you create
      your Azure integration in Observability Cloud:

      * :guilabel:`Display name`
      * :guilabel:`Application (client) ID`
      * :guilabel:`Directory (tenant) ID`
      * :guilabel:`Object ID`

   #. Click :guilabel:`Certificates & settings`. The Certificate is your public key, and the client secret is your password.
   #. Create a client secret by providing a description and setting the duration to the longest possible interval, then click :guilabel:`Save`.
   #. The Azure portal displays the client secret. Save this value; you need the client secret to create your Azure integration in Observability Cloud.

#. Specify subscriptions and set subscription permissions:

   #. In the Azure portal, navigate to :menuselection:`All services`, click :menuselection:`Everything`, then click :guilabel:`Subscriptions`.
   #. Find a subscription you want to monitor, and click on the subscription name.
   #. Navigate to :menuselection:`Access control (IAM)`, click :menuselection:`Add`, then select :menuselection:`Add role assignment`.
   #. On the :guilabel:`Add role assignment page`, perform the following steps:

      #. From the :guilabel:`Role` drop-down list, select :menuselection:`Monitoring Reader`.
      #. Leave the :guilabel:`Assign access to` drop-down list unchanged.
      #. In the :guilabel:`Select` text box, start entering the name of the Azure application you just created.
         The Azure portal automatically suggests names as you type. After you enter the application name,
         click :guilabel:`Save`.

   Repeat these steps for each subscription you want to monitor.

You also have the option of connecting to Azure through the Observability Cloud API. For details, see :new-page:`Integrate Microsoft Azure Monitoring with Splunk Observability Cloud <https://dev.splunk.com/observability/docs/integrations/msazure_integration_overview/>` in the Splunk developer documentation.

.. raw:: html

  <embed>
    <h2>Connect to Azure<a name="connect-to-azure" class="headerlink" href="#connect-to-azure" title="Permalink to this headline">¶</a></h2>
  </embed>

From Splunk Observability Cloud, connect to Azure by following these steps:

   #. On the Splunk Observability Cloud home page, open the Navigation menu and click :menuselection:`Data Setup` to display the Connect Your Data page.
   #. Navigate to the :guilabel:`Azure Services` section.
   #. Select :strong:`Microsoft Azure`. The Microsoft Azure setup page is displayed.
   #. To start configuring the connection to Azure, click :guilabel:`New Integration`.
   #. In the text boxes for Splunk Infrastructure Monitoring setup, enter the following information:

      * :guilabel:`Name`: Unique name for this connection to Azure. The name field helps you create multiple connections to Azure,
        each with its own name.
      * :guilabel:`Directory ID`: Azure Directory ID you saved in a previous step.
      * :guilabel:`App ID`: The Azure app (client) ID you saved in a previous step.
      * :guilabel:`Client Secret`: The client secret (password) you saved in a previous step.
   #. Select the type of Azure connection you created in the previous steps:

      * For an Azure Government instance, click :guilabel:`Azure Government`.
      * For all other Azure connections, click :guilabel:`Azure`.
   #. Select the rate at which you want Splunk Observability Cloud to poll Azure for data:

      * For a poll rate of 1 minute, click :guilabel:`1 Min`. This is the default.
      * For a poll rate of 5 minutes, click :guilabel:`5 Min`.

   #. Optional: Use the :guilabel:`Add Tag` button to create a tag if you want to monitor only tagged data sources, filling out the ``tag name`` and ``tag value`` fields separately to create a tag pair.
   #. Click :guilabel:`Save`. Observability Cloud saves the connection details and attempts to validate the integration. A :ok:`Validated!` message confirms that the integration was successful.

Splunk Observability Cloud begins receiving metrics from Azure for the subscriptions and services that you specified in the Observability Cloud settings for your Azure connection.

.. raw:: html

  <embed>
    <h3>Connect to Azure using the Splunk Observability Cloud API<a name="connect-to-azure-using-API" class="headerlink" href="#connect-to-azure-using-API" title="Permalink to this headline">¶</a></h3>
  </embed>

You can can use the Splunk API to integrate Azure with Splunk Observability Cloud.

For instructions on how to connect to Azure through the API, see :new-page:`Integrate Microsoft Azure monitoring with Splunk Observability Cloud <https://dev.splunk.com/observability/docs/integrations/msazure_integration_overview/>` in the Splunk developer documentation.

.. note:: Azure tag filtering configured through the UI applies an ``OR`` operator to the ``name:value`` pairs that you specify in separate fields. Values for ``tag name`` and ``tag value`` are what you anticipate for monitored data sources. To apply more complex rules not governed exclusively by the OR operator, connect to Azure through the Observability Cloud API and modify the contents of the ``resourceFilterRules`` field there.

.. raw:: html

  <embed>
    <h3>Install the Splunk Distribution of OpenTelemetry Collector<a name="install-splunk-otel-collector" class="headerlink" href="#install-splunk-otel-collector" title="Permalink to this headline">¶</a></h3>
  </embed>

If you installed Azure while going through the Quick Start guide, continue by installing the :new-page:`Splunk Distribution of OpenTelemetry Collector <https://docs.splunk.com/Observability/gdi/opentelemetry/resources.html>`.

The Azure integration provides an Azure mode for the :new-page:`navigator <https://docs.splunk.com/Observability/infrastructure/navigators/navigators.html#nav-Splunk-Infrastructure-Monitoring-navigators>`, and includes :new-page:`default dashboards <https://docs.splunk.com/Observability/infrastructure/navigators/azure.html#use-default-dashboards-to-monitor-azure-services>` to help you
monitor Microsoft Azure services.

You can also connect to Azure and the subscriptions and services running on it by using the Splunk Distribution of OpenTelemetry Collector. To learn more, see :ref:`otel-intro`.

OTel Collector offers a higher
degree of customization than the Azure integration, and you might prefer it if you want to see metrics at a resolution lower than one minute, or when you need fine-grained control over the filtering of what metrics are sent.

.. raw:: html

  <embed>
    <h2>Supported Azure services<a name="supported-azure-services" class="headerlink" href="#supported-azure-services" title="Permalink to this headline">¶</a></h2>
  </embed>

Splunk Observability Cloud syncs with a subset of Azure services. During your Azure setup, if
you select :guilabel:`All Services` when you specify subscriptions, Observability Cloud syncs with the following services:

* API Management: Used to publish APIs
* App Service: Creates cloud apps for web and mobile
* Application Gateway: Builds web front ends in Azure
* Automation: Process automation for cloud management
* Azure Analysis Services: Analytics engine as a service
* Azure Autoscale: Dynamically scales apps to meet changing demand
* Azure Cosmos DB: NoSQL database with open APIs
* Azure Data Explorer: Scalable data exploration service
* Azure Database for Maria DB: Managed MariaDB database service for app developers
* Azure Database for MySQL: Managed and scalable MySQL database
* Azure Database for PostgreSQL: Intelligent and scalable PostgreSQL
* Azure DDoS Protection: Protection against Distributed Denial of Service attacks
* Azure DNS: Support for hosting your DNS domain in Azure

* Azure Firewall: Cloud-native protection for Azure Virtual Network resources
* Azure Front Door: Cloud content delivery service
* Azure Kubernetes Service: Managed Kubernetes
* Azure Location Based Services: APIs for mapping, search, routing, traffic, and time zones
* Azure Machine Learning: Machine learning to build and deploy models
* Azure Maps: Secure location APIs that provide geospatial context for data
* Azure SignalR Service: Add real-time web functionalities
* Azure SQL Managed Instances: Managed SQL instance in the cloud
* Azure Web PubSub: build real-time messaging web apps using WebSockets and the publish-subscribe pattern

* Batch: Job scheduling and compute management
* Container Instances: Run containers without managing servers
* Cognitive Services: Deploy AI models as APIs
* Container Registry: Store and manage container images across deployments
* Content Delivery Network (CDN): Content delivery
* Customer Insights: Map, match, merge, and enrich customer-based data

* Data Factory: Hybrid data integration
* Data Lake Analytics: Distributed analytics
* Data Lake Store: Secure data lake for high-performance analytics
* Event Grid (Domains): Event delivery at scale
* Event Grid (Event Subscriptions): Event delivery at scale
* Event Grid (Extension Topics): Event delivery at scale
* Event Grid (System Topics): Event delivery at scale
* Event Grid (Topics): Event delivery at scale
* Event Hubs: Receive telemetry from millions of devices
* ExpressRoute: Dedicated private network fiber connections to Azure
* HDInsight: Provision cloud Hadoop, Spark, R Server, HBase, and Storm clusters

* Key Vault: Safeguard and maintain control of keys and other secrets
* Load Balancer: Supports high availability and network performance for apps
* Logic apps: Automate the access and use of data across clouds
* Network Interfaces: Adds network interface to Azure VMs
* Notification Hubs: Send push notifications to any platform from any back end
* Power BI: Customer-facing dashboards and analytics
* Redis Cache: High-throughput, low-latency data caching for apps
* Relays: Securely expose services that run in your corporate network to the public cloud
* Search Services: Enterprise-scale search for app development
* Service Bus: Connect across private and public cloud environments
* Storage: Support for storage endpoints
* Stream Analytics: Real-time analytics on streaming data

* SQL Database: Managed SQL in the cloud
* SQL Elastic Pools: Manage multiple databases with varying and unpredictable usage demands
* SQL Servers: Host enterprise SQL Server apps in the cloud
* Traffic Manager: Route incoming traffic for high performance and availability
* Virtual Machines: Provision Windows and Linux VMs
* Virtual Machines (Classic): Older deployment model for Azure VMs
* Virtual Machine Scale Sets: Manage and scale up to thousands of Linux and Windows VMs
* VPN Gateway: Secure cross-premises connectivity


.. raw:: html

  <embed>
    <h2>Next steps<a name="next-azure-steps" class="headerlink" href="#next-azure-steps" title="Permalink to this headline">¶</a></h2>
  </embed>

To validate your setup, examine the details of your Azure integration as displayed in the list at the end of the setup page.

See :ref:`azure-metrics` for a list of the available Azure resources.

For instructions on how to monitor your Azure services, see :new-page:`Monitor Azure <https://docs.splunk.com/Observability/infrastructure/navigators/azure.html#infrastructure-azure>`.
