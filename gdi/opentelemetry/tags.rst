.. _otel-tags:

**********************************************************************************
Add tags to your configuration
**********************************************************************************

.. meta::
      :description: Add tags to your Splunk Distribution of OpenTelemetry Collector configuration. You can include span tags in settings for the batch processor in your configuration YAML file.

Tags are key-value pairs of data associated with recorded measurements to provide contextual information, distinguish, and group metrics during analysis and inspection. When measurements are aggregated to become metrics, tags are used as the labels to break down the metrics.

Include span tags in settings for the ``batch`` processor in your configuration YAML file. See :ref:`data processing <otel-data-processing>` for more information on batch processing.

You can add span tags as ``attributes/newenvironment``, which adds span tags to any spans that don’t already have the tags, or ``attributes/copyfromexistingkey``, which overrides an existing span tag value.

The settings look like this in the configuration YAML file:

.. code-block:: yaml

   processors:
   # Overrides an existing tag for a span.
   attributes/copyfromexistingkey:
     actions:
       - key: SPAN_TAG_KEY
         from_attribute: "SPAN_TAG_VALUE"
         action: upsert
   # Adds a tag to spans without a tag.
   attributes/newenvironment:
     actions:
       - key: SPAN_TAG_KEY
         value: "SPAN_TAG_VALUE"
         action: insert

If you include the ``batch`` processor, you have to add the processor to your pipelines, as shown in the following example:

.. code-block:: yaml

   service:
     pipelines:
       traces:
         processors: [batch]
