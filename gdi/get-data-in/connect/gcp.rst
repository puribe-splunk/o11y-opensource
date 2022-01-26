.. _get-started-gcp:

**************
Connect to GCP
**************

.. meta::
   :description: Connect your GCP account to Splunk Observability Cloud.

.. toctree::
   :hidden:

With a Google Cloud Platform (GCP) integration in Splunk Infrastructure Monitoring, you can track your Google StackDriver metrics and monitor your GCP services in one place using the navigator in StackDriver-powered mode and built-in dashboards.

=======================
Create a GCP connection
=======================

To configure a GCP integration with Splunk Infrastructure Monitoring, create a new service account key and specify services to monitor in your GCP account.

Prerequisites
=============

You must be an administrator of your Splunk Observability Cloud organization to create a GCP connection.

.. _gcp-one:

Part 1: Select a role for the GCP service account
=================================================

* If you want to use the Project Viewer role, skip to :ref:`gcp-two`. Choosing this role ensures that any functionality update implemented in Infrastructure Monitoring will not require changes to your GCP setup.
* If you want to use a role with more restrictive permissions than those available to Project Viewer, make sure your selected role has sufficient permissions to connect to Infrastructure Monitoring.

.. note::

   If your GCP service account role has insufficient permissions, you'll get an error message when you try to connect to Infrastructure Monitoring. Review and enable any missing permissions, or change the role to Project Viewer.

The following table specifies the permissions required for the GCP integration.

.. list-table::
   :header-rows: 1
   :widths: 40 60

   * - :strong:`Permission`
     - :strong:`Required?`

   * - ``monitoring.metricDescriptors.get``
     - Yes

   * - ``monitoring.metricDescriptors.list``
     - Yes

   * - ``monitoring.timeSeries.list``
     - Yes

   * - ``resourcemanager.projects.get``
     - Yes, if you want to sync project metadata, such as labels

   * - ``compute.instances.list``
     - Yes, if the Compute Engine service is enabled

   * - ``compute.machineTypes.list``
     - Yes, if the Compute Engine service is enabled

   * - ``spanner.instances.list``
     - Yes, if the Spanner service is enabled

   * - ``storage.buckets.list``
     - Yes, if the Spanner service is enabled

.. _gcp-two:

Part 2: Configure GCP
=====================

.. note:: Repeat the following steps for each project you want to monitor with the GCP integration.

#. In a new window or tab, go to the Google Cloud Platform website and log into your GCP account.
#. Open the GCP web console.
#. Select a project you want to monitor.
#. From the sidebar, select :menuselection:`IAM & admin --> Service Accounts`.
#. Click :guilabel:`Create Service Account` at the top of the screen.
#. Complete the following fields:

   .. list-table::
      :header-rows: 1
      :widths: 40 60

      * - :strong:`Field`
        - :strong:`Description`

      * - Service account name
        - Enter ``Splunk``.

      * - Service account ID
        - This field is autofilled after you enter ``Splunk`` for Service account name.

      * - Service account description
        - Enter the description for your service account.

#. Click :guilabel:`CREATE`.
#. (Optional) Select a role to grant this Service account access to the selected project, then click :guilabel:`CONTINUE`.
#. Enable Key type :guilabel:`JSON`, and then click :guilabel:`CREATE`. A new service account key JSON file is then downloaded to your computer.
#. In a new window or tab, go to :new-page:`Cloud Resource Manager API <https://console.cloud.google.com/apis/library/cloudresourcemanager.googleapis.com?pli=1>` and enable the Cloud Resource Manager API. You need to enable this API so Splunk Infrastructure Monitoring can use it to validate permissions on the service account keys.

.. _gcp-three:

Part 3: Start the integration
==============================

By default, all available services are monitored, and any new services added later are also monitored. When you set integration parameters, you can choose to import metrics from a subset of the available services.

#. On the Splunk Observability Cloud home page, click :guilabel:`Data Setup`. The :strong:`Connect Your Data` page appears.
#. Select :strong:`All`.
#. Select :strong:`GCP` from the list of integration tiles.
#. Click :guilabel:`New Integration`.
#. Enter a name for this GCP integration.
#. Click :guilabel:`Add Project`.
#. Click :guilabel:`Import Service Account Key`.
#. Select one or more of the JSON key files that you downloaded from GCP in step 9 of :ref:`gcp-two`. 
#. Click :guilabel:`Open`. You can then see the project IDs corresponding to the service account keys you selected.
#. To import metrics from only some of the available services, follow these steps: 

   #. Click :guilabel:`All Services` to display a list of the services you can monitor.
   #. Select the services you want to monitor.
   #. Click :guilabel:`Apply`.

#. (Optional): List any additional GCP service domain names that you want to monitor, using commas to separate domain names in the :strong:`Custom Metric Type Domains` field.

   .. note:: For examples of custom metric type domain syntax, see :new-page:`Custom metric type domain examples <https://dev.splunk.com/observability/docs/integrations/gcp_integration_overview#Custom-metric-type-domain-examples>` in the Splunk developer documentation.

12. Set the poll rate to 300 |nbsp| seconds (5 minutes) or 60 |nbsp| seconds (1 minute).
13. (Optional): If you select Compute Engine as one of the services to monitor, you can enter a comma-separated list of Compute Engine Instance metadata keys to send as properties. These metadata keys are sent as properties named ``gcp_metadata_<metadata-key>`` in the :ref:`gce-metrics` table.
14. Click :strong:`Save`.

Your GCP integration is now complete.

If you prefer, you can integrate GCP with Splunk Observability Cloud using the GCP API. See :new-page:`Integrate Google Cloud Platform Monitoring with Splunk Observability Cloud <https://dev.splunk.com/observability/docs/integrations/gcp_integration_overview#Specifying-custom-metric-type-domains>` for details.


StackDriver metrics
===================

After you connect to GCP, metrics from StackDriver under :new-page:`Google Cloud metrics <https://cloud.google.com/monitoring/api/metrics_gcp>` in the StackDriver metric list sync with Infrastructure Monitoring. Agent metrics from AWS or StackDriver do not sync. You can use AWS integration to monitor those metrics.

The metrics from StackDriver contain dimensions that correspond to the Labels described in the Google Cloud metrics reference and the :new-page:`StackDriver Monitored Resource Types <https://cloud.google.com/monitoring/api/resources>` reference. Use the ``monitored_resource`` dimension to determine which metric corresponds to a particular resource.

====================
Ingest GCP log data
====================

Scenario documentation in the GCP Cloud Architecture Center describes both pull- and push-based ways to ingest Google Cloud data, but Splunk Observability Cloud supports only the push-based method.

To export :new-page:`Cloud Logging data <https://cloud.google.com/architecture/exporting-stackdriver-logging-for-splunk>` from GCP to Splunk Observability Cloud, prepare GCP log export by creating a Pub/Sub subscription and using the :strong:`Pub/Sub to Splunk Dataflow template` to create a Dataflow job that pulls messages from the Pub/Sub subscription, converts payloads into Splunk HEC event format, and forwards those payloads to Splunk Observability Cloud, where the whole event (JSON payload and its information) is ingested.

Use the example shown in :new-page:`Option A: Stream logs using Pub/Sub to Splunk Dataflow <https://cloud.google.com/architecture/exporting-stackdriver-logging-for-splunk#deploy_splunk_dataflow_template>`, with the following changes:

- Change the Access token in the sample syntax (``token=your-splunk-hec-token``) to the Splunk Observability Cloud access token instead of the Splunk HEC token. Splunk Observability Cloud exposes the Real-time Data Ingest endpoint to be used instead of Splunk HTTP Event Collector endpoint, and you do not have to create any endpoint manually.

- Revise the URL in the sample syntax to point to the Real-time Data Ingest endpoint for Splunk Observability Cloud as shown in your profile.


.. note::

   Any response code that is not 2xx, including throttling, indicates a message delivery failure. If message delivery fails, you can replay unprocessed messages manually by following the instructions in the "Handling delivery failures" section of :new-page:`GCP documentation <https://cloud.google.com/architecture/deploying-production-ready-log-exports-to-splunk-using-dataflow#replay_failed_messages>` for deploying production-ready log exports to Splunk using Dataflow.


.. _gcp-metrics:

========
Metrics
========

These are the metrics available for the Google Cloud Platform integration with Splunk Observability Cloud, grouped by GCP resource. All metrics are included by default.

Google App Engine metrics
=========================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-appengine/metrics.yaml"></div>

Google BigQuery metrics
=======================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-bigquery/metrics.yaml"></div>

Google Cloud Bigtable metrics
=============================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-cloud-bigtable/metrics.yaml"></div>

Google Cloud Datastore metrics
==============================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-cloud-datastore/metrics.yaml"></div>

Google Cloud Functions metrics
==============================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-cloud-functions/metrics.yaml"></div>

Google Cloud Pub/Sub metrics
============================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-cloud-pubsub/metrics.yaml"></div>

Google Cloud Router metrics
===========================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-cloud-router/metrics.yaml"></div>

Google Cloud Spanner metrics
============================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-cloud-spanner/metrics.yaml"></div>

Google Cloud Storage metrics
============================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-cloud-storage/metrics.yaml"></div>

.. _gce-metrics:

Google Compute Engine metrics
=============================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-compute-engine/metrics.yaml"></div>

Google Container Engine metrics
===============================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-container-engine/metrics.yaml"></div>

Google Kubernetes Engine metrics
================================

.. raw:: html

   <div class="metrics-yaml" category="included" url="https://raw.githubusercontent.com/signalfx/integrations/master/google-kubernetes-engine/metrics.yaml"></div>

For a list of the Google Cloud Platform services that send metrics to Splunk Infrastructure Monitoring, see the GCP list in the :new-page:`Supported Integrations <https://docs.splunk.com/Observability/gdi/get-data-in/integrations.html#google-cloud-platform-services>` topic.
