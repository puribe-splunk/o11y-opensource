.. _apm-span-tags:

*************************************************************
Analyze services with span tags in Splunk APM
*************************************************************

.. meta::
   :description: Learn how to use span tags to add additional context about the operations that spans represent, so that you can drill deeply into service performance. Using these tags, you can analyze requests, errors, and latency for spans that contain specific span tags.

.. toctree::
   :hidden:

   add-context-trace-span
   index-span-tags
   tag-spotlight
   index-tag-tips

:guilabel:`Span tags` are key-value pairs added to spans through instrumentation. In OpenTelemetry, these key-value pairs are known as :guilabel:`attributes`.

Use span tags to add additional context about the operations that spans represent, so that you can drill deeply into service performance.

.. raw:: html

  <embed>
    <h2>How span tag indexing generates Troubleshooting MetricSets</h2>
  </embed>

Default span tags include attributes such as the endpoint, operation, and HTTP method associated with a span. Using these tags, you can analyze requests, errors, and latency for spans that contain specific span tags. This context lets you understand service performance at a glance and helps you discover the root cause of issues faster.

When you index a span tag, APM generates Troubleshooting MetricSets (TMS), which are high cardinality metrics used for troubleshooting.

.. note::
  See :ref:`otel-span-tags` to learn how to add span tags as attributes in the Splunk OpenTelemetry Collector configuration YAML file.

For an overview of MetricSets in APM, see :ref:`apm-metricsets`.

.. raw:: html

  <embed>
    <h2>Use cases for indexed span tags and TMS</h2>
  </embed>


Indexing span tags lets you analyze services in the following ways:

- Break down service performance by indexed tags in the :ref:`service map<apm-service-map>` so that you can see which specific services attributes are associated with performance issues.

- View charts of service performance metrics by indexed span tags in :ref:`Tag Spotlight <apm-tag-spotlight>` to discover relationships between specific span tag values and service behaviors.

- Associate logically related spans so that you can track multiple traces for a specific activity using :ref:`Business Workflows <apm-workflows>`.

.. raw:: html

  <embed>
    <h2>Analyze your services using span tags</h2>
  </embed>

Follow these steps to add, index, and start troubleshooting with span tags:

1. Before you can index any span tags, you need to add span tags to the spans you send to APM. See :ref:`apm-add-context-trace-span` to learn how.
2. To learn how to generate TMS, see :ref:`apm-index-span-tags`.
3. To learn how to use Tag Spotlight, see :ref:`apm-tag-spotlight`.
4. For details on how APM defines span tags and information to help you decide which span tags to index, see :ref:`apm-index-tag-tips`.

You can also download traces and filter or process data by span tags manually. To learn how to download a trace, see :ref:`apm-download-traces`.

.. note::
  Your Splunk Observability Cloud contract may determine the number of span tags you can index to generate Troubleshooting MetricSets. If you choose not to index any additional span tags, you can still view tags in your spans when analyzing whole traces.
