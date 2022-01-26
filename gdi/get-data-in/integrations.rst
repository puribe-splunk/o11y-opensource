.. _supported-data-sources:

********************************************************************************
Supported integrations
********************************************************************************

.. meta::
   :description: This page provides a listing of integrations supported by Splunk Observability Cloud.

This page provides a list of integrations supported by Splunk Observability Cloud.


.. _aws-integrations:

Amazon Web Services
----------------------------------

For details about how to get data from these Amazon Web Services into Observability Cloud, see :ref:`get-started-aws`.

For details about the metrics provided by an Amazon Web Services integration, see :ref:`aws-metrics`.

.. list-table::
   :header-rows: 1
   :widths: 50 16 16 16
   :class: monitor-table

   * - :strong:`Service`
     - :strong:`Provides metrics`
     - :strong:`Provides traces`
     - :strong:`Provides logs`

   * - :ref:`Amazon API Gateway <get-started-aws>`
     - :strong:`X`
     -
     -

   * - :ref:`Amazon CloudWatch Events <get-started-aws>`
     - :strong:`X`
     -
     - :strong:`X`

   * - :ref:`Amazon DynamoDB <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon Elastic Compute Cloud (EC2) <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon EC2 Container Service (ECS) <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon ElastiCache <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon Elastic Block Store (EBS) <get-started-aws>`
     - :strong:`X`
     -
     -

   * - :ref:`Amazon Elastic Kubernetes Service (EKS) <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon Kinesis <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon Kinesis Analytics <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon Kinesis Streams <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon Redshift <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon Relational Database Service <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon Route 53 <get-started-aws>`
     - :strong:`X`
     -
     -

   * - :ref:`Amazon Simple Notification Service <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Amazon Simple Queue Service <get-started-aws>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`AWS Auto Scaling <get-started-aws>`
     - :strong:`X`
     -
     - :strong:`X`

   * - :ref:`AWS CloudFront <get-started-aws>`
     - :strong:`X`
     -
     - :strong:`X`

   * - :ref:`AWS Compute Optimizer <get-started-aws>`
     - :strong:`X`
     -
     - :strong:`X`

   * - :ref:`AWS Elastic Load Balancing <get-started-aws>`
     - :strong:`X`
     -
     - :strong:`X`

   * - :ref:`AWS Elastic Load Balancing (Classic Load Balancers) <get-started-aws>`
     - :strong:`X`
     -
     -

   * - :ref:`AWS Lambda<wrapper-ingest>`
     - :strong:`X`
     - :strong:`X`
     - :strong:`X`


.. _gcp-integrations:

Google Cloud Platform services
----------------------------------

These Google Cloud Platform (GCP) services send metrics to Splunk Infrastructure Monitoring:

- App Engine
- BigQuery
- Cloud Bigtable
- Cloud Composer
- Cloud Dataflow
- Cloud Dataproc
- Cloud Datastore
- Cloud Endpoints APIs
- Cloud Filestore
- Cloud Functions
- Cloud Internet of Things Core
- Cloud Machine Learning
- Cloud Pub/Sub
- Cloud Router
- Cloud Run
- Cloud Spanner
- Cloud Storage
- Cloud SQL
- Cloud Tasks
- Cloud VPN
- Container Engine
- Dedicated Interconnect
- Firebase Database
- Firebase Hosting
- HTTP(S) Load Balancing
- Knative
- Kubernetes Engine
- Memorystore for Redis
- Stackdriver Logging
- Stackdriver Monitoring

For details about how to get metrics from these Google Cloud Platform services into Infrastructure Monitoring, see :ref:`get-started-gcp`.

For details about the metrics provided by a Google Cloud Platform integration, see :ref:`gcp-metrics`.


.. _azure-integrations:

Microsoft Azure services
----------------------------------

For details about how to get data from these Microsoft Azure services into Observability Cloud, see :ref:`get-started-azure`.

For details about the metrics provided by a Microsoft Azure integration, see :ref:`azure-metrics`.

.. list-table::
   :header-rows: 1
   :widths: 50 16 16
   :class: monitor-table

   * - :strong:`Service`
     - :strong:`Provides metrics`
     - :strong:`Provides traces`

   * - :ref:`Azure App Service <get-started-azure>`
     - :strong:`X`
     - :strong:`X`

   * - :ref:`Azure Batch <get-started-azure>`
     - :strong:`X`
     -

   * - :ref:`Azure Event Hubs <get-started-azure>`
     - :strong:`X`
     -

   * - :ref:`Azure Functions <get-started-azure>`
     - :strong:`X`
     - :strong:`X`

   * - :ref:`Azure Kubernetes Service <get-started-azure>`
     - :strong:`X`
     - :strong:`X`

   * - :ref:`Azure Logic apps <get-started-azure>`
     - :strong:`X`
     - :strong:`X`

   * - :ref:`Azure Redis Cache <get-started-azure>`
     - :strong:`X`
     -

   * - :ref:`Azure Storage <get-started-azure>`
     - :strong:`X`
     -

   * - :ref:`Azure SQL Database <get-started-azure>`
     - :strong:`X`
     -

   * - :ref:`Azure SQL Elastic Pools <get-started-azure>`
     - :strong:`X`
     -

   * - :ref:`Azure Virtual Machines <get-started-azure>`
     - :strong:`X`
     - :strong:`X`

   * - :ref:`Azure Virtual Machine Scale Sets <get-started-azure>`
     - :strong:`X`
     -


Platforms
----------------------------------

Install the Splunk Distribution of OpenTelemetry Collector on your infrastructure to start sending data to Splunk Observability Cloud.

.. list-table::
   :header-rows: 1
   :widths: 50 16 16 16
   :class: monitor-table

   * - :strong:`Data source`
     - :strong:`Provides metrics`
     - :strong:`Provides traces`
     - :strong:`Provides logs`

   * - :ref:`Kubernetes <get-started-k8s>`
     - :strong:`X`
     - :strong:`X`
     - :strong:`X`

   * - :ref:`Linux <get-started-linux>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Microsoft Windows <get-started-windows>`
     - :strong:`X`
     - :strong:`X`
     -


APM instrumentation
--------------------------------------------

Instrument your applications to send metrics and traces to Observability Cloud.

.. list-table::
   :header-rows: 1
   :widths: 50 16 16 16
   :class: monitor-table

   * - :strong:`Language`
     - :strong:`Provides metrics`
     - :strong:`Provides traces`
     - :strong:`Provides logs`

   * - :new-page:`C++ <https://github.com/signalfx/splunk-otel-cpp>`
     -
     - :strong:`X`
     -

   * - :ref:`Java <get-started-java>`
     - :strong:`X`
     - :strong:`X`
     -

   * - :ref:`Microsoft .NET <dotnet>`
     -
     - :strong:`X`
     -

   * - :ref:`Node.js <get-started-nodejs>`
     -
     - :strong:`X`
     -

   * - :ref:`PHP <get-started-php>`
     -
     - :strong:`X`
     -

   * - :ref:`Python <get-started-python>`
     -
     - :strong:`X`
     -

   * - :ref:`Ruby <get-started-ruby>`
     -
     - :strong:`X`
     -


Metric instrumentation
--------------------------------------------

Instrument your applications to send metrics to Infrastructure Monitoring.

- :new-page:`Go <https://github.com/signalfx/signalfx-go>`

- :new-page:`Java <https://github.com/signalfx/signalfx-java>`

- :new-page:`Node.js <https://github.com/signalfx/signalfx-nodejs>`

- :new-page:`Python <https://github.com/signalfx/signalfx-python>`

- :new-page:`Ruby <https://github.com/signalfx/signalfx-ruby>`


RUM instrumentation
--------------------------------------------

Instrument your web and mobile applications to send metrics, web vitals, errors, and other forms of data to Splunk Real User Monitoring.

For more information, see :ref:`browser-rum-gdi`.


Application receivers
--------------------------------------------

An application receiver gathers metrics from its associated application and the host the application is running on and sends them to Infrastructure Monitoring.

.. using an include for this table because it also appears on gdi/index.rst

.. include:: /_includes/application-receiver-table.rst


Community integrations
---------------------------------------------------------------------------------

- Istio
- Jaeger
- Linkerd
- Micrometer
- Prometheus
- Spring Boot
- Telegraf Agent
- Zipkin

For information about these integrations, access Splunk Observability Cloud, open the navigation menu and go to :strong:`Data Setup` > :strong:`Community Integrations`.


Notification services
--------------------------------------------

These integrations enable you to send Observability Cloud alert notifications to the following third-party notification services:

- Amazon EventBridge
- BigPanda
- Jira
- Microsoft Teams
- Opsgenie
- PagerDuty
- ServiceNow
- Slack
- Splunk On-Call
- Webhook
- xMatters

For more information about integrating with notification services, see :ref:`admin-notifs-index`.


Login services
--------------------------------------------

These login service integrations enable your users to single sign-on (SSO) to Observability Cloud using a third-party identity provider (IdP) that uses SAML SSO or a custom URL that you specify.

- Active Directory FS
- Azure Active Directory
- Google Cloud Identity
- Google Sign-In
- Okta
- OneLogin
- PingOne
- SAML

For more information about configuring an SSO integration, see :ref:`sso-label`.


Data link destinations
--------------------------------------------

Data links enable you to link metadata to the following destinations outside of Observability Cloud:

- Splunk Cloud Platform
- Splunk Enterprise
- Kibana

For more information about creating data links, see :ref:`link-metadata-to-content`.


Other integrations
----------------------------------------------------------------------------------------------

- :new-page:`Grafana <https://grafana.com/grafana/plugins/grafana-splunk-monitoring-datasource/>`

- :new-page:`LaunchDarkly <https://docs.launchdarkly.com/integrations/signalfx>`

- :new-page:`Pulumi <https://www.pulumi.com/docs/intro/cloud-providers/signalfx/>`

- :new-page:`Terraform <https://registry.terraform.io/providers/splunk-terraform/signalfx/latest/docs>`
