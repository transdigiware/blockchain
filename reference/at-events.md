---

copyright:
  years: 2019
lastupdated: "2019-05-23"

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:table: .aria-labeledby="caption"}


# Activity Tracker events
{: #at_events}

When you provision a service instance of {{site.data.keyword.blockchainfull}} Platform in {{site.data.keyword.cloud_notm}}, you see can related events in [{{site.data.keyword.cloud_notm}} Activity Tracker](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-activity_tracker_ov).
{: shortdesc}

## [Example] Events generated when interacting with catalog services
{: #ibp_catalog}

When you provision a service, remove a service, and change the name of a service, an event is generated and sent to the space domain in {{site.data.keyword.cloudaccesstrailshort}} that is associated with the Cloud Foundry space where the service is available in the account.

For example, you provision service A in space B in the us-south region. An event is generated. To see the event, you must provision the {{site.data.keyword.cloudaccesstrailshort}} service in us-south, in the same space where you have provisioned the service. Then, you can see the event through the {{site.data.keyword.cloudaccesstrailshort}} service UI.

The following table lists the catalog actions that generate events:

<table>
  <caption>Catalog actions</caption>
  <tr>
    <th>Action</th>
	  <th>Description</th>
  <tr>
  <tr>
    <td>`audit.service_instance.create`</td>
	<td>An event is created when you provision a service in a Cloud Foundry space.</td>
  </tr>
  <tr>
    <td>`audit.service_instance.update`</td>
	<td>An event is created when you change the name of a service that is available in a Cloud Foundry space.</td>
  </tr>
  <tr>
    <td>`audit.service_instance.delete`</td>
	<td>An event is created when you remove a service from a Cloud Foundry space in the account.</td>
  </tr>
</table>

## Where to view the events
{: #ui}

Option 1: Add the following sentence if your service is provisioned within a Cloud Foundry org and space.

{{site.data.keyword.cloudaccesstrailshort}} events are available in the {{site.data.keyword.cloudaccesstrailshort}} **space domain** that is associated with the Cloud Foundry space where the {{site.data.keyword.cloudaccesstrailshort}} service is provisioned.

Option 2: Add the following sentence if your service sends events to the account domain.

{{site.data.keyword.cloudaccesstrailshort}} events are available in the {{site.data.keyword.cloudaccesstrailshort}} **account domain** that is available in the {{site.data.keyword.Bluemix_notm}} region where the events are generated.
