.. _java-otel-metrics-attributes:

***************************************************************
Metrics and attributes collected by the Splunk OTel Java agent
***************************************************************

.. meta:: 
   :description: The Splunk Distribution of OpenTelemetry Java collects the following application metrics data and WebEngine attributes. You can also collect custom metrics through Micrometer.

The agent of the Splunk Distribution of OpenTelemetry Java collects the following application metrics data and attributes in addition to all that the upstream OpenTelemetry agent collects.

.. _java-otel-metrics:

Application metrics
====================================================

The agent of the Splunk Distribution of OpenTelemetry Java can collect Java application metrics when metrics collection is enabled. See :ref:`metrics-configuration-java`.

.. note:: Application metrics collection is an experimental feature subject to future changes.

.. _default_app_metrics-java:

Default metric dimensions
----------------------------------------------------

The following dimensions are automatically added to all metrics exported by the agent:

.. list-table:: 
   :header-rows: 1
   :widths: 40 60

   * - Dimension
     - Description
   * - ``deployment.environment``
     - Value of the ``deployment.environment`` resource attribute, if present.
   * - ``runtime``
     - Value of the ``process.runtime.name`` resource attribute, for example ``OpenJDK Runtime Environment``.
   * - ``process.pid``
     - The Java process identifier (PID).
   * - ``service``
     - Value of the ``service.name`` resource attribute.

.. _supported_libraries_java_metrics:

Supported libraries
------------------------------------------------------------

The agent collects the following metrics through the following libraries:

.. list-table:: 
   :header-rows: 1
   :widths: 45 20 50

   * - Library/Framework
     - Instrumentation
     - Supported versions
   * - :ref:`jvm-metrics`
     - ``jvm-metrics``
     - Java runtimes version 8 and higher
   * - :ref:`Apache DBCP2 connection pool metrics <connection-pool-metrics>`
     - ``commons-dbcp2``
     - Version 2.0 and higher
   * - :ref:`c3p0 connection pool metrics <connection-pool-metrics>`
     - ``c3p0``
     - Version 0.9.5 and higher 
   * - :ref:`HikariCP connection pool metrics <connection-pool-metrics>`
     - ``hikaricp``
     - Version 3.0 and higher
   * - :ref:`Oracle Universal Connection Pool metrics (UCP) <connection-pool-metrics>`
     - ``oracle-ucp``
     - Version 11.2.0.4 and higher
   * - :ref:`Tomcat JDBC connection pool metrics <connection-pool-metrics>`
     - ``tomcat-jdbc``
     - Version 8.5 and higher
   * - :ref:`Vibur DBCP connection pool metrics <connection-pool-metrics>`
     - ``vibur-dbcp``
     - Version 20.0 and higher
   * - :ref:`Tomcat thread pool metrics <thread-pool-metrics>`
     - ``tomcat``
     - Version 8.5 and higher
   * - :ref:`WebSphere Liberty thread pool metrics <thread-pool-metrics>`
     - ``liberty``
     - Version 20.0.0.12
   * - :ref:`WebLogic thread pool metrics <thread-pool-metrics>`
     - ``weblogic``
     - Versions 12.x and 14.x

.. _jvm-metrics:

JVM metrics
=============================================================

The Splunk OTel Java agent collects the following Java Virtual Machine (JVM) metrics when metrics collection is enabled:

.. _classloader-metrics:

ClassLoader metrics
----------------------------------------------------------------

The agent collects the following ClassLoader metrics:

.. list-table:: 
   :header-rows: 1

   * - Metric
     - Description
   * - ``runtime.jvm.classes.loaded``
     - Number of loaded classes.
   * - ``runtime.jvm.classes.unloaded``
     - Total number of unloaded classes since the process starts.

.. _gc-metrics:

Garbage collection metrics
------------------------------------------------------------------

The agent collects the following garbage collection (GC) metrics:

.. list-table:: 
   :header-rows: 1

   * - Metric
     - Description
   * - ``runtime.jvm.gc.concurrent.phase.time``
     - Time spent in concurrent phase, in milliseconds.
   * - ``runtime.jvm.gc.live.data.size``
     - Size of long-lived heap memory pool after reclamation, in bytes.
   * - ``runtime.jvm.gc.max.data.size``
     - Maximum size of long-lived heap memory pool, in bytes.
   * - ``runtime.jvm.gc.memory.allocated``
     - Increase in the size of the young heap memory pool after one garbage collection and before the next.
   * - ``runtime.jvm.gc.memory.promoted``
     - Count of positive increases in the size of the old generation memory pool from before to after garbage collection.
   * - ``runtime.jvm.gc.pause``
     - Time spent in garbage collection pause, in milliseconds.

.. _jvm-memory-metrics:

Memory metrics
----------------------------------------------------------------------

The agent collects the following memory metrics:

.. list-table:: 
   :header-rows: 1
   :widths: 40 60

   * - Metric
     - Description
   * - ``runtime.jvm.memory.committed``
     - Amount of memory available to the Java Virtual Machine, in bytes.
   * - ``runtime.jvm.memory.max``
     - Maximum amount of memory available for memory management, in bytes.
   * - ``runtime.jvm.memory.used``
     - Amount of used memory, in bytes.

All memory pool metrics share the following tags:

.. list-table:: 
   :header-rows: 1
   :width: 100%
   :widths: 30 70

   * - Tag
     - Value
   * - ``area``
     - Either ``heap`` or ``nonheap``
   * - ``id``
     - Name of the memory pool. For example, ``Perm Gen``

.. _jvm-thread-metrics:

Thread metrics
----------------------------------------------------------------------

The agent collects the following thread metrics:

.. list-table:: 
   :header-rows: 1
   :widths: 40 60

   * - Metric
     - Description
   * - ``runtime.jvm.threads.daemon``
     - Number of live daemon threads.
   * - ``runtime.jvm.threads.live``
     - Number of live threads, including both daemon and nondaemon threads.
   * - ``runtime.jvm.threads.peak``
     - Peak live thread count since the JVM started or peak was reset.
   * - ``runtime.jvm.threads.states``
     - Number of threads per ``state`` as a metric tag.

.. _connection-pool-metrics:

Connection pool metrics
----------------------------------------------------------------------

The Splunk Distribution of OpenTelemetry Java instruments several Java Database Connectivity (JDBC) connection pool implementations:

- Apache DBCP2
- c3p0
- HikariCP
- Oracle Universal Connection Pool (UCP)
- Tomcat JDBC
- Vibur DBCP
- WebSphere Liberty
- WebLogic thread pools

Each of the connection pools reports a subset of the following metrics:

.. list-table:: 
   :header-rows: 1
   :widths: 40 60

   * - Metric
     - Description
   * - ``db.pool.connections``
     - Number of open connections.
   * - ``db.pool.connections.active``
     - Number of open connections that are in use.
   * - ``db.pool.connections.idle``
     - Number of open connections that are idle.
   * - ``db.pool.connections.idle.max``
     - Maximum number of idle open connections allowed.
   * - ``db.pool.connections.idle.min``
     - Minimum number of idle open connections allowed.
   * - ``db.pool.connections.max``
     - Maximum number of open connections allowed.
   * - ``db.pool.connections.pending_threads``
     - Number of threads that are waiting for an open connection.
   * - ``db.pool.connections.timeouts``
     - Number of connection timeouts that have happened since the application started.
   * - ``db.pool.connections.create_time``
     - Time it took to create a new connection.
   * - ``db.pool.connections.wait_time``
     - Time it took to get an open connection from the pool.
   * - ``db.pool.connections.use_time``
     - Time between borrowing a connection and returning it to the pool.

All connection pool metrics share the following tags:

.. list-table:: 
   :header-rows: 1
   :widths: 40 60

   * - Tag
     - Value
   * - ``pool.name``
     - Name of the connection pool. Spring bean name if Spring is used, JMX object name otherwise.
   * - ``pool.type``
     - Type or implementation of the connection pool. For example, ``c3p0``, ``dbcp2``, or ``hikari``.

.. _thread-pool-metrics:

The Splunk Distribution of OpenTelemetry Java instruments the following thread pool implementations:

- Tomcat connector thread pools
- WebSphere Liberty web request thread pool
- Weblogic thread pools

Each of the supported connection pools reports a subset of the following metrics:

.. list-table:: 
   :header-rows: 1
   :widths: 40 60

   * - Metric
     - Description
   * - ``executor.threads``
     - Number of threads in the pool.
   * - ``executor.threads.active``
     - Number of threads that are executing code.
   * - ``executor.threads.idle``
     - Number of threads that aren't executing code.
   * - ``executor.threads.core``
     - Core thread pool size, expressed as the number of threads that are always kept in the pool.
   * - ``executor.threads.max``
     - Maximum number of threads in the pool.
   * - ``executor.tasks.submitted``
     - Total number of tasks submitted to the executor.
   * - ``executor.tasks.completed``
     - Total number of tasks completed by the executor.

All thread pool metrics have the following tags:

.. list-table:: 
   :header-rows: 1
   :widths: 40 60

   * - Tag
     - Value
   * - ``executor.name``
     - Name of the thread pool.
   * - ``executor.type``
     - Type/implementation of the connection pool. For example, ``tomcat``, ``liberty``, or ``weblogic``.

.. _webengine-attributes-java-otel:

WebEngine attributes
=========================================================

The Splunk Distribution of OpenTelemetry Java captures data about the application server and adds the following attributes to `SERVER` spans:

.. list-table:: 
   :header-rows: 1

   * - Span attribute
     - Description
   * - ``webengine.name``
     - Name of the applications server. For example, ``tomcat``.
   * - ``webengine.version``
     - Version of the application server.

For a list of supported application servers, see the OpenTelemetry documentation at https://github.com/open-telemetry/opentelemetry-java-instrumentation/blob/main/docs/supported-libraries.md#application-servers.

.. _java-otel-custom-metrics:

Send custom Java application metrics
========================================================

The Splunk Distribution of OpenTelemetry Java agent detects if the instrumented application is using Micrometer and injects a special ``MeterRegistry`` implementation that lets the agent collect user-defined meters.

Follow these steps to enable custom application metrics:

- :ref:`add-micrometer-dep`
- :ref:`add-meter-registry`

.. _add-micrometer-dep:

Add the micrometer-core dependency
------------------------------------------------------

To export custom metrics through the Java agent, add a dependency on the ``micrometer-core`` library with version 1.5 or higher:

.. tabs::

   .. code-tab:: xml Maven

      <dependency>
         <groupId>io.micrometer</groupId>
         <artifactId>micrometer-core</artifactId>
         <version>1.7.5</version>
      </dependency>

   .. code-tab:: java Gradle

      implementation("io.micrometer:micrometer-core:1.7.5")

.. _add-meter-registry:

Register each custom meter
---------------------------------------------------

You must register each custom meter in the global ``Metrics.globalRegistry`` instance provided by the Micrometer library. You can use one of meter factory methods provided by the ``Metrics`` class, or use meter builders and reference the ``Metrics.globalRegistry`` directly, as in the following example:

.. code:: java

   class MyClass {
     Counter myCounter = Metrics.counter("my_custom_counter");
     Timer myTimer = Timer.builder("my_custom_timer").register(Metrics.globalRegistry);

     int foo() {
       myCounter.increment();
       return myTimer.record(this::fooImpl);
     }

     private int fooImpl() {
       // ...
     }
   }

For more information on the Micrometer API, see the Micrometer official documentation.