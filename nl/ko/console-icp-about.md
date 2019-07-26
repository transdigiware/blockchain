---

copyright:
years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform, IBM Cloud Private, system requirements, Kubernetes Helm chart, behind a firewall

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

# {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud 정보
{: #console-icp-about}

{{site.data.keyword.blockchainfull}} Platform은 퍼블릭 및 프라이빗 클라우드(예: {{site.data.keyword.cloud_notm}}), 자체 데이터 센터 및 서드파티 퍼블릭 클라우드에 배치할 수 있습니다. 이 멀티클라우드 배치는 IBM의 Kubernetes 기반 컨테이너 오케스트레이션 플랫폼인 {{site.data.keyword.cloud_notm}} Private을 통해 가능합니다. 이 릴리스는 {{site.data.keyword.cloud_notm}} Private 클러스터에서 블록체인 컴포넌트를 배치 및 관리하는 데 사용할 수 있는 블록체인 콘솔 사용자 인터페이스를 제공합니다. {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private은 Hyperledger Fabric v1.4.1 코드 베이스를 사용하며 {{site.data.keyword.cloud_notm}} Private v3.2.0에 대한 배치를 지원합니다.
{:shortdesc}

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private에서 제공하는 기능
{: #console-icp-about-offers}

이 오퍼링을 사용하여 {{site.data.keyword.cloud_notm}} Private의 배치 시 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 설치할 수 있습니다. 그런 다음 콘솔에서 Hyperledger Fabric 블록체인의 모든 기본 컴포넌트(인증 기관, 순서 지정 서비스 및 피어)를 로컬 클러스터에 배치할 수 있습니다. Hyperledger Fabric 네트워크의 구성 요소에 대한 자세한 정보는 [블록체인 컴포넌트 개요](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview)를 참조하십시오. 콘솔에서 다른 {{site.data.keyword.cloud_notm}} Private 클러스터 또는 {{site.data.keyword.cloud_notm}}에 배치된 노드를 가져와서 분산 멀티클라우드 네트워크를 운영할 수도 있습니다.

이 {{site.data.keyword.blockchainfull_notm}} Platform 릴리스에는 다음과 같은 주요 기능이 포함되어 있습니다.

**빌드 ---- 통합된 개발자 경험**
- 스마트 계약을 Node.js, Golang 또는 Java로 **쉽게 코딩**하고, 새로운 {{site.data.keyword.blockchainfull_notm}} VS Code 확장을 사용하여 클라이언트 애플리케이션을 작성하며, 사용자 인터페이스 콘솔과의 **SDK 통합**을 활용하고, 다양한 튜토리얼 및 샘플에서 학습할 수 있습니다.
- **단순화된 DevOps**를 사용하면 더 많은 컴포넌트를 추가할 수 있도록 Kubernetes 자원을 스케일링하여 개발에서 테스트까지 단일 환경의 프로덕션으로 이동할 수 있습니다.
- **Kubernetes 서비스 통합.** 모니터링을 위해 로깅 및 Kibana에 대한 서비스(예: Grafana 및 Prometheus)를 활용합니다.
- **최신 Fabric 주요 기능**. Hyperledger Fabric v1.4.1의 최신 기능을 활용합니다.
  - [Raft 순서 지정 서비스](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [개인용 데이터 콜렉션](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) - gossip 프로토콜을 통해 권한 부여된 피어만 원장 데이터를 공유할 수 있도록 하여 강화된 데이터 개인정보 보호를 제공합니다.
  - [서비스 발견](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} - 애플리케이션이 네트워크와 상호작용하는 방식을 동적으로 검색하고 업데이트할 수 있습니다.
  - [채널 액세스 제어 목록 ](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} - 채널 및 스마트 계약을 추가적으로 통제할 수 있습니다.

**조작 --- 종합적인 배치 제어**
- **필요한 컴포넌트만 배치.**. 피어를 여러 채널 및 네트워크에 연결하거나 비즈니스 파트너가 연결할 수 있는 순서 지정 서비스를 호스팅합니다.
- **ID 완전 제어 유지**. 사용자 고유의 보안 환경에서 노드를 관리하는 데 사용되는 키를 저장하고 관리합니다.
- **통합 운영**. {{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 사용하면 {{site.data.keyword.IBM_notm}} 또는 기타 공급업체에 의존하여 순서 지정 노드 또는 인증 기관을 관리할 필요 없이 **하나의 콘솔**에서 모든 조직과 노드를 배치하고 관리할 수 있습니다. 또한 콘솔에서 블록체인 컨소시엄에 대해 구성원을 추가하거나 제거할 수 있으며 채널을 작성하고 가입할 수 있으며 스마트 계약을 설치하고 인스턴스화할 수 있습니다.
- **네트워크 호스팅 또는 가입**. 조직이 인프라와 독립적으로 노드를 관리하는 동안 클러스터에서 호스팅되는 피어를 다중 클라우드의 여러 채널에 배치하거나 사용자의 컨소시엄 또는 채널에 가입하도록 기타 조직을 초대할 수 있습니다.
- 노드를 관리 또는 모니터링할 수 있는 사용자의 **액세스를 관리**합니다.
- Kubernetes Service에서 노드 **로그에 대한 직접 액세스**. 로그를 추출하고 분석하려면 지원되는 서드파티 서비스를 사용하십시오.
- Kubernetes Service를 사용하여 **노드 팟(Pod)과 직접 상호작용**.
- 채널 구성에 대한 협업 통제를 더 효과적으로 제어할 수 있도록 하는 **동적 서명 콜렉션**

**성장 --- 확장성 및 유연성**
- **컴퓨팅 선택**. CPU, 메모리 및 Kubernetes 클러스터에서 프로비저닝할 스토리지의 양을 유연하게 결정할 수 있습니다. 자세한 정보는 [콘솔이 Kubernetes 클러스터와 상호작용하는 방법](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction)을 참조하십시오.
- 필요한 만큼만 비용을 지불하고 Kubernetes 클러스터 내의 자원 **규모를 확장 및 축소**할 수 있습니다. 자세한 정보는 [가격](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing)을 참조하십시오.
- **재해 복구 및 다중 구역 고가용성**. 이 옵션은 여러 구역에 걸쳐 Kubernetes 배치를 복제하므로 컴포넌트의 고가용성(HA) 및 재해 복구(DR)를 사용할 수 있습니다.
- **어디서나 실행**. {{site.data.keyword.blockchainfull_notm}} Platform 콘솔의 **통합 코드 베이스**를 통해 {{site.data.keyword.cloud_notm}} Private에서 지원하는 모든 환경에서 컴포넌트를 실행할 수 있습니다.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private은 {{site.data.keyword.cloud_notm}} Private의 번들화된 제품으로, Kubernetes Helm 차트로 제공됩니다. {{site.data.keyword.cloud_notm}} Private에 대한 자세한 정보는 [{{site.data.keyword.cloud_notm}} Private 버전 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}용 문서를 참조하십시오.

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private의 적합성
{: #console-icp-about-suitable}

{{site.data.keyword.blockchainfull_notm}} Platform을 {{site.data.keyword.cloud_notm}} 외부에서 실행할 경우 블록체인 네트워크에 가입하거나 해당 네트워크를 확장할 수 있는 유연성이 제공됩니다. 이를 통해 네트워크 시작자는 자신이 선택한 플랫폼을 사용하면서 새 구성원이 가입할 수 있도록 허용하여 자신의 네트워크를 확장할 수 있습니다. 이 경우 블록체인 네트워크에 가입하려는 조직에서 해당 피어를 기존 애플리케이션과 함께 배치하거나 SOR(System of Record)에 통합할 수 있습니다.

이 오퍼링의 사용자는 자체 보안 및 인프라를 관리하게 됩니다. {{site.data.keyword.cloud_notm}}에서는 해당 서비스를 제공하지 않습니다. 시작하기 전에 다음 절에 있는 [고려사항 및 제한사항](#console-icp-about-considerations)을 검토하십시오.

## 고려사항 및 제한사항
{: #console-icp-about-considerations}

시작하기 전에 다음 제한사항을 이해해야 합니다.

- 블록체인 컴포넌트의 상태 모니터링, 보안, 로깅 및 리소스 사용량 관리 작업은 사용자가 수행해야 합니다.
- 콘솔에서는 Hyperledger Fabric v1.4.1 이상을 기반으로 하는 컴포넌트를 작성하고 제어할 수만 있습니다.
- 하나의 콘솔 피어 Kubernetes 네임스페이스만 배치할 수 있습니다. 예를 들어 개발, 스테이징 및 프로덕션 목적의 여러 환경을 작성하기 위해 블록체인 네트워크를 여러 개 작성하려는 경우 고유한 네임스페이스를 환경별로 작성해야 합니다.
- 다른 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔에서 내보낸 노드만 가져올 수 있습니다. 가져온 피어 또는 순서 지정 노드가 콘솔에서 작동하려면 연관된 노드의 조직 MSP 정의와 관리자 ID도 콘솔로 가져와야 합니다.
- {{site.data.keyword.blockchainfull_notm}} Platform은 {{site.data.keyword.cloud_notm}} Private v3.2 Enterprise Edition에서만 지원됩니다.  {{site.data.keyword.cloud_notm}} Private v3.2 Community Edition은 지원되지 않습니다.
- {{site.data.keyword.blockchainfull_notm}} Platform Helm 차트를 설치하기 위해 {{site.data.keyword.IBM_notm}} Multicloud Manager를 사용할 수 없습니다.

{{site.data.keyword.cloud_notm}} Private에서 지원하는 환경을 검토하려면 [지원되는 환경](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}을 참조하십시오.
{:important}

## 시스템 전제조건
{: #console-icp-about-prerequisites}

{{site.data.keyword.cloud_notm}} Private에 대한 하드웨어 및 소프트웨어 전제조건 목록의 경우 [시스템 요구사항](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_system_config/system_reqs.html){: external}을 참조하십시오. 그러나 {{site.data.keyword.blockchainfull_notm}} Platform은 Linux on Power(ppc64le) POWER8 시스템에서 지원되지 않음에 유의하십시오.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm 차트는 다음과 같은 작업자 노드 및 백업 스토리지를 사용하여 Ubuntu Linux 기반 {{site.data.keyword.cloud_notm}} Private v3.2 클러스터에서 실행되도록 유효성 검증되었습니다.

- **LinuxONE 및 {{site.data.keyword.IBM_notm}} Z**: z/VM 및 KVM(NFS 사용)
- **x86**: Linux 64비트(GlusterFS 사용)

## 라이센스
{: #console-icp-about-license}

라이센스 부여 및 가격에 대한 자세한 정보는 [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud의 가격](/docs/services/blockchain?topic=blockchain-ibp-software-pricing)을 참조하십시오.

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 설치
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private은 로컬 {{site.data.keyword.cloud_notm}} Private 클러스터에 설치될 수 있는 Helm 차트 파일로 제공됩니다. Helm 차트를 설치한 후에는 {{site.data.keyword.cloud_notm}} Private 카탈로그에서 애플리케이션으로 {{site.data.keyword.blockchainfull_notm}} Platform을 찾을 수 있습니다. Helm 차트 및 필요한 전제조건을 설치하는 방법에 대한 지시사항은 [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 설치](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#helm-console-install)를 참조하십시오.

{{site.data.keyword.cloud_notm}} Private의 신규 사용자이며 {{site.data.keyword.cloud_notm}} Private 설치 및 배치에 대한 정보와 팁이 필요한 경우 [{{site.data.keyword.cloud_notm}} Private 설정](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup)을 참조하십시오.

공용 인터넷에 액세스하지 않고 {{site.data.keyword.blockchainfull_notm}} Platform을 방화벽 뒤에 배치할 수 있습니다. 다운로드된 Helm 차트 패키지에는 배치 중에 DockerHub에서 가져오지 않고 {{site.data.keyword.blockchainfull_notm}} Platform에서 사용하는 모든 Fabric 컴포넌트 Docker 이미지가 포함되어 있습니다.

## 보안 고려사항
{: #console-icp-about-security}

이러한 컴포넌트는 자체 인프라에 배치되기 때문에 사용자가 해당 보안을 관리해야 합니다. 여기에는 키 관리 및 데이터 암호화 등의 중요한 보안 영역이 포함되어 있습니다. 컴포넌트에 대한 보안을 고려할 때 다음 주제를 검토하십시오.

### 데이터 보안
{: #console-icp-about-security-data}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}에서는 각 서비스 인스턴스에 의해 작성된 모든 데이터를 보호하려면 [대칭 키 암호화](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external}를 기반으로 전체 디스크 암호화를 사용합니다. 피어 데이터를 보호하려면 자체 환경에서 유사한 단계를 수행해야 합니다.

LevelDB를 사용하는지 또는 CouchDB를 사용하는지 여부에 관계 없이 상태 데이터베이스의 데이터는 암호화되지 않습니다. 애플리케이션 레벨 암호화를 사용하여 상태 데이터베이스에 있는 저장 데이터를 보호할 수 있습니다.

<!--
### Data residency
{: #console-icp-about-security-data-residency}

Data residency requirements can mandate that the processing and storage of all blockchain ledger data remain within the border of a single country (or within some other defined boundary). For more information about how data residency can be accomplished, see [Data residency](#console-icp-about-data-residency).
-->

### 키 관리
{: #console-icp-about-security-key-management}

키 관리는 보안 측면에서 매우 중요한 요소입니다. 개인 키가 손상되거나 유실되는 경우 적대적인 액터가 데이터 및 기능에 액세스할 수도 있습니다. {{site.data.keyword.IBM_notm}}은 HSM([Hardware Security Modules](/docs/services/blockchain?topic=blockchain-glossary#glossary-hsm))이라는 물리적 어플라이언스를 사용하여 {{site.data.keyword.blockchainfull_notm}} Platform 엔터프라이즈 플랜 네트워크의 개인 키를 저장합니다.

개인 키 관리를 담당합니다. {{site.data.keyword.blockchainfull_notm}} Platform 콘솔 UI를 사용하여 개인 키를 생성할 수 있더라도 이러한 키는 {{site.data.keyword.cloud_notm}} Private 내에서나 콘솔에 의해 저장되지 않습니다. 키가 손상되지 않도록 안전하게 저장하는 것이 중요합니다.

Key Escrow를 사용하여 유실된 개인 키를 복구할 수 있습니다. 이 작업은 키 유실 전에 수행해야 합니다. 개인 키를 복구할 수 없으면 새 ID를 인증 기관에 등록하여 새 개인 키를 가져와야 합니다. 가입한 모든 채널에서 signCert를 제거하고 바꿔야 합니다.

### TLS
{: #console-icp-about-security-tls}

TLS([Transport Layer Security](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external})는 Hyperledger Fabric의 신뢰 모델에 임베드됩니다. 모든 {{site.data.keyword.blockchainfull_notm}} Platform 컴포넌트는 TLS를 사용하여 서로 인증 및 통신합니다. 따라서 {{site.data.keyword.cloud_notm}} Private의 노드는 다른 컴포넌트 및 애플리케이션과의 TLS 핸드쉐이크를 완료할 수 있어야 합니다. 이로 인한 영향 중 하나는 예를 들면 클라이언트 앱에서 노드로의 방화벽에서 화이트리스트를 사용하여 패스스루를 사용으로 설정해야 한다는 것입니다.

### 애플리케이션 보안
{: #console-icp-about-security-appl}

모든 체인코드 호출에 서명되었으므로 Fabric에서 애플리케이션 보안을 관리합니다. Fabric에는 ACL 기반 애플리케이션 레벨 검사도 포함됩니다.

## 지원 받기
{: #console-icp-about-support}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}에 대한 지원을 받는 방법과 문제점을 해결하는 데 사용할 수 있는 무료 블록체인 개발자 리소스 및 지원 포럼에 대한 자세한 정보는 [지원 받기](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support)를 참조하십시오.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 라이센스를 구매한 사용자가 고객 지원 센터에 문의하려면 [{{site.data.keyword.IBM_notm}} 지원 커뮤니티 액세스 및 지원 티켓 열기](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}에 대한 정보를 참조하십시오.
