.. _admin-manage-team-membership:

***************************************************
Manage teams in Splunk Observability Cloud
***************************************************

.. meta::
   :description: Learn how to how to manage teams and team membership.

.. note:: To learn more about using teams in Splunk Observability Cloud, see :new-page-ref:`admin-manage-teams`

.. _admin-create-team:

Create a team
============================================================================

To create a team, follow these steps:

#. From the main menu, select :menuselection:`Organization Settings > Teams`.
#. Click :guilabel:`Create New Team`.
#. In the :guilabel:`Choose team name` dialog box, type a name for the team.
#. From the :guilabel:`Choose the users who will be a part of this team` list, find the name of
   user you want to add to the team and click :guilabel:`Add`. You can search for users with the
   search text box.
#. Continue to add users to the team.
#. When you're finished adding users, click :guilabel:`Done`.
#. The new team name appears in the list of teams. If you added yourself to the team,
   the Member label appears next to the number of team members.

.. _admin-delete-team:

Delete a team
============================================================================

To delete a team, follow these steps:

#. From the main menu, select :menuselection:`Organization Settings > Teams`.
#. A table of current teams appears in the main panel.
#. Find the name of the team.
#. Click the :guilabel:`Actions` menu icon next the team name, then select :menuselection:`Delete Team`.
#. Observability Cloud displays a dialog box that asks you to confirm the deletion. Click :guilabel:`Delete`.
#. The team no longer appears in the list of teams.


Change team name
============================================================================

To change the team name, follow these steps:

#. From the main menu, select :menuselection:`Organization Settings > Teams`.
#. A table of current teams appears in the main panel.
#. Find the name of the team.
#. Click the :guilabel:`Actions` menu icon next the team name, then select :menuselection:`Edit Team`.
#. In the :guilabel:`Choose team name` dialog box, edit the team name.
#. The team no longer appears in the list of teams.
#. When you're finished editing the name, click :guilabel:`Done`.

Add or remove team members
============================================================================

#. From the main menu, select :menuselection:`Organization Settings > Teams`.
#. A table of current teams appears in the main panel.
#. Find the name of the team.
#. Click the :guilabel:`Actions` menu icon next the team name, then select :menuselection:`Edit Team`.
#. In the :guilabel:`Choose the users who will be a part of this team:` dialog box, add or remove team members by
   following one these steps:

   * To add a team member, click :guilabel:`Add` next to the email address of the member.
   * To remove a team member, click :guilabel:`Remove` next to the email address of the member.
#. To complete your work, click :guilabel:`Done`.

.. _admin-team-controls:

Restrict users from joining any team
============================================================================

|hr|

:strong:`Available in Enterprise Edition`

|hr|

.. note::
   - You must be an administrator in Splunk Observability Cloud to restrict team access.
   - Any user with an administrator role can override team access restriction to add and remove members from any team.

When you create a new team in Splunk Observability Cloud, any user in your organization can see and join the team. To restrict adding and removing permissions to only members on your team, follow these steps:

#. From the navigation menu, select :guilabel:`Organization Settings > Organization Overview`.
#. Select :guilabel:`General Settings` from the navigation menu.
#. Check :guilabel:`Restrict Access`. When you check this box, the setting is applied to all teams in the organization.
#. Click :guilabel:`Apply` to save the new setting.
