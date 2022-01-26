.. _apm-inferred-services:

**********************************
View inferred services
**********************************

.. meta::
   :description: Download traces from Splunk APM in JSON.

In certain cases, a remote service might not have tracing enabled, either because it is not instrumented yet, or because instrumentation is not possible. In these cases, Splunk APM can infer the presence of the remote service, or inferred service, if the span calling this remote service has the necessary information. 

Inferred services often include external service providers, pub/subs, Remote Procedure Calls (RPCs), and databases.

.. Splunk APM provides additional analytics based on inferred SQL databases. See <db-visibility> for more information about database insights in Splunk APM. 

Splunk APM adds an inferred span to the relevant trace to represent an operation occurring in an inferred service. For further details on how services are inferred, see :ref:`how-apm-infers-services` below.

What information is available about inferred services?
========================================================
Because inferred services are not instrumented, Splunk APM only has access to spans from instrumented services that call inferred services. Splunk APM places inferred services in their inferred locations in the service map and provides :ref:`Troubleshooting MetricSets<troubleshooting-metricsets>` (TMS), or request, error, and duration (RED) metrics, based on the associated client-side spans. You can continue to troubleshoot with these metrics in :ref:`Tag Spotlight<apm-tag-spotlight>`. If the inferred service is servicing other uninstrumented services, these reported RED metrics might not provide a complete picture of the inferred service's interactions. 

Splunk APM does not create :ref:`Monitoring MetricSets<monitoring-metricsets>` (MMS) for inferred services because MMS are created in real time from spans where ``span.kind = SERVER`` or ``span.kind = CONSUMER``, and Splunk APM does not receive server- or consumer-side spans from uninstrumented services.  Even though inferred services do appear in the list of services on the landing page, you cannot see real-time data on inferred services in the APM landing page or in dashboards and alerts, and you cannot create detectors based on inferred services. 

.. _inferred-service-map:

Viewing inferred services in the service map
---------------------------------------------
Inferred services appear in their inferred locations in the service map. When you click on an inferred service in the service map, you can view its service type, service name(s), and request, error rate, and latency (RED) metrics. Note that these metrics are based on referring spans from instrumented services and may not provide a complete picture of the inferred service's interactions.

.. _inferred-service-trace-view:

Viewing inferred services in trace view
---------------------------------------------
Inferred services and databases also appear as spans in the Trace Waterfall view. They are shown in a gray box with italicized print, as in the following screenshot. 


..  image:: /_images/apm/inferred-services/inferred-service-trace-view.png
    :width: 95%
    :alt: This screenshot shows an example of inferred spans appearing in Trace View. 

When you click on an inferred span in the Trace Waterfall, it expands to show the metadata of the corresponding parent span. The length of the operation represented as the gray striped bar in the waterfall visualization is also inherited from the parent span and might not be exactly representative of the operation duration in the inferred service.

.. _how-apm-infers-services:

How Splunk APM identifies inferred services
============================================
When a client or producer span does not have a corresponding server span, Splunk APM checks whether the unpaired span contains tags that indicate interaction with an uninstrumented service. 

To identify an inferred service, Splunk APM first checks for tags that indicate the ``type`` of the inferred service, and then checks for tags that indicate the service ``name``. Once Splunk APM identifies a client or producer span with a tag or tags that indicate interaction with one of these service types, it creates an inferred span to represent the operation in the uninstrumented service. 

The table in :ref:`inferred-service-types` below provides a list of types of inferred services and the tags and the inference methods that Splunk APM uses to identify each type of inferred service. 

In the case of inferred pub/sub services, the inferred span inherits the metadata from the corresponding client or producer span and is attached directly to that span. 

For inferred service types other than pub/sub, Splunk APM applies additional logic to ensure it captures only the most important application-level information. If there are multiple client spans without a corresponding server span, the inferred span inherits the metadata from the parent-most of these client spans. The inferred service span is then attached to the most recent of these client spans. Otherwise, if there is just a single client span without a corresponding server span, the inferred span inherits the metadata from that client span and is attached to that same span. 

.. _inferred-service-types:

Types of inferred services and how they're inferred
====================================================

The following table provides a list of types of inferred services and the tags and the inference methods that Splunk APM uses to identify each type of inferred service. These inference rules are based on OpenTelemetry semantic conventions.

.. list-table::
   :header-rows: 1
   :widths: 20, 80

   * - :strong:`Inferred service type`
     - :strong:`Tags and methods for inference`

   * - AWS services
     - The ``kind`` of the referring span is equal to ``client``.
       
       To identify AWS services, the span must contain ``http.url``. Splunk APM applies heuristics on this tag's value to determine the AWS Service type from the url.
   * -  Database
     - The ``kind`` of the referring span is equal to ``client``.
       
       To identify a database, the span must contain at least one of the following tags: 
            
            * ``db.system``
            * ``db.name``
            * ``db.type``
            * ``db.instance``

       To determine the ``name`` of an inferred service, Splunk APM applies this logic in the following order: 

            * ``db.system``: if this tag exists, its value is used to specify the type of database being queried, e.g. ``mysql``, ``redis``, etc. If only this tag is present, its value is also used as the ``service.name`` for the inferred database.
            * ``db.name``: if this tag exists, its value is concatenated with ``db.system`` to form the name of the inferred service: ``db.system:db.name`` (e.g. ``mysql:sql_db_1``).
            * ``db.connection_string``: if this tag is present and its value conforms to a known format such as Java database connectivity (JDBC), Splunk APM extracts the database name portion of the url and concatenates it with the value of ``db.system`` to form the database name, such as ``mysql:dbname``. If the value of ``db.connection_string`` does not conform to a known format or the database portion cannot be extracted and ``db.name`` also does not exist, Splunk APM uses the raw value of ``db.connection_string`` as the database name. If ``db.system`` also exists, the two values are concatenated. 

   * -  Generic remote procedure call (RPC)
     - The ``kind`` of the referring span is equal to ``client``.
       
       To identify an RPC call, the span must contain ``rpc.system``. This tag is used to identify the remote system, such as ``grpc``, ``java_rmi``, or ``wcf``. If this is the only value present, this tag is used to specify the name of the service being called, potentially including the package name.
       
       If the trace contains ``net.peer.name``, its value is used as the ``name`` of the inferred service.
   * -  HTTP
     - The ``kind`` of the referring span is equal to ``client``.
       
       To identify a remote HTTP call, the span must contain at least one of the following tags: 

            * ``peer.service``
            * ``destination.workload.name``
            * ``destination.name``
            * ``net.peer.name``
            * ``http.host``
            * ``peer.hostname``
            * ``peer.address``
            * ``http.url``
        
       To determine the name of a remote HTTP call, these tags are considered in the following order:
        
            * ``net.peer.name``: this tag's value provides the hostname of a remote connection. If this tag exists and contains only the hostname, not a full URL, its value is used as the inferred service ``name`` without modification. 
            * ``http.host``: if this tag exists, its value is used as the inferred service ``name`` without modification. 
            * ``http.url``: this tag's value provides the URL of a remote connection. If the tag contains a valid HTTP url, Splunk APM extracts the hostname and port and uses them as the ``name`` of the inferred service.

   * -  Pub/sub
     - The ``kind`` of the referring span is equal to ``producer`` or ``client``.
       
       To identify a pub/sub inferred service, the span must contain ``messaging.destination``. This tag's value is used to specify the name of the topic or channel that messages are being sent to.

    
Types of inferred services enabled by default
----------------------------------------------
By default, all types of inferred services in the table above except HTTP inferred services are enabled. There are typically a lot of different HTTP inferred services, so they are excluded to prevent a crowded service map. To enable HTTP inferred services in your account, contact Splunk Support. See :new-page:`Support and Services <https://www.splunk.com/en_us/customer-success.html>`.


   