.. _otel-data-processing:

*********************************************************************
How the Splunk OpenTelemetry Collector uses pipelines to process data
*********************************************************************

.. meta::
      :description: Learn how to process data collected with Splunk Distribution of OpenTelemetry Collector.

A pipeline defines a path the data follows in the Splunk OpenTelemetry Collector starting from reception, then further processing or modification, and finally exiting the Splunk OpenTelemetry Collector through exporters.

Pipelines operate on three data types: logs, traces, and metrics. The data type is a property of the pipeline defined by its configuration. Receivers, exporters, and processors used in a pipeline must support the particular data type, otherwise the "ErrDataTypeIsNotSupported" error message is reported when the configuration is loaded.

There can be one or more receivers in a pipeline. Data from all receivers is pushed to the first processor, which performs processing on it and then pushes it to the next processor and so on until the last processor in the pipeline pushes the data to the exporters. Each exporter gets a copy of each data element. The last processor uses a data fan-out connector to fan out (distribute) the data to multiple exporters.

The pipeline is constructed during collector startup based on the pipeline definition. See :ref:`otel-components`.

Common processing scenarios
=========================================
A pipeline configuration typically looks like this:

.. code-block:: yaml

   service:
     pipelines:
     # Pipelines can contain multiple subsections, one per pipeline.
       traces:
       # Traces is the pipeline type.
         receivers: [otlp, jaeger, zipkin]
         processors: [memory_limiter, batch]
         exporters: [otlp, jaeger, zipkin]

|br|

This example defines a pipeline for ``traces``, with three receivers, two processors, and three exporters. The following table describes the receivers, processors, and exporters used in this example.

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Component
     - Description
     - Pipeline type
   * - Receiver
     - ``otlp``: Receives data through gRPC or HTTP using OTLP format.
     - Traces, metrics, logs
   * - Receiver
     - ``jaeger``: Receives trace data in Jaeger format.
     - Traces
   * - Receiver
     - ``zipkin``: Receives spans from Zipkin (V1 and V2).
     - Traces
   * - Processor
     - ``memory_limiter``: Prevents out of memory situations.
     - Metrics, traces, logs
   * - Processor
     - ``batch``: Accepts spans, metrics, or logs and places them into batches. Batching helps better compress the data and reduce the number of outgoing connections required to transmit the data.
     - Metrics, traces, logs
   * - Exporter
     - ``otlp``: Exports data through gRPC using OTLP format. By default, this exporter requires TLS and offers queued retry capabilities.
     - Traces, metrics
   * - Exporter
     - ``jaeger``: Exports data through gRPC to Jaeger destinations. By default, this exporter requires TLS and offers queued retry capabilities.
     - Traces
   * - Exporter
     - ``zipkin``: Exports data to a Zipkin server. By default, this exporter requires TLS and offers queued retry capabilities.
     - Traces

Processing logs
=========================================

See :ref:`logs-pipeline`.

Processing metrics
=========================================

See :ref:`org-metrics` and :ref:`otel-tags`.

Processing traces
==========================================
See :ref:`apm-traces-spans`.

Metadata transformations
============================================

Metadata refers to the name/value pair added to telemetry data. OpenTelemetry calls this ``Attributes`` on ``Spans``, ``Labels`` on ``Metrics``, and ``Fields`` on ``Logs``. See :new-page:`Resource SDK <https://github.com/open-telemetry/opentelemetry-specification/blob/49c2f56f3c0468ceb2b69518bcadadd96e0a5a8b/specification/resource/sdk.md>`, :new-page:`Metrics API <https://github.com/open-telemetry/opentelemetry-specification/blob/49c2f56f3c0468ceb2b69518bcadadd96e0a5a8b/specification/metrics/api.md>`, and :new-page:`Trace Semantic Conventions <https://github.com/open-telemetry/opentelemetry-specification/blob/52cc12879e8c2d372c5200c00d4574fa73996369/specification/trace/semantic_conventions/README.md>` in GitHub for additional details.

Attributes
--------------------------
Attributes are a list of zero or more key-value pairs. An attribute must have the following properties:

* The attribute key, which must be a non-null and non-empty string.
* The attribute value, which is one of these types:

  * A primitive type: string, boolean, double precision floating point (IEEE 754-1985) or signed 64-bit integer.
  * An array of primitive type values. The array must be homogeneous. That is, it must not contain values of different types. For protocols that do not natively support array values, represent those values as JSON strings.

Attribute values expressing a numerical value of zero, an empty string, or an empty array are considered meaningful and must be stored and passed on to processors or exporters.

Attribute values of ``null`` are not valid and attempting to set a ``null`` value is undefined behavior.

``null`` values are not allowed in arrays. However, if it is impossible to make sure that no ``null`` values are accepted (for example, in languages that do not have appropriate compile-time type checking), ``null`` values within arrays MUST be preserved as-is (that is, passed on to span processors/exporters as ``null``). If exporters do not support exporting ``null`` values, you can replace those values by 0, ``false``, or empty strings. Changing these values is required for map and dictionary structures represented as two arrays with indices that are kept in sync (for example, two attributes ``header_keys`` and ``header_values``, both containing an array of strings to represent a mapping ``header_keys[i] -> header_values[i]``).

Labels
-----------------------------------------
Labels are name/value pairs added to metric data points. Labels are deprecated from the OpenTelemetry specification. Use attributes instead of labels.

Fields
---------------------------------------
Fields are name/value pairs added to log records. Each record contains two kinds of fields:

* Named top-level fields of specific type and meaning.
* Fields stored as ``map<string, any>``, which can contain arbitrary values of different types. The keys and values for well-known fields follow semantic conventions for key names and possible values that allow all parties that work with the field to have the same interpretation of the data.
