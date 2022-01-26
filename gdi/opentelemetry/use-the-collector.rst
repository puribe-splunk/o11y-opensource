.. _otel-using:

***************************************************************
Use the OpenTelemetry Collector with Splunk Observability Cloud
***************************************************************

.. meta::
      :description: Describes the resources needed to use Splunk Distribution of OpenTelemetry Collector.

.. toctree::
    :maxdepth: 4
    :titlesonly:
    :hidden:

    components.rst
    data-processing.rst
    deployment-modes.rst
    exposed-endpoints.rst
    monitoring.rst
    plan-deployment.rst
    security.rst
    tags.rst
    Configuration file translator <translation-tool.rst>

The following table describes everything you need to start using the Splunk OpenTelemetry Collector.

.. list-table::
   :widths: 40 60
   :header-rows: 1

   * - Resource
     - Description
   * - Access token
     - Use an access token to track and manage your resource usage. Where you see ``<access_token>``, replace it with the name of your access token. See :ref:`admin-org-tokens`.
   * - Realm
     - A realm is a self-contained deployment that hosts organizations. You can find your realm name on your profile page in the user interface. Where you see ``<REALM>``, replace it with the name of your organization's realm. See :new-page:`realms <https://dev.splunk.com/observability/docs/realms_in_endpoints/>`.
   * - Agent or Gateway mode
     - In Agent mode, the Splunk OpenTelemetry Collector runs with the application or on the same host as the application. In Gateway mode, one or more collectors run a standalone service, for example, a container or deployment. See :ref:`otel-deployment-mode`.
   * - Ports
     - Check exposed ports to make sure your environment doesn't have conflicts. You can change the ports in the Splunk OpenTelemetry Collector's configuration. See :ref:`otel-exposed-endpoints`.
   * - Configuration file translator
     - Splunk provides an experimental command-line tool, ``translatesfx``, that translates a SignalFx Smart Agent configuration file into a configuration file that can be used by the Splunk OpenTelemetry Collector. See :ref:`otel-translation-tool`.

