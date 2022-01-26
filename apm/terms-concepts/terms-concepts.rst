.. _apm-terms-concepts:

*********************************
Terms and concepts in Splunk APM
*********************************

.. meta::
   :description: Learn about important terms and concepts in Splunk APM.

.. toctree::
   :hidden:

   environments
   identities
   metricsets
   traces-spans

.. list-table::
   :header-rows: 1
   :widths: 20, 80

   * - :strong:`Concept`
     - :strong:`Description`
   
   * - Environment
     - A distinct deployment in Splunk APM that doesn't interact directly with other deployments of the same application. For more information, see :ref:`apm-environments`.

   * - Identity
     - A unique set of indexed span tags that corresponds to a Splunk APM object. For more information, see :ref:`apm-identities`.

   * - Monitoring MetricSets
     - Metrics for traces and spans to monitor and alert the performance of applications and services. For more information, see :ref:`apm-metricsets`.

   * - Troubleshooting MetricSets
     - Metrics for traces and spans to troubleshoot issues with applications and services. For more information, see :ref:`apm-metricsets`.

   * - Span 
     - A single operation within a trace. For more information, see :ref:`apm-traces-spans`.

   * - Trace 
     - A collection of operations that represents a unique transaction an application handles. For more information, see :ref:`apm-traces-spans`.

     
