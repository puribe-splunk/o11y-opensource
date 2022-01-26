.. _instrument-dotnet-applications:

***************************************************************************
Instrument a .NET application for Splunk Observability Cloud
***************************************************************************

.. meta:: 
   :description: The SignalFx Instrumentation for .NET automatically instruments .NET applications, Windows services running .NET applications, ASP.NET applications deployed on IIS, and Azure App Service apps. Follow these steps to get started.

The SignalFx Instrumentation for .NET automatically instruments .NET applications, Windows services running .NET applications, ASP.NET applications deployed on IIS, and Azure App Service applications.

.. tip:: To generate all the basic install commands for your environment and application, open the Observability Cloud guided setup in :strong:`Data Setup > APM Instrumentation > .NET > Add Connection`.

.. _install-dotnet-instrumentation:

Instrument a .NET application
===================================================================

Follow these steps to automatically instrument your application:

#. Check that you meet the requirements. See :ref:`dotnet-requirements`.

#. Download the latest release of the SignalFx Instrumentation for .NET for your operating system from the :new-page:`Releases page on GitHub <https://github.com/signalfx/signalfx-dotnet-tracing/releases/latest>`.

#. Install the package for your operating system:

   .. tabs:: 

      .. code-tab:: shell Windows x64

         msiexec /i signalfx-dotnet-tracing-x64.msi /quiet

      .. code-tab:: shell Windows x86

         msiexec /i signalfx-dotnet-tracing-x86.msi /quiet

      .. code-tab:: bash rpm

         rpm -ivh signalfx-dotnet-tracing.rpm
         ./opt/signalfx/createLogPath.sh # Optional

      .. code-tab:: bash deb

         dpkg -i signalfx-dotnet-tracing.deb
         ./opt/signalfx/createLogPath.sh # Optional

      .. code-tab:: bash tar (qlibc)

         tar -xf signalfx-dotnet-tracing.tar.gz -C /opt/signalfx
         ./opt/signalfx/createLogPath.sh # Optional

#. Set the following environment variables:

   .. tabs::

      .. code-tab:: shell Windows PowerShell
      
         # Set the following variables in the process scope
         $Env:COR_ENABLE_PROFILING = "1"                                   
         $Env:COR_PROFILER = "{B4C89B0F-9908-4F73-9F59-0D77C5A06874}"     
         $Env:CORECLR_ENABLE_PROFILING = "1"                             
         $Env:CORECLR_PROFILER = "{B4C89B0F-9908-4F73-9F59-0D77C5A06874}"
         $Env:SIGNALFX_SERVICE_NAME = "<my-service-name>"
         $Env:SIGNALFX_ENV = "<your-environment>"

      .. code-tab:: shell Linux
      
         export CORECLR_ENABLE_PROFILING="1"                               
         export CORECLR_PROFILER="{B4C89B0F-9908-4F73-9F59-0D77C5A06874}" 
         export CORECLR_PROFILER_PATH="/opt/signalfx/SignalFx.Tracing.ClrProfiler.Native.so"
         export SIGNALFX_DOTNET_TRACER_HOME="/opt/signalfx"
         export SIGNALFX_SERVICE_NAME="<my-service-name>"
         export SIGNALFX_ENV="<your-environment>"

   .. warning:: Avoid setting the environment variables in the system or user scopes in Windows unless you require permanent autoinstrumentation. See :ref:`advanced-dotnet-configuration` for more information on how to include or exclude processes for autoinstrumentation.

#. Run your application.

By default, the SignalFx Instrumentation for .NET uses the Splunk OTel Collector to send trace data to Observability Cloud. If no data appears in :strong:`Observability > APM`, see :ref:`common-dotnet-troubleshooting`.

.. note:: If you need to add custom attributes to spans or want to manually generate spans, instrument your .NET application or service manually. See :ref:`dotnet-manual-instrumentation`.

.. _instrument-windows-service:

Instrument a Windows service running a .NET application
===================================================================

To instrument a Windows service, install the instrumentation and set the following environment variables:

.. code-block:: shell

   $svcName = "MySrv"    # Name of the Windows service you want to instrument
   [string[]] $vars = @(
      "COR_ENABLE_PROFILING=1",                                  # Enable .NET Framework Profiler
      "COR_PROFILER={B4C89B0F-9908-4F73-9F59-0D77C5A06874}",     # Select .NET Framework Profiler
      "CORECLR_ENABLE_PROFILING=1",                              # Enable .NET (Core) Profiler
      "CORECLR_PROFILER={B4C89B0F-9908-4F73-9F59-0D77C5A06874}", # Select .NET (Core) Profiler
      "SIGNALFX_SERVICE_NAME=<my-service-name>"                  # Set service name
      "SIGNALFX_ENV=<environment-name>"                          # Set environment name
   )
   Set-ItemProperty HKLM:SYSTEM\CurrentControlSet\Services\$svcName -Name Environment -Value $vars
   # Every time you start the service, it will be auto-instrumented.

For more information on the default service name, see :ref:`dotnet-default-service-name`.

.. _instrument-aspnet-iis:

Instrument an ASP.NET application deployed on IIS
===================================================================

To instrument an ASP.NET application running on IIS, install the instrumentation and edit the ``web.config`` file to add the following settings. See :ref:`configuration-methods-dotnet` for more information.

.. tabs::

   .. tab:: ASP.NET 4.x and higher

      Add the following settings inside the ``<appSettings>`` block of your ``web.config`` file:

      .. code-block:: xml

         <add key="SIGNALFX_SERVICE_NAME" value="service-name" />
         <add key="SIGNALFX_ENV" value="environment-name" />

   .. tab:: ASP.NET Core

      Add the following settings inside the ``<aspNetCore>`` block of your ``web.config`` file:

      .. code-block:: xml

         <environmentVariables>
            <environmentVariable name="CORECLR_ENABLE_PROFILING" value="1" />
            <environmentVariable name="CORECLR_PROFILER" value="{B4C89B0F-9908-4F73-9F59-0D77C5A06874}" />
            <environmentVariable name="SIGNALFX_SERVICE_NAME" value="service-name" />
            <environmentVariable name="SIGNALFX_ENV" value="environment-name" />
         </environmentVariables>

.. note:: By default, the installer enables IIS instrumentation for .NET Framework by setting the ``Environment`` registry key for W3SVC and WAS services located in the ``HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`` folder.

.. _instrument-azure-app:

Instrument an application in Azure App Service
===================================================================

To instrument an application or service in Azure App Service, follow these steps:

#. Select your application in App Service.

#. Go to :guilabel:`Development Tools > Extensions`.

#. Find and install the :strong:`SignalFx .NET Tracing` extension.

#. Go to :guilabel:`Settings > Configuration`.

#. Click :strong:`New application setting` to add the following settings:

   .. list-table:: 
      :header-rows: 1
      :width: 100%
      :widths: 40 60

      * - Name
        - Value
      * - ``SIGNALFX_ACCESS_TOKEN``
        - Your Splunk access token. To obtain an access token, see :ref:`admin-api-access-tokens`.
      * - ``SIGNALFX_ENDPOINT_URL``
        - The ingest endpoint URL. The default value is ``https://ingest.<realm>.signalfx.com/v2/trace``, where ``realm`` is the :new-page:`O11y realm <https://dev.splunk.com/observability/docs/realms_in_endpoints>`; for example, ``us0``.
      * - ``SIGNALFX_SERVICE_NAME``
        - The name of your service or application.
      * - ``SIGNALFX_ENV``
        - The name of your environment where you're instrumenting the application.

#. Restart the application in App Service.

.. tip:: To reduce latency and benefit from OTel Collector features, set the endpoint URL to a Collector instance running in Azure VM over an Azure VNet.

.. _kubernetes_dotnet:

Deploy the .NET instrumentation in Kubernetes
==========================================================

To deploy the .NET instrumentation in Kubernetes, configure the Kubernetes Downward API to expose environment variables to Kubernetes resources.

The following example shows how to update a deployment to inject environment variables by adding the agent configuration under the ``.spec.template.spec.containers.env`` section:

.. code-block:: yaml

   apiVersion: apps/v1
   kind: Deployment
   spec:
     selector:
       matchLabels:
         app: your-application
     template:
       spec:
         containers:
           - name: myapp
             env:
               - name: SPLUNK_OTEL_AGENT
                 valueFrom:
                   fieldRef:
                     fieldPath: status.hostIP
               - name: SIGNALFX_SERVICE_NAME
                 value: '{{serviceName}}'
               - name: SIGNALFX_ENDPOINT_URL
                 value: '{{endpoint}}'
               - name: SIGNALFX_ENV
                 value: '{{environmentName}}'
               - name: CORECLR_ENABLE_PROFILING
                 value: "1"
               - name: CORECLR_PROFILER
                 value: '{B4C89B0F-9908-4F73-9F59-0D77C5A06874}'
               - name: CORECLR_PROFILER_PATH
                 value: '/opt/signalfx/SignalFx.Tracing.ClrProfiler.Native.so'
               - name: SIGNALFX_DOTNET_TRACER_HOME
                 value: '/opt/signalfx'