.. _rum-terminology-concepts:

******************************************************
Terminology and concepts in Splunk RUM
******************************************************

.. meta::
   :description: Learn about important terminology and concepts in Splunk RUM for Browser 

The following sections introduce important terminology and concepts in Splunk RUM for Browser and Splunk RUM for Mobile. 


Traces and spans
==================

A trace is a collection of operations that represents a unique transaction handled by an application and its constituent services. A span is a single operation within a trace. A session is made up of a collection of spans and traces. The definitions for trace and span are the same in both Splunk RUM for Browser and Splunk APM. For more information about traces in APM, see :ref:`apm-add-context-trace-span`.

Span types in RUM for Browser 
-----------------------------
A browser span can represent one of the following actions:

- document load
- resource request
- network request
- UI calls to the server
- user xpath requests and interactions with pages



Session
========

A session refers to a group of user interactions on an application for a maximum of 4 hours. A Session begins when a user loads the front-end application and ends when the application is terminated or expires. Sessions expire after 15 minutes of inactivity.


Session ID
================

A session ID is created by the browser. The session ID is a field name that identifies each session in your application that you are monitoring with Splunk RUM for Browser.

Browser trace
================

A browser trace is a collection of spans that specifically represents activity on the browser, such as an xhr request or a document load.

Backend traces
================

Backend traces are collections of backend spans. Backend spans are calls that microservices make to each other, such as an account service making a request to a database.


Example session
================

An example session is a session that contains a certain behavior you want to analyze. For instance, suppose users are encountering 404 errors on your application. In Splunk RUM for Browser you can view example sessions that represent sessions where users encountered 404 errors so that you can better understand the cause and impact of the problem.

Tag
================

A tag is a field name that is an attribute of your span that you can use to filter events. An example of a tag is a property about the user, such as the operating system or the browser the user uses to view your application.

Node
================
In Splunk RUM for Browser, a node is a resource, page, or view that is instrumented and emits data about itself. For example, suppose you have a "cart" page in your application. This page reports when it was loaded, generated an error, or initiated a network request.

Inferred node
================

An inferred node is a resource, page or view that does not directly report data about itself to Splunk RUM for Browser. You can glean information about inferred nodes by investigating the interactions other nodes have with the inferred node. For example, in this diagram, the page is an instrumented node that reports data about itself. The endpoint, an inferred node, reports back on the signals executed by the network, service and database. In this example, the endpoint, network, service and database don't directly self-report data to Splunk RUM for Browser.

..  image:: /_images/rum/inferred_node.png
    :width: 99%
    :alt: <This diagram shows how nodes relate to inferred nodes. The page node communicates with the endpoint, an inferred node. The endpoint reports back the information about the network, service, and database.>

Edge
================

An edge is an interaction between a page and an endpoint. Edges are represented by arrows in Splunk RUM for Browser.



App crash
================

In Splunk RUM for Mobile, an application crash is defined as when an application encounters an error and exits. For example, an application might crash because of an unhandled exception, an incompatible OS version, or an unexpected API change. 



App start
================

In Splunk RUM for Mobile, App start is when the app is responsive and the user can interact with the app. For example, when a user opens your application, it might take a few milliseconds or seconds to initialize the code or application before app start and then the OS reports that the app is responsive.

Presentation transition
================================

Presentation transitions are screen transitions and screen changes, such as when a user goes from the login screen to the home screen.
