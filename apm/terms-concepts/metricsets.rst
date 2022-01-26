.. _apm-metricsets:

**********
MetricSets
**********

.. meta::
   :description: Learn about MetricSets in Splunk Observability Cloud. MetricSets are categories of metrics about traces and spans in Splunk APM.

MetricSets are categories of key indicators, such as request rate, error rate and durations, calculated based on your traces and spans in Splunk APM. There are two categories of MetricSets: Troubleshooting MetricSets (TMS), used for high-cardinality troubleshooting, and Monitoring MetricSets (MMS), used for real-time monitoring.

MetricSets are specific to Splunk APM, but Monitoring MetricSets are similar to the metric time-series used in Splunk Infrastructure Monitoring to populate charts and generate alerts. See :ref:`monitoring-metricsets` below for more information.

.. _troubleshooting-metricsets:

Troubleshooting MetricSets
==========================

Troubleshooting MetricSets (TMS) are metric time series used for high-cardinality troubleshooting of identities in APM and for historical comparison among spans and workflows. Splunk APM generates Troubleshooting MetricSets based on indexed span tags.To learn how to index span tags to generate new Troubleshooting MetricSets, see :ref:`apm-index-span-tags`.

The following metrics are generated for every identity in your service map: 

-  Request rate
-  Error rate
-  Root cause error rate
-  Latency (minimum, maximum, P50, P90, P99)

These TMS appear in the Troubleshooting page, which contains the service map, and in Tag Spotlight. You can use TMS to filter the service map and perform breakdowns across the values of a given indexed span tag. 

TMS are stored for the same amount of time as raw traces; by default, this retention is 8 days. 

For more details about Troubleshooting MetricSets, see :ref:`apm-index-tag-tips`. 

.. _monitoring-metricsets:

Monitoring MetricSets
=====================

Monitoring MetricSets (MMS) are a collection of 12 of the :ref:`metric time series <metric-time-series>` used in Splunk Infrastructure Monitoring. MMS are used to monitor and alert on the performance of your services in real time. They power the real-time APM Landing Page and the dashboard view, and are the metrics that detectors monitor and use to generate alerts. They are stored for thirteen months by default. 

While TMS are generated for every identity in APM, MMS are available only for a specific endpoint, for the aggregate of all endpoints in a service, or for a specific Business Workflow. 

Endpoint-level MMS reflect the activity of a single endpoint in a service, while service-level MMS aggregate the activity of all of the endpoints in the service. Endpoint-level and service-level MMS are therefore limited to spans where the ``span.kind = SERVER`` or ``span.kind = CONSUMER``.
Spans may lack or have a different a ``kind`` value when they originate in self-initiating operations or inferred services, or when there has been an error in instrumentation. 

Workflow-level MMS aggregate data from all traces identified as part of a given Business Workflow. 

Comparing Monitoring MetricSets and Troubleshooting MetricSets
---------------------------------------------------------------

Because endpoint-level and service-level MMS include a subset of the metrics that TMS include, you may notice that metric values for a service are slightly different depending on the context in Splunk APM. 
This is because the dashboard view is powered by MMS, which are restricted to the ``SERVER`` or ``CONSUMER`` ``kind``, while the Troubleshooting and Tag Spotlight views are powered by TMS, which do not restrict the metrics they collect. For example, values for ``checkout`` service metrics displayed in the host dashboard may be different from the metrics displayed in the Troubleshooting view because there are multiple span ``kind`` values associated with this service that the MMS powering the dashboard do not monitor.

If you want to restrict your TMS to endpoint-only data so that you can compare MMS and TMS directly, simply filter to a specific endpoint or break down the service map by endpoint. 

Metrics and Dimensions of Monitoring MetricSets
-----------------------------------------------

Each MMS has a set of metrics and dimensions for spans and traces you can use to monitor and alert on service performance. 

To prevent overcounting metrics in aggregations, the built-in dashboards and charts in Splunk APM automatically exclude custom dimensionalized MMS. 
Custom dimensionalized MMS have a marker dimension, ``sf_dimensionalized:true``, to enable this filtering.
When you create your own dashboards and charts, you can exclude custom dimensionalized MMS by adding a filter on ``!sf_dimensionalized:true``. 
If you do want to look at the time series of a custom dimensionalized MMS in your charts, filter on ``sf_dimensionalized:true`` and then aggregate by the custom dimension you want to look at. 

The following tables provide the metrics and dimensions for MMS based on services, workflows, spans, and traces:

Service metrics and dimensions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1

   * - :strong:`Metrics`
     - :strong:`Dimensions`

   * - - ``service.request.count``
       - ``service.request.duration.ns.min``
       - ``service.request.duration.ns.median``
       - ``service.request.duration.ns.max``
       - ``service.request.duration.ns.p90``
       - ``service.request.duration.ns.p99``
     - - ``sf_environment``
       - ``sf_service``
       - ``sf_error``

Workflow metrics and dimensions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1

   * - :strong:`Metrics`
     - :strong:`Dimensions`

   * - - ``workflows.count``
       - ``workflows.duration.ns.min``
       - ``workflows.duration.ns.median``
       - ``workflows.duration.ns.max``
       - ``workflows.duration.ns.p90``
       - ``workflows.duration.ns.p99``
     - - ``sf_environment``
       - ``sf_workflow``
       - ``sf_error``

Span metrics and dimensions
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1

   * - :strong:`Metrics`
     - :strong:`Dimensions`

   * - - ``spans.count``
       - ``spans.duration.ns.min``
       - ``spans.duration.ns.median``
       - ``spans.duration.ns.max``
       - ``spans.duration.ns.p90``
       - ``spans.duration.ns.p99``
     - - ``sf_environment``
       - ``sf_service``
       - ``sf_operation``
       - ``sf_kind``
       - ``sf_error``
       - ``sf_httpMethod``, where relevant

Trace metrics and dimensions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1

   * - :strong:`Metrics`
     - :strong:`Dimensions`

   * - - ``traces.count``
       - ``traces.duration.ns.min``
       - ``traces.duration.ns.median``
       - ``traces.duration.ns.max``
       - ``traces.duration.ns.p90``
       - ``traces.duration.ns.p99``
     - - ``sf_environment``
       - ``sf_service``
       - ``sf_operation``
       - ``sf_httpMethod``
       - ``sf_error``

Default identities and MetricSets
=================================

The total number of Troubleshooting MetricSets is the sum of all identities. The total number of Monitoring MetricSets is the sum of ``Service``, ``Endpoint``, and ``Workflow`` identities.

Splunk APM creates the following :ref:`identities <apm-identities>` and MetricSets:

.. list-table::
   :header-rows: 1

   *  -  :strong:`APM object`
      -  :strong:`Description`
      -  :strong:`Identity example`
      -  :strong:`Troubleshooting MetricSet`
      -  :strong:`Monitoring MetricSet`

   *  -  Service
      -  Identities for all services
      -  Service |hyph| 1
      -  Yes
      -  Yes

   *  -  Endpoint
      -  Identities for all endpoints
      -  Service |hyph| 1.Endpoint |hyph| 1.HTTPMethod

         Service |hyph| 1.InitEndpoint |hyph| 1 `if HTTPMethod is absent`
      -  Yes
      -  Yes

   *  -  Workflow
      -  Identities for all initiating endpoints
      -  Service |hyph| 1.InitEndpoint |hyph| 1.HTTPMethod

         Service |hyph| 1.InitEndpoint |hyph| 1 `if HTTPMethod is absent`
      -  Yes
      -  Yes

   *  -  Edge
      -  Identities for all edges between services
      -  Service |hyph| 1.Endpoint |hyph| 1.HTTPMethod |hyph| >Service |hyph| 2.Endpoint |hyph| 2.HTTPMethod
      -  Yes
      -  No

   *  -  Operation
      -  Identities for all spans within services
      -  Service |hyph| 1.Operation |hyph| 1
      -  No
      -  No