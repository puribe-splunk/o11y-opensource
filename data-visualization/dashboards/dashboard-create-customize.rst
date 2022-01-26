.. _dashboard-create-customize:

********************************************************************************
Create and customize dashboards
********************************************************************************

.. meta::
   :description: Learn how to create and customize a dashboard.

.. _create-dashboard:

Create a dashboard
==================

Before getting started, see :ref:`dashboard-basics` for context.

Splunk infrastructure Monitoring makes it easy for you to quickly add simple charts to a new dashboard, then share a copy of that dashboard with others. For more information, see :ref:`simple-charts-dashboards`.

To create a dashboard:

1. Select :guilabel:`Dashboard` from the Splunk Observability Cloud home page.
2. Click the Create menu (plus sign) on the navigation bar.
3. Select :guilabel:`Dashboard`. 

.. note::

  - If you see :guilabel:`Dashboard (unsaved)`, you have already added charts to a new dashboard and haven't saved the dashboard yet. Clicking this option opens the unsaved dashboard. You can only have one unsaved dashboard at a time.
  - If you see :guilabel:`Dashboard with <n> copied charts`, you have copied some charts from one or more dashboards and haven't yet added any charts to a new (unsaved) dashboard. Clicking this option creates a dashboard and pastes the copied charts into it.
  - In the Chart Builder, when saving a new chart or using :guilabel:`Save as` to save a copy of an existing chart, you can create a dashboard on which to place the chart.

.. _change-dashboard-name-description:

Change a dashboard's name or description
=========================================

To rename a dashboard or change the dashboard's description, select :guilabel:`Rename` on the dashboard's Actions menu to display the Dashboard Info tab. Make any desired changes, then click :guilabel:`Save and close`.

.. _modify-built-in-dashboard:

Modify built-in dashboards and their charts
===========================================

There are various reasons you might want to modify built-in dashboards and charts.

- You might want to change the filter or time range on a built-in dashboard as you would with any dashboard.
- Similarly, you might want to make changes to a chart on a built-in dashboard as you would with a chart on any other dashboard.

.. Bring this back in place of the para below it when this feature is implemented --brs
.. Because built-in dashboards are read |hyph| only, you cannot directly save filters or time ranges to the dashboard, nor can you make any changes to the charts on the dashboard. Instead, you can use :menuselection:`Save as` to save a copy of the dashboard, or create a :ref:`dashboard mirror<dashboard-mirroring>`.

Because built-in dashboards are read |hyph| only, you cannot directly save filters or time ranges to the dashboard, nor can you make any changes to the charts on the dashboard. Instead, you can use :guilabel:`Save as` to save a copy of the dashboard.

Alternatively, if you only want to modify a single chart in the dashboard and save it to a different dashboard, click the name of a chart on a built-in dashboard to open it in the Chart Builder view. From there, you can make any changes you like. When you are finished, click :guilabel:`Save As` at the upper right to save the chart, including any changes you've made, to a dashboard of your choice. If you don't want to save your changes, click :guilabel:`Close`.
