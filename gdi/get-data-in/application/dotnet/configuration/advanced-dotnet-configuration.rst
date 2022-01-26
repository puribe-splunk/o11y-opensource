.. _advanced-dotnet-configuration:

********************************************************************
Configure the SignalFx Instrumentation for .NET
********************************************************************

.. meta:: 
   :description: Configure the SignalFx Instrumentation for .NET to suit your instrumentation needs, such as correlating traces with logs and enabling custom sampling.

You can configure the SignalFx Instrumentation for .NET to suit your instrumentation needs. In most cases, modifying the basic configuration is enough to get started. More advanced settings are also available. 

.. _configuration-methods-dotnet:

Configuration methods
===========================================================

You can change the settings of the SignalFx Instrumentation for .NET in the following ways:

#. Set environment variables. On Windows, set them in the process scope unless you want to enable autoinstrumentation globally for all .NET applications.

#. Edit the ``web.config`` or ``app.config`` file. For example:

   .. code-block:: xml

      <configuration>
         <appSettings>
            <add key="SIGNALFX_SERVICE_NAME" value="my-service-name" />
         </appSettings>
      </configuration>

#. Generate a JSON configuration file and set the ``SIGNALFX_TRACE_CONFIG_FILE`` environment variable to the path of the file. You can define settings as key-value pairs:

   .. code-block:: json

      {
         "SIGNALFX_SERVICE_NAME": "my-service-name"
      }

.. note:: Settings defined using environment variables override settings in XML and JSON configuration files.

.. _main-dotnet-agent-settings:

General settings
=========================================================================

The following settings are common to most instrumentation scenarios:

.. list-table:: 
   :header-rows: 1
   :width: 100
   :widths: 40 60

   * - Setting
     - Description
   * - ``SIGNALFX_ENV``
     - The value for the ``deployment.environment`` tag added to all spans.	
   * - ``SIGNALFX_SERVICE_NAME``
     - The name of the application or service. If not set, the instrumentation looks for a suitable default name. See :ref:`dotnet-default-service-name`.
   * - ``SIGNALFX_VERSION``
     - The version of the application. When set, it adds the ``version`` tag to all spans.
   * - ``SIGNALFX_PROFILER_PROCESSES``
     - Names of the executable files that the profiler can instrument. Supports multiple comma-separated values, for example: ``MyApp.exe,dotnet.exe``.
   * - ``SIGNALFX_PROFILER_EXCLUDE_PROCESSES``
     - Names of the executable files that the profiler cannot instrument. Supports multiple comma-separated values, for example: ``ReservedProcess.exe,powershell.exe``.
   * - ``SIGNALFX_TRACE_CONFIG_FILE``
     - Path of the JSON configuration file. Set this environment variable if you're configuring the instrumentation using a JSON file. See :ref:`configuration-methods-dotnet` for more information.
   * - ``SIGNALFX_TRACE_ENABLED``
     - Set to ``false`` to disable the tracer. The default value is ``true``.
   * - ``SIGNALFX_AZURE_APP_SERVICES``
     - Set to ``true`` to indicate that the profiler is running in the context of Azure App Services.	The default value is ``false``.

.. _dotnet-instrumentation-settings:

Instrumentation settings
================================================

The following settings control instrumentations and tracing behavior:

.. list-table:: 
   :header-rows: 1
   :width: 100
   :widths: 40 60

   * - Setting
     - Description
   * - ``SIGNALFX_TRACE_GLOBAL_TAGS``
     - Comma-separated list of key-value pairs that specify global span tags. For example: ``key1:val1,key2:val2``.
   * - ``SIGNALFX_RECORDED_VALUE_MAX_LENGTH``
     - Maximum length of the value of an attribute. Values longer than this value are truncated. Values are discarded entirely when set to ``0``, and ignored when set to a negative value. The default value is ``12000``.
   * - ``SIGNALFX_DISABLED_INTEGRATIONS``
     - Comma-separated list of library instrumentations you want to disable. Each value must match an internal instrumentation ID. See :ref:`supported-dotnet-libraries` for a list of integration identifiers.
   * - ``SIGNALFX_TRACE_{0}_ENABLED``
     - Enables or disables a specific instrumentation library. For example, to disable the Kafka instrumentation, set ``SIGNALFX_TRACE_Kafka_ENABLED`` to ``false``. The value must match an internal instrumentation ID. See :ref:`supported-dotnet-libraries` for a list of integration identifiers.

.. _dotnet-exporter-settings:

Exporter settings
================================================

The following settings control trace exporters and their endpoints:

.. list-table:: 
   :header-rows: 1
   :width: 100
   :widths: 40 60

   * - Setting
     - Description
   * - ``SIGNALFX_ENDPOINT_URL``
     - The URL to where the trace exporter sends traces. The default value is ``http://localhost:9411/api/v2/spans``.
   * - ``SIGNALFX_ACCESS_TOKEN``
     - Splunk Observability Cloud access token for your organization. The token enables sending traces directly to the Observability Cloud ingest endpoint.	To obtain an access token, see :ref:`admin-api-access-tokens`. 
   * - ``SIGNALFX_TRACE_PARTIAL_FLUSH_ENABLED``
     - Enable to export traces that contain a minimum number of closed spans, as defined by ``SIGNALFX_TRACE_PARTIAL_FLUSH_MIN_SPANS``. The default value is ``false``.	
   * - ``SIGNALFX_TRACE_PARTIAL_FLUSH_MIN_SPANS``
     - Minimum number of closed spans in a trace before it's exported. The default value is ``500``. Requires the value of the ``SIGNALFX_TRACE_PARTIAL_FLUSH_ENABLED`` environment variable to be ``true``.

To send traces directly to Splunk Observability Cloud and bypass the Splunk OpenTelemetry Collector, set the following environment variables:

.. tabs::

   .. code-tab:: shell Windows PowerShell

      $env:SIGNALFX_ACCESS_TOKEN=<access_token>
      $env:SIGNALFX_ENDPOINT_URL=https://ingest.<realm>.signalfx.com/v2/trace	

   .. code-tab:: shell Linux
      
      export SIGNALFX_ACCESS_TOKEN=<access_token>
      export SIGNALFX_ENDPOINT_URL=https://ingest.<realm>.signalfx.com/v2/trace	

To obtain an access token, see :ref:`admin-api-access-tokens`.

In the ingest endpoint URL, ``realm`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`. For example, ``us0``. 

.. _dotnet-trace-propagation-settings:

Trace propagation settings
================================================

The following settings control trace propagation:

.. list-table:: 
   :header-rows: 1
   :width: 100
   :widths: 40 60

   * - Setting
     - Description
   * - ``SIGNALFX_PROPAGATORS``
     - Comma-separated list of propagators for the tracer. Available propagators are: ``B3`` and ``W3C``. The default value is ``B3``.

.. _dotnet-instrumentation-libraries-settings:

Library-specific instrumentation settings
================================================

The following settings control the behavior of specific instrumentations:

.. list-table:: 
   :header-rows: 1
   :width: 100
   :widths: 40 60

   * - Setting
     - Description
   * - ``SIGNALFX_HTTP_CLIENT_ERROR_STATUSES``
     - Comma-separated list of HTTP client response statuses or ranges for which the spans are set as errors, for example: ``300, 400-499``. The default value is ``400-599``.
   * - ``SIGNALFX_HTTP_SERVER_ERROR_STATUSES``
     - Comma-separated list of HTTP server response statuses or ranges for which the spans are set as errors, for example: ``300, 400-599``. The default value is ``500-599``.
   * - ``SIGNALFX_INSTRUMENTATION_ELASTICSEARCH_TAG_QUERIES``
     - Enables the tagging of a ``PostData`` command as ``db.statement``. It might introduce overhead for direct streaming users. The default value is ``true``.
   * - ``SIGNALFX_INSTRUMENTATION_MONGODB_TAG_COMMANDS``
     - Enables the tagging of a ``BsonDocument`` command as ``db.statement``. The default value is ``true``.	
   * - ``SIGNALFX_INSTRUMENTATION_REDIS_TAG_COMMANDS``
     - Enables the tagging of Redis commands as ``db.statement``. The default value is ``true``.
   * - ``SIGNALFX_TRACE_DELAY_WCF_INSTRUMENTATION_ENABLED``
     - Enables the updated WCF instrumentation, which delays execution until later in the WCF pipeline when the WCF server exception handling is established. The default value is ``false``.
   * - ``SIGNALFX_TRACE_HEADER_TAGS``
     - Comma-separated map of HTTP header keys to tag names, automatically applied as tags on traces.	For example: ``x-my-header:my-tag,header2:tag2``.
   * - ``SIGNALFX_TRACE_HTTP_CLIENT_EXCLUDED_URL_SUBSTRINGS``
     - Comma-separated list of URL substrings. Matching URLs are ignored by the tracer. For example, ``subdomain,xyz,login,download``.
   * - ``SIGNALFX_TRACE_KAFKA_CREATE_CONSUMER_SCOPE_ENABLED``
     - Enable to close consumer scope upon entering a method and starting a new one on method exit. The default value is ``true``.	
   * - ``SIGNALFX_TRACE_ROUTE_TEMPLATE_RESOURCE_NAMES_ENABLED``
     - Enable to base ASP.NET span and resource names on routing configuration, if applicable. The default value is ``true``.

.. _server-trace-information-dotnet:

Server trace information
==============================================

To connect Real User Monitoring (RUM) requests from mobile and web applications with server trace data, enable Splunk trace response headers by setting the following environment variable:

.. tabs::

   .. code-tab:: shell Windows PowerShell

      $env:SIGNALFX_TRACE_RESPONSE_HEADER_ENABLED=true

   .. code-tab:: shell Linux
   
      export SIGNALFX_TRACE_RESPONSE_HEADER_ENABLED=true

When you set this environment variable, your application instrumentation adds the following response headers to HTTP responses:

.. code-block::

   Access-Control-Expose-Headers: Server-Timing 
   Server-Timing: traceparent;desc="00-<serverTraceId>-<serverSpanId>-01"

The ``Server-Timing`` header contains the ``traceId`` and ``spanId`` parameters in ``traceparent`` format. See the following W3C documents for more information about the ``Server-Timing`` header:

-  https://www.w3.org/TR/server-timing
-  https://www.w3.org/TR/trace-context/#traceparent-header

.. _dotnet-debug-logging-settings:

Diagnostic logging settings
================================================

The following settings control the internal logging of the SignalFx Instrumentation for .NET:

.. list-table:: 
   :header-rows: 1
   :width: 100
   :widths: 40 60

   * - Setting
     - Description
   * - ``SIGNALFX_DIAGNOSTIC_SOURCE_ENABLED``
     - Enable to generate troubleshooting logs using the ``System.Diagnostics.DiagnosticSource`` class. The default value is ``true``.
   * - ``SIGNALFX_FILE_LOG_ENABLED``
     - Enables file logging. The default value is ``true``.
   * - ``SIGNALFX_MAX_LOGFILE_SIZE``
     - The maximum size for tracer log files, in bytes. The default value is ``245760``, or 10 megabytes.
   * - ``SIGNALFX_STDOUT_LOG_ENABLED``
     - Enables ``stdout`` logging. The default value is ``false``.
   * - ``SIGNALFX_STDOUT_LOG_TEMPLATE``
     - Configures the ``stdout`` log template using the Serilog formatting conventions. The default value is ``[{Level:u3}] {Message:lj} {NewLine}{Exception}{NewLine}``.
   * - ``SIGNALFX_TRACE_DEBUG``
     - Enable to activate debugging mode for the tracer. The default value is ``false``.
   * - ``SIGNALFX_TRACE_LOG_DIRECTORY``
     - Directory of the .NET tracer logs. Overrides the value in ``SIGNALFX_TRACE_LOG_PATH`` if present.	The default value is ``/var/log/signalfx/dotnet/`` for Linux and ``%ProgramData%\SignalFx .NET Tracing\logs\`` for Windows.
   * - ``SIGNALFX_TRACE_LOGGING_RATE``
     - The number of seconds between identical log messages for tracer log files. Setting this environment variable to ``0`` disables rate limiting. The default value is ``60``.
   * - ``SIGNALFX_TRACE_STARTUP_LOGS``
     - Enable to activate diagnostic logs at startup. The default value is ``true``.

.. _dotnet-default-service-name:

Changing the default service name
=============================================

By default, the SignalFx Instrumentation for .NET retrieves the service name by trying the following steps until it succeeds:

#. For the SignalFx .NET Tracing Azure Site Extension, the default service name is the site name as defined by the ``WEBSITE_SITE_NAME`` environment variable.

#. For ASP.NET applications, the default service name is ``SiteName[/VirtualPath]``.

#. For other applications, the default service name is the name of the entry assembly. For example, the name of your .NET project file.

#. If the entry assembly is not available, the instrumentation tries to use the current process name. The process name can be ``dotnet`` if launched directly using an assembly. For example, ``dotnet InstrumentedApp.dll``.

If all the steps fail, the service name defaults to ``UnknownService``. 

To override the default service name, set the ``SIGNALFX_SERVICE_NAME`` environment variable.