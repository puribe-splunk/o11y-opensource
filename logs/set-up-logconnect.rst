.. _logs-set-up-logconnect:

*******************************************************************
Set up Splunk Log Observer Connect
*******************************************************************

Set up Log Observer Connect by integrating Log Observer with Splunk Enterprise. To set up Log Observer Connect, follow these steps:

1. In Observability Cloud, go to :guilabel:`Settings > Log Observer Connect` and click :guilabel:`Add new connection`.

2. Follow the instructions in the integration wizard to do the following in Splunk Enterprise:

   a. Create a new Splunk Enterprise role.

   b. Select the Splunk Enterprise indexes that you want to search in Log Observer Connect. 

   c. Create and configure a new Splunk Enterprise user.

   d. Obtain certificates for securing inter-Splunk communication. See :new-page:`Configure and install certificates in Splunk Enterprise for Splunk Log Observer Connect <https://quickdraw.splunk.com/redirect/?product=Observability&location=splunk.integration.third.party&version=current>` to learn how.

.. note:: Manage concurrent search limits using your current strategy in Splunk Enterprise. All searches initiated by Log Observer Connect users go through the service account you create in Splunk Enterprise. For each active Log Observer Connect user, four backend searches occur when a user performs a search in the Log Observer Connect UI. For example, if there are three concurrent users accessing the Log Observer Connect UI at the same time, the service account for Log Observer Connect initiates 12 searches in Splunk Enterprise.

