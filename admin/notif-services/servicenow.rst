.. _servicenow:


**************************************************************************
Send alert notifications to ServiceNow using Splunk Observability Cloud
**************************************************************************

You can configure Splunk Observability Cloud to automatically send alert notifications to ServiceNow when a detector alert condition is met and when the alert clears.

To send Observability Cloud alert notifications to ServiceNow, complete the following configuration tasks:

* :ref:`servicenow1`

  You must be a ServiceNow administrator to perform this task.

* :ref:`servicenow2`

   You must be an Observability Cloud administrator to perform this task.

* :ref:`servicenow3`


.. _servicenow1:

Step 1: Create a ServiceNow user for your Observability Cloud integration
=================================================================================

In this step, you'll create a ServiceNow user that you'll use to receive alert notifications from Observability Cloud. You must be a ServiceNow administrator to perform this task.

If you have an existing ServiceNow user that you want to use to receive alert notifications, the user has the :strong:`web_service_admin` and :strong:`itil` roles assigned, and you know the user ID and password, you can skip to :ref:`servicenow2`.

To set up a ServiceNow user for your Observability Cloud integration:

#. Log in to ServiceNow.

#. In the left navigation panel, scroll to :strong:`User Administration` and click :strong:`Users`.

#. Click :strong:`New`.

#. Enter :strong:`User ID`, :strong:`First name`, and :strong:`Last name` values that clearly communicate that the user is associated with Observability Cloud notifications. Make note of the :strong:`User ID` value for use in subsequent steps.

#. Enter a :strong:`Password` value. Make note of this value for use in :ref:`servicenow2`.

#. Select the :strong:`Active` check box.

#. Click :strong:`Submit`.

#. Find your new user by either searching for the user ID or doing a reverse chronological sort on the :strong:`Created` column. Click the user ID to open the user information window. Scroll down and click the :strong:`Roles` tab. Click :strong:`Edit`.

#. In the :strong:`Collection` search field, enter :strong:`web_service_admin`. Select the :strong:`web_service_admin` role and click :strong:`>` to move it the :strong:`Roles List` panel.

#. Similarly, in the :strong:`Collection` search field, search for :strong:`itil`. Select the :strong:`itil` role and click :strong:`>` to move it the :strong:`Roles List` panel.

#. Click :strong:`Save`. :strong:`web_service_admin` and :strong:`itil` display on the :strong:`Roles` tab for the user, possibly along with other additional roles.


.. _servicenow2:

Step 2: Create a ServiceNow integration in Observability Cloud
=================================================================================

You must be an Observability Cloud administrator to perform this task.

To create a ServiceNow integration in Observability Cloud:

#. In the Observability Cloud navigation menu, select :strong:`Data Setup`.

#. In the :strong:`CATEGORIES` menu, select :strong:`Notification Services`.

#. Click the :strong:`ServiceNow` tile.

#. Click :strong:`New Integration` to display the configuration options.

#. By default, the name of the integration is :strong:`ServiceNow`. Give your integration a unique and descriptive name. For information about the downstream use of this name, see :new-page-ref:`About naming your integrations <naming-note>`.

#. In the :strong:`Username` field, enter the user ID from ServiceNow in :ref:`servicenow1`.

#. In the :strong:`Password` field, enter the password from ServiceNow in :ref:`servicenow1`.

#. In the :strong:`Instance Name` field, enter your ServiceName instance name. For example, the instance name must use the format ``example.service-now.com``. Do not include a leading ``https://`` or a trailing ``/``. Additionally, you cannot use local ServiceNow instances.

   To troubleshoot potential blind server-side request forgeries (SSRF), Observability Cloud has included ``\*.service-now.com`` on an allow list. As a result, if you enter a domain name that is rejected by Observability Cloud, contact observability-support@splunk.com to update the allow list of domain names.

#. Click :strong:`Incident` or :strong:`Problem` to indicate the issue type you want the integration to create in ServiceNow.

   If necessary, you can create a second integration using the other issue type. This allows you to create an incident issue for one detector rule and a problem issue for another detector rule.

#. Click :strong:`Save`.

#. If Observability Cloud is able to validate the ServiceNow username, password, and instance name combination, a :strong:`Validated!` success message displays. If an error displays instead, make sure that the values you entered match the values in ServiceNow.


.. _servicenow3:

Step 3: Add a ServiceNow integration as a detector alert recipient in Observability Cloud
=================================================================================================

..
  once the detector docs are migrated - this step may be covered in those docs and can be removed from these docs. below link to :ref:`detectors` and :ref:`receiving-notifications` instead once docs are migrated

To add a ServiceNow integration as a detector alert recipient in Observability Cloud:

#. Create or edit a detector that you want to configure to send alert notifications using your ServiceNow integration.

    For more information about working with detectors, see :ref:`create-detectors` and :ref:`subscribe`.

#. In the :strong:`Alert recipients` step, click :strong:`Add Recipient`.

#. Select :strong:`ServiceNow` and then select the name of the ServiceNow integration you want to use to send alert notifications. This is the integration name you created in :ref:`servicenow2`.

#. Activate and save the detector.

Observability Cloud will send an alert notification to create an incident in ServiceNow when an alert is triggered by the detector. When the alert clears, it will send a notification that sets the incident state to :strong:`Resolved`.

This ServiceNow integration sets the :strong:`Impact` and :strong:`Urgency` fields on the ServiceNow incident based on the Observability Cloud alert severity (see :ref:`severity`) as follows:

.. list-table::
   :header-rows: 1

   * - :strong:`Observability Cloud severity`
     - :strong:`ServiceNow Impact and Urgency fields`

   * - Critical
     - 1

   * - Major or Minor
     - 2

   * - Warning or Info
     - 3
