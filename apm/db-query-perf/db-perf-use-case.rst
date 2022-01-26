.. _db-perf-use-case:

**************************************************************************
Use case: Identify slow database queries using Database Query Performance
**************************************************************************

.. meta::
   :description: Follow a site reliability engineer (SRE)'s workflow using Database Query Performance to examine the relationship between service latency and the performance of database queries in their system.


Jax, a site reliability engineer (SRE), is in charge of identifying the root cause of service latency issues in their system. In this section, follow their workflow using Database Query Performance to examine the relationship between service latency and the performance of database queries in their system.

Noticing unusually high latency
==================================

Jax is monitoring their system performance and notices in the APM landing page that the ``order-processor`` service is experiencing an unusually high latency of 1.2 minutes:

.. image:: /_images/apm/db-query-perf/high-latency.png
   :width: 70%
   :alt: This screenshot shows a closeup of the Top Services by Latency section of the APM landing page, where the ``order-processor`` has a latency of 1.2 minutes.

They click on the latency value to the service map view of this service and its dependencies. The right-hand panel shows a graph of the service's latency over time compared to the latency of its dependencies. They click :guilabel:`Dependencies` to open the Dependencies panel and get a closer look at the dependent services for ``order-processor``. Scrolling down to Outbound Dependencies, they notice that the P90 latency for the ``mysql:mysql-prod-1`` database is also very high:

.. image:: /_images/apm/db-query-perf/service-map-outbound-dep.png
   :width: 90%
   :alt: This screenshot shows a closeup of the ``order-processor`` service and its dependencies in the service map. The Dependencies panel shows that the high latency is coming from the ``mysql:mysql-prod-1`` database. 

They click on the ``mysql:mysql-prod-1`` database in the service map to see summary charts of its requests and latency in the sidebar. The :guilabel:`Database Query Performance` panel in the sidebar shows the P90 latency of the slowest SQL queries within the current time range. They notice that the P90 latency of the top query is also 1.4 minutes:

.. image:: /_images/apm/db-query-perf/db-perf-card.png
   :width: 60%
   :alt: This screenshot shows the Database Query Performance panel containing a table of the query text and P90 latency of the top 5 slowest queries in the selected database.  

Jax clicks on this panel to open Database Query Performance. The data they see in Database Query Performance applies to the ``mysql:mysql-prod-1`` database selected in the filter bar: 

.. image:: /_images/apm/db-query-perf/db-landing.png
   :width: 100%
   :alt: This screenshot shows the Database Query Performance view of Splunk APM with data for the selected database. The page shows a table of the top 10 queries by total time, with a sidebar containing charts that show requests and errors and latency for the selected database over time. 

Jax could also use the database picker to select from among available SQL databases, but right now they want to focus on ``mysql:mysql-prod-1``.  

The table under :guilabel:`Top Queries` shows a list of the top 10 queries to ``mysql:mysql-prod-1``, sorted by default in descending order by total time. Jax can use the :guilabel:`Sort` picker to sort by P90 latency or requests per second instead. They can also use the :guilabel:`Compare to:` picker to compare the current data against a past time range. This overlays a historical line based on the selected time range on the chart in each row of the query table, enabling Jax to determine whether the problem is recent, or has been occuring over time. 

The charts under :guilabel:`Database Overview` in the right sidebar summarize the latency and request and error rate for all queries in the selected database, illustrating trends in query latency over time. The charts are based on the current state of the filter bar. Changing the time range or environment pickers in the filter bar changes the data shown in the chart.

Jax clicks on the query with the highest latency to get more details. The :guilabel:`Query Details` panel opens, showing the full text of the query along with the :guilabel:`Requests & Errors` and :guilabel:`Latency` charts for that particular query. They notice that the query's latency has been as high as 1.5 minutes:  

.. image:: /_images/apm/db-query-perf/query-details.png
   :width: 65%
   :alt: This screenshot shows the Database Query Performance sidebar, showing the full text of the top query and charts of latency and requests and errors specific to the top query. 

Based on this evidence, Jax identifies this query as the possible culprit of the high service latency. Jax clicks the copy button to copy the text of the statement so they can give it to their team’s DBA. 

Finding example traces
========================
To determine whether the high latency is coming from multiple spans with many executions of a particular query, or from a single, long span with one execution of a slow query, Jax decides to look for example traces containing this problematic query. 

Jax clicks within the latency chart to isolate a time on the x-axis and load a table of example traces from that time. They confirm that each of the loaded traces contains ``mysql:mysql-prod-1`` as a span tag. They also notice that these traces all have unusually long durations, probably due to the slow database query they have identified: 

.. image:: /_images/apm/db-query-perf/db-traces.png
   :width: 100%
   :alt: This screenshot shows a list of example traces from a time selected in the latency chart for the specified database. Span tags containing the database name, as well as the trace durations, are highlighted to show they are associated with the slow database query. 

From the table of traces, Jax clicks on the trace in the table with the longest duration to open its Trace View page and waterfall chart. They could also click specifically on a span tag containing the database name to open the span with the problematic query to see the exact query in context. 

In Trace View, Jax scrolls to find a span that references the ``mysql:mysql-prod-1`` database, which is identified as an inferred service in the Trace Waterfall. They click this span to open it and view the query under the ``db.statement`` span tag, confirming that it's the same SQL statement that raised their suspicions in Database Query Performance view. 

Jax notices that the span containing the database call is much longer than all of the other spans in the trace, indicating the high latency they're seeing is related to one long span. The Performance Summary in the right sidebar also shows that 99.8% of the trace workload is occupied by the Database category: 

.. image:: /_images/apm/db-query-perf/trace-view.png
   :width: 100%
   :alt: This screenshot shows Trace View for a particular trace associated with the database latency issue. A specific span within the trace is expanded, and the ``db.statement`` span tag is highlighted to show that the span contains the same database query that Jax identified in Database Query Performance view. The Performance Summary and name of the database are also highlighted. 

.. tip:: If the database query is long and has been truncated in the span, you can click on the :strong:`Related Content` tile at the bottom of the page to pivot into Log Observer and view the full query in logs.

Armed with this knowledge, Jax contacts the DBA who supports this database and provides the exact text of the query that’s causing problems. Jax asks them to investigate the source of this latency and optimize the database call to prevent further issues with service latency.

To continue to investigate the wider impact of this latency issue, Jax could use Tag Spotlight to determine which database span tags and values are associated with the highest latency, and see which Business Workflows are affected by this issue. For an example use case of troubleshooting using Tag Spotlight, see :ref:`troubleshoot-tag-spotlight`. 

Learn more
============
See the following links for more information about Database Query Performance: 

* For an overview of Database Query Performance, see :ref:`db-query-performance`.
* To enable Database Query Performance, see :ref:`enable-db-perf`. 
* To troubleshoot issues with Database Query Performance, see :ref:`db-perf-troubleshooting`. 
* For reference material about Database Query Performance, see :ref:`db-perf-reference`.