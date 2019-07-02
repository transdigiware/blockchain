---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

# {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 시작하기
{: #get-started-console-icp}

{{site.data.keyword.blockchainfull}} Platform은 퍼블릭 및 프라이빗 클라우드(예: {{site.data.keyword.cloud_notm}}), 자체 데이터 센터 및 서드파티 퍼블릭 클라우드에 배치할 수 있습니다. 이 멀티클라우드 배치는 {{site.data.keyword.IBM_notm}}의 Kubernetes 기반 컨테이너 오케스트레이션 플랫폼인 {{site.data.keyword.cloud_notm}} Private을 통해 가능합니다. 이 오퍼링을 사용하여 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 {{site.data.keyword.cloud_notm}} Private 배치에 설치한 다음 콘솔을 사용하여 {{site.data.keyword.blockchainfull_notm}} 컴포넌트를 로컬 클러스터에 작성할 수 있습니다. 콘솔에서 다른 {{site.data.keyword.cloud_notm}} Private 클러스터 또는 {{site.data.keyword.cloud_notm}}에 배치된 노드를 가져와서 분산 멀티클라우드 네트워크를 운영할 수도 있습니다.
{:shortdesc}

**대상 독자**: 이 주제는 {{site.data.keyword.cloud_notm}} Private에 {{site.data.keyword.blockchainfull_notm}} Platform 배치할 책임이 있는 시스템 관리자를 위해 설계되었습니다.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private은 {{site.data.keyword.cloud_notm}} Private의 번들화된 제품입니다. {{site.data.keyword.cloud_notm}} Private에 대한 자세한 정보는 [{{site.data.keyword.cloud_notm}} Private 버전 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}용 문서를 참조하십시오.

## 1단계: {{site.data.keyword.cloud_notm}} Private에서 Kubernetes 클러스터 설정
{: #get-started-console-icp-step-one-set-up-k8s-on-icp}

첫 번째 단계는 {{site.data.keyword.cloud_notm}} Private의 원하는 클라우드 플랫폼에서 Kubernetes 클러스터를 설정하는 것입니다.
권장사항 및 지침은 [{{site.data.keyword.cloud_notm}} Private 설정](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup)을 참조하십시오. 

프로덕션에 사용할 네트워크를 빌드하는 경우 고가용성을 구현하도록 클러스터 배치 및 블록체인 네트워크를 설정해야 합니다.

- 클러스터 구성에 대한 권장사항은 [{{site.data.keyword.cloud_notm}} Private에서 고가용성 구현](https://www.ibm.com/cloud/garage/practices/manage/high-availability-ibm-cloud-private){: external}을 참조하십시오.
- 네트워크 빌드를 시작할 준비가 된 경우 [피어에 대한 고가용성 고려사항](/docs/services/blockchain?topic=blockchain-ibp-console-ha#ibp-console-ha-peers) 및 [순서 지정 서비스에 대한 고가용성 고려사항](/docs/services/blockchain?topic=blockchain-ibp-console-ha#ibp-console-ha-ordering-service)을 참조하십시오.

## 2단계: {{site.data.keyword.blockchainfull_notm}} Platform 설치
{: #get-started-console-icp-step-two-deploy-console}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private은 PPA(Passport Advantage)에서 다운로드할 수 있는 Helm 차트로 제공됩니다. 로컬 클러스터에 Helm 차트를 설치하는 자세한 방법은 [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 설치](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install)를 참조하십시오. 

## 3단계: {{site.data.keyword.blockchainfull_notm}} Platform 콘솔 배치
{: #get-started-console-icp-step-three-deploy-console}

Helm 차트를 설치한 후에는 카탈로그 페이지에서 {{site.data.keyword.blockchainfull_notm}} Platform 애플리케이션 타일을 클릭하여 로컬 클러스터에 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 설치할 수 있습니다. 콘솔 구성 방법과 블록체인 컴포넌트에 필요한 리소스에 대한 자세한 정보는 [{{site.data.keyword.blockchainfull_notm}} Platform 콘솔 배치](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp)를 참조하십시오. 

## 4단계: 콘솔에 사용자를 관리자로 추가
{: #get-started-console-icp-step-four-add-console-admin}

콘솔 관리자는 배치 중에 제공된 이메일 주소와 비밀번호를 사용하여 콘솔에 로그인할 수 있습니다. 제공된 비밀번호는 콘솔의 기본 비밀번호가 되며 모든 신규 사용자가 콘솔에 처음 로그인할 때 사용됩니다. 그러면 관리자는 신규 사용자를 콘솔에 추가할 수 있으므로, 다른 사용자가 로그인하여 {{site.data.keyword.blockchainfull_notm}} 노드 작업을 시작할 수 있습니다. 관리자는 또한 기본 비밀번호를 새로 설정할 수도 있습니다. 자세한 정보는 [콘솔에서 사용자 관리](/docs/services/blockchain/howto?topic=blockchain-console-icp-manage#console-icp-manage-users)를 참조하십시오.

## 5단계: 콘솔을 사용하여 컴포넌트 작성
{: #get-started-console-icp-build-network}

콘솔을 배치한 후에는 콘솔을 사용하여 {{site.data.keyword.blockchainfull_notm}} 컴포넌트를 로컬 클러스터에서 작성, 운영 및 제어할 수 있습니다. 콘솔 UI를 사용하여 시작하려면 [네트워크 빌드 튜토리얼](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network)을 참조하십시오.

## 6단계: 클라우드 전체에서 네트워크 연결
{: #get-started-console-icp-import-nodes}

콘솔을 사용하여 다른 {{site.data.keyword.cloud_notm}} Private 클러스터 또는 {{site.data.keyword.cloud_notm}}에서 실행 중인 컴포넌트를 조작할 수 있습니다. 먼저, 컴포넌트가 처음 배치된 콘솔에서 JSON 파일로 컴포넌트 정보를 내보내야 합니다. 그런 다음 노드 JSON 파일을 로컬 클러스터에 배치된 콘솔로 가져와서 클라우드 전체에서 컴포넌트를 관리할 수 있습니다. 자세한 정보는 [노드 가져오기](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-nodes)를 참조하십시오.
