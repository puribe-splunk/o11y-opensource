.. _get-started-php:

*******************************
Instrument a PHP application
*******************************

.. meta::
   :description: Instrument a PHP application to export metrics and spans to Splunk Observability Cloud.


The SignalFx Tracing Library for PHP provides an OpenTracing-compatible tracer and automatically configurable instrumentations for many popular PHP libraries and frameworks. This is a native extension that supports PHP versions 5.4+ running on the Zend Engine.

For all available configuration options and their default values, see :new-page:`the README file <https://github.com/signalfx/signalfx-php-tracing#readme>`.

Start the integration
========================

To start a PHP integration, follow these steps:

1. In the Observability Cloud main menu, select :strong:`Data Setup`.

2. In the :strong:`CATEGORIES` menu, select :strong:`APM Instrumentation`.

3. Click :strong:`PHP Tracing`.

4. Click :strong:`Add Connection`. The integration guided setup appears.

5. Follow the steps in the guided setup.

6. At the end of the guided setup, go to APM to see a live view of your data flowing into the application.
