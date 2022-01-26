.. _dashboard-share-clone-mirror:


*****************************************************************
Share, clone, and mirror dashboards in Splunk Observability Cloud
*****************************************************************

Splunk Observability Cloud dashboards are groupings of charts and visualizations of metrics that make it quick and easy to find the metrics you monitor. This topic explains how to share, clone and mirror dashboards to suit your specific needs.

See the following sections for more information on how to:

- :ref:`share-dashboard`

- :ref:`clone-dashboard`

- :ref:`mirror-dashboard`


.. _share-dashboard:

Share a dashboard
=================================================================

The following section describes how to share a dashboard from Splunk Observability Cloud. 



.. _share-menu:

Use the share menu option
------------------------------------------------------------

This method allows you to share a copy of the current state of a dashboard. Copies include unsaved changes at the time you share, and auto-expire unless the recipient saves them. Sharing a copy is useful for when you make a change that you want to show to team members, but don’t want to modify the original dashboard. In the share menu there are two ways to share the dashboard:

Share directly
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

- To share a dashboard copy, select :guilabel:`Share` from the :guilabel:`Dashboard actions` menu. A pop-out window will open with sharing options.

- To share directly, Click :guilabel:`Add Recipients` and add email addresses or select any available notification integrations as your sharing method. 

- After adding recipients, click :guilabel:`Share`. Recipients will receive a link to the dashboard copy. When they open it, they can edit and save their copy without affecting the original.

.. caution::
    Administrators can add email addresses of people who aren’t members of your organization. Recipients who aren't members will be asked to create a user account before they can view the shared content. Be sure the email addresses you enter for non-members are correct, especially if the item you are sharing contains any sensitive or proprietary information.


Copy link
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Alternatively, you might want to send out an email or post a link to the dashboard copy on an internal communication tool as opposed to sharing directly to each individual member. 

- To do this, click :guilabel:`Copy` next to the link provided in the pop-out window and paste this link into your communication. 

- If you share the dashboard link with a group, be aware that only members of your organization with an account are able to view the dashboard.

Use the browser URL
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
You can share a dashboard browser URL. However, using the URL shares the original dashboard rather than a copy. Share browser URLs for a dashboard with caution; any changes made to the dashboard are visible to all viewing the dashboard, and can overwrite changes others have made to the dashboard. 
 


.. _clone-dashboard:

Clone a dashboard
=================================================================

There are various reasons you might want to clone a dashboard. Dashboard cloning allows you to modify an existing dashboard without making changes to the original. You can also clone a dashboard that is read-only or that you don’t have write permissions for in order to modify it. 

- To clone a dashboard, select :guilabel:`Save as` from the dashboard actions menu.

- You’ll be asked to specify a dashboard name and the dashboard group in which to save the new dashboard.

- Rename the dashboard to avoid multiple dashboards with the same name.

- You can save the dashboard to an existing custom or user dashboard group, or you can create a new dashboard group. If you create a new group, the group is added as a Custom Dashboard group.



.. _mirror-dashboard:

Mirror a dashboard
=================================================================

|hr|
:strong:`Available in Enterprise Edition`
|hr|

Dashboard mirroring allows the same dashboard to be added to multiple dashboard groups or multiple times to one dashboard group. A dashboard can be edited from any of its mirrors and the changes made are reflected on all mirrors. However the dashboard name, filters, and dashboard variables can all be customized at the mirror level, without affecting other mirrors. These local customizations allow users to see the same metrics in the same charts, but the mirror can be filtered so that each user is presented with the metrics relevant to them.



Why mirror dashboards?
-------------------------------------------------------------

Common use cases for dashboard mirrors:

- You create standard dashboards for use by teams throughout your organization. You want all teams to see any changes to the charts in the dashboard, and you want members of each team to be able to set dashboard variable and filter customizations relevant to their requirements. Each team has a dashboard group linked to their team, so you add a mirror of the dashboard to each of these dashboard groups.

- You have created a dashboard in your user dashboard group, which another user in your organization has found useful. They want to follow any changes you make to the dashboard so they add a mirror of your dashboard to their user dashboard group.


Dashboard mirror example
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The following example provides a common use case of dashboard mirroring:

In this example, there is a non-mirrored dashboard named CPU Utilization in dashboard group Project‑1. The dashboard is filtered on ``AWS availability zone us‑east‑1a``. The Project-2 dashboard group needs the same dashboard but filtered on ``AWS availability zone us‑east‑1b``. 

Since filters are customizable within each mirrored dashboard this can be accomplished by adding a mirror of this dashboard in the Project‑2 dashboard group, and filtering on ``AWS availability zone us‑east‑1b``.

Now there are two mirrors of the same dashboard, seen in two different places with different filters. If dashboard group Project-1 edited the mirror in group Project‑1, by adding a chart “Mean CPU Utilization”, the filter in this dashboard is still ``AWS availability zone us‑east‑1a``. When they open the mirror in group Project‑2, they will see the added chart, but with the groups ``AWS availability zone us‑east‑1b`` filter applied.



.. _create-mirror:

Create a mirror
------------------------------------------------------------

Any Splunk Observability Cloud user can create a mirror of any custom or user dashboard. Users simply need write permission for the dashboard group where they want to place the mirror.


.. note:: If you are working with a dashboard you control, be sure to set appropriate write permissions on the dashboard, to prevent inadvertent edits by other users who might be viewing a mirror of the dashboard. 


To create a mirror, select :guilabel:`Add a mirror` from the dashboard actions menu.


When you create a mirror, you have a number of ways to customize how the mirror will be displayed in the target dashboard group. Dashboard mirrors can also be added to the same group as the current dashboard. This is useful if you want to have quick access to the same set of charts but with different filters or dashboard variable settings.


Select a dashboard group
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Select or search for a group where you want the mirror to be placed. Dashboard groups for which you don’t have write permissions will not be available as targets for the mirror.

Customize the dashboard name and description
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Specify a name for the mirror in the target group. The default name suggested when creating a new dashboard mirror is the name of the original dashboard, which may be different from the displayed name of the dashboard you are currently mirroring if that dashboard itself is a mirror.

Specify a new description for the mirror in the target group. As with the name, the default will come from the dashboard. A dashboard or mirror’s description is visible when you select :guilabel:`Dashboard Info` from the Actions menu.

Customize dashboard filters
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


Specify any filters you want applied to the mirror. By default, the mirror will have the same filter(s) as the dashboard you are mirroring. Setting filters here means the target mirror will have different default filters applied. Filters can also be set later by any user with write permissions for that group. 

Once the dashboard mirror is created, there are two ways to customize the dashboard filters; from the Overrides bar or the Dashboard Info tab. As with any dashboard, changes you make to filters on the Overrides bar are applied immediately, which lets you modify your view and explore your data in real time.

If you apply filters and want them to be displayed on the mirror by default, click :guilabel:`Save` to save the mirror with the filters applied. Once saved, the new filters will be stored in the customization section in the dashboard info tab.

On the Dashboard Info tab, anyone with :guilabel:`dashboard write permissions` can apply filters to the dashboard (in the top portion of the tab). These filters will be applied to all mirrors that don’t have filter customizations applied.

If you specify that you want to apply a filter override, you can either specify a filter to use in place of the dashboard default filter, or you can leave the filter value blank. Leaving the filter value blank means the mirror will not have any filter applied by default.


Customize dashboard variables
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You can specify various dashboard variable settings that will apply to this mirror in this dashboard group. Select :guilabel:`Dashboard Variables` from the mirror’s Actions menu.

When these settings are saved, the dashboard variable and the suggested values now reflect the customizations you specified.

Implementation notes about fitler and variable customization on mirrored dashboards:

- You can make changes directly on the :guilabel:`Overrides` bar; if you save the mirror, these settings will be saved as default values in the :guilabel:`Variable Details` section of the :guilabel:`Dashboard Variables` tab.

- When you save customization options that you set in the :guilabel:`Dashboard Variables` tab, these changes are automatically saved as default settings for this mirror.

- On the :guilabel:`Dashboard Variables` tab, anyone with dashboard write permissions can add, delete, and edit dashboard variables and their settings. These variables will be applied to all mirrors that don’t have variable customizations applied.

- If you want to override the dashboards default variables with no variables, you can leave the value blank. Doing so means you are overriding the dashboard variable default value with a setting of “no default value.”


.. _dashboard-mirror-permissions:

Dashboard mirrors and permissions
------------------------------------------------------------

Dashboard mirrors can only inherit permissions from the dashboard group where they are saved to. Therefore, when you create a new dashboard mirror, teams and users with read and/or write permissions on the dashboard group will have the same permissions on all mirrors.

The following table shows the prerequisites you need to do dashboard mirror actions.
      
.. list-table::
   :header-rows: 1

   * - :strong:`Action`
     - :strong:`Dashboard Permissions`
     - :strong:`Group Permissions`
   
   * - Add a dashboard mirror to a dashboard group
     - | - For an original dashboard configured with :strong:`Inherit from Dashboard Group`, you only need read permissions to create a mirror
       | - For an original dashboard with customized permissions, you must have write permissions to convert the original dashboard permission to :strong:`Inherit from Dashboard Group` before you can create a mirror
     - Read permissions for the target group

   * - View a dashboard mirror :sup:`*`
     - No permissions needed for the original dashboard
     - Read permissions for the dashboard group where the mirror is saved to
 
   * - Make changes to charts within a dashboard mirror
     - Write permissions for the original dashboard
     - No group permission needed
  
   * - Add a new chart to a dashboard mirror
     - Write permissions for the original dashboard
     - No group permission needed
      
   * - Edit settings on a dashboard mirror :strong:`Overrides` bar
     - No permissions needed for the original dashboard
     - Write permissions for the target dashboard group, as the mirror inherits permissions from the dashboard group it is saved to

   * - Edit the :strong:`Dashboard Info` and :strong:`Dashboard Variables` pages of a dashboard mirror
     - Write permissions for the original dashboard
     - Write permissions for the target dashboard group, as the mirror inherits permissions from the dashboard group it is saved to
    
   * - Delete a dashboard mirror from a group :sup:`**, ***`
     - No permissions needed for the original dashboard
     - Write permissions for the target dashboard group, as the mirror inherits permissions from the dashboard group it is saved to

:sup:`*` When you view the :strong:`Mirrors of this dashboard` list on the :strong:`Dashboard Info` page of a dashboard, not all mirrors might show up. The list only shows mirrors for which you have read permissions.

:sup:`**` When a dashboard has one or more mirrors, the :guilabel:`Delete dashboard` option is not available; it is replaced with the :guilabel:`Remove mirror` option. If all mirrors have been removed from the groups in which they were placed, the :guilabel:`Delete dashboard` option will be available on the last mirror.

:sup:`***` If you want to delete the last dashboard mirror in the same group as the original dashboard, and the original dashboard inherits permissions from this group, you have to change the permission settings of the original dashboard so that it inherits permissions from another group. 


