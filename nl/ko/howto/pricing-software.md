---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: pricing model, VPC, CPU, vCPU, virtual core, cost, scalability, estimation, optimize your cost

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud의 가격
{: #ibp-software-pricing}

이 안내서는 {{site.data.keyword.blockchainfull}} Platform for Multicloud에 대한 가격 모델과 Hyperledger Fabric v1.4.1을 기반으로 하는 피어, 순서 지정 노드 및 인증 기관 컴포넌트의 블록체인 네트워크를 개발하고 확장할 때 지불할 비용을 이해하는 데 도움이 됩니다.
{:shortdesc}

_이 가격 모델은 {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud에만 적용됩니다. 스타터 플랜 또는 엔터프라이즈 플랜을 사용 중이고 가격에 대한 질문이 있는 경우 스타터 플랜 및 엔터프라이즈 플랜 [가격](/docs/services/blockchain?topic=blockchain-ibp-pricing)을 참조하십시오._

## 라이센스
{: #ibp-software-pricing-license}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud는 자체 인프라에서 블록체인 네트워크를 실행하는 데 필요한 컴포넌트를 제공하며 {{site.data.keyword.cloud_notm}} Private을 사용하여 배치됩니다. PPA(Passport Advantage)에 액세스하여 [Helm 차트를 다운로드](/docs/services/blockchain?topic=blockchain-console-helm-install#console-helm-install-importing)할 수 있습니다. {{site.data.keyword.blockchainfull_notm}}의 기술 지원도 구매에 포함됩니다.

## 가격
{: #ibp-software-pricing-pricing}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud의 가격은 사용되는 가상 프로세서 코어(VPC)에 따라 달라집니다. VPC 또는 vCPU는 가상 서버에 지정된 가상 코어 또는 파티션되지 않은 서버의 실제 프로세서 코어가 될 수 있습니다. {{site.data.keyword.blockchainfull_notm}} Platform에서 사용할 수 있는 각각의 VPC에 대한 라이센스를 받아야 합니다. 필요한 vCPU 또는 CPU 수는 인프라, 네트워크 디자인 및 성능 요구사항에 따라 달라질 수 있습니다.  

{{site.data.keyword.cloud_notm}} Private의 VPC에 대한 자세한 정보는 {{site.data.keyword.IBM_notm}} Knowledge Center에서 [가상 프로세서 코어(VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}를 참조하십시오. [{{site.data.keyword.IBM_notm}} License Metric Tool](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/welcome/LMT_welcome.html){: external}을 사용하여 라이센스가 필요한 VPC의 수를 판별하기 위해 사용할 수 있는 보고서를 구성 및 작성할 수 있습니다.

다양한 라이센싱 옵션을 사용할 수 있습니다. 자세한 정보는 [문의](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}를 참조하십시오.
