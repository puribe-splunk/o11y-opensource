.. _get-started-compute:

***************************************
Collect infrastructure metrics and logs
***************************************

.. meta::
   :description: Start sending application metrics and logs to Splunk Observability Cloud.

..	toctree::
   :hidden:

   k8s
   linux
   windows

Deploy the Splunk Distribution of OpenTelemetry Collector on your infrastructure to start sending application metrics and spans to Splunk Observability Cloud.

See the following topics to set up the Splunk OpenTelemetry Collector on each of these hosts:

 - :ref:`get-started-k8s`
 - :ref:`get-started-linux`
 - :ref:`get-started-windows`

 .. note::
    If you use Docker, then continue using the :new-page:`SignalFx Smart Agent <https://docs.splunk.com/Observability/gdi/smart-agent/smart-agent-resources.html#nav-Install-and-configure-the-SignalFx-Smart-Agent>` rather than the Splunk Distribution of OpenTelemetry Collector. While OpenTelemetry development continues, only the Smart Agent currently supports Docker.
