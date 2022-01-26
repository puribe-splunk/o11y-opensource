.. _use-cases-mobile:

***********************************************
Use case: Start monitoring your mobile data 
***********************************************

Follow these steps to learn how to start monitoring your data in Splunk RUM for Mobile. 

==========
Overview
==========

The overview page shows a summary of aggregate metrics about your mobile application. From the overview page, you can click on any link to open the Tag Spotlight. 

.. image:: /_images/rum/mobile-overview-docs.png
    :width: 99%
    :alt: <This image shows the overview page for Splunk RUM for Mobile.>

====================
Tag Spotlight
====================

The Tag Spotlight view gives you a breakdown of sessions where you can filter aggregates by endpoint, pages, environments, operation, and more. The :strong:`Example Sessions` tab in the Tag Spotlight view shows example sessions that contain a certain behavior you want to analyze based on the filters you selected. The following image shows request errors filtered by the provider of the URL. 


.. image:: /_images/rum/tag-spotlight-rum-mobile.png
    :width: 99%
    :alt: <This image shows the tag spotlight view for Splunk RUM for Mobile.>


====================
Session Details
====================

There are two ways to search for a full session. You can either do a full fidelity search, or open a session through the :strong:`Tag Spotlight`. In the Tag Spotlight, open the :strong:`Example Sessions` tab. Click on any session to open a full :strong:`Session Details` view. You can also search for a specific session by applying filters. The following image shows ...  


.. image:: /_images/rum/mobile-rum-session-details.png
    :width: 99%
    :alt: <This image shows the Session Details page for Splunk RUM for Mobile.>

====================
Example workflow 
====================

Suppose you want to investigate a spike in front-end errors. The overview page shows a summary of  aggregate metrics about your mobile application. In :strong:`HTTP errors` you click on :strong:`See all` to open the Tag Spotlight view. The :strong:`Tag Spotlight` view gives you a breakdown of sessions where you can filter aggregates by endpoint, pages, environments, operation, and more.