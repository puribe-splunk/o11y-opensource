.. _admin-manage-teams:

*********************************************************
Create and manage teams in Splunk Observability Cloud
*********************************************************

.. meta::
   :description: Learn how to manage teams in Splunk Observability Cloud.

.. toctree::
   :hidden:

   Manage teams <manage-membership>
   Manage team landing pages <configure-page>
   Manage team notifications <team-notifications>
   Link detectors and dashboards to teams <associate-team>

As a Splunk Observability Cloud administrator, you can:

* Organize users with teams

   As a Splunk Observability Cloud administrator, you can create and manage teams to organize users by functional area. For example, you might create the following teams:

   * Security: Teams who monitor hardware and software security
   * Hardware operations: Teams who add, replace, or fix computer hardware
   * DevOps: Teams responsible for maintaining system uptime
   * Infrastructure IT: Teams who manage user access to systems

   In these areas, each team might monitor specific metrics related to their functional area, or they might monitor a general set of metrics for the specific systems they manage, or both.

   For example, your security team might focus on metrics that indicate login failures, because these failures indicate attempts to break into systems. You might also have several DevOps teams, each of which monitors cpu temperature and network response time metrics for the systems they manage.

   To learn more about managing and creating teams, see :new-page-ref:`admin-manage-team-membership`.

* Create relationships between teams and Observability Cloud features

   As a Splunk Observability Cloud administrator, you can create relationships between teams and the metrics they use, create links between dashboard groups and teams, or detectors and teams, or both.

   To learn more about linking teams and content, see :new-page-ref:`admin-associate-team`.

   Use these content links to create a team landing page that displays important content in a single place. To learn more about team landing pages, see :new-page-ref:`admin-configure-page`.

   Another way you can create relationships between teams and metrics is to create team notifications. To learn more, see :new-page-ref:`admin-team-notifications`.


As an Observability Cloud user, you can use team features to explore Observability Cloud. For example, you can join existing teams, but you can't create or delete teams or add others to teams. You can also view a team's landing page, link dashboards and detectors to a team, and send notifications to a team.
