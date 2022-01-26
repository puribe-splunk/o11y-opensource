.. _metrics-dimensions-mts:

*****************************************************************
Metrics, dimensions, and metric time series
*****************************************************************

.. meta::
    :description: Data coming into Splunk Observability Cloud has a metric name, and the data can also contain metadata. A set of data points that have the same metric name and metadata constitute a metric time series.

Data comes into Splunk Observability Cloud as data points associated with a metric name and additional metadata. The
most important metadata for a metric is a collection of key-value pairs called dimensions. A set of data points that
have the same metric name and dimensions constitute a metric time series.

.. _metrics:


Metrics
============================================================================

Metrics are measurable numeric values that change over time. The number of HTTP errors as a percentage of all requests
in a one-second time interval is an example of a metric. Another example is the number of network packets sent in
a ten-minute interval. The name of a machine isn't a metric, because it's not numeric and it usually doesn't change over time.

Metrics always have a metric name. The software that reports the measurement also sets this name.
For example, both AWS and OpenTelemetry Collector both send metrics to which they assign names.
You can also create your own metric names for data you send using the Observability Cloud API.

The metric name usually identifies the nature of the measurement. For example, the AWS metric ``4xxErrorRate`` represents
the percentage of all HTTP requests for which the HTTP status code is 4xx. Observability Cloud receives data for this metric when
you connect Observability Cloud and AWS.

.. _metrics-metadata:


Metadata
================================================================================

Observability Cloud has three  types of metadata:

.. list-table::
   :header-rows: 1
   :widths: 25, 50

   * - :strong:`Metadata`
     - :strong:`Description`
   * - Dimensions
     - You can use dimensions to analyze metric time series. You can also create charts for each dimension.
   * - Tags
     - Tags are useful when working with metric time series (MTS). For example, you can filter MTS by tag :code:`test_system` to identify metric streams from test hosts.
   * - Custom properties
     - The most common use of a custom dimension is to rename a dimension.
  
Considerations for metadata
---------------------------
The following table outlines more information on metadata criteria:

.. list-table::
   :header-rows: 1
   :widths: 25, 50

   * - :strong:`Metadata`
     - :strong:`Description`
   * - Dimensions
     -
        * Created when Observability Cloud ingests data.
        * Comprised of a key:value pair
        * Two key:value pairs with different keys are different dimensions, regardless of value.
        * Two key:value pairs that have the same key but different values are different dimensions.
        * Two key:value pairs with the same key and value are the same dimension.
        * You can filter or group by custom properties. 
   * - Tags
     -
       * Defined through the user interface or REST API.
       * Metadata for a dimension, chart, or detector.
       * Applied to a dimension, chart, or detector.
       * Tags are string values.
       * You can filter on tags. 
   * - Custom properties
     -
       * Defined through the user interface or REST API.
       * Metadata for a dimension, tag, chart, or detector.
       * Applied to a dimension, tag, chart, or detector.
       * You can filter or group by custom properties.  



* You can apply custom properties to tags as well as dimensions. For example, if you associate the :code:`tier:web` custom property with the :code:`apps-team` tag, any metric or dimension that you tag with :code:`apps-team` also gets the :code:`tier:web` custom property.

* The optional description property lets you provide a detailed description of a metric, dimension, or tag. You can use up to 1024 UTF-8 characters for a description


.. _metadata-dimension:

Dimensions
================================================================================
Monitoring software that sends metrics also sends metadata for the metrics. The most important metadata for a metric is its
set of key-value pairs that provide additional information about the metric. An example of this kind of metadata is
the name of the host that sent the metric; in this case, the host can send the metadata in the form ``"hostname": "host1"``.

The key-value pairs for this metadata are known as dimensions.

Observability Cloud sometimes assigns different names to dimensions that come from an integration or monitor. For example,
the AWS EC2 integration sends the ``instance-id`` dimension, and Observability Cloud renames the dimension to
``aws_instance_id``. The renamed dimension is also known as a custom property.

Dimensions criteria
----------------------

You may define up to 36 dimensions per MTS

Dimension name criteria:

- UTF-8 string, maximum length of 128 characters (512 bytes).
- Must start with an uppercase or lowercase letter.
- Must not start with an underscore (_).
- After the first character, the name may contain letters, numbers, underscores (_) and hyphens (-).
- Must not start with the prefix ``sf_``, except for dimensions defined by Observability Cloud such as ``sf_hires``.
- Must not start with the prefix ``aws_``, ``gcp_``, or ``azure_``.

.. _data-points:

Data points
================================================================================

Data points are the measurements of a metric at a specific point in time. They
contain the following information:

* Metric type
* Metric name, set by the software that sent the data
* Measurement
* Dimensions, set by the software that sent the data
* Timestamp. Either the software that sent the data sets the timestamp, or
  Observability Cloud sets it to the time at which the data arrives. The
  timestamp is expressed in \*nix time.

.. _metric-time-series:

Metric time series
================================================================================

A metric time series (MTS) is a set of data points that all have the
same metric type, metric name, and dimensions. The dimension is usually one or
more descriptions of the source of the metric, such as the host name or
application name.

A single MTS needs to have the same dimension keys and values. For that reason
the following sets of data points are in three separate MTS:

1. Gauge metric ``cpu.utilization``, dimension ``"hostname": "host1"``
2. Gauge metric ``cpu.utilization``, dimension ``"source_host": "host1"``
3. Gauge metric ``cpu.utilization``, dimension ``"hostname": "host2"``

* MTS 2 has the same host value as MTS 1, but not the same host name.
* MTS 3 has the same host name as MTS 1, but not the same hostname value.





.. _custom-properties:

Custom properties
===================

The Splunk Observability Cloud sometimes assigns different names to dimensions that come from an integration or monitor. For example,
the AWS EC2 integration sends the ``instance-id`` dimension, and Observability Cloud renames the dimension to
``aws_instance_id``. The renamed dimension is also known as a custom property. For more information on naming conventions, see :ref:`Guidance for metric and dimension names
<metric-dimension-names>`.

The :ref:`metric-dimension-names` section describes how Observability Cloud uses custom properties to
rename dimensions generated by monitoring software. You can also assign your own custom properties to
existing MTS. For example, you can add the custom property ``"use": "QA"`` to an MTS
to indicate that the host that is sending the data is used for QA.

You can also add custom properties to charts and detectors. In this case, the custom properties
identify some aspect of the chart or detector that you can query.

Custom properties criteria
----------------------------

You may define up to 75 custom properties per dimension.

Custom property name and value criteria:

* Names must be UTF-8 strings with a maximum length of 128 characters (512 bytes).
* Values must be UTF-8 strings with a maximum length of 256 characters (1024 bytes).

In custom property values, Observability Cloud stores numbers as numeric strings.


.. _metadata-tags:

Tags
==========

.. note:: Note that metadata tags in Splunk Infrastructure Monitoring are distinct from span tags in Splunk APM, which are key-value pairs added to spans through instrumentation to provide information and context about the operations that the spans represent. To learn more about span tags, see :ref:`span-tags`.

Tags are labels or keywords that you assign to dimensions. A tag is a string
rather than a key-value pair. Use tags when you want to give the same
searchable value to multiple dimensions.

For example, suppose you have
multiple hosts, each of which is running a set of apps. To identify the apps that a particular host is running,
create a tag for each app, then apply one or more of these tags to the host:name dimension to specify the apps that
are running on the named host.

You can also apply custom properties to tags. When you do this, anything
that has that tag inherits the properties associated with the tag. For
example, if you associate the ``"tier:web"`` custom property with the
``"apps-team"`` tag, Observability Cloud attaches the ``"tier:web"`` custom property to
any metric or dimension that has the ``"apps-team"`` tag.

You can assign tags to charts and detectors and use them the same as
with dimensions.


Tags criteria
----------------
Tags are UTF-8 strings with a maximum length of 256 UTF-8 characters/1024 bytes.

* You may have up to 50 tags per dimension.
* You may have up to 50 tags per custom property.
* You may only have 50 tags per chart or detector
