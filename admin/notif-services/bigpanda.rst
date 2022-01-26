.. _bigpanda:

************************************************************************
Send alert notifications to BigPanda using Splunk Observability Cloud
************************************************************************

You can configure Splunk Observability Cloud to automatically send alert notifications to BigPanda when a detector alert condition is met and when the alert clears.

To send Observability Cloud alert notifications to BigPanda, complete the following configuration tasks:

* :ref:`bigpanda1`

  You must be a BigPanda administrator to perform this task.

* :ref:`bigpanda2`

  You must be an Observability Cloud administrator to perform this task.

* :ref:`bigpanda3`


.. _bigpanda1:

Step 1: Create an Observability Cloud integration in BigPanda
=================================================================================

You must be a BigPanda administrator to perform this task.

To create an Observability Cloud integration in BigPanda:

#. Log in to BigPanda.

#. Access the Integrations page and click :strong:`New Integration`.

#. Hover over the :strong:`ALERTS REST API` tile and click :strong:`Integrate`.

#. Enter a descriptive name for the integration and click :strong:`Generate App Key`.

#. The app key displays. Copy the app key for use in :ref:`bigpanda2`.

#. Click :strong:`ALERTS REST API`. Copy the bearer token for use in :ref:`bigpanda2`.


.. _bigpanda2:

Step 2: Create a BigPanda integration in Observability Cloud
=================================================================================

You must be an Observability Cloud administrator to perform this task.

To create a BigPanda integration in Observability Cloud:

#. In the Observability Cloud navigation menu, select :strong:`Data Setup`.

#. In the :strong:`CATEGORIES` menu, select :strong:`Notification Services`.

#. Click the :strong:`BigPanda` tile.

#. Click :strong:`New Integration` to display the configuration options.

#. By default, the name of the integration is :strong:`BigPanda`. Give your integration a unique and descriptive name. For information about the downstream use of this name, see :new-page-ref:`About naming your integrations <naming-note>`.

#. In the :strong:`App Key` field, enter the app key you copied from BigPanda in :ref:`bigpanda1`.

#. In the :strong:`Token` field, enter the token you copied from BigPanda in :ref:`bigpanda1`.

#. Click :strong:`Save`.

#. If Observability Cloud is able to validate the BigPanda app key and token, a :strong:`Validated!` success message displays. If an error displays instead, make sure that the app key and token values you entered match the values displayed in BigPanda in :ref:`bigpanda1`.


.. _bigpanda3:

Step 3: Add a BigPanda integration as a detector alert recipient in Observability Cloud
=================================================================================================

..
  once the detector docs are migrated - this step may be covered in those docs and can be removed from these docs. below link to :ref:`detectors` and :ref:`receiving-notifications` instead once docs are migrated

To add a BigPanda integration as a detector alert recipient in Observability Cloud:

#. Create or edit a detector that you want to configure to send alert notifications using your BigPanda integration.

    For more information about working with detectors, see :ref:`create-detectors` and :ref:`subscribe`.

#. In the :strong:`Alert recipients` step, click :strong:`Add Recipient`.

#. Select :strong:`BigPanda` and then select the name of the BigPanda integration you want to use to send alert notifications. This is the integration name you created in :ref:`bigpanda2`.

#. Activate and save the detector.

Observability Cloud will send an alert notification to BigPanda when an alert is triggered by the detector and when the alert clears.

In addition to sending a subject, description, and other information to BigPanda, the integration maps certain pieces of Observability Cloud detector information to corresponding BigPanda properties as described in the following table.

.. list-table::
   :header-rows: 1

   * - :strong:`Splunk Observability Cloud information`
     - :strong:`BigPanda property and value`

   * - Alert severity is Critical
     - status: Critical

   * - Alert severity is Major, Minor, Warning, or Informational
     - status: Warning

   * - Alert is cleared or manually resolved, or detector is stopped
     - status: OK

   * - Detector rule name
     - check: Detector rule name

   * - Metric has a dimension named ``cluster``
     - cluster: Value of the ``cluster`` dimension

   * - Metric has a dimension named ``host``
     - host: Value of the ``host`` dimension

   * - Metric has any other dimension(s)
     - Custom properties, each named ``sfx_<dimension-name>``: Value of the dimension.

If there are any name collisions between Observability Cloud dimensions and BigPanda ``status`` or ``check`` properties, Observability Cloud creates a new custom property in BigPanda. For example, if there is an Observability Cloud dimension named ``status``, Observability Cloud creates a custom property named ``sfx_status`` and stores the value of the ``status`` dimension there.
