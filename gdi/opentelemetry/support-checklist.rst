.. _otel-support-checklist:

*****************************************************************
Gather information to open a support request
*****************************************************************

.. meta::
      :description: Gather support information before opening a support request. Use this checklist to gather relevant information.

When opening a support request, it is important to include as much information about the issue as possible. Use this checklist to gather relevant information.

Basic information
=============================

* What did you try to do?
* What happened?
* What did you expect to happen?
* Have you found a workaround?
* How impactful is the issue?
* Can you reproduce the issue?

End-to-end architecture information
=========================================

* What is generating the data?
* Where was the data configured to go to?
* What format was the data sent in?
* How is the next hop configured?
* Where is the data configured to go from here?
* What format was the data sent in?
* Is there any DNS, firewall, networking, or proxy information to be aware of?

Configuration files
============================

* Kubernetes: Run ``kubectl get configmap my-configmap -o yaml >my-configmap.yaml`` to retrieve the logs.
* Linux: View the file at ``/etc/otel/collector``.

Logs and debug logs
============================

* Docker: Run ``docker logs my-container >my-container.log`` to retrieve the logs.
* Journald: Run ``journalctl -u my-service >my-service.log`` to retrieve the logs.
* Kubernetes: Run ``kubectl describe pod my-pod kubectl logs my-pod otel-collector >my-pod-otel.log kubectl logs my-pod fluentd >my-pod-fluentd.log`` to retrieve the logs.
* Support bundle scripts are provided to make it easier to collect Linux information,if installer script was used. Run ``/etc/otel/collector/splunk-support-bundle.sh`` to run the script.
