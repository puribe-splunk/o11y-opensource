.. _apm-traces-spans:

****************
Traces and spans
****************

.. meta::
  :description: Learn about traces and spans in Splunk Observability Cloud. Spans are operations, and traces are collections of spans.

A trace is a collection of operations that represents a unique transaction handled by an application and its constituent services. A span is a single operation within a trace.

Each span has a name that represents the operation captured by the span and a service name that represents where the operation took place. Additionally, spans may refer to another span as their parent, defining a relationship between operations the trace captures to process that transaction. Spans can also include additional information and context with span tags. 

Each span contains information about the method, operation, or block of code that it captures, including these characteristics:

-  The service name 

-  The operation name

-  The start time of the operation

-  The duration of the operation

-  The name and IP address of the service where the operation took place

.. _apm-4xx-errors:

How Splunk APM handles 4xx errors
====================================

By default, Splunk APM doesn't display traces with ``4xx`` status codes as errors, just ``5xx`` status codes. This is because ``4xx`` status codes could be the result of issues with requests and not issues with services handling requests.

For example, if a user makes a request to ``endpoint/that/does/not/exist``, the ``404`` status code the service returns is not an error with the service, but with the request. Similarly, if a user tries to access a resource they don't have access to, the service could return a ``401`` status code, which is typically not the result of an error on the server side.

Depending on your application's logic, a ``4xx`` status code could be an error, particularly for client-side requests. There are a couple ways to address this:

- Break down performance by HTTP status code span tags, if available.

- Configure custom instrumentation to report ``4xx`` status codes as errors.

.. _span-tags:

Span tags
=============================================================================

.. note:: Note that span tags in Splunk APM are distinct from metadata tags in Splunk Infrastructure Monitoring, which are searchable labels or keywords you can assign to metric dimensions in the form of strings rather than as key-value pairs. To learn more about metadata tags, see :ref:`metadata-tags`.

Spans can include additional, free-form metadata to provide information and context about the operations that they represent in the form of span tags. Use span tags to query and filter traces or to provide extra information about each operation when inspecting the spans of a trace during troubleshooting.

Span tags are key-value pairs that correspond to a span. Span tag keys for a single span have to be unique. Both keys and values are strings. Span tags are most useful when they follow a simple, dependable system of naming conventions. For reference, see :ref:`span-tag-naming` to learn about OpenTelemetry naming conventions for span tags. 

Each span contains default span tags, but you can also add custom span tags when you instrument an application. For more information about using span tags to analyze service performance, see :ref:`apm-span-tags`.

Limits on spans and traces
=============================================================================

These are the limits for span and trace length and count in Splunk APM:

.. list-table::
   :header-rows: 1

   * - :strong:`Attribute`
     - :strong:`Description`

   * - Length 
     - The total length of all span tag keys, span tag values, and span annotations can't exceed 64 kB.

   * - Count
     - The maximum number of spans for each trace is 10,000 spans per trace.


Refresh the list of traces
============================================

The list of traces for a service does not refresh unless the time frame is changed. If you refresh your browser, the list does not get updated because it is a sharable URL that is valid for 8 days. 

APM stores a given set of results attached to the shareable URL. To refresh the results, change the time frame or remove the ``jobId`` parameter from the URL.
