---

copyright:
  years: 2020
lastupdated: "2020-10-15"

subcollection: blockchain

keywords: shared responsibilties, certificate expiration, upgrading your cluster, upgrading your nodes.

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Understanding your responsibilities when using the IBM Blockchain Platform
{: #blockchain-responsibilities}


Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.blockchainfull}} Platform. For a high-level view of the service types in {{site.data.keyword.Bluemix}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{:shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.blockchainfull_notm}} Platform. For the overall terms of use, see [{{site.data.keyword.Bluemix}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms).

{{site.data.keyword.containerlong_notm}} is a managed service in the [{{site.data.keyword.cloud_notm}} shared responsibility model](/docs/overview/terms-of-use?topic=overview-shared-responsibilities). Review the following table of who is responsible for particular cloud resources when using {{site.data.keyword.containerlong_notm}}. Then, you can view more granular tasks for shared responsibilities in [Tasks for shared responsibilities by area](#task-responsibilities).
{: shortdesc}

If you use other {{site.data.keyword.cloud_notm}} products such as {{site.data.keyword.cos_short}}, responsibilities that are marked as yours in the following table, such as disaster recovery for Data, might be IBM's or shared. Consult those products' documentation for your responsibilities.
{: note}  

**{{site.data.keyword.blockchainfull_notm}} Platform console**

| Resource | [Incident and operations management](#incident-and-ops) | [Change management](#change-management) | [Identity and access management](#iam-responsibilities) | [Security and regulation compliance](#security-compliance) | [Disaster Recovery](#disaster-recovery) |
| - | - | - | - | - | - |
| [Data](#applications-and-data) | You | You | You | You | You |
| [Applications](#applications-and-data) | You | You | You | You | You |
| Monitoring and Observability | [Shared](#incident-and-ops) | IBM | [Shared](#iam-responsibilities) | IBM | IBM |
| {{site.data.keyword.blockchainfull_notm}} Platform console| IBM | IBM | You | IBM | IBM |
| Customer Kubernetes cluster| You | You | You | You | You |
| App networking | [Shared](#incident-and-ops) | IBM | IBM | IBM | IBM |
| Cluster networking | [Shared](#incident-and-ops) | IBM | IBM | IBM | IBM |
| Cluster version | IBM | [Shared](#change-management) | IBM | IBM | IBM |
| Worker nodes | [Shared](#incident-and-ops) | [Shared](#change-management) | IBM | IBM | IBM |
| Master | IBM | IBM | IBM | IBM | IBM |
| Service | IBM | IBM | IBM | IBM | IBM |
| Virtual storage | IBM | IBM | IBM | IBM | IBM |
| Virtual network | IBM | IBM | IBM | IBM | IBM |
| Hypervisor | IBM | IBM | IBM | IBM | IBM |
| Physical servers and memory | IBM | IBM | IBM | IBM | IBM |
| Physical storage in IBM Cloud | IBM | IBM | IBM | IBM | IBM |
| Physical storage outside IBM Cloud (SDS Portworx) | You | You | You | You | You |
| Physical network and devices | IBM | IBM | IBM | IBM | IBM |
| Facilities and Data Centers | IBM | IBM | IBM | IBM | IBM |
{: summary="The rows are read from left to right. The resource area of comparing responsibilities is in the first column. The next five columns describe whether you, IBM, or both have shared responsibilities for a particular area."}
{: caption="Table 1. Responsibilities by resource." caption-side="top"}

**Customer Kubernetes cluster**

| Resource | [Incident and operations management](#incident-and-ops) | [Change management](#change-management) | [Identity and access management](#iam-responsibilities) | [Security and regulation compliance](#security-compliance) | [Disaster Recovery](#disaster-recovery) |
| - | - | - | - | - | - |
| [Data](#applications-and-data) | You | You | You | You | You |
| [Applications](#applications-and-data) | You | You | You | You | You |
| Monitoring and Observability | [Shared](#incident-and-ops) | IBM | [Shared](#iam-responsibilities) | IBM | IBM |
| {{site.data.keyword.blockchainfull_notm}} Platform console| IBM | IBM | You | IBM | IBM |
| Customer Kubernetes cluster| You | You | You | You | You |
| App networking | [Shared](#incident-and-ops) | IBM | IBM | IBM | IBM |
| Cluster networking | [Shared](#incident-and-ops) | IBM | IBM | IBM | IBM |
| Cluster version | IBM | [Shared](#change-management) | IBM | IBM | IBM |
| Worker nodes | [Shared](#incident-and-ops) | [Shared](#change-management) | IBM | IBM | IBM |
| Master | IBM | IBM | IBM | IBM | IBM |
| Service | IBM | IBM | IBM | IBM | IBM |
| Virtual storage | IBM | IBM | IBM | IBM | IBM |
| Virtual network | IBM | IBM | IBM | IBM | IBM |
| Hypervisor | IBM | IBM | IBM | IBM | IBM |
| Physical servers and memory | IBM | IBM | IBM | IBM | IBM |
| Physical storage in IBM Cloud | IBM | IBM | IBM | IBM | IBM |
| Physical storage outside IBM Cloud (SDS Portworx) | You | You | You | You | You |
| Physical network and devices | IBM | IBM | IBM | IBM | IBM |
| Facilities and Data Centers | IBM | IBM | IBM | IBM | IBM |
{: summary="The rows are read from left to right. The resource area of comparing responsibilities is in the first column. The next five columns describe whether you, IBM, or both have shared responsibilities for a particular area."}
{: caption="Table 1. Responsibilities by resource." caption-side="top"}


## Incident and operations management
{: #incident-and-ops}




Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Compute (CPU and memory) and storage usage| Fulfill automation requests to help recover worker nodes.  | <ul><li>Use the provided API, CLI, or console tools to adjust compute and storage capacity to meet the needs of your workload.</li><li> Request that worker nodes are rebooted, reloaded, or replaced, and troubleshoot issues such as when the worker nodes are in an unhealthy state.</li></ul> |
| Certificate expiration|  |  |
{: row-headers}
{: caption="Table 1. Responsibilities for incident and operations" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Change management
{: #change-management}

You and IBM share responsibilities for keeping your clusters at the latest container platform and operating system versions, along with recovering infrastructure resources that might require changes. You are responsible for change management of your application data.
{: shortdesc}

| Resource | IBM responsibilities | Your responsibilities |
| -------- | -------------------- | --------------------- |
| Worker nodes | <ul><li>Provide worker node patch operating system (OS), version, and security updates.</li><li>Fulfill automation requests to update and recover worker nodes.</li></ul> | <ul><li>Use the API, CLI, or console tools to [apply](/docs/containers?topic=containers-update#update) the provided worker node updates that include operating system patches; or to request that worker nodes are rebooted, reloaded, or replaced.</li></ul> |
| Cluster version | <ul><li>Provide a suite of tools to automate cluster management, such as the {{site.data.keyword.containerlong_notm}} [API](https://containers.cloud.ibm.com/global/swagger-global-api/#/){: external}, [CLI plug-in](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli), and [console](https://cloud.ibm.com/kubernetes/clusters){: external}.</li><li>Automatically apply Kubernetes master patch OS, version, and security updates.</li><li>Make major and minor updates for master nodes available for you to apply.</li><li>Provide worker node major, minor, and patch OS, version, and security updates.</li><li>Fulfill automation requests to update cluster master and worker nodes.</li></ul> | <ul><li>Use the API, CLI, or console tools to [apply](/docs/containers?topic=containers-update#update) the provided major and minor Kubernetes master updates and major, minor, and patch worker node updates.</li></ul> |
{: summary="The rows are read from left to right. The resource area of comparing responsibilities is in the first column, with the responsibilities of IBM in the second column and your responsibilities in the third column."}
{: caption="Table 3. Responsibilities for change management" caption-side="top"}

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Worker nodes |  |  |
| Cluster | | |
| Blockchain components | | |
{: row-headers}
{: caption="Table 2. Responsibilities for change management" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Identity and access management
{: #iam-responsibilities}




Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Task 1| {{site.data.keyword.IBM_notm}} responsibility description  | Customer responsibility description |
|Task 2| {{site.data.keyword.IBM_notm}} responsibility description  | Customer responsibility description |
|Task 3| {{site.data.keyword.IBM_notm}} responsibility description  | Customer responsibility description |
{: row-headers}
{: caption="Table 3. Responsibilites for identity and access management" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Security and regulation compliance
{: #security-compliance}




Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Task 1| {{site.data.keyword.IBM_notm}} responsibility description  | Customer responsibility description |
|Task 2| {{site.data.keyword.IBM_notm}} responsibility description  | Customer responsibility description |
|Task 3| {{site.data.keyword.IBM_notm}} responsibility description  | Customer responsibility description |
{: row-headers}
{: caption="Table 4. Responsibilites for security and regulation compliance" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Disaster recovery
{: #disaster-recovery}




Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

|  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Task 1| {{site.data.keyword.IBM_notm}} responsibility description  | Customer responsibility description |
|Task 2| {{site.data.keyword.IBM_notm}} responsibility description  | Customer responsibility description |
|Task 3| {{site.data.keyword.IBM_notm}} responsibility description  | Customer responsibility description |
{: row-headers}
{: caption="Table 5. Responsibilites for disaster recovery" caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}
