.. _instr-troubleshooting:

*****************************************************************
Troubleshoot your instrumentation
*****************************************************************

If you have instrumented an application but are not seeing the relevant data in Splunk APM, use the following guidelines to troubleshoot your instrumentation.

Language-specific troubleshooting
============================================

- :ref:`common-java-troubleshooting`
- :ref:`common-python-troubleshooting`
- :ref:`common-nodejs-troubleshooting`

General troubleshooting
============================================
 
1. Make sure the application on the relevant runtime is active. Traces can only be captured from the inbound traffic to the application. 

2. Check that the runtime to which you attached the instrumentation agent has started. For example, for Java you can run ``jps -lvm`` in the command line to list all active Java processes. The output of the command is the list of the JVMs currently running in the machine. Make sure the JVM in question appears among them. If not, the JVM failed to start, and you should check the JVM or application logs to find the cause of the problem.

3. Check that the instrumentation is applied to the runtime. For example, for Java you can confirm that the ``javaagent`` was correctly attached in the configuration by exposing the following:

.. code-block:: shell

   me@my-precious:~$ jps -lvm
   24910 root.war -Xmx512m -ea 
   -javaagent:/otel/splunk-otel-javaagent.jar 
   -Dotel.resource.attributes="service.name=billing,environment=test"

   24914 jdk.jcmd/sun.tools.jps.Jps -lvm 
   -Dapplication.home=/usr/lib/jvm/jdk-11.0.10 -Xms8m 
   -Djdk.module.main=jdk.jcmd

   my-precious:~ me$


If the output above does not contain the ``-javaagent`` section for the JVM in question, the parameter specified in the startup script was not picked up. To proceed, you should debug the startup scripts to see where the added or modified parameters are being lost.

4. Check that the additional startup parameters are correctly passed to the runtime. You should be seeing the service name being applied correctly. For instance, the example above refers to ``service.name=billing,environment=test``.

If steps 1-4 reveal nothing of interest, check the application ``stdout`` and ``stderr`` logs after enabling debugging mode in your instrumentation. Search for error messages containing ``opentelemetry``. OpenTelemetry errors might reveal errors similar to the following, which exposes a misconfigured Collector endpoint:

.. code-block:: bash

    2021-03-24 13:01:28.597  INFO 1 --- [           main] 
    o.hibernate.annotations.common.Version   : HCANN000001: Hibernate 
    Commons Annotations {5.1.0.Final}

    [opentelemetry.auto.trace 2021-03-24 13:01:29:072 +0000] 
    [BatchSpanProcessor_WorkerThread-1] WARN 
    io.opentelemetry.exporter.jaeger.thrift.JaegerThriftSpanExporter 
    - Failed to export spans

    io.jaegertracing.internal.exceptions.SenderException: Could not send 16 spans 

        at 
    io.jaegertracing.thrift.internal.senders.HttpSender.send(HttpSender.java:69)
    
    ... stacktrace cut for brevity ...

        at 
    io.jaegertracing.thrift.internal.senders.HttpSender.send(HttpSender.java:67)

    2021-03-24 13:01:29.831  INFO 1 --- [           main]
    org.hibernate.dialect.Dialect            : HHH000400: Using 
    dialect: org.hibernate.dialect.MySQL57Dialect


If the steps above don't help you instrument the service, contact observability-support@splunk.com for further assistance.