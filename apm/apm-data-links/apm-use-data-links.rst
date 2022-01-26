.. _apm-use-data-links:

**********************************************************************
Access relevant resources linked to APM services, traces, and spans
**********************************************************************

.. meta::
   :description: Use global data links to view resources for services, traces, and spans.

   :keywords: Splunk, APM, global data links, traces, services, spans, monitoring, dashboards

Use global data links to access relevant resources directly from Splunk APM properties, including services, traces, spans, and span tags.

For example, you can use a global data link to easily navigate from an APM property to a resource, such as an Infrastructure Monitoring dashboard, Splunk instance, Kibana log, or custom URL. Global data links dynamically transfer the context of the property you're viewing to the resource, helping you get to relevant information faster.

For information about creating global data links, see :ref:`apm-create-data-links`.


Access global data links for a service
=========================================

**To access global data links for a service on the Overview page:**

#. Open the Observability Cloud main menu.

#. Select :guilabel:`APM`. The Overview page displays.

#. On the :guilabel:`Services` tab, click the :guilabel:`More` icon for the service you're investigating. The menu displays global data links for the service.

**To view global data links for a service on the Explore page:**

#. Open the Observability Cloud main menu.

#. Select :guilabel:`APM`. The Overview page displays.

#. In the sidebar, click the :guilabel:`Explore` card. The Explore page displays.

#. In the service map, click the service you're investigating. The service detail drawer displays.

#. In the service detail drawer, click the :guilabel:`More` icon next to the service name. The menu displays global data links for the service.


Access global data links for a trace
=======================================

**To access global data links for a trace:**

#. Open the Observability Cloud main menu.

#. Select :guilabel:`APM`. The Overview page displays.

#. In the sidebar, click the :guilabel:`Traces` card. The Traces page displays.

#. Click the trace ID you're investigating. The trace ID detail sidebar displays.

#. In the trace detail sidebar, click the :guilabel:`More` icon next to the :guilabel:`Trace ID` value. The menu displays global data links for the trace. The :guilabel:`More` icon displays only if global data links have been created for the trace.


Access global data links for a span
======================================

**To access global data links for a span:**

#. Open the Observability Cloud main menu.

#. Select :guilabel:`APM`. The Overview page displays.

#. In the sidebar, click the :guilabel:`Traces` card. The Traces page displays.

#. Click the :guilabel:`Trace ID` value you're investigating.

#. On the :guilabel:`Waterfall` tab, click a :guilabel:`Service: Operation` value you're interested in.

#. Click the :guilabel:`More` icon next to the :guilabel:`Span ID` value. The menu displays global data links for the span. The :guilabel:`More` icon displays only if global data links have been created for the span.

#. If the span has a parent, you can click the :guilabel:`More` icon next to the :guilabel:`Parent ID` value. The menu displays global data links for the span. The :guilabel:`More` icon displays only if global data links have been created for the span.


Access global data links for a span tag
==========================================

**To access global data links for a span tag:**

#. Open the Observability Cloud main menu.

#. Select :guilabel:`APM`. The Overview page displays.

#. In the sidebar, click the :guilabel:`Traces` card. The Traces page displays.

#. Click the :guilabel:`Trace ID` value you're investigating.

#. On the ::guilabel:`Waterfall` tab, click a :guilabel:`Service: Operation` value you're interested in.

#. In the :guilabel:`Tags` section, click the :guilabel:`More` icon next to a span tag field-value pair you're interested in. The menu displays global data links for the span tag. The :guilabel:`More` icon displays only if global data links have been created for the span tag.
