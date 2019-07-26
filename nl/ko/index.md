---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform offerings, IBM Cloud Private, AWS, VS code extension, IBM Cloud

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# {{site.data.keyword.blockchainfull_notm}} Platform 시작하기
{: #get-started-ibp}

{{site.data.keyword.blockchainfull}} Platform은 사용자가
직접 선택한 환경에서 블록체인 컴포넌트를 배치할 수 있는 관리되는 전체 스택의
BaaS(blockchain-as-a-service) 오퍼링을 제공합니다. 환경은
{{site.data.keyword.cloud_notm}}, {{site.data.keyword.cloud_notm}} Private을 통한 온프레미스,
AWS(Amazon Web Services) 등의 서드파티일 수 있습니다. 이 튜토리얼에서는
{{site.data.keyword.blockchainfull_notm}} Platform을 사용하여
기본 블록체인 네트워크를 설정하는 일반적인 프로세스에 대해 설명합니다.
{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform 오퍼링을 사용하기 전에 [면책사항](/docs/services/blockchain?topic=blockchain-disclaimer#disclaimer) 절에서 기술 및 지원 정보를 읽으십시오.
{: important}


## 시작하기 전에
{: #get-started-ibp-prereqs}

{{site.data.keyword.blockchainfull_notm}} Platform은 모든 유형의 사용자가 블록체인 과정을 시작하고 애플리케이션을 프로덕션으로 이동하는 데 도움이 되는 여러 오퍼링을 제공합니다. 사용할 환경 및 오퍼링을 결정해야 합니다. 그림 1에는 다양한 오퍼링에 대한 기능, 대상 독자, 가격 및 클라우드 플랫폼이 나열되어 있습니다. 각 오퍼링에 대한 자세한 정보를 보려면 다음 표에서 오퍼링을 클릭하십시오.

| **오퍼링** | **포함된 내용** | **청구 정책** | **Cloud 플랫폼** |
| ------------------------- |-----------|-----------|-----------|-----------|
| [**{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode) | 개발자는 스마트 계약을 신속하게 개발하기 위해 명령 팔레트에서 액세스 가능한 탐색기 및 명령을 제공하는 IDE로 시작할 수 있습니다. | 무료 |로컬 시스템에서 실행 |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**](/docs/services/blockchain/howto?topic=blockchain-ibp-console-overview#ibp-console-overview) | {{site.data.keyword.cloud_notm}} Kubernetes 클러스터에서 블록체인 컴포넌트를 배치하고 관리하는 데 사용될 수 있는 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔 및 API | [VPC 가격 $0.29 USD/VPC-시간](/docs/services/blockchain/howto?topic=blockchain-ibp-saas-pricing) | {{site.data.keyword.cloud_notm}} |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about) | 블록체인 컴포넌트 프로비저닝 및 관리를 위해 Kubernetes Helm 차트 및 API를 사용하여 {{site.data.keyword.cloud_notm}} Private 클러스터에 배치된 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔. | [VPC 가격](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) | {{site.data.keyword.cloud_notm}} Private |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for AWS**](/docs/services/blockchain/howto?topic=blockchain-remote-peer-aws-about#remote-peer-aws-about) | {{site.data.keyword.cloud_notm}} 외부의 원격 피어를 배치하기 위한 AWS 빠른 시작 템플릿.| 무료 | AWS |

*그림 1. {{site.data.keyword.blockchainfull_notm}} Platform 오퍼링*


## 1단계: 오퍼링 얻기
{: #get-started-ibp-step1}

{{site.data.keyword.blockchainfull_notm}} Platform 오퍼링을 얻으려면 클라우드 계정 또는 PPA 라이센스가 필요합니다.

* **{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**

  이 VS Code 확장은 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external}에서 무료로 사용 가능하며, 최종적으로  {{site.data.keyword.blockchainfull_notm}}에 배치하기 위해 스마트 계약을 개발하고 디버그하며 테스트하는 데 사용될 수 있습니다.

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  이 오퍼링은 {{site.data.keyword.cloud_notm}}의 [{{site.data.keyword.cloud_notm}} 카탈로그 대시보드](https://cloud.ibm.com/catalog){: external}에서 사용할 수 있습니다.

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  이 오퍼링은 [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html)을 통해 배치 가능한 Helm 차트로 제공됩니다.

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  이 오퍼링은 AWS에서 사용 가능하며 [빠른 시작 템플리트](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}를 사용하여 AWS에서 블록체인 피어를 배치할 수 있습니다.

## 2단계: {{site.data.keyword.blockchainfull_notm}} Platform 배치
{: #get-started-ibp-step2}

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  {{site.data.keyword.cloud_notm}}에 로그인하여 오퍼링을 사용하여 서비스 인스턴스를 작성하십시오. 마법사를 따라 네트워크의 초기 구성을 완료하십시오. 자세한 정보는 [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Kubernetes Service 시작하기](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks)를 참조하십시오.

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  네트워크를 배치하기 전에 {{site.data.keyword.cloud_notm}} Private 클러스터에 Helm 차트를 설치해야 합니다. 그런 다음 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 배치하여 로컬 클러스터에서 블록체인 컴포넌트를 배치 및 작동하는 데 사용할 수 있습니다. 자세한 정보는 [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 시작하기](/docs/services/blockchain?topic=blockchain-get-started-console-icp#get-started-console-icp)를 참조하십시오.

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  이 오퍼링은 AWS에서 사용 가능하며 [빠른 시작 템플리트](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}를 사용하여 AWS에서 블록체인 피어를 배치할 수 있습니다.

## 다음 단계
{: #get-started-ibp-next-steps}

선택한 환경에 {{site.data.keyword.blockchainfull_notm}} Platform을 배치한 후에
컨소시엄, 노드, 채널, 스마트 계약을 사용하여 네트워크를 설정하고 해당 네트워크에서 트랜잭션을 시작할 수 있습니다. 자세한 정보는
이 문서의 왼쪽 탐색에 있는 **HOW TO** 절 아래의 태스크 주제를 참조하십시오.

## 지원 받기
{: #get-started-ibp-getting-support}

{{site.data.keyword.IBM_notm}}은 {{site.data.keyword.IBM_notm}}에서 구현된
블록체인 솔루션에서 여러 지원 옵션을 제공합니다. {{site.data.keyword.blockchainfull_notm}} Platform
지원에 대한 자세한 정보는 [지원 받기](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support)를 참조하십시오.
