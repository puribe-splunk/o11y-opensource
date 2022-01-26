.. _docs-home-page:

********
Welcome!
********

Use Splunk Observability Cloud to monitor critical information about your applications, infrastructure, and cloud services.

Observability Cloud provides a unified experience for collecting and monitoring metrics, logs, and traces from common data sources. Data collection and monitoring in one place enables full-stack, end-to-end observability of your entire infrastructure.

.. role:: icon-cloud-upload
.. rst-class:: cardlong

:strong:`Start getting data in` Get data in. Learn how to monitor your infrastructure. :ref:`get-started-get-data-in` :icon-cloud-upload:`.`

.. role:: icon-tree
.. rst-class:: card

:strong:`Monitor your infrastructure` View the health of your infrastructure. Monitor cloud services, hosts, and containers. :ref:`get-started-infrastructure` :icon-tree:`.`

.. role:: icon-display
.. rst-class:: card

:strong:`Monitor your applications` View the performance of applications. Monitor inbound and outbound dependencies for each service. :ref:`get-started-apm` :icon-display:`.`

.. role:: icon-users
.. rst-class:: card

:strong:`Monitor user experiences` Monitor user interactions. Analyze web application performance. :ref:`get-started-rum`:icon-users:`.`

.. role:: icon-wrench
.. rst-class:: card

:strong:`Troubleshoot with logs` Troubleshoot incidents with your infrastructure and cloud services. :ref:`get-started-logs` :icon-wrench:`.`

.. role:: icon-user-tie
.. rst-class:: cardlong

:strong:`Administration` Manage users and teams. Control access with authentication tokens. :ref:`admin-admin` :icon-user-tie:`.`


.. ----- This comment separates the landing page from the TOC -----

.. toctree::
   :caption: GET STARTED
   :maxdepth:   2

   get-started/welcome

.. toctree::
   :maxdepth:   3

   Get started <get-started/o11y>

.. toctree::
   :maxdepth:   3

   Enable Related Content <get-started/relatedcontent>

.. toctree::
   :maxdepth:   2

   Use case: Troubleshoot an issue from the browser to the backend <get-started/use-case>


.. toctree::
   :maxdepth:   3

   Data retention <get-started/data-retention>


.. toctree::
   :caption: DATA SETUP
   :maxdepth:   3

   gdi/get-data-in/get-data-in

.. toctree::
   :maxdepth:   3

   gdi/get-data-in/integrations

.. toctree::
   :maxdepth:   3

   gdi/get-data-in/connect/connect

.. toctree::
   :maxdepth:   3

   gdi/get-data-in/compute/compute

.. toctree::
   :maxdepth:   3

   Instrument back-end services <gdi/get-data-in/application/application>

.. toctree::
   :maxdepth:   3

   Instrument serverless functions <gdi/get-data-in/serverless/instrument-serverless-functions>

.. toctree::
   :maxdepth:  3

   Instrument front-end applications <gdi/get-data-in/rum/rum-instrumentation>

.. toctree::
   :maxdepth:   3

   gdi/index

.. toctree::
   :maxdepth: 3

   gdi/opentelemetry/opentelemetry

.. toctree::
   :maxdepth: 3

   gdi/smart-agent/smart-agent-resources

.. toctree::
   :maxdepth: 3

   gdi/other-ingestion-methods/send-custom-metrics

.. toctree::
   :maxdepth:   3

   gdi/agent-commands/otel-commands

.. toctree::
   :maxdepth:   3

   gdi/agent-commands/smart-agent-commands

.. toctree::
   :caption: APM
   :maxdepth:   3

   apm/intro-to-apm

.. toctree::
   :maxdepth:   3

   apm/apm

.. toctree::
   :maxdepth:   3

   apm/terms-concepts/terms-concepts

.. toctree::
   :maxdepth:   3

   Use cases: Troubleshoot errors and monitor application performance <apm/apm-use-cases/apm-use-cases-intro>

.. toctree::
   :maxdepth:   3

   Analyze services with span tags <apm/span-tags/span-tags>

.. toctree::
   :maxdepth:   3

   apm/workflows/workflows

.. toctree::
   :maxdepth:   3

   apm/apm-dashboards

.. toctree::
   :maxdepth:   3

   apm/detectors-alerts/apm-alerts

.. toctree::
   :maxdepth:   3

   apm/apm-data-links/apm-index

.. toctree::
   :maxdepth:   3

   apm/inferred-services

.. toctree::
   :maxdepth:   3

   apm/download-traces

.. toctree::
   :maxdepth:   3

   apm/db-query-perf/db-query-performance

.. toctree::
   :maxdepth:   3

   apm/extended-trace-retention

.. toctree::
   :maxdepth:   3

   apm/sensitive-data-controls

.. toctree::
   :maxdepth:   3

   apm/troubleshoot

.. toctree::
   :maxdepth:   3

   apm/span-formats

.. toctree::
   :caption: PROFILING
   :maxdepth:   3

   Introduction to AlwaysOn Profiling <profiling/intro-profiling>

.. toctree::
   :maxdepth:   3

   Use case: Find performance issues <profiling/profiling-use-case>

.. toctree::
   :maxdepth:   3

   Get AlwaysOn Profiling data in <profiling/get-data-in-profiling>

.. toctree::
   :maxdepth:   3

   Browse stack traces linked to spans <profiling/spans-stack-traces>

.. toctree::
   :maxdepth:   3

   Understand and use the flame graph <profiling/using-the-flamegraph>
   
.. toctree::
   :maxdepth:   3

   Profiling terms and concepts <profiling/concepts-terms-profiling>

.. toctree::
   :maxdepth:   3

   profiling/profiling-troubleshooting

.. toctree::
   :maxdepth:   3

   Third-party acknowledgements <profiling/profiling-third-party-credits>

.. toctree::
   :caption: INFRASTRUCTURE
   :maxdepth:   3

   infrastructure/intro-to-infrastructure

.. toctree::
   :maxdepth:   2

   Quick start tutorial <infrastructure/quickstart-imm>

.. toctree::
   :maxdepth:   3

   infrastructure/infrastructure

.. toctree::
   :maxdepth:   3

   infrastructure/terms-concepts/terms-concepts

.. toctree::
   :maxdepth:   3

   infrastructure/navigators/navigators

.. toctree::
   :maxdepth:   3

   infrastructure/analytics/infrastructure-monitoring-analytics

.. toctree::
   :maxdepth:   3

   infrastructure/analytics/signalflow

.. toctree::
   :maxdepth:   3

   infrastructure/system-limits

.. toctree::
   :caption: LOG OBSERVER
   :maxdepth:   3

   logs/intro-to-logs

.. toctree::
   :maxdepth:   3

   logs/logs

.. toctree::
   :maxdepth:   3

   logs/intro-logconnect.rst

.. toctree::
   :maxdepth:   3

   logs/set-up-logconnect

.. toctree::
   :maxdepth:   3

   logs/live-tail

.. toctree::
   :maxdepth:   3

   logs/timeline

.. toctree::
   :maxdepth:   3

   logs/raw-logs-display

.. toctree::
   :maxdepth:   3

   logs/keyword

.. toctree::
   :maxdepth:   3

   logs/filter-logs-by-field

.. toctree::
   :maxdepth:   3

   logs/individual-log

.. toctree::
   :maxdepth:   3

   logs/aggregations

.. toctree::
   :maxdepth:   3

   logs/search-time-rules

.. toctree::
   :maxdepth:   3

   logs/save-share

.. toctree::
   :maxdepth:   3

   logs/pipeline

.. toctree::
   :maxdepth:   3

   logs/processors

.. toctree::
   :maxdepth:   3

   logs/metricization

.. toctree::
   :maxdepth:   3

   logs/infinite

.. toctree::
   :maxdepth:   3

   logs/limits

.. toctree::
   :caption: RUM
   :maxdepth:   3

   rum/intro-to-rum

.. toctree::
   :maxdepth:   3

   Set up Splunk RUM <rum/set-up-rum>

.. toctree::
   :maxdepth:   3

   rum/data-collected

.. toctree::
   :maxdepth:   3

   rum/rum-terminology-concepts

.. toctree::
   :maxdepth:   3

   rum/RUM-custom-events

.. toctree::
   :maxdepth:   3

   rum/error-aggregates

.. toctree::
   :maxdepth:   3

   rum/rum-alerts

.. toctree::
   :maxdepth:   3

   rum/identify-span-problems

.. toctree::
   :maxdepth:   3

   rum/use-cases-mobile

.. toctree::
   :maxdepth:   3

   rum/sample-app

.. toctree::
   :maxdepth:   3

   rum/rum-third-party-software

.. toctree::
   :caption: MOBILE
   :maxdepth:   3

   mobile/intro-to-mobile

.. toctree::
   :maxdepth:   3

   Download the app <mobile/download-mobile>

.. toctree::
   :maxdepth:   3

   View dashboards and alerts <mobile/use-mobile>

.. toctree::
   :caption: DASHBOARDS AND CHARTS
   :maxdepth:   3

   Dashboards<data-visualization/dashboards/dashboards>

.. toctree::
   :maxdepth:   3

   Charts<data-visualization/charts/charts>

.. toctree::
   :maxdepth:   3

   Use chart and dashboard features<data-visualization/chart-and-dashboard-features/use-chart-and-dashboard-features>

.. toctree::
   :caption: ALERTS AND DETECTORS
   :maxdepth:   3

   Introduction to alerts and detectors <alerts-detectors-notifications/alerts-detectors-notifications>

.. toctree::
   :maxdepth:   3

   Create detectors to trigger alerts <alerts-detectors-notifications/create-detectors-for-alerts>

.. toctree::
   :maxdepth:   3

   alerts-detectors-notifications/detector-manage-permissions

.. toctree::
   :maxdepth:   3

   Link detectors to charts <alerts-detectors-notifications/link-detectors-to-charts>

.. toctree::
   :maxdepth:   3

   Manage notification subscribers <alerts-detectors-notifications/manage-notifications>

.. toctree::
   :maxdepth:   3

   Detector options <alerts-detectors-notifications/detector-options>

.. toctree::
   :maxdepth:   3

   Preview detector alerts <alerts-detectors-notifications/preview-detector-alerts>

.. toctree::
   :maxdepth:   3

   View alerts <alerts-detectors-notifications/view-alerts>

.. toctree::
   :maxdepth:   3

   View detectors <alerts-detectors-notifications/view-detectors>

.. toctree::
   :maxdepth:   3

   Add context to metrics using events <alerts-detectors-notifications/view-data-events>

.. toctree::
   :maxdepth:  3

   Built-in alert conditions <alerts-detectors-notifications/alert-condition-reference/index>

.. toctree::
   :maxdepth:   3

   Mute alert notifications <alerts-detectors-notifications/mute-notifications>

.. toctree::
   :maxdepth:   3

   Auto-clearing alerts <alerts-detectors-notifications/auto-clearing-alerts>

.. toctree::
   :maxdepth:   3

   Troubleshoot detectors <alerts-detectors-notifications/troubleshoot-detectors>

.. toctree::
   :caption: METRICS
   :maxdepth:   3

   Introduction to metrics <metrics-and-metadata/metrics>

.. toctree::
   :maxdepth:   3

   metrics-and-metadata/metric-types

.. toctree::
   :maxdepth:   3

   metrics-and-metadata/metrics-dimensions-mts

.. toctree::
   :maxdepth:   3

   metrics-and-metadata/metric-names

.. toctree::
   :maxdepth:   3

   metrics-and-metadata/metrics-finder-metadata-catalog

.. toctree::
   :caption: ADMINISTER OBSERVABILITY CLOUD
   :maxdepth:   3

   admin/admin

.. toctree::
   :maxdepth:   3

   Create and manage users <admin/users/manage-users>

.. toctree::
   :maxdepth:   3

   Create and manage teams <admin/teams/manage-teams>

.. toctree::
   :hidden:
   :maxdepth:   3

   Configure SSO integrations <admin/sso>

.. toctree::
   :hidden:
   :maxdepth:   3

   Send alert notifications to third-party services <admin/notif-services/admin-notifs-index>

.. toctree::
   :hidden:
   :maxdepth:   3

   admin/allow-services

.. toctree::
   :maxdepth:   3

   Create and manage access tokens <admin/authentication-tokens/tokens>

.. toctree::
   :hidden:
   :maxdepth:   3

   Link metadata to related resources using global data links <admin/link-metadata-to-content>

.. toctree::
   :hidden:
   :maxdepth:   3

   admin/apm-billing-usage/apm-billing-usage-index

.. toctree::
   :hidden:
   :maxdepth:   3

   admin/imm-billing-usage/monitor-imm-billing-usage

.. toctree::
   :hidden:
   :maxdepth:   3

   admin/imm-billing-usage/dpm-usage

.. toctree::
   :maxdepth:   3

   View organization metrics <admin/org-metrics>

.. toctree::
   :maxdepth:   3

   Use case: Maintain a secure organization with many teams and users <admin/use-case-org-security>

.. toctree::
   :caption: REFERENCES
   :maxdepth:   3

   Third-party software credits <references/third-party-credits>

.. toctree::
   :maxdepth:   3

   Glossary <references/glossary>
