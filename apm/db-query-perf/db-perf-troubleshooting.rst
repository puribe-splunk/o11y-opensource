.. _db-perf-troubleshooting:

************************************************************************
Troubleshoot Database Query Performance
************************************************************************

.. meta::
   :description: Follow these steps to troubleshoot Database Query Performance issues.

Follow the steps in the following sections to troubleshoot Database Query Performance issues:

* :ref:`dbq-not-enabled`
* :ref:`no-dbs-avail`
* :ref:`tms-limits-exceeded`
* :ref:`non-query-issues`

.. _dbq-not-enabled:

Database Query Performance is not enabled
------------------------------------------------
If you open Database Query Performance and notice the feature is not enabled, go to MetricSets Configuration and check if indexing for Database Query Performance tags is ``ACTIVE``. If it's not, see :ref:`enable-db-perf` for instructions to enable indexing for Database Query Performance tags. 

.. _no-dbs-avail: 

No supported databases available
----------------------------------------
If you open Database Query Performance and notice there are no supported databases available, check the following:

- Ensure you have one or more of the types of SQL databases listed in :ref:`supported-dbs` in your system.
- Try adjusting the Environment, Workflow, Service, or Tag filters in the filter bar to bring databases into view.
- Ensure you are using the recommended instrumentation version to make sure the database-related span tags are being ingested. See :ref:`db-lib-versions` for the list of recommended versions. If necessary, upgrade the instrumentation library you're using. 

.. _tms-limits-exceeded:

New query data is not being processed because cardinality limits have been exceeded
------------------------------------------------------------------------------------
Enabling Database Query Performance turns on indexing for a set of 5 database-related span tags. These tags count toward cardinality limits for indexing span tags. 

When the limit is exceeded across all indexed tags in your account, Splunk APM temporarily pauses indexing all Database Query Performance tags to allow for transient spikes in cardinality and manage billing in your account.  After 15 minutes, Splunk APM attempts to restart indexing Database Query Performance tags automatically. 

If you're having persistent cardinality issues, try turning off indexing for other high-cardinality span tags to free up cardinality for Database Query Performance. See :ref:`apm-limits-metricsets` to learn more about managing cardinality. 

.. _non-query-issues:

Database issues are not originating with problematic queries
--------------------------------------------------------------
If you are seeing database latency, but the query itself is not causing the issue, the problem might be a slow connection or a bottleneck waiting for a queue of database queries. To identify these kinds of issues, you need to instrument the database itself. For instructions on instrumenting a SQL database, see the following links:

* :ref:`sql`
* :ref:`mysql`
* :ref:`microsoft-sql-server`
* :ref:`postgresql`

You can also use the Related Content tiles to pivot to Infrastructure Monitoring or Log Observer and further analyze database issues. See :ref:`get-started-relatedcontent` to learn more about Related Content.

Learn more
--------------
See the following links for more information about Database Query Performance: 

* For an overview of Database Query Performance, see :ref:`db-query-performance`.
* To enable Database Query Performance, see :ref:`enable-db-perf`. 
* For a detailed use case using Database Query Performance, see :ref:`db-perf-use-case`. 
* For reference material about Database Query Performance, see :ref:`db-perf-reference`.