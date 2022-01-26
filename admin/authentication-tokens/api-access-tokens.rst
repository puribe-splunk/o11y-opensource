.. _admin-api-access-tokens:

****************************************************************************
Create and manage user API access tokens using Splunk Observability Cloud
****************************************************************************

.. meta::
   :description: Learn how to how to create and manage user API access tokens.

This topic describes how to create a user API access token that you can use to authenticate API requests.

Some API requests :ref:`require a user API access token created by an administrator <apis-require-admin-token>`.

To authenticate API requests that send data to Splunk Observability Cloud, you must use an :ref:`organization access token<admin-org-tokens>`, not a user API access token.


Create a user API access token (session token)
=========================================================

To create a user API access token:

#. Open the navigation :strong:`Menu`.
#. Select your profile name and click :strong:`Account Settings`.
#. Click :strong:`Generate User API Access Token`.
#. Click :strong:`Show User API Access Token`.
#. Click :strong:`Copy` to copy the token to your clipboard.
#. If you are going to keep your Account Settings screen open, click :strong:`Hide` to prevent others from seeing your token.

After you generate and copy the user API access token, you can't see it again in your profile. Instead, generate a new token. You can generate as many as you want.

You don't need to delete your user API access tokens. Instead, you can let them expire.


User API access token expiration
---------------------------------------

A user API access token you create on the Account Settings page expires when you log out of Splunk Observability Cloud, or after 30 days, whichever comes first.

To create a user API access token that doesn't expire when you log out of Splunk Observability Cloud but still expires after 30 days, use the :code:`v2/session` endpoint. For more information, see :new-page:`Sessions Tokens <https://dev.splunk.com/observability/reference/api/sessiontokens/latest>`.


.. _apis-require-admin-token:

API requests that require a user API access token created by an administrator
================================================================================

These API requests require a user API access token created by an administrator.

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - :strong:`API`
     - :strong:`Task`

   * - Integration
     - Create, update, delete, or validate an integration

   * - Org token
     - Create, update, or delete an org (access) token, or rotate an org token secret

   * - Dashboards and dashboard groups
     - Change or remove write permissions for a user other than yourself

   * - Detectors
     - Change or remove write permissions for a user other than yourself

   * - Organizations
     - The following API requests require a User API access token associated with an administrator:

       * Retrieve information for your organization
       * Retrieve information for one or more organization users
       * Create, update, or delete a custom metric category
       * Invite a user to your organization
       * Invite a group of users to your organization
       * Grant administrative access to a user
       * Delete a user from your organization

   * - Teams
     - Create, update, or delete a team, or remove a team member other than yourself.

You can manage permissions on items for which you already have permissions, even if you're not an administrator.

If you're an administrator, you can see how a user created or updated an object using a particular user API access token by selecting :strong:`Info` from the object's :strong:`Actions` menu.

For example, to see information for a dashboard, select :menuselection:`Dashboard > Info` from the dashboard's :strong:`Actions` menu.

To track API calls by user, ask your users to obtain and use their own user API access tokens.
