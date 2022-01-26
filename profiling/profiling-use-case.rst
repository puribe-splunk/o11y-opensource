.. _profiling-use-case:

**********************************************************
Use case: Find performance issues using AlwaysOn Profiling
**********************************************************

.. meta:: 
   :description: Learn how you can use AlwaysOn Profiling in Splunk APM to identify performance issues in the code of your applications.

Birdlympics is a Java game launched by Buttercup Games. At the beginning of each round, a player selects three racers. After each race, Birdlympics stores the results in a PostgreSQL database, which feeds the statistics for each bird. The following animation shows the Birdlympics game in action:

..  image:: /_images/apm/profiling/birdlympics-demo-game.gif
    :width: 99%
    :alt: A round from the fictitious Birdlympics game.

The day the Buttercup Games marketing team starts a campaign to promote Birdlympics, Sasha, the lead developer, gets a high CPU consumption alert for the machine hosting the game. He opens the host details in Splunk Infrastructure Monitoring and notices unusually high CPU consumption:

..  image:: /_images/apm/profiling/infra-monitoring.png
    :width: 99%
    :alt: The host view for the Birdlympics server. Notice the high CPU consumption.

Sasha suspects that the cause of the performance issue might lie in the code of Birdlympics. Because he has instrumented the game using the Splunk Java agent with AlwaysOn Profiling enabled, he uses Splunk APM to troubleshoot the issue and fix the bottleneck. He takes the following steps:

1. :ref:`use-case-profiling-enable-profiling`
2. :ref:`use-case-profiling-flame-graph`
3. :ref:`use-case-profiling-check-code`
4. :ref:`use-case-profiling-fix-code`

.. _use-case-profiling-enable-profiling:

Explore slow spans and their stack traces
======================================================

Eager to understand what might be causing the high load, Sasha opens Splunk APM, selects the Birdlympics environment, and clicks :guilabel:`Traces`. He filters the available traces so that the minimum duration is 1 second. Almost all results include the ``stats/races/fastest`` operation:

..  image:: /_images/apm/profiling/traces.png
    :width: 99%
    :alt: The list of traces from the Birdlympics application, filtered to only show those with a duration of 1 second or higher.

Sasha opens the slowest trace to examine the spans. Many spans have call stacks available, which show which functions Birdlympics called and in which order at that point in time. Sasha expands one of the spans and scrolls the stack trace to identify the code behind the bottleneck:

..  image:: /_images/apm/profiling/scroll-stack-traces.gif
    :width: 99%
    :alt: The stack trace of a slow span in the game, with the prefix of the game classes highlighted.

Sasha identifies some classes that connect to the database. The :guilabel:`Span Performance` view confirms that the ``StatsController.fastestRace`` function is responsible for 84% of the workload. The ``fastestRace`` function calculates which contestant has been faster at the end of the race, and performs database calls:

..  image:: /_images/apm/profiling/span-performance.png
    :width: 99%
    :alt: The Span Performance tab of the trace, showing the operations responsible for the total workload.

.. _use-case-profiling-flame-graph:

Browse the aggregated data using the flame graph
======================================================

The stack trace puts Sasha on the right track, but he decides to gather even more evidence, so that he can confirm what needs to be optimized. From the span view, he clicks :guilabel:`View in AlwaysOn Profiler`. Sasha filters the view so that the flame graph highlights only the application code, and he proceeds to scroll down to analyze the stacks:

..  image:: /_images/apm/profiling/filter-narrow-down.gif
    :width: 99%
    :alt: Filtering the flame graph results and zooming in.

The bulk of the application code impacting performance comes from line 32 of the ``StatsController.java`` file, followed by line 69 in the ``BirdDao.java`` file. The self time for both, which summarizes the time spent by each function, is also higher than expected:

..  image:: /_images/apm/profiling/stack-frames.png
    :width: 99%
    :alt: The Span Performance tab of the trace, showing the operations responsible for the total workload.

.. _use-case-profiling-check-code:

Check the code for bugs and bottlenecks
======================================================

Using the information gathered from Splunk APM and its flame graph, Sasha opens the code editor to check the first file, ``StatsController.java``. He discovers that someone added a looped Pi calculation to the race results controller as a practical joke:

.. code-block:: java
   :emphasize-lines: 6,7,8,9,10,11

   public String results(@RequestBody RaceResults results) throws Exception {
      Runnable saveResults = () -> {
         birdDao.saveRaceResults(results);
         birdDao.addWin(results.getWinnerId());
         results.getLosers().forEach(birdDao::addLoss);
         while ( i != 0 )
            {
               i-- ;
               PiFinder.PIcalculation(34343434);
               }
         };

As for the ``BirdDao.java`` file, the problem turns out to be rather trivial. An inefficient loop at line 69 is causing an increase in database queries, which in turn drives CPU consumption under heavy load:

.. code-block:: java
   :emphasize-lines: 4,5,6,7,8,9,10,11

   public RaceResults getFastestRace() {
      List<Integer> ids = jdbc.queryForList("SELECT id from races", Integer.class);
      AtomicInteger fastestRaceId = new AtomicInteger(-1);
      ids.forEach(id -> {
         int thisRaceTime = queryRaceTimeById(id);
         boolean fastest = true;
         // See if this one is faster than all the other ones...
         for (Integer otherId : ids) {
            int otherTime = queryRaceTimeById(otherId);
            if(thisRaceTime > otherTime){
               fastest = false;
               }
            }

.. _use-case-profiling-fix-code:

Fix the code and measure performance again
======================================================

After improving the code and restarting the application, the impact on performance is largely gone, and the flame graph shows much narrower stack frames for the functions that were causing bottlenecks. Sasha passes the good news to the marketing team and reassures them that the marketing campaign can go ahead.

The following image shows the hosts navigator of Splunk Infrastructure Monitoring. The highlighted square is the virtual machine where the game server is hosted:

..  image:: /_images/apm/profiling/low-consumption.png
    :width: 99%
    :alt: The host now shows low CPU consumption in Infrastructure Monitoring.

Summary
====================================================================================

By using a combination of Splunk Infrastructure Monitoring, Splunk APM, and AlwaysOn Profiling, Sasha managed to quickly identify and fix two major performance issues in the Birdlympics game, allowing the marketing campaign to continue while avoiding the need to scale up resources.

Learn more
--------------------

- For more information on AlwaysOn Profiling and how to start using it, see :ref:`profiling-intro`.
- For more information on the stack traces in spans, see :ref:`spans-stack-traces`.
- For more information on the profiling flame graph, see :ref:`flamegraph-howto`.
- For more Splunk APM use cases, see :ref:`apm-use-cases-intro`.
- For more information on monitoring hosts, see :ref:`infrastructure-hosts`.