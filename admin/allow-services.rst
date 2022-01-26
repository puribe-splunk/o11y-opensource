.. _allow-services:

*****************************************************************
Allow Splunk Observability Cloud services in your network
*****************************************************************

.. meta::
      :description: There are several options for securing your implementation, including proxies and allow lists.
      :keywords:  security proxy allow list

Splunk Observability Cloud is composed of a number of different services. If your organization has stringent networking security policies that apply to sending data to third parties, use one of the following methods to ensure network access to Splunk Observability Cloud services:

- :ref:`simple-http-proxy`
- :ref:`otel-connector`


.. _simple-http-proxy:

Use a simple HTTP/HTTPS proxy
================================

If you can get out to the internet using a proxy, then using an HTTP/HTTPS proxy is your simplest option.

Ensure that you give the proxy the ability to resolve the network names and make outbound HTTP/HTTPS network connections to the URLs listed in :ref:`allow-urls` or the domains listed in :ref:`allow-domains`.


.. _otel-connector:

Use Splunk OpenTelemetry Collector
=========================================

Use :new-page:`Splunk OpenTelemetry Collector <https://docs.splunk.com/Observability/gdi/opentelemetry/deployment-modes.html>` in gateway mode. You can forward metrics locally to Splunk OpenTelemetry Collector and it will serve as your local store-and-forward service for telemetry.

Ensure that you give Splunk OpenTelemetry Collector the ability to resolve the network names and make outbound HTTPS network connections to the URLs listed in :ref:`allow-urls` or the domains listed in :ref:`allow-domains`.


Replace the SignalFx Gateway with Splunk OpenTelemetry Collector
=======================================================================

If you are using the SignalFx Gateway, replace it with the Splunk OpenTelemetry Collector running in :new-page:`gateway mode <https://docs.splunk.com/Observability/gdi/opentelemetry/deployment-modes.html>`.

.. _allow-urls:

URLs to allow in your network
================================

.. include:: /_includes/realm-note.rst

If your organization’s networking security policies require that services delivered over the internet be individually allowed, ensure that the following service URLs are allowed by your network.

.. code:: shell

   \*.signalfx.com

   \*.<YOUR_REALM>.signalfx.com

If you are unable to allow all URLs as shown here, see :ref:`allow-domains`.

.. _allow-domains:

Domains to allow in your network
===================================

If you are unable to allow all URLs as described in :ref:`allow-urls`, ensure that the following domains are allowed by your network.

.. code:: shell

   # SignalFx API base URL (https://dev.splunk.com/observability/docs/apibasics/api_list)
   api.<YOUR_REALM>.signalfx.com

   # Splunk Observability Cloud user interface
   app.<YOUR_REALM>.signalfx.com
   customer-api.<YOUR_REALM>.signalfx.com

   # CDN for Splunk Observability Cloud files and installers
   dl.signalfx.com

   # Backfill API base URL (https://dev.splunk.com/observability/reference/api/backfill/latest)
   backfill.<YOUR_REALM>.signalfx.com

   # Data ingest API base URL (https://dev.splunk.com/observability/docs/datamodel/ingest/)
   ingest.<YOUR_REALM>.signalfx.com

   # SignalFlow API base URL (https://dev.splunk.com/observability/reference/api/signalflow/latest)
   stream.<YOUR_REALM>.signalfx.com

   # For td-agent/Fluentd on Linux and Windows
   packages.treasuredata.com
   
   # For DEB/RPM collector packages
   splunk.jfrog.io 
   
.. note:: For more information, see :new-page:`Endpoint Summary <https://dev.splunk.com/observability/docs/apibasics/api_list>` in the Developer Guide.