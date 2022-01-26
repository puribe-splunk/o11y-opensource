.. _db-perf-reference:

************************************************************************
Reference for Database Query Performance
************************************************************************

.. meta::
   :description: Reference material for using Database Query Performance in Splunk APM. 

.. _supported-dbs:

Supported databases
--------------------------
Database Query Performance provides insights for the following types of SQL databases:

    - Oracle
    - MySQL
    - Microsoft SQL Server
    - PostgreSQL
    - Aurora
    - MariaDB
    - Db2

.. _db-tags:

Database-related span tags
--------------------------
Enabling Database Query Performance enables indexing for the following database-related span tags: 

.. list-table::
   :header-rows: 1
   :widths: 30 20 50

   * - :strong:`Key`
     - :strong:`Value type`
     - :strong:`Value description`

   * - Normalized db.statement (``_sf_normalized_db_statement``)
     - string
     - The database statement with dynamic elements replaced with placeholders to reduce cardinality

   * - ``db.name``
     - string
     - Name of the database
    
   * - ``db.operation``
     - string
     - Operation in the database

   * - ``db.sql.table``
     - string
     - Table in the database

   * - ``db.system`` 
     - string
     - System of the database

.. Recommended instrumentation library versions 

.. _db-lib-versions:

Recommended instrumentation library versions 
----------------------------------------------------
For the best experience with Database Query Performance, use the following instrumentation library versions: 

.. list-table::
   :header-rows: 1
   :widths: 15 25 60
   
   * - :strong:`Language`
     - :strong:`Recommended version`
     - :strong:`Link to docs in GitHub`

   * - Golang
     - v0.6.0 (pre-release)
     - https://github.com/signalfx/splunk-otel-go

   * - Java
     - v1.6.0
     - https://github.com/signalfx/splunk-otel-java
    
   * - Node.js
     - v1.0.1
     - https://github.com/open-telemetry/opentelemetry-js

   * - Python
     - v1.3.0
     - https://github.com/signalfx/splunk-otel-python 

   * - Ruby
     - v0.50.1 (pre-release)
     - https://github.com/open-telemetry/opentelemetry-ruby

Learn more
-------------
See the following links for more information about Database Query Performance: 

* For an overview of Database Query Performance, see :ref:`db-query-performance`.
* To enable Database Query Performance, see :ref:`enable-db-perf`. 
* For a detailed use case using Database Query Performance, see :ref:`db-perf-use-case`. 
* To troubleshoot issues with Database Query Performance, see :ref:`db-perf-troubleshooting`. 
