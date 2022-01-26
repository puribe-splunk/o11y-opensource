.. _get-started-use-case:

******************************************************************************************************
Use case: Troubleshoot an issue from the browser to the back end using Splunk Observability Cloud
******************************************************************************************************

.. meta::
   :description: This use case-based documentation describes how a site reliability engineer (SRE) can use Splunk Observability Cloud products and features to troubleshoot a site issue starting with the end user's browser-based experience all the way to microservices on the backend.

Buttercup Games, a fictitious company, runs an e-commerce site to sell its products. They recently refactored their site to use a cloud-native approach with a microservices architecture and Kubernetes for the infrastructure.

Buttercup Games site reliability engineers (SREs) and service owners work together to monitor and maintain the site to be sure that people have a great experience when they visit. One of the many reasons they decided to take a cloud-native approach is because it would make their system observable by tools. They implemented Splunk Observability Cloud as their observability solution.

This use case describes how Kai, an SRE, and Deepu, a service owner, performed the following tasks using Splunk Observability Cloud to troubleshoot and identify the root cause of a recent Buttercup Games site incident:

:ref:`receive-alerts-xpuc`

:ref:`assess-user-impact-xpuc` using Splunk Real User Monitoring (RUM)

:ref:`investigate-root-cause-xpuc` using Splunk Application Performance Monitoring (APM)

:ref:`check-infra-health-xpuc` using Splunk Infrastructure Monitoring

:ref:`look-for-patterns-xpuc` in Splunk APM

:ref:`review-logs-xpuc` using Splunk Log Observer

:ref:`monitor-a-fix-xpuc` using Splunk Log Observer

:ref:`take-preventative-action-xpuc`

For a video version of this use case, see :new-page:`Splunk’s Observability Cloud Demo <https://www.splunk.com/en_us/resources/videos/watch-splunks-observability-cloud-demo.html>`.


.. _receive-alerts-xpuc:

1. Receive alerts about outlier behavior
=================================================================================

Kai, the SRE on-call, receives an alert showing that the number of purchases on the Buttercup Games site has dropped significantly in the past hour and the checkout completion rate is abnormally low.

Kai trusts that these are true outlier behaviors because the alert rule their team set up in Splunk Observability Cloud takes into account the time and day of week as a dynamic baseline, rather than simply using a static threshold.

Kai logs in to Splunk Observability Cloud on their laptop to investigate.

Learn more
####################

* For details about setting up detectors and alerts, see :ref:`get-started-detectoralert`.

* For details about integrating alerts with notification services, like Splunk On-Call, PagerDuty, and Jira, see :ref:`admin-notifs-index`.

* For details about creating a team to coordinate work in Splunk Observability Cloud, see :ref:`admin-manage-teams`.


.. _assess-user-impact-xpuc:

2. Assess user impact
===========================

The first thing Kai wants to know about the alert they just received is: What's the user impact?

Kai opens Splunk Real User Monitoring (RUM) to look for clues about the issue based on the site's browser-based performance. They notice two issues that may be related to the drop in purchases and the low checkout completion rate:

* A spike in the number of frontend errors

  .. image:: /_images/get-started/fe-errors.png
    :width: 35%
    :alt: This screenshot shows the FE (frontend) Errors module in Splunk Real User Monitoring. The module shows the error rate for the last 15 minutes. The error rate is 74 errors per second.

* High backend endpoint latency

  .. image:: /_images/get-started/endpoint-latency.png
    :width: 75%
    :alt: This screenshot shows the Endpoint Latency module in Splunk Real User Monitoring. The module shows a latency of 8 seconds for the /cart/checkout endpoint.

Kai isn’t sure if the two issues are related or whether they are the cause of the problems on the site. They decide to dig into the high latency of the :code:`/cart/checkout` endpoint because the page load time and largest contentful paint for :code:`cart/checkout` are also high.

Kai clicks the :strong:`/cart/checkout` endpoint link and sees multiple errors in the Tag Spotlight view in Splunk RUM. The errors don’t seem to be related to any one tag in particular, so they click the :strong:`Example Sessions` tab to look at example sessions.

Kai sees one session that seems to be taking longer than the others. They click it to see the full trace, from the front end through to the back end, where they can see that it is taking longer to complete than normal. Based on this example data, Kai understands that the latency isn’t a front end problem and that they need to follow the trace through to the back end.

.. image:: /_images/get-started/session-details.png
  :width: 100%
  :alt: This screenshot shows the Session Details page in Splunk RUM displaying the session timeline from the front end through to the back end, where the /cart/checkout endpoint is taking longer than expected to complete.

Kai clicks the :strong:`APM` link to get a performance summary, as well as access to the session’s trace and workflow details.

.. image:: /_images/get-started/performance-summary.png
  :width: 80%
  :alt: This screenshot shows the Performance Summary menu in Splunk RUM displaying a link to the frontend:/cart/checkout workflow, as well as performance and trace details.

Kai decides to take a look at the end-to-end transaction workflow.

Learn more
####################

For details about using Splunk RUM to identify and troubleshoot frontend errors, see :ref:`rum-identify-span-problems`.


.. _investigate-root-cause-xpuc:

3. Investigate the root cause of a business workflow error
===============================================================

In Splunk RUM, Kai clicks the :strong:`frontend:/cart/checkout` business workflow link to display its service map in Splunk Application Performance Monitoring (APM). A business workflow is a group of logically related traces, such as a group of traces that reflect an end-to-end transaction in your system.

The service map shows Kai the dependency interactions among  the full set of services backing the :code:`/cart/checkout` action that they're troubleshooting, including the propagation of errors from one service to another.

.. image:: /_images/get-started/service-map.png
  :width: 100%
  :alt: This screenshot shows a service map in Splunk APM displaying the paymentservice as the source of root errors.

In particular, Kai sees that the :strong:`paymentservice` is having issues. Splunk APM has identified the issues as root cause errors, meaning that the :strong:`paymentservice` has the highest number of downstream errors that are contributing to a degraded experience for the workflow.

Kai selects the :strong:`paymentservice`. In addition to displaying more details about the service’s errors and latency, Splunk Observability Cloud surfaces Related Content tiles that provide access to relevant data in other areas of the application.

For example, Kai can look at the health of the Kubernetes cluster where the :strong:`paymentservice` is running or examine logs being issued by the :strong:`paymentservice`.

.. image:: /_images/get-started/related-content.png
  :width: 100%
  :alt: This screenshot shows a service map in Splunk APM providing access to two Related Content tiles: K8s cluster(s) for paymentservice and Logs for paymentservice.

Kai decides to take a look at the Kubernetes cluster to see if the errors are based on an infrastructure issue.

Learn more
####################

* For details about business workflows, see :ref:`apm-workflows`.

* For details about using Related Content, see :ref:`get-started-relatedcontent`.

* For more Splunk APM-specific use case-based documentation, see :ref:`apm-use-cases-intro`.


.. _check-infra-health-xpuc:

4. Check on infrastructure health
===============================================================

Kai clicks the :strong:`K8s cluster(s) for paymentservice` Related Content tile in Splunk APM to display the Kubernetes navigator in Splunk Infrastructure Monitoring, where their view is automatically narrowed down to the :strong:`paymentservice` to preserve the context they were just looking at in Splunk APM.

They click the :strong:`paymentservice` pod in the cluster map to dive deeper into the data.

.. image:: /_images/get-started/k8s-pod.png
  :width: 80%
  :alt: This screenshot shows a Kubernetes pod menu in Splunk Infrastructure Monitoring displaying details about the pod, including its name and status.

Kai sees that the pod looks stable with no errors or events.

.. image:: /_images/get-started/k8s-pod-detail.png
  :width: 100%
  :alt: This screenshot shows the Kubernetes Pod Detail tab in Splunk Infrastructure Monitoring displaying metrics that indicate the pod is stable.

Now that Kai can rule out the Kubernetes infrastructure as the source of the issue, they decide to return to their investigation in Splunk APM. Kai clicks the :strong:`paymentservice in map` Related Content tile in their current view of Splunk Infrastructure Monitoring.

Learn more
####################

For details about using the Kubernetes navigator and other navigators, see :ref:`infrastructure-navigators`.


.. _look-for-patterns-xpuc:

5. Look for patterns in application errors
===============================================================

Now back in Splunk APM, Kai clicks into Tag Spotlight to look for correlations in tag values for the errors they're seeing.

For example, when Kai looks at the :strong:`tenant.level` module, they see that errors are occurring for all levels, so the root cause is likely not tenant-specific.

.. image:: /_images/get-started/tenant-level.png
  :width: 60%
  :alt: This screenshot shows the tenant.level module in Splunk APM displaying errors evenly spread across gold, silver, and bronze tenant levels.

However, when Kai looks at the :strong:version module, they see an interesting pattern: errors are happening on version :strong:`v350.10` only and not on the earlier :strong:`v350.9` version.

.. image:: /_images/get-started/version.png
  :width: 60%
  :alt: This screenshot shows the version module in Splunk APM displaying errors for v350.10 only and no errors for v350.9.

This seems like a strong lead, so Kai decides to dig into the log details. They click the :strong:`Logs for paymentservice` Related Content tile.

Learn more
####################

For details about using Tag Spotlight, see :ref:`apm-tag-spotlight`.


.. _review-logs-xpuc:

6. Examine error logs for meaningful messages and patterns
===============================================================

Now in Splunk Log Observer, Kai’s view is automatically narrowed to display log data coming in for the :strong:`paymentservice` only.

Kai sees some error logs, so they click one to see more details in a structured view.

.. image:: /_images/get-started/error-log.png
  :width: 100%
  :alt: This screenshot shows the details of an error log in Splunk Log Observer, including the error severity and an error message.

As Kai looks at the log details, they see this error message: :strong:`Failed payment processing through ButtercupPayments: Invalid API Token (test-20e26e90-356b-432e-a2c6-956fc03f5609)`.

In the error message, Kai sees what they think is a clear indication of the error. The API token starts with :strong:`test`. It seems that a team pushed v350.10 live with a test token that doesn't work in production.

Just to double-check their hypothesis, Kai clicks the error message and selects :strong:`Add to filter` to show only the logs that contain this error message.

Next, Kai changes the :strong:`Group by method` from :strong:`severity` to :strong:`version`.

Now, Kai can see that all of the logs that contain this test API token error are on version :strong:`v350.10` and none are on version v350.9.

.. image:: /_images/get-started/group-by-version.png
  :width: 100%
  :alt: This screenshot shows the Log Observer page with events filtered down by the error message and grouped by a version of v350.10. All of the logs that display are error logs.

Just to be sure, Kai clicks the "eye" icon for the message filter value to temporarily exclude the filter. Now there are logs that show up for version v350.9 too, but they don’t include the error message.

This exploration convinces Kai that the test API token in v350.10 is the most likely source of the issue. Kai notifies Deepu, the :strong:`paymentservice` owner about their findings.

Learn more
####################

For details about using Splunk Log Observer as described in this use case, see :ref:`logs-aggregations`.


.. _monitor-a-fix-xpuc:

7. Monitor a fix
==============================================================================================================================

Based on Kai's findings, Deepu, the :strong:`paymentservice` owner, looks at the error logs in Splunk Log Observer. They agree with Kai's assessment that the test API token is the likely cause of the problem.

Deepu decides to implement a temporary fix by reverting back to version v350.9 to try to bring the Buttercup Games site back into a known good state, while the team works on a fix to v350.10.

As one way to see if reverting to version v350.9 fixed the issue, Deepu opens the time picker in the upper left corner of Splunk Log Observer and selects :strong:`Live Tail`. Live Tail provides Deepu with a real-time streaming view of a sample of incoming logs.

.. image:: /_images/get-started/live-tail-verify.gif
  :width: 100%
  :alt: This animated GIF shows Deepu opening the time picker in the upper left corner of Splunk Log Observer and selecting Live Tail. Once Deepu selects Live Tail, the error logs with the failed payment messages are cleared and no new logs with the with error message are received.

Deepu watches the Live Tail view and sure enough, the failed payment messages have stopped appearing in :strong:`paymentservice` logs. Reassured that the Buttercup Games site is back in a stable state, Deepu moves on to helping their team fix v350.10.

Learn more
####################

For details about using Splunk Log Observer Live Tail view, see :ref:`logs-live-tail`.


.. _take-preventative-action-xpuc:

8. Take preventative action and create metrics from logs to power dashboards and alerts
==============================================================================================================================

Now that Kai knows that this particular issue can cause a problem on the Buttercup Games site, they decide to do some preventative work for their SRE team. Kai takes the query they created in Splunk Log Observer and saves it as a metric.

.. image:: /_images/get-started/save-as-metric.png
  :width: 50%
  :alt: This screenshot shows the Save as Metric option in the More menu in Log Observer.

Doing this defines log metricization rules that create a log-derived metric that shows aggregate counts. Kai's team can embed this log-derived metric in charts, dashboards, and alerts that can help them identify this issue faster should it come up again in the future.

Learn more
####################

* For details about creating metrics from logs and displaying them in a chart, see :ref:`logs-metricization`.

* For details about creating detectors to issue alerts based on charts or metrics, see :ref:`create-detectors`.
