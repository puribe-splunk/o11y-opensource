.. _detector-options:

************************************
Detector options
************************************

.. meta::
  :description: How to set detector options in the Options tab.

The Options tab lets you specify some of the same settings that are available in the :ref:`Chart Options tab<chart-options-tab>` of a chart.


Show events as lines
=======================

.. if text is changed here, also change it in :ref:`event-lines`

Specifies whether vertical lines are displayed at times where event markers are shown.


Show data markers
========================

.. if text is changed here, also change it in :ref:`show-markers`

Specifies whether small dots are displayed on the chart, indicating the times at which there are data points.


Max delay
====================

Splunk Infrastructure Monitoring sets the Max Delay parameter based on estimates of how 'on time' the time series are. By default, the Auto setting lets Infrastructure Monitoring detect and apply a reasonable value automatically, based on how your data is coming in.

If you know that some of your data is being delayed, and you don't want to wait for that data to arrive before your charts are updated, then you can set max delay accordingly. To ensure that charts are updated as quickly as possible (in other words, to not wait for late data at all), select 1s for max delay.

To specify your own values, enter the number in milliseconds. For example, enter 2000 to specify a max delay of 2 seconds. The upper limit is 15 minutes (900000 ms), but values over 5 minutes can be impractical. If you enter a value higher than 15 minutes, max delay will be set to 15 minutes. For more information, see :ref:`delayed-datapoints`.

.. Minimum resolution
.. -------------------------------------------------------------------
..
.. .. if text is changed here, also change it as necessary in :ref:`min-resolution`
..
.. Specifies the minimum interval for which Splunk Infrastructure Monitoring should :ref:`roll up<rollups>` values to display a data point on a chart. For example, if you are tracking the number of support calls received per hour, you might not want to see a chart that shows data points representing the number of calls received every 15 |nbsp| minutes, even if data is available at that resolution. Setting this option to 1h ensures that the data points will always represent values for periods of 1h or more.
..
.. The detector is still triggered as configured, regardless of the resolution of the chart's display.

Disable chart display sampling
================================

.. if text is changed here, also change it as necessary in :ref:`chart-sampling`

In cases where a large number of time series would be displayed, for example, if you choose a metric being reported by 500 servers, Splunk Infrastructure Monitorig samples a subset of those time series so the chart will render more quickly. The sampled display provides you with an approximate sense of the values in those time series. If you disable sampling, any time series data that were previously omitted will be shown. Depending on the number of time series, disabling sampling may cause the chart to render more slowly.

The detector is still triggered as configured, regardless of the number of time series displayed on the chart.



.. _detector-cal-time-zone:

Calendar time zone
=====================

The :ref:`time zone<cal-window-time-zone>` used for aligning data timestamps and interpreting calendar cycles in analytics functions that perform  :ref:`calculations over calendar windows<calendar-window>`. All such functions in a chart use the same calendar time zone. The value set here can also be viewed and changed while editing any calendar window function in the chart builder. This option has no effect if there are no functions using calendar windows.
