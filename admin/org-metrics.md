(org-metrics)=

# View organization metrics for Splunk Observability Cloud

Splunk Infrastructure Monitoring provides the following data for measuring your usage:

* Ingest metrics: Measure the data you're sending to Infrastructure Monitoring, such as the number of data points you've sent

* App usage metrics: Measure your use of application features, such as the number of dashboards in your organization

* Integration metrics: Measure your use of cloud services integrated with your organization, such as the number of calls to the AWS CloudWatch API

* Metrics that measure your use of resources that you can specify limits for, such as the number of custom metric time series (MTS) you've created

You're not charged for these metrics and they do not count against any limits.


## View organization metrics

If you're an admin, you can view some of these metrics in built-in charts on the Organization Overview page.

To access the Organization Overview page, follow these steps:

1. Open the Observability Cloud main menu.

2. Hover over **Organization Settings** and select **Organization Overview**.

3. Click the **DATA INGEST** tab to view metrics about the volume and nature of data being processed and stored, such as the number of data points or events received per minute.

4. Click the **ENGAGEMENT** tab to view metrics about users and the entities they've created. Observability Cloud displays metrics for the following entities:

    - Charts

    - Detectors

    - Dashboards

    - Dashboard groups

    - Teams

To view these metrics in custom charts, you don't have to be an admin.


## Interpret and work with usage metrics

This section provides tips that can help you interpret and work with usage metrics.


### Metrics for values by token

In some cases, Infrastructure Monitoring has two similar metrics:

* One metric, such as `sf.org.numAddDatapointCalls`, represents the total across your entire organization.

* The similar metric, `sf.org.numAddDatapointCallsByToken` represents the total for each unique access token you use.

The sum of all the by token metric values for a measurement might be less than the total value metric value. For example, the sum, of all `sf.org.numAddDatapointCallsByToken` values might be less than the value of `sf.org.numAddDatapointCalls`. The sums differ because Infrastructure Monitoring doesn't use a token to retrieve data from cloud services you've integrated. Infrastructure Monitoring counts the data point calls for the integrated services, but it doesn't have a way to count the calls for any specific token.

This difference in values applies to AWS CloudWatch, GCP StackDriver, AppDynamics, and New Relic integrations.


### Metrics with values for each metric type

Some metrics have a value for each metric type (counter, cumulative counter, or gauge), so you have three MTS per metric. Each MTS has a dimension named `category` with a value of `COUNTER`, `CUMULATIVE_COUNTER`, or `GAUGE`. Because you can have multiple MTS for these metrics, you need to use the `sum()` SignalFlow function to see the total value.

For example, you might receive three MTS for `sf.org.numMetricTimeSeriesCreated`, one for the number of MTS that are counters, another for the number of MTS that are cumulative counters, and a third for the number of MTS that are gauges.

Also, you can filter by a single value of `category`, such as `GAUGE`, to see only the metrics of that type.


### A metric that counts stopped detectors

The metric `sf.org.numDetectorsAborted` monitors the number of detectors that Infrastructure Monitoring stopped because the detector reached a resource limit. In most cases, the detector exceeds the limit of 250K MTS. This condition also generates the event `sf.org.abortedDetectors`, which records details including the detector ID, the reason it stopped, and the value or limit of MTS or data points, whichever caused the detector to stop.

To learn more, see [Add context to metrics using events](../alerts-detectors-notifications/view-data-events.rst).


### Metrics that track system limits

These metrics track limits that Infrastructure Monitoring enforces for your organization:



* `sf.org.limit.activeTimeSeries` (gauge): Maximum number of active MTS, within a moving window of the past 25 hours, that your organization can have. If you exceed this limit, Infrastructure Monitoring stops accepting data points for new MTS, but continues to accept data points for existing MTS. To monitor your usage against the limit, use the metric `sf.org.numActiveTimeSeries`.

* `sf.org.limit.containers` (gauge): Maximum number of containers that can send data to your organization. This limit is higher than your contractual limit to allow for burst and overage usage. If you exceed this limit, Infrastructure Monitoring drops data points from new containers but keeps accepting data points for existing containers. To monitor your usage against the limit, use the metric `sf.org.numResourcesMonitored` and filter for the dimension `resourceType:containers`.

* `sf.org.limit.customMetricTimeSeries` (gauge): Maximum number of active custom MTS, within a moving window of the previous 60 minutes, that can send data to your organization. If you exceed this limit, Infrastructure Monitoring drops data points for the custom MTS that exceeded the limit, but it continues to accept data points for custom MTS that already existed. you’ve defined, use the metric `sf.org.numCustomMetrics`. To learn more about custom MTS, see the section [About custom, bundled, and high-resolution metrics](https://docs.splunk.com/Observability/admin/monitor-imm-billing-usage.html#about-custom-bundled-and-high-resolution-metrics) in the user documentation.

* `sf.org.limit.detector` (gauge): Maximum number of detectors that you can use for your organization. After you reach this limit, you can't create new detectors. To monitor the number of detectors you create, use the metric `sf.org.num.detector`.

* `sf.org.limit.hosts` (gauge): Maximum number of hosts that can send data to your organization. The limit is higher than your contractual limit to allow for burst and overage usage. If you exceed this limit, Infrastructure Monitoring drops data points from new hosts but keeps accepting data points for existing hosts. To monitor your usage against the limit, use the metric `sf.org.numResourcesMonitored` and filter for the dimension `resourceType:hosts`.

* `sf.org.limit.metricTimeSeriesCreatedPerMinute` (gauge): Maximum rate at which you can create new MTS in your organization, measured in MTS per minute. If you exceed this rate, Infrastructure Monitoring stops accepting data points for new MTS, but continues to accept data points for existing MTS. To monitor the number of metrics you've created overall, use the metric `sf.org.numMetricTimeSeriesCreated`.


### Compare `gross` and `num` metric values

Some metrics report a `gross` value and a `num` value. Compare the `gross` and `num` values of a metric to learn about how the system limits or filters data for whatever the metric represents.

A `gross` metric reports the total number of data points the system receives before any throttling or filtering kicks in.

A `num` metric reports the total number of data points the system receives after it completes any throttling or filtering.


## Metric Details

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/integrations/master/signalfx-org-metrics/metrics.yaml"></div>

## Troubleshooting

```{include} /_includes/troubleshooting.md
```
