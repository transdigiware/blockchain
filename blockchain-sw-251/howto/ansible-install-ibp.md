---

copyright:
  years: 2020
lastupdated: "2020-09-22"

keywords: ansible playbooks, docker image, blockchain network, APIs, ansible galaxy

subcollection: blockchain-sw-251

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:javascript: data-hd-programlang="javascript"}
{:tip: .tip}
{:pre: .pre}

<blockchain-sw-251>
# Deploy the service from an Ansible playbook
{: #ansible-install-ibp}

Customers can use an Ansible playbook to automate their installation of the {{site.data.keyword.blockchainfull}} Platform network deployments on a Kubernetes distribution.
{:shortdesc}

**Target audience:** This topic is designed for system administrators or network creators who are responsible for installing {{site.data.keyword.blockchainfull_notm}} Platform networks on a Kubernetes cluster and are new to Ansible playbooks.

This playbook can be used to deploy the {{site.data.keyword.blockchainfull_notm}} Platform on any [supported Kubernetes distribution](/docs/blockchain-sw-251?topic=blockchain-sw-251-console-ocp-about#console-ocp-about-prerequisites). It is not available for Kubernetes clusters that are running in {{site.data.keyword.cloud_notm}}.
{: important}

In just a few minutes, you can have an instance of the {{site.data.keyword.blockchainfull_notm}} Platform v2.5 up and running on your cluster.

## Prerequisites
{: #ansible-install-ibp-prereqs}

Before using the playbook, you must have completed the following steps:
- Purchase an entitlement to {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1. When you configure the playbook, you will need to provide your entitlement key and the email address of your IBMid account that you use to access the My IBM dashboard.
- Review the topic on [getting started with using {{site.data.keyword.blockchainfull_notm}} Platform with Ansible](/docs/blockchain?topic=blockchain-ansible#ansible-getting-started).
- [Install the prerequisites](https://ibm-blockchain.github.io/ansible-collection/installation.html#requirements) on the system where you will run Ansible.
- If you are running on Azure Kubernetes Service, Amazon Web Services, Rancher, Amazon Elastic Kubernetes Service, or Google Kubernetes Engine then you need to set up the NGINX Ingress controller and it needs to be running in [SSL passthrough mode](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#ssl-passthrough){: external}.

### Gather your Kubernetes cluster details
{: #ansible-install-ibp-k8s-cluster}

Before you can deploy the {{site.data.keyword.blockchainfull_notm}} Platform to your Kubernetes cluster, you need to gather the following information:

- The name of your Kubernetes `namespace`, or `project`, if your are using OpenShift Container Platform.
- The domain name of your Kubernetes cluster. This domain name is used as the base domain name for all ingress or routes that are created by the IBM Blockchain Platform.
  - If you are running OpenShift Container Platform, you can find this value by using the OpenShift web console. After logging in to the console, use the drop-down menu next to **OpenShift Container Platform** at the upper left of the page to switch from Service Catalog to Cluster Console. Examine the URL for that page. It will be similar to `console.xyz.abc.com/k8s/cluster/projects`. The value of the domain then would be `xyz.abc.com`, after you remove `console` and `/k8s/cluster/projects`.
  - If you are running any other supported Kubernetes distribution, follow instructions from the distribution documentation to retrieve the domain name after configuring the NGINX Ingress controller.

## Build the Ansible playbook
{: #ansible-install-ibp-playbook}

You are now ready to complete instructions in the [Ansible Collection](https://ibm-blockchain.github.io/ansible-collection/tutorials/installing.html#installing-the-ibm-blockchain-platform){: external} to install the {{site.data.keyword.blockchainfull_notm}} Platform.

When the installation completes successfully you see the message:
```
TASK [console : Print console URL] *************************************************************************************************************************************
ok: [localhost] => {
    "msg": "IBM Blockchain Platform console available at https://xm0507-ibp-console-console.ibp20openshifttestcluster-0defdaa0c51bd4a2dcb024eab4bf04a1-0001.us-south.containers.appdomain.cloud"
}
```

Congratulations! You have successfully installed the {{site.data.keyword.blockchainfull_notm}} Platform on your cluster. Record your console URL from the playbook output and verify the deployment by logging in to the console. Specify the `<console_email>` and `<console_default_password>` that you provided in the `install-ibp.yml` file.

The first time you log in, you are required to change your password, and then log in again.
{: note}

## Next steps
{: #ansible-install-ibp-playbook-next}

If you are simply interested in automating the process of installing the {{site.data.keyword.blockchainfull_notm}} Platform, you are done. You can use the console UI or APIs to create your blockchain components. If you are not familiar with the process to build a network using the console, check out the [tutorial](/docs/blockchain?topic=blockchain-ibp-console-build-network) that walks you through the steps of building a sample network.

Already familiar with how to build a network and want to continue to learn more about the available Ansible playbooks? Then take a look at the tutorial on [Building an {{site.data.keyword.blockchainfull_notm}} Platform network using Ansible playbooks](/docs/blockchain-sw-251?topic=blockchain-sw-251-ansible-build).

</blockchain-sw-251>

