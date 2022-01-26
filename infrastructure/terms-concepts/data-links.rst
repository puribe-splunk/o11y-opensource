.. _overview-data-links:

****************************************************
Data links in Splunk Infrastructure Monitoring
****************************************************

.. When you are looking at properties for data in the :ref:`data table<data-table>` for a chart (in the chart builder or on a dashboard), the information you see includes columns for various properties (dimensions or custom properties), such as host, AWS region, GCP function name, etc. Properties are also shown in other locations throughout SignalFx, such as in lists below the Infrastructure Navigator visualization, in the Metrics Catalog, in list charts, and in alert messages that are shown when you open an alert. 
.. 
.. Your organization might have a custom or user dashboard that contains a number of charts related to a particular property. You can create a data link to open a specified dashboard when someone clicks on that property in a chart or in an alert message, filtered to the value for that property. Target dashboards can also be opened from properties you are viewing in the Metrics Catalog.

Data links are dynamic links available for properties that appear in several locations throughout the application, such as in a chart's data table, in list charts, in alert messages that are displayed when you open an alert, and in lists below the Infrastructure Navigator visualization. 

Data links can take you to a Splunk Infrastructure Monitoring dashboard or an external system, such as a Splunk instance or a custom-defined URL, and include the context of the metadata you clicked on. Some examples of how you might use data links include:

- Drilling down from a property displayed in a dashboard to another dashboard that shows related data. For example, clicking on a link for the property ``aws_region`` in one dashboard takes you to a related dashboard, filtered by the region. 
    
- Troubleshooting by linking from a property to external systems that reference that property. For example, clicking on a link for the property ``host`` in an alert message takes you to a search for log entries about that host in Splunk. 
    
- Constructing specialized links for a specific purpose. For example, specifying a link for the property-value pair ``service:analytics`` takes you to a destination specific to the service, such as a dashboard containing charts related to analytics or an internal web page about managing your Analytics service.

To learn how to work with data links, see :ref:`navigate-with-data-links`.

.. _local-global:

Local and global data links
============================================================

Two types of data links can be defined for a property, local and global. 

- :strong:`Local data links` are available on just one dashboard; anyone with permission to edit a dashboard can add data links that appear on that dashboard. For example, while viewing a dashboard, you can create a link from any value of ``InstanceId`` to the built-in dashboard "EC2 Instance." Following the link opens the EC2 Instance dashboard filtered to the value of ``InstanceId`` you click on. This link is available for any appearance of ``InstanceId`` in that dashboard; it doesn't appear as a link on any other dashboard or in alert messages.

- :strong:`Global data links` are available on all dashboards and alert messages, as well as in the Infrastructure Monitoring navigators. Only Splunk Observability Cloud administrators can create global links. For example, if you create the link described in the previous example as a global instead of local link, the link is available for every appearance of ``InstanceId`` in Splunk Infrastructure Monitoring, including dashboards created after the link is created.


.. important::  If you want your link to be available for properties that appear in alert messages, you must create a global data link.