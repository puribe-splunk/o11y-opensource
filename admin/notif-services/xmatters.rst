.. _xmatters:

************************************************************************
Send alert notifications to xMatters using Splunk Observability Cloud
************************************************************************

You can configure Splunk Observability Cloud to automatically send alert notifications to xMatters when a detector alert condition is met and when the alert clears.

To send Observability Cloud alert notifications to xMatters, complete the following configuration tasks:

* :ref:`xmatters1`

  You must be an xMatters administrator to perform this task.

* :ref:`xmatters2`

   You must be an Observability Cloud administrator to perform this task.

* :ref:`xmatters3`


.. _xmatters1:

Step 1: Create an Observability Cloud integration in xMatters
=================================================================================

For information about how to create an Observability Cloud integration in xMatters, you can search "Splunk Infrastructure Monitoring" on xMatters website.


.. _xmatters2:

Step 2: Create an xMatters integration in Observability Cloud
=================================================================================

You must be an Observability Cloud administrator to perform this task.

To create an xMatters integration in Observability Cloud:

#. In the Observability Cloud navigation menu, select :strong:`Data Setup`.

#. In the :strong:`CATEGORIES` menu, select :strong:`Notification Services`.

#. Click the :strong:`xMatters` tile.

#. Click :strong:`New Integration` to display the configuration options.

#. Enter a name for the integration. Give your integration a unique and descriptive name. For information about the downstream use of this name, see :new-page-ref:`About naming your integrations <naming-note>`.

#. In the :strong:`URL` field, enter the URL you copied from xMatters in :ref:`xmatters1`.

#. Click :strong:`Save`.

#. If Observability Cloud is able to validate the xMatters URL, a :strong:`Validated!` success message displays. If an error displays instead, make sure that the URL you entered matches the URL displayed in xMatters in :ref:`xmatters1`.


.. _xmatters3:

Step 3: Add an xMatters integration as a detector alert recipient in Observability Cloud
=================================================================================================

..
  once the detector docs are migrated - this step may be covered in those docs and can be removed from these docs. below link to :ref:`detectors` and :ref:`receiving-notifications` instead once docs are migrated

To add an xMatters integration as a detector alert recipient in Observability Cloud:

#. Create or edit a detector that you want to configure to send alert notifications using your xMatters integration.

    For more information about working with detectors, see :ref:`create-detectors` and :ref:`subscribe`.

#. In the :strong:`Alert recipients` step, click :strong:`Add Recipient`.

#. Select :strong:`xMatters` and then select the name of the xMatters integration you want to use to send alert notifications. This is the integration name you created in :ref:`xmatters2`.

#. Activate and save the detector.

Observability Cloud will send an alert notification to xMatters when an alert is triggered by the detector and when the alert clears.
