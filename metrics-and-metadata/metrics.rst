.. _get-started-metrics:

**********************************************************************
Introduction to metrics
**********************************************************************

.. meta::
   :description: description.


A metric is a measurable number that varies over time. Multiple sources or emitters, such as host machines, usually report the same metric. Infrastructure Monitoring can store dimension metadata for these sources or emitters. A combination of a metric, a set of dimension names and values, and a set of data points forms a metric time series.

Individual metric values in an MTS are called data points.

- CPU utilization % of a server
- Response time in milliseconds of an API call
- The number of unique users who logged in over the previous 24-hour period

For more information about metrics and the metadata available for finding, filtering, and aggregating data for building charts and configuring detectors for alerting, see :ref:`metrics-dimensions-mts`.


Metric time series
===================
A metric time series (MTS) is defined by the unique combination of a metric and a set of dimensions. The most common dimension is a source, for example, a host or instance for infrastructure metrics, or an application component or service tier for application metrics. The output of analytics pipelines is a stream object in SignalFlow.

The same metric can be reported from multiple sources or emitters, and each unique combination of a source and a metric is referred to as a metric time series in Infrastructure Monitoring. For example, suppose you report on the CPU utilization of 10 hosts in a cluster. Then, CPU utilization is the metric and you have time series that represent the CPU utilization over time for each host.

Data points
============

The individual measurements that make up a time series are data points. A data point is a single reported value of a metric from a particular source at a specific point in time. Each data point sent to Infrastructure Monitoring contains four pieces of information: metric type, metric name, metric value, and zero or more dimensions. For example, a data point can be the CPU utilization of server ‘foo’ or the count of API calls for service ‘bar’ on 12 March 2015 at 10:05:15 pm.


Metric type
===========

One of three predefined types: counter, cumulative counter, or gauge. The metric type specified determines the default visualization rollup that Infrastructure Monitoring uses when combining multiple values of this metric for longer time scales. For more information on Metric types, see :ref:`metric-types`.

Metric name
===========

A metric name identifies the values that you send into Infrastructure Monitoring. For example: :code:`memory.free`, :code:`CPUUtilization`, :code:`transaction.cost`, and :code:`page_visits`. For information on naming constraints, see name requirements. For more information, see :ref:`metric-dimension-names`.


Metric value
============

The actual measurement from your system, represented as a number.

Dimensions
===========

Key/value pairs that are used to identify the source of the data point and other useful contextual information. Common examples of dimensions include host names, the environment from which the metric is being generated, or the name of the service with which it is associated.

Dimensions are one of several forms of metadata in Infrastructure Monitoring that are useful in aggregating and filtering metrics. For example, sending an environment dimension with each data point would allow you to suppress alerts on metrics coming from the test environment, but direct alerts from production to your operations team’s preferred messaging system.

