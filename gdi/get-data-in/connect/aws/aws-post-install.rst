.. _aws-post-install:


****************************************
Leverage data from integration with AWS
****************************************

.. meta::
   :description: After connecting your AWS account to Splunk Observability Cloud, you can perform the actions described in this topic.


You can use Splunk tooling to monitor, collect, process, and send AWS data after you integrate your Amazon Web Services (AWS) with Splunk Observability Cloud.

Explore your AWS inventory and data
====================================

Review your AWS inventory in Splunk Observability Cloud:

- If you have access to Splunk Log Observer and selected the CloudWatch Logs option during configuration, you can review detailed log information. Select the :menuselection:`Log Events` tab and click :guilabel:`Explore Log Events` to view more details using Splunk Log Observer.

  For more information, see :ref:`Introduction to Splunk Log Observer <get-started-logs>`.

- Select the :menuselection:`Metric Data` tab and click :guilabel:`Explore Metric Data` to access the Infrastructure Monitoring navigators and explore the collection of technologies used to build, run, and deploy applications in your data ecosystem. See :ref:`Use navigators in Splunk Infrastructure Monitoring <use-navigators>` for more information.


Explore your AWS data using default dashboards:

1. To access these dashboards, click :guilabel:`Menu` and select :guilabel:`Dashboards`. The Dashboards page displays. See :ref:`Dashboards in Splunk Observability Cloud <dashboards>` for details.

2. Search for :guilabel:`AWS`. Several AWS dashboard groups display.

3. Click a link to access a dashboard.


Locate the metrics that you want to monitor
============================================

- Use the Metric Finder to get a list of categories you can browse, drawn from your integrations or custom categories, if configured. See :ref:`Metric Finder <metrics-finder-and-metadata-catalog>` for details.
- Use the Metadata Catalog to find metadata associated with the metrics you send to Splunk Infrastructure Monitoring. See :new-page:`Use the Metadata Catalog <https://docs.splunk.com/Observability/metrics-and-metadata/metrics-finder-metadata-catalog.html#use-the-metadata-catalog>` for details.

Create detectors and alerts
================================

You can create detectors and alerts based on your AWS data.

- Detectors define rules for identifying conditions of interest and the notifications to send when those conditions occur or stop occurring.

- Alerts indicate that incoming data has triggered one of your detectors.

See :ref:`Introduction to alerts and detectors in Splunk Observability Cloud <get-started-detectoralert>` for details.

Collect, process, and send data
====================================

Splunk Observability Cloud uses OpenTelemetry to support efficient instrumentation so that you can see your metrics, traces, and logs.

If you haven't already done so, you can install the Splunk Distribution of OpenTelemetry Collector <otel-install-platform>` to collect, process, and send data. See :ref:`Install the Splunk OpenTelemetry Collector <otel-install-platform>` for details.

You can also set up Splunk APM :ref:`Splunk APM <get-started-apm>` to monitor traces from your applications, provided you've already installed the Splunk OpenTelemetry Collector. See :ref:`Introduction to Splunk APM <get-started-apm>` for details.

Verify your metrics collection method
======================================

You can use either Splunk Observability Cloud or your AWS CloudWatch console to confirm whether your metrics are collected by polling or by CloudWatch metric streams:

- In Observability Cloud: Use the :ref:`Plot Editor <specify-signal>` for Splunk Infrastructure Monitoring to select the org metric ``sf.org.num.awsServiceCallCount`` and filter by the ``method`` property using check boxes to select the following values: ``putMetricStream``, ``getMetricData``, ``getMetricStatistics``, ``getMetricStream``.
- In the AWS CloudWatch console: At :guilabel:`All > Usage > By AWS resource > CallCount`, open the ``CallCount`` metric and use check boxes to select the CloudWatch service resources (values)  ``putMetricStream``, ``getMetricData``, ``getMetricStatistics``, ``getMetricStream``.
