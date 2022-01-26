.. _metrics-finder-and-metadata-catalog:


*****************************************************************
Use the Metric Finder and Metadata Catalog
*****************************************************************

The Splunk Infrastructure Monitoring Metric Finder and Metadata Catalog makes it quick and easy to find the metrics you monitor, across infrastructure and over diverse applications and sources. This topic shows you how to leverage the Metric Finder and Metadata Catalog to find, view, and edit information about the metadata in your system.

See the following sections for more information on how to:

- :ref:`metric-finder`

- :ref:`metadata-catalog`


.. _metric-finder:

Use the Metric Finder
=================================================================

To open the Metric Finder, hover over :guilabel:`Metrics` on the navigation bar then select Metric Finder.

You can also find metrics while you build a dashboard or edit a chart. To learn more see :ref:`use-metrics-sidebar`.

To learn more about searching for metrics by name, or by related attributes like dimensions, see :ref:`searching-metrics`.

To learn about how to browse for metrics, see :ref:`browsing-metrics`.

.. _browsing-metrics:

Browse for metrics
------------------------------------------------------------

Click Metrics then Metrics Finder on the navigation bar, the Metric Finder opens with a browsable list of categories, drawn from your Infrastructure Monitoring integrations and custom categories, if configured. Custom categories help you quickly find metrics related to commonly used dimensions in your organization.

If your administrators have not created any custom categories, the custom categories section will not be visible.

If there are no custom categories, Infrastructure Monitoring administrators will see an option to add them. See :ref:`managing-custom-categories`. 

When you select a custom category value or integration, a key-value pair is added as a search filter, and a metric search is run.

If there are more than a few values for a custom category, you can click :guilabel:`Show more` to see the first 100 results. If you don’t see the value that you’re looking for in the longer list, you can type it in the search field to return more relevant search results.



.. _metric-descriptions:

Add metric descriptions
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Descriptions can help users understand what metrics are measuring, especially when the names of metrics are jargon or difficult to recognize. If a metric has a description, it is displayed next to the metric in the search results. 

If a metric has a description, you can find it underneathe the metric title. To add a metric description click :guilabel:`Add description`, or click :guilabel:`Edit description` to edit the custom description. Some metrics have built-in descriptions (for example, from one of our integrations), this provided description is always shown and is not editable. 

Descriptions are lmited to 1,024 characters. Although the descriptions are included in the search result, the text of metric descriptions is not analyzed by the search.


.. _metric-tooltip:

View metric tooltips
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

If you hover over a metric name, a tooltip opens that provides information about the metric. Information you can view in the metric tooltip includes the metric name, description, type, the time it was created, and the number of time series that it reports (filtered by any filters that have been applied to your search query).       


.. _searching-metrics:

Search metrics
------------------------------------------------------------

You can search for metrics using any information you know about what you’re looking for. This could include the name of the metric, the name of a dimension that’s reported along with it, or the value of a dimension or property that is associated with the metric, in any combination. Click :guilabel:`Search metrics` or hit Enter to run a search. Each search result is the name of a metric. Search results are URL-addressable; you can link to a set of search results using the URL for that search.


.. note:: The Metric Finder does not support any special search syntax. Any non-alphanumeric characters in search terms are not included in matches (though these characters can be included in filter values). Advanced search operations like combining search terms with boolean operators, wildcard matching in plain text search terms, or exact matches on multiple search terms are not supported. 

.. The Metric Finder tokenizes search input by non-alphanumeric characters. For example, a plaintext search for ``cpu.utilization`` will match metrics with ``cpu`` and metrics with ``utilization``.   

On the Metrics page, type search terms into the search field:

- Search whatever you know: part of a metric name, the integration that sends it, or a property of the environment it's reporting from.

- Search for metadata (dimensions, properties, and tags) relevant to your target metric.

- Paste exact values into the search field. For example, search a hostname to find out what's reporting from the host.

For example, a plaintext search for ``docker cpu prod`` will return the top 100 metrics that contained ``docker``, ``cpu``, or ``prod`` in their name or metadata. The metric name or metadata will be highlighted to show which search term it matches. The following illustration shows the matches in one search result.


When you’re typing in the search field, you can type in a dot (.) to see a list of possible completions for the prefix you’ve already typed. Keep typing to refine the list of suggested components. Click on a suggested component, or highlight one with the arrow keys and press TAB or Enter to select it.


You can also type in the name of a dimension or property followed by (:), to see a list of possible values for that key in your data. Keep typing to refine the list of suggestions, then choose one to add it as a filter. 


.. _refining-your-search:

Refine your search
------------------------------------------------------------

You can refine a search by typing more search terms or by adding filters. You can add filters by clicking on facets in the left sidebar, or on matching metadata in any of the search results.

You can include wildcards in your filters. For example, ``host:test-*`` filters the results to only those with a value of ``host`` beginning with ``test-``. 

You can use (!) (NOT) in your filters to exclude results. For example, ``!env:qa`` filters the results to exclude any metrics with a value of ``env`` equal to ``qa``.  
      

.. _filter-or-exclude-sidebar:

Use the filter or exclude sidebar
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. Basically, facets are interesting metadata in this metric search that can help you refine your search. 

The left sidebar surfaces relevant metadata from the search results as facets to help refine your search. If your organization has configured custom categories, any that appear in the search results will be surfaced at the top of the left sidebar above other facets. Any value found in the sidebar can be added to the search field as a filter or exluded from the search, and the results will be refreshed. 

If there are more than a few values for a facet, you can click :guilabel:`Show more` to see the first 100 results. If you don’t see the value that you’re looking for in the longer list, you can type it in the search field to return more relevant search results.

.. Refine your results by picking and choosing the filters that should apply to your search by clicking on facets in the left sidebar, or matching metadata to filter or exclude results from your search. 

Hovering anywhere over a value in the left sidebar highlights the row and displays the :guilabel:`Filter` and :guilabel:`Exclude Button`. Click on a value, or the :guilabel:`Filter`, to add it to your search as a filter. To exclude a value from your search results, click the :guilabel:`Exclude Button`.

When filters are excluded from a search, they are indicated by an exclamation point (!) at the beginning to distinguish them from regular search terms. Click :guilabel:`Search metrics` or hit Enter to run a search. 

Properties and dimensions of a metric are shown directly with each search result. This is the same list that is shown in the Related Properties panel of the Metadata Catalog for a given metric.


.. Any related property selected will be added to the search field as a filter (possibly a NOT/exclusion filter), and the results will be refreshed.


.. _matching-metadata:

Match metadata
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

When a search term that you typed also matches metric metadata (such as a dimension name or a property value), that match will be shown under the metric name with a grey outline. Click on the match, or the (+) icon, to add it to your search as a filter. To exclude the metadata from your search results, click the (-) icon.

.. _finding-more-results:

Increase search results
------------------------------------------------------------

If your search did not match any metrics, change the query or remove a filter. Shorter search terms (like ``util``) are likely to match more results than longer terms (like ``utilization``). If you don’t see any results using a long search term, try shortening it to a prefix or separating it into a few smaller terms. For example, break ``NumRequests`` into ``num requests``.

You can also uncheck :guilabel:`Active metrics only` to include inactive metrics that are no longer actively sending data to Infrastructure Monitoring in your search. (By default, the Metric Finder will only look for metrics that are actively sending data.) If this control is unchecked, the time series count shown when you hover over a metric name will include matching inactive time series as well as active time series.


.. _open-chart-from-metric:

Open a chart from a metric
------------------------------------------------------------

When you have found the metric you want, click on the metric name to open the Chart Builder and to start building a new chart with that metric. The new metric plot includes any filters that were part of your search, as well as any matching metadata on the search result that you clicked on. For more information on using the Chart Builder, see :ref:`chart-builder`.


To return to search results from the new chart, click either the :guilabel:`Close` button or the Back button in your browser. If you want to save the chart to a dashboard before exiting, click the :guilabel:`Save as` button.

.. _managing-custom-categories:

Manage custom categories
------------------------------------------------------------

Use custom categories to browse for metrics using features that are unique to your organization’s data, like custom tags or properties. If you use custom metrics, you can set up custom categories to surface key dimensions from your data to help your users get started. Custom categories are defined for the entire organization. Only Infrastructure Monitoring users with admin privileges will see a button to:guilabel:`Add custom categories`. 


To select dimensions or properties to be displayed as custom categories on the Metrics page, click :guilabel:`Add custom categories`. The number of categories for each organization is limited. Once the limit has been reached, the "+" button will be disabled. Click :guilabel:`Save and close` when you have finished adding categories. 

.. Hovering over the disabled button shows a tooltip with an explanation.    

The custom categories you added are now available for use on the Metrics page. Clicking :guilabel:`Edit` lets you add, delete, or update existing custom categories. Non-administrators do not see the option to edit custom categories.
      

.. If the organization has data but no integrations, show "Add custom categories" and "Add new integration"

.. If the organization has no data at all, show only "Add new integration"

.. _metadata-catalog:

Use the Metadata Catalog
=================================================================


Use the metadata catalog to find, view, and edit information about the metadata in your system, such as dimensions, properties, and tags.

To display the Metadata Catalog:

#. Open the navigation bar.
#. Hover over :guilabel:`Metrics` then select :guilabel:`Metric Metadata`.

.. note::
   Enhancements to Infrastructure Monitoring have changed the function of the Metadata Catalog. The Metadata Catalog previously served many purposes that are now better served by the following features:

  - Use the Metric Finder to find metrics and related properties. To learn more, see :ref:`metric-finder`.
  - Use dashboards to see groupings of charts and visualizations of metrics. To learn more, see :ref:`dashboards`.
  - Use navigators to see a data-driven visualization of resources in your environment that are visible to Infrastructure Monitoring. To learn more, see :ref:`infrastructure-navigators`.
  - Use global search to search all available data.


Search the Metadata Catalog
------------------------------------------------------------

To search the Metadata Catalog for dimensions, properties, or metrics:

1. Enter your search criteria in the :guilabel:`Search bar`.


2. Select an item from the search results to view information about that property, dimension, or metric.
 

Add or edit metadata
------------------------------------------------------------

To add or edit an existing description, tag, or property, select the add or edit link for the appropriate section. Note that you can only add or edit properties and tags, not dimensions. For more information, see :ref:`Guidance for metric and dimension names <metric-dimension-names>`.
