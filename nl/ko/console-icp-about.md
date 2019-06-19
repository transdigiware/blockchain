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

{{site.data.keyword.blockchainfull}} Platform은 퍼블릭 및 프라이빗 클라우드(예: {{site.data.keyword.cloud_notm}}), 자체 데이터 센터 및 서드파티 퍼블릭 클라우드에 배치할 수 있습니다. 이 멀티클라우드 배치는 IBM의 Kubernetes 기반 컨테이너 오케스트레이션 플랫폼인 {{site.data.keyword.cloud_notm}} Private을 통해 가능합니다. 이 릴리스는 {{site.data.keyword.cloud_notm}} Private 클러스터에서 블록체인 컴포넌트를 배치 및 관리하는 데 사용할 수 있는 콘솔 사용자 인터페이스를 제공합니다. {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private은 Hyperledger Fabric v1.4.1 코드 베이스를 사용하며 {{site.data.keyword.cloud_notm}} Private v3.2에 대한 배치를 지원합니다.
{:shortdesc}

이 {{site.data.keyword.blockchainfull_notm}} Platform 릴리스에는 다음과 같은 주요 기능이 포함되어 있습니다.

**빌드 ---- 통합된 개발자 경험**
- 스마트 계약을 Node.js, Golang 또는 Java로 **쉽게 코딩**하고, 새로운 {{site.data.keyword.blockchainfull_notm}} VS Code 확장을 사용하여 클라이언트 애플리케이션을 작성하며, 사용자 인터페이스 콘솔과의 **SDK 통합**을 활용하고, 다양한 튜토리얼 및 샘플에서 학습할 수 있습니다. 
- **단순화된 DevOps**를 사용하면 더 많은 컴포넌트를 추가할 수 있도록 Kubernetes 자원을 스케일링하여 개발에서 테스트까지 단일 환경의 프로덕션으로 이동할 수 있습니다.
- **Kubernetes 서비스 통합.** 모니터링을 위해 로깅 및 Kibana에 대한 서비스(예: Grafana 및 Prometheus)를 활용합니다. 
- **최신 Fabric 주요 기능**. Hyperledger Fabric v1.4.1의 최신 기능을 활용합니다.
  - [Raft 순서 지정 서비스](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [개인용 데이터 콜렉션](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data) - gossip 프로토콜을 통해 권한 부여된 피어만 원장 데이터를 공유할 수 있도록 하여 강화된 데이터 개인정보 보호를 제공합니다. 
  - [서비스 발견](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} - 애플리케이션이 네트워크와 상호작용하는 방식을 동적으로 검색하고 업데이트할 수 있습니다. 
  - [채널 액세스 제어 목록 ](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} - 채널 및 스마트 계약을 추가적으로 통제할 수 있습니다. 

**조작 --- 종합적인 배치 제어**
- **필요한 컴포넌트만 배치.**. 피어를 여러 채널 및 네트워크에 연결하거나 비즈니스 파트너가 연결할 수 있는 순서 지정 서비스를 호스팅합니다.
- **ID 완전 제어 유지**. 사용자 고유의 보안 환경에서 노드를 관리하는 데 사용되는 키를 저장하고 관리합니다. 
- **통합 운영**. {{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 사용하면 {{site.data.keyword.IBM_notm}} 또는 기타 공급업체에 의존하여 순서 지정자 또는 인증 기관을 관리할 필요 없이 **하나의 콘솔**에서 모든 조직과 노드를 배치하고 관리할 수 있습니다. 또한 콘솔에서 블록체인 컨소시엄에 대해 구성원을 추가하거나 제거할 수 있으며 채널을 작성하고 가입할 수 있으며 스마트 계약을 설치하고 인스턴스화할 수 있습니다.
- **네트워크 호스팅 또는 가입**. 조직이 인프라와 독립적으로 노드를 관리하는 동안 클러스터에서 호스팅되는 피어를 다중 클라우드의 여러 채널에 배치하거나 사용자의 컨소시엄 또는 채널에 가입하도록 기타 조직을 초대할 수 있습니다.
- 노드를 관리 또는 모니터링할 수 있는 사용자의 **액세스를 관리**합니다.
- Kubernetes Service에서 노드 **로그에 대한 직접 액세스**. 로그를 추출하고 분석하려면 지원되는 서드파티 서비스를 사용하십시오. 
- Kubernetes Service를 사용하여 **노드 팟(Pod)과 직접 상호작용**. 
- 채널 구성에 대한 협업 통제를 더 효과적으로 제어할 수 있도록 하는 **동적 서명 콜렉션**

**성장 --- 확장성 및 유연성**
- **컴퓨팅 선택**. CPU, 메모리 및 Kubernetes 클러스터에서 프로비저닝할 스토리지의 양을 유연하게 결정할 수 있습니다. 자세한 정보는 [콘솔이 Kubernetes 클러스터와 상호작용하는 방법](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction)을 참조하십시오. 
- 필요한 만큼만 비용을 지불하고 Kubernetes 클러스터 내의 자원 **규모를 확장 및 축소**할 수 있습니다. 자세한 정보는 [가격](/docs/services/blockchain/howto/pricing.html#ibp-pricing)을 참조하십시오.
- **재해 복구 및 다중 구역 고가용성**. 이 옵션은 여러 구역에 걸쳐 Kubernetes 배치를 복제하므로 컴포넌트의 고가용성(HA) 및 재해 복구(DR)를 사용할 수 있습니다. 
- **어디서나 실행**. {{site.data.keyword.blockchainfull_notm}} Platform 콘솔의 **통합 코드 베이스**를 통해 {{site.data.keyword.cloud_notm}} Private에서 지원하는 모든 환경에서 컴포넌트를 실행할 수 있습니다.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private은 {{site.data.keyword.cloud_notm}} Private의 번들화된 제품으로, Kubernetes Helm 차트로 제공됩니다. {{site.data.keyword.cloud_notm}} Private에 대한 자세한 정보는 [{{site.data.keyword.cloud_notm}} Private 버전 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}용 문서를 참조하십시오. 

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private에서 제공하는 기능

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private을 사용하면 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 {{site.data.keyword.cloud_notm}} Private 배치에 설치할 수 있습니다. 그런 다음 콘솔에서 Hyperledger Fabric 블록체인의 모든 기본 컴포넌트(인증 기관, 순서 지정 서비스 및 피어)를 로컬 클러스터에 배치할 수 있습니다. Hyperledger Fabric 네트워크의 구성 요소에 대한 자세한 정보는 [블록체인 컴포넌트 개요](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview)를 참조하십시오. 콘솔에서 다른 {{site.data.keyword.cloud_notm}} Private 클러스터 또는 {{site.data.keyword.cloud_notm}}에 배치된 노드를 가져와서 분산 멀티클라우드 네트워크를 운영할 수도 있습니다. 

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private의 적합성

{{site.data.keyword.blockchainfull_notm}} Platform을 {{site.data.keyword.cloud_notm}} 외부에서 실행할 경우 블록체인 네트워크에 가입하거나 해당 네트워크를 확장할 수 있는 유연성이 제공됩니다. 이를 통해 네트워크 시작자는 자신이 선택한 플랫폼을 사용하면서 새 구성원이 가입할 수 있도록 허용하여 자신의 네트워크를 확장할 수 있습니다. 이 경우 블록체인 네트워크에 가입하려는 조직에서 해당 피어를 기존 애플리케이션과 함께 배치하거나 SOR(System of Record)에 통합할 수 있습니다.

이 오퍼링의 사용자는 자체 보안 및 인프라를 관리하게 됩니다. {{site.data.keyword.cloud_notm}}에서는 해당 서비스를 제공하지 않습니다. 시작하기 전에 다음 절에 있는 [고려사항 및 제한사항](#console-icp-about-considerations)을 검토하십시오.

## 고려사항 및 제한사항
{: #console-icp-about-considerations}

시작하기 전에 다음 제한사항을 이해해야 합니다. 

- 블록체인 컴포넌트의 상태 모니터링, 보안, 로깅 및 리소스 사용량 관리 작업은 사용자가 수행해야 합니다. 
- 콘솔에서는 Hyperledger Fabric v1.4.1 이상을 기반으로 하는 컴포넌트를 작성하고 제어할 수만 있습니다. 
- 다른 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔에서 내보낸 노드만 가져올 수 있습니다. 가져온 피어 또는 순서 지정자가 콘솔에서 작동하려면 연관된 노드의 조직 MSP 정의와 관리자 ID도 콘솔로 가져와야 합니다. 

{{site.data.keyword.cloud_notm}} Private에서 지원하는 환경을 검토하려면 [지원되는 환경](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}을 참조하십시오.
{:important}

## 시스템 전제조건
{: #console-icp-about-prerequisites}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private에서는 다음과 같은 운영 체제를 지원합니다.
- RHEL(Red Hat Enterprise Linux) 7.3, 7.4, 7.5
- Ubuntu 18.04 LTS 및 16.04 LTS
- SLES(SUSE Linux Enterprise Server) 12 SP3

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm 차트는 다음과 같은 작업자 노드 및 백업 스토리지를 사용하여 Ubuntu Linux 기반 {{site.data.keyword.cloud_notm}} Private v3.2 클러스터에서 실행되도록 유효성 검증되었습니다. 

- **LinuxONE 및 {{site.data.keyword.IBM_notm}} Z**: z/VM 및 KVM(NFS 사용)
- **x86**: Linux 64비트(GlusterFS 사용)

## 라이센스
{: #console-icp-about-license}

라이센스 부여 및 가격 책정에 대한 자세한 정보는 [{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud의 가격](/docs/services/blockchain?topic=blockchain-ibp-software-pricing)을 참조하십시오. 

## {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 설치
{: #console-icp-about-install}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private은 로컬 {{site.data.keyword.cloud_notm}} Private 클러스터에 설치될 수 있는 Helm 차트 파일로 제공됩니다. Helm 차트를 설치한 후에는 {{site.data.keyword.cloud_notm}} Private 카탈로그에서 애플리케이션으로 {{site.data.keyword.blockchainfull_notm}} Platform을 찾을 수 있습니다. Helm 차트 및 필요한 전제조건을 설치하는 방법에 대한 지시사항은 [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 설치](/docs/services/blockchain/howto/console-helm-install.html#helm-console-install)를 참조하십시오. 

{{site.data.keyword.cloud_notm}} Private의 신규 사용자이며 {{site.data.keyword.cloud_notm}} Private 설치 및 배치에 대한 정보와 팁이 필요한 경우 [{{site.data.keyword.cloud_notm}} Private 설정](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup)을 참조하십시오. 

공용 인터넷에 액세스하지 않고 {{site.data.keyword.blockchainfull_notm}} Platform을 방화벽 뒤에 배치할 수 있습니다. 다운로드된 Helm 차트 패키지에는 배치 중에 DockerHub에서 가져오지 않고 {{site.data.keyword.blockchainfull_notm}} Platform에서 사용하는 모든 Fabric 컴포넌트 Docker 이미지가 포함되어 있습니다.

## 보안 고려사항
{: #console-icp-about-security}

이러한 컴포넌트는 자체 인프라에 배치되기 때문에 사용자가 해당 보안을 관리해야 합니다. 여기에는 키 관리 및 데이터 암호화 등의 중요한 보안 영역이 포함되어 있습니다. 컴포넌트에 대한 보안을 고려할 때 다음 주제를 검토하십시오.

### 데이터 보안
{: #console-icp-about-security-data}

{{site.data.keyword.blockchainfull_notm}} Platform 스타터 및 엔터프라이즈 플랜은 [대칭 키 암호화](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external}를 기반으로 하는 전체 디스크 암호화를 사용하여 네트워크에서 사용되는 모든 데이터를 보호합니다. 피어 데이터를 보호하려면 자체 환경에서 유사한 단계를 수행해야 합니다.

LevelDB를 사용하는지 또는 CouchDB를 사용하는지 여부에 관계 없이 상태 데이터베이스의 데이터는 암호화되지 않습니다. 애플리케이션 레벨 암호화를 사용하여 상태 데이터베이스에 있는 저장 데이터를 보호할 수 있습니다.

### 데이터 상주
{: #console-icp-about-security-data-residency}

데이터 상주 요구사항으로 인해 모든 블록체인 원장 데이터의 처리 및 저장이 단일 국가의 경계 내에서(또는 정의되어 있는 일부 다른 경계 내에서) 수행되어야 할 수도 있습니다. 데이터 상주를 수행하는 방법에 대한 자세한 정보는 [데이터 상주](#console-icp-about-data-residency)를 참조하십시오.

### 키 관리
{: #console-icp-about-security-key-management}

키 관리는 보안 측면에서 매우 중요한 요소입니다. 개인 키가 손상되거나 유실되는 경우 적대적인 액터가 데이터 및 기능에 액세스할 수도 있습니다. {{site.data.keyword.IBM_notm}}은 HSM([Hardware Security Modules](/docs/services/blockchain/glossary.html#glossary-hsm))이라는 물리적 어플라이언스를 사용하여 {{site.data.keyword.blockchainfull_notm}} Platform 엔터프라이즈 플랜 네트워크의 개인 키를 저장합니다. 

{{site.data.keyword.cloud_notm}} Private에 컴포넌트를 배치하는 경우 사용자가 개인 키를 관리해야 합니다. {{site.data.keyword.blockchainfull_notm}} Platform에서 개인 키를 생성하지만 해당 키는 Platform에 저장되지 않습니다. 키가 손상되지 않도록 안전하게 저장하는 것이 중요합니다. 컴포넌트의 개인 키는 피어 MSP의 키 저장소 폴더(컴포넌트 내에 있는 `/mnt/crypto/peer/peer/msp/keystore/` 디렉토리)에 있습니다. 피어 내에 있는 인증서에 대한 자세한 정보를 확인하려면 [{{site.data.keyword.blockchainfull_notm}} Platform에서 인증서 관리](/docs/services/blockchain/certificates.html#managing-certificates) 튜토리얼의 [MSP(Membership Services Provider)](/docs/services/blockchain/certificates.html#managing-certificates-msp) 절을 참조하십시오.

Key Escrow를 사용하여 유실된 개인 키를 복구할 수 있습니다. 이 작업은 키 유실 전에 수행해야 합니다. 개인 키를 복구할 수 없으면 새 ID를 인증 기관에 등록하여 새 개인 키를 가져와야 합니다. 가입한 모든 채널에서 signCert를 제거하고 바꿔야 합니다.

### TLS
{: #console-icp-about-security-tls}

TLS([Transport Layer Security](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external})는 Hyperledger Fabric의 신뢰 모델에 임베드됩니다. {{site.data.keyword.blockchainfull_notm}} Platform의 모든 스타터 및 엔터프라이즈 컴포넌트는 TLS를 사용하여 서로 인증하고 통신합니다. 따라서 Platform의 네트워크 컴포넌트는 피어와의 TLS 핸드쉐이크를 완료할 수 있어야 합니다. 이로 인한 영향 중 하나는 예를 들면 클라이언트 앱에서 피어로의 방화벽에서 화이트리스트를 사용하여 패스스루를 사용으로 설정해야 한다는 것입니다.

### Membership Services Provider 구성
{: #console-icp-about-security-MSP}

{{site.data.keyword.blockchainfull_notm}} Platform의 컴포넌트에서는 MSP(Membership Services Provider)를 통해 ID를 이용합니다. MSP는 CA에서 발행하는 인증서를 네트워크 및 채널 역할과 연관시킵니다. 피어에서 MSP가 작동하는 방법에 대한 자세한 정보는 [MSP(Membership Service Provider)](/docs/services/blockchain/certificates.html#managing-certificates-msp)를 참조하십시오.

### 애플리케이션 보안
{: #console-icp-about-security-appl}

모든 체인코드 호출에 서명되었으므로 Fabric에서 애플리케이션 보안을 관리합니다. Fabric에는 ACL 기반 애플리케이션 레벨 검사도 포함됩니다.

## 데이터 상주
{: #console-icp-about-data-residency}

블록체인 네트워크는 처리되는 데이터 유형을 감지하지 못하기 때문에 특정 종류의 데이터를 보호하기 위해 때로는 추가 단계를 수행해야 합니다. 데이터 상주의 가장 일반적인 요구사항은 특정 국가의 법률과 관련되어 있습니다. 해당 법률에 따르면 IT 시스템에서 처리하고 저장하는 모든 데이터는 특정 국가 내에 유지해야 합니다. 마찬가지로 정부, 보건, 금융 서비스와 같이 고도로 규제된 일부 업계에서는 데이터를 완전히 방화벽 뒤에 보관해야 합니다. 따라서 데이터 상주를 수행하려면 블록체인 네트워크의 모든 컴포넌트가 동일한 [채널](/docs/services/blockchain/glossary.html#glossary-channel)의 일부여야 하며 한 국가 내에 있어야 합니다.

데이터 상주 요구사항을 처리하려면 {{site.data.keyword.blockchainfull_notm}} Platform의 기초가 되는 Hyperledger Fabric 아키텍처를 이해하는 것이 중요합니다. 이 아키텍처는 순서 지정 서비스(순서 지정자로 구성됨), 인증 기관(CA) 및 피어라는 세 가지 핵심 컴포넌트를 중심으로 집중되어 있습니다. 피어에서 순서 지정 서비스에서 순서 지정된 상태 업데이트를 블록의 양식으로 받고 해당 상태와 원장을 유지보수합니다. 따라서 피어와 순서 지정 서비스는 직접적인 관계가 있습니다. 원장에는 트랜잭션 로그에 포함된 모든 키와 데이터의 최신 값이 들어 있습니다.

또한 클라이언트 애플리케이션에서는 [Fabric SDK](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external}를 사용하여 트랜잭션을 피어와 순서 지정 서비스에 보냅니다. 이러한 트랜잭션에는 원장에 키-값 쌍을 포함하는 [읽기-쓰기 세트](https://hyperledger-fabric.readthedocs.io/en/release-1.4/readwrite.html){: external} 데이터가 포함되어 있습니다. 

국가 내 데이터 상주가 요구사항인 경우 순서 지정자, 피어 및 클라이언트 애플리케이션이 동일한 국가 내에 상주해야 합니다. {{site.data.keyword.blockchainfull_notm}} Platform 네트워크가 {{site.data.keyword.cloud_notm}}에 작성되면 네트워크의 위치를 선택할 수 있습니다. <!--For a Starter Plan network, you can select from US South, United Kingdom, and Sydney. For an Enterprise Plan network, you can select from currently available locations, which include Dallas, Frankfurt, London, Sao Paulo, Tokyo, and Toronto. -->지역과 위치에 관한 자세한 정보는 [{{site.data.keyword.blockchainfull_notm}} Platform 지역 및 위치](/docs/services/blockchain/reference/ibp_regions.html#ibp-regions-locations)를 참조하십시오.

### 데이터 상주에 대한 유스 케이스

네 개 조직의 컨소시엄과 함께 순서 지정자 및 CA가 포함되어 있는 {{site.data.keyword.blockchainfull_notm}} Platform 네트워크를 고려해 보십시오. 조직에는 하나 이상의 피어 노드가 포함되어 있고, 네 개의 조직이 모두 단일 채널에 속해 있으며, 네트워크의 모든 컴포넌트가 {{site.data.keyword.blockchainfull_notm}} Platform 네트워크가 배치된 지역(예: 프랑크프루트)에 상주합니다. 마지막으로 피어와 상호작용하는 클라이언트 애플리케이션도 독일 내에 상주합니다.

이 예제에서는 데이터 상주가 유지됩니다.

![모든 컴포넌트가 동일한 국가 내에 있는 경우 데이터 상주](images/remote_peer_data_res_1.png "모든 컴포넌트가 동일한 국가 내에 있는 경우 데이터 상주")

이제 **분산 피어**가 해당 조직 중 하나에 가입하는 경우 그 영향에 대해 고려해 보십시오. 분산 피어는 나머지 네트워크와 동일한 지역에 상주할 수도 있고 {{site.data.keyword.blockchainfull_notm}} Platform 네트워크 지역 외부의 임의 위치에 상주할 수도 있습니다.

-	피어가 나머지 네트워크와 같은 국가에 상주하는 경우 데이터 상주는 유지보수됩니다. 모든 원장 데이터는 상기 **그림 1**과 같이 독일 내에서 유지됩니다.
-	피어가 다른 국가(예를 들어, 미국과 같은)에 상주하는 경우 피어 원장의 데이터가 국경 밖에서 공유되기 때문에 데이터 상주가 더 이상 유지보수되지 않습니다.

이 문제점을 해결하려면 **채널**을 사용하여 네트워크의 피어 서브세트로 데이터를 분리할 수 있습니다. {{site.data.keyword.blockchainfull_notm}} Platform 네트워크에 국가 경계에 걸친 분산 피어 및 순서 지정자가 포함되어 있는 경우 채널을 통해 원장 데이터를 국가 경계 외부에 있는 피어를 사용하는 조직으로부터 격리할 수 있습니다.

**참고:** 순서 지정자는 항상 네트워크를 호스팅하도록 선택한 데이터 센터 지역 내에 있습니다. 복수의 순서 지정자가 국경을 넘어 존재할 수 없습니다. 하지만 피어는 데이터 센터 내에 있거나 {{site.data.keyword.cloud_notm}} 외부의 원격 위치에 있을 수 있습니다.

![피어가 {{site.data.keyword.blockchainfull_notm}} Platform 지역의 국가 외부에 상주하는 경우 데이터 상주](images/remote_peer_data_res_2.png "피어가 {{site.data.keyword.blockchainfull_notm}} Platform 지역의 국가 외부에 상주하는 경우 데이터 상주")

**그림 2**에서 `OrgC` 및 `OrgD`의 경우 데이터 상주가 필요하지 않습니다. 실제로 현재 `OrgD`에는 *미국*에 상주하는 두 개의 분산 피어(`OrgD-peer1` 및 `OrgD-peer2`)가 포함되어 있습니다. 따라서 독일에 상주하는 `OrgA`, `OrgB`와 각각의 해당 클라이언트 애플리케이션 및 피어에서 원장 데이터를 채널 `X`에 격리하기 위해 `OrgC` 및 `OrgD`를 위한 새 채널 `Y`가 작성됩니다.

{{site.data.keyword.blockchainfull_notm}} Platform 네트워크의 데이터 플로우에 대한 자세한 정보는 [트랜잭션 플로우에 대한 Fabric 문서](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}를 참조하십시오. 

향후에는 Hyperledger Fabric의 새로운 기술을 통해 [개인용 데이터 콜렉션](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html){: external} 및 제로 지식 증명을 사용하여 추가적인 데이터 상주를 달성하도록 기능을 개선할 예정입니다. 

- 개인용 데이터 콜렉션을 사용하면 인증된 피어(예: 한 국가에 있는 피어)만 볼 수 있도록 개인용 데이터를 피어 투 피어로 공유할 수 있습니다(gossip 프로토콜 사용). 데이터는 피어의 개인 데이터베이스에 저장됩니다. 순서 지정 서비스는 여기에 관련되지 않으며 개인용 데이터를 볼 수 없습니다. 해당 데이터의 해시가 채널에 있는 모든 피어의 원장에 기록됩니다. 상태 유효성 검증에 사용된 해시는 트랜잭션의 증거로 사용되며 감사 용도로 사용할 수 있습니다. 개인용 데이터는 Fabric 버전 1.2.1 이상에서 실행되는 {{site.data.keyword.blockchainfull_notm}} Platform의 네트워크에 사용 가능합니다. 그러나 {{site.data.keyword.cloud_notm}} Private에서 실행 중인 피어의 경우 현재는 {{site.data.keyword.cloud_notm}} Private 네트워크에서 gossip을 사용하지 않기 때문에 개인용 데이터 기능을 사용할 수 없습니다.

- 제로 지식 증명(ZKP)을 사용하는 경우 “증명자”가 시크릿 자체를 노출하지 않고도 “확인자”에게 시크릿을 알고 있음을 보증할 수 있습니다.

이 기술에 대한 자세한 정보는 [Hyperledger Fabric을 사용하는 개인 및 기밀 트랜잭션](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}에 대한 백서에서 확인할 수 있습니다. 

## 지원 받기
{: #console-icp-about-support}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}에 대한 지원을 받는 방법과 문제점을 해결하는 데 사용할 수 있는 무료 블록체인 개발자 리소스 및 지원 포럼에 대한 자세한 정보는 [지원 받기](/docs/services/blockchain/ibmblockchain_support.html#blockchain-support)를 참조하십시오.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 라이센스를 구매한 사용자가 고객 지원 센터에 문의하려면 [{{site.data.keyword.IBM_notm}} 지원 커뮤니티 액세스 및 지원 티켓 열기](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}에 대한 정보를 참조하십시오. 
