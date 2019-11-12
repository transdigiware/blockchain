
---

copyright:
  years: 2017, 2019
  lastupdated: "2019-11-12"

keywords:  sysdig, monitoring, resource consumption

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:note: .note}
{:tip: .tip}
{:download: .download}_

# IBM Cloud SysDig
{: #ibp-sysdig}

{{site.data.keyword.cloud_notm}} includes the {{site.data.keyword.mon_full_notm}} service which is useful for monitoring your {{site.data.keyword.blockchainfull}} Platform peer, orderer and certificate authority nodes and smart contract containers.
{: shortdesc}

This tutorial describes how to configure {{site.data.keyword.mon_full_notm}} service to work with your {{site.data.keyword.blockchainfull_notm}} Platform service instance. First, you need to create an instance of SysDig in {{site.data.keyword.cloud_notm}}.  Then, you can configure the SysDig dashboard from the **Monitoring** tab of the **Observability** dashboard in {{site.data.keyword.cloud_notm}}. To learn more about the service and what it offers, see the [{{site.data.keyword.mon_full_notm}} documentation](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-about).

## Step one: Provision an instance of the {{site.data.keyword.mon_full_notm}} service
{: #ibp-sysdig-provision}

Deploy an instance of the {{site.data.keyword.mon_full_notm}} service in your {{site.data.keyword.cloud_notm}} account. Complete the steps in the [Sysdig Getting started tutorial](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-getting-started){: external} tutorial.

{{site.data.keyword.mon_full_notm}} data is colocated in the region where the instance is provisioned. For example, metric data for an instance that is provisioned in US South is hosted in the US South region. Note that the Monitoring instance should reside in the same region as your {{site.data.keyword.blockchainfull_notm}} instance.

## Step two: Configure the {{site.data.keyword.mon_full_notm}} dashboard for monitoring your {{site.data.keyword.blockchainfull_notm}} Platform nodes
{: #ibp-sysdig-configure}

After you have provisioned the Sysdig service, click **View Sysdig** from the Monitoring tab of the Observability dashboard in {{site.data.keyword.cloud_notm}}.
1. On the **Welcome to SysDig Monitor** panel click **Next**.
2. Choose **Kubernetes | GKE | OpenShift** as the installation method.
3. If you have not already configured the instance, you can do so now by following the instructions.
4. After a few minutes, the service is configured and you can click **Go to next step!** then  **Let’s get started** to  launch the SysDig dashboard.

A good place to start monitoring your {{site.data.keyword.blockchainfull_notm}} Platform is the **Dashboards** tab where you can copy and tailor default dashboards according to your monitoring requirements. A dashboard shows groups of metrics that report on the health, performance, and state of your infrastructure, applications, and services for a single host or a group of hosts. Dashboards offer a specialized insight into network data, application data, topology, services, hosts, and containers. Use dashboards to monitor your infrastructure, applications, and services.





