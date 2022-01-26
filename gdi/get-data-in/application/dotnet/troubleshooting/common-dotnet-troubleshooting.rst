.. _common-dotnet-troubleshooting:

*******************************************************************
Troubleshoot .NET instrumentation for Splunk Observability Cloud
*******************************************************************

.. meta::
   :description: If your instrumented .NET application is not sending data to Splunk Observability Cloud, or data is missing, follow these steps to identify the issue and solve it.

When you instrument a .NET application using the SignalFx Instrumentation for .NET and you don't see your data in Observability Cloud, review these solutions.

.. _enable-dotnet-debug-logging:

General troubleshooting
===================================================

Follow these steps to troubleshoot general instrumentation issues:

#. Check that you've configured all settings according to your needs. See :ref:`advanced-dotnet-configuration`.

#. Check what environment variables apply to your process using tools such as Process Explorer. On Linux, run ``cat /proc/<pid>/environ`` where ``<pid>`` is the process ID.

.. include:: /_includes/troubleshooting-steps.rst

Enable debug logging
----------------------------------------------------

You can enable debug logging to obtain more information about the issue:

#. Set the ``SIGNALFX_TRACE_DEBUG`` environment variable to ``true`` before starting your instrumented application. 

#. Run your application or service and generate some activity.

#. Collect the debug logs. By default, log files are in the following locations:

   * Windows: ``%ProgramData%\SignalFx .NET Tracing\logs\``
   * Linux: ``/var/log/signalfx/dotnet/``. If it doesn't exist, run ``/opt/signalfx/createLogPath.sh``.

You can change the default location by updating the ``SIGNALFX_TRACE_LOG_DIRECTORY`` environment variable. See :ref:`dotnet-debug-logging-settings` for more information and settings.

.. note:: Enable debug logging only when needed. Debug mode requires more resources.

.. _dotnet-troubleshoot-linux:

.NET instrumentation not working on Linux
=====================================================

Installing the instrumentation on Linux might fail if you use an incompatible package.

Make sure that you're using an installation package that is compatible with your Linux distribution. To find out your distribution or package manager, run the following commands:

.. code-block:: shell

   lsb_release -a
   cat /etc/*release
   cat /etc/issue*
   cat /proc/version

.. _dotnet-troubleshoot-cpu:

High CPU usage
====================================================

By default, the SignalFx Instrumentation for .NET instruments all .NET processes running on the host automatically. This might significantly increase CPU usage if you've enabled the instrumentation in the system or user scope. Make sure that the instrumentation's environment variables are always set in the process or terminal scope.

To restrict global instrumentation to a set of processes, use the ``SIGNALFX_PROFILER_PROCESSES`` and ``SIGNALFX_PROFILER_EXCLUDE_PROCESSES`` environment variables, which include and exclude processes for instrumentation. See :ref:`advanced-dotnet-configuration` for more information.