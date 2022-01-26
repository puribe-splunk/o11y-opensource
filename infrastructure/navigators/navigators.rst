.. _infrastructure-navigators:

****************************************************
Navigators in Splunk Infrastructure Monitoring
****************************************************



.. meta::
    :description: Learn about navigators in Splunk Infrastructure Monitoring 

.. toctree::
    :maxdepth: 3
    :hidden:

    use-navigators
    aws
    azure
    gcp
    k8s
    hosts

When you navigate to Splunk Infrastructure Monitoring, you first see the Infrastructure Monitoring landing page, which provides a data-driven visualization of the physical servers, virtual machines, AWS instances, and other resources in your environment that are visible to Infrastructure Monitoring.

The landing page shows all available navigators, where each navigator is a set of screens that allow you to orient and explore your tech stack. 

Public clouds and containers are groups of navigators. Hosts under My Data Center is a navigator, as well as each of its services. The cards on the landing page correspond to the services you integrate with Splunk Observability Cloud and each card represents a navigator. Click a card to see the navigator for the corresponding service.

.. note::
  
  Once you configure certain integrations to send data to Splunk Observability Cloud, a navigator for that integration shows up as a card on Infrastructure Monitoring landing page and doesn't go away even if you disable the integration. Instead, the card only says "No Data Found" as there is currently no option to hide disabled integration.

For more detail on how to work with a navigator, see :ref:`use-navigators`.

See the following pages for more information about using navigator to monitor each public cloud, container, or host integration:

* Public clouds

  - :ref:`infrastructure-aws`
  - :ref:`infrastructure-gcp`
  - :ref:`infrastructure-azure`

* Containers
  
  - :ref:`infrastructure-k8s`

* My Data Center
  
  - :ref:`infrastructure-hosts`
