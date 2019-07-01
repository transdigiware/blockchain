---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: HA, highly availability, multiregion

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

# 다중 영역 고가용성(HA) 배치 설정
{: #ibp-console-hadr-mr}

다중 영역 HA 구성은 가능한 가장 높은 정도의 HA 적용 범위를 제공합니다. 여러 지리적 영역에 피어를 배치하면 한 영역이 사용 불가능해질 경우 다른 영역의 피어가 계속해서 트랜잭션을 수행할 수 있게 됩니다. CA에 대한 다중 영역 HA 지원과 순서 지정 서비스는 현재 사용할 수 없음에 유의하십시오. 

## 개요
{: #ibp-console-hadr-overview}

피어에 대한 다중 영역 HA 지원을 설정할 경우 다음 태스크를 수행합니다. 
- Kubernetes 클러스터에 각각 바인드되는 {{site.data.keyword.cloud}}의 다중 서비스 인스턴스를 다른 영역에 작성하십시오. 
- 블록체인 노드를 다른 영역에 작성하십시오. 
- 노드 내보내기/가져오기 기능을 사용하여 단일 콘솔에서 노드를 작성하십시오. 

## 구성 단계
{: #ibp-console-hadr-config}

각 조직에 대해 중복 피어를 작성하여 다중 영역 HA를 구성하려면 블록체인 네트워크를 구성할 때 다음 단계를 완료하십시오. 

1. 선호하는 영역에 3개의 {{site.data.keyword.cloud_notm}} Kubernetes 클러스터를 작성하십시오. 고성능을 위해 상대적으로 근접해야 하지만 이러한 클러스터를 원하는 영역에 위치시킬 수 있습니다. 예를 들어 미국 동해안, 미국 서해안 및 캐나다 영역 설정이 미국 서해안, 런던 및 도쿄 영역 설정보다 좋습니다. 
2. 새 {{site.data.keyword.blockchainfull_notm}} Platform 인스턴스를 배치하고 이를 첫 번째 영역의 클러스터에 링크하십시오. 그런 후에 다른 {{site.data.keyword.blockchainfull_notm}} Platform 인스턴스를 배치하고 이를 두 번째 영역의 클러스터에 링크하십시오. 이러한 단계를 반복하여 세 번째 서비스 인스턴스를 세 번째 영역의 클러스터에 링크하십시오. 완료되면 각각 다른 영역에서 3개의 별도 콘솔과, 3개의 별도 클러스터에 링크된 3개의 별도 {{site.data.keyword.blockchainfull_notm}} Platform 인스턴스를 가지게 됩니다. 

이 튜토리얼에서는 순서 지정 서비스가 피어가 가입할 수 있다고 정의한 채널과 함께 있다고 가정합니다.
{: important}

### 1단계: 클러스터 1에서 피어 조직의 CA 및 메타데이터 작성
{: #ibp-console-hadr-peerCA}

1. [피어 조직의 CA 작성](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-CA-org1CA)에 대한 네트워크 튜토리얼 빌드에 있는 지시사항에 따라 첫 번째 클러스터에 CA를 배치하십시오. CA를 다른 클러스터로 가져올 때 CA **등록 ID** 및 **시크릿**의 값을 제공해야 하므로 이러한 값을 기록해 두십시오. 
2. CA 타일 상태 표시기가 녹색 `Running` 상태가 되면, [CA로 ID 등록](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1)의 지시사항을 따르십시오. 이러한 지시사항에서 2개의 ID를 작성하는데, 하나는 피어용이고 다른 하나는 피어 조직 관리자 ID용입니다. 
3. 첫 번째 클러스터의 피어 조직에 대한 [피어 조직 MSP 정의를 작성](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peers-org1)하십시오. 
4. 첫 클러스터에서 [피어를 작성](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-peer-create)하십시오. 
5. 피어에 [스마트 계약을 설치](/docs/services/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install)하십시오. 

채널에서 스마트 계약을 인스턴스화하려면 다음 단계에 따라 순서 지정 서비스에서 채널에 피어를 가입시켜야 합니다. 
- [피어의 조직 정의를 내보내고](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-org2-remote) 이를 순서 지정 서비스 관리자와 공유하십시오. 
- 순서 지정 서비스 관리자는 다음 단계에 따라 [피어 조직을 가져오고](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-msp) 이를 컨소시엄에 추가해야 합니다. 또한 이러한 지시사항의 단계를 완료하여 순서 지정 서비스를 JSON 파일로 내보내야 합니다. 
- [순서 지정 서비스 JSON 파일을 콘솔로 가져오십시오](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-orderer).
- [피어를 채널에 가입시키십시오](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2).
- 마지막으로 서비스 검색, 개인 데이터 및 피어 gossip을 사용하는 경우, 순서 지정 서비스 관리자는 채널에 대한 [앵커 피어를 구성](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers)해야 합니다. HA의 경우 앵커 피어로 각 중복 피어를 추가하는 것이 좋습니다. 피어 중 하나를 사용할 수 없는 경우, 조직 간의 gossip을 계속할 수 있습니다.    

피어를 채널에 가입시켰으므로 채널에서 [스마트 계약을 인스턴스화](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2)할 수 있습니다. 

### 2단계: 클러스터 1에서 메타데이터 및 ID 내보내기
{: #ibp-console-hadr-export-meta1}

1. CA 정의를 JSON 파일로 내보내십시오. 
   - **노드** 탭에서 CA를 여십시오.
   - 다운로드 아이콘을 클릭하여 브라우저 세션에서 CA JSON 파일을 생성하십시오. 
2. 피어의 조직 MSP 정의를 JSON 파일에 내보내십시오. 
   - **조직** 탭에서 MSP 정의로 이동하십시오.
   - 타일에서 다운로드 아이콘을 클릭하십시오. 
3. 지갑에서 피어 조직의 관리자 ID를 내보내십시오. 
   - **지갑** 탭으로 이동하십시오. 
   - 피어의 조직 관리자 ID를 클릭하고 **ID 내보내기**를 클릭하십시오.
   - 조직 관리자 인증서가 포함된 JSON 파일이 작성됩니다. 다른 클러스터에서 추가 피어를 작성할 때 필요하므로 파일 이름을 기록해 두고 파일을 보호하십시오.

### 3단계: 클러스터 2 및 3으로 메타데이터 및 ID 가져오기
{: #ibp-console-hadr-import-meta23}

1. 클러스터 2 및 3에서는 클러스터 1에서 내보낸 [CA JSON 파일을 가져오십시오](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-ca).  
2. JSON 파일을 업로드하고 나면 CA 관리자 등록 ID 및 시크릿, TLS CA 관리자 등록 ID 및 클러스터 1에서 CA를 배치할 때 사용한 시크릿을 입력해야 합니다. 
2. 클러스터 1에서 내보낸 피어 조직 MSP 정의 JSON 파일을 가져오십시오. 
   - **조직** 탭에서 **MSP 정의 가져오기**를 클릭하십시오.
   - **파일 추가**를 클릭하여 JSON 파일을 업로드하십시오. 
   - 다른 구역에서 새 피어를 작성할 때 이 MSP 정의를 사용하도록 허용하려면 **MSP 정의에 대한 관리자 ID 있음** 선택란을 클릭하십시오. 
   - **MSP 정의 가져오기**를 클릭하십시오.
3. 클러스터 1에서 가져온 조직 관리자 ID JSON 파일을 클러스터 2 및 3의 콘솔 지갑으로 가져오십시오. 

### 4단계: 클러스터 2 및 3에서 새 피어를 작성하고 채널에 가입
{: #ibp-console-hadr-create-new-peers}

1. 클러스터 1에서 피어 ID를 등록할 때 수행한 것과 동일한 단계에 따라 클러스터 2 및 3에서 CA를 사용하여 새 피어 사용자를 등록하십시오. 피어 사용자만 작성하면 되며, 다음 단계에서 조직 관리자 ID를 가져올 것이므로 이를 다시 작성할 필요는 없습니다. 
2. 클러스터 1에서 가져온 CA를 사용하고 피어 조직 MSP 정의로 클러스터 1에서 가져온 피어 조직 MSP를 사용하여 새 피어를 작성하십시오. 
3. 클러스터 2 및 3에서는 이제 클러스터 1의 피어와 동일한 채널에 피어를 가입시키기 위해 실행한 단계를 반복할 수 있습니다. 
4. 이러한 중복 피어에 스마트 계약을 설치하고 나면 모든 피어 간에 원장이 자동으로 동기화됩니다. 
5. 다시 말해 서비스 검색, 개인 데이터 및 피어 gossip을 사용하려는 경우, 각 중복 피어를 하나의 앵커 피어로 만들어야 합니다.   

이제 단일 영역의 실패가 피어 워크로드에 영향을 주지 않도록 네트워크가 구성됩니다.   

HA를 더욱 최대화하려면 다음 추가 옵션을 고려하십시오. 
- 클러스터 2 및 3에서 작성한 피어를 내보내고 이를 클러스터 1의 콘솔로 가져오십시오. 그러고 나면 모두 단일 클러스터에서 관리할 수 있습니다. 
- 그러나 클러스터 1이 실패할 수 있습니다. 따라서 추후의 클러스터 실패를 대비하여 모든 피어를 세 개의 콘솔 각각으로 가져올 수 있습니다. 따라서 하나의 콘솔이 포함된 클러스터가 실패하는 경우 다른 2개의 클러스터에 있는 콘솔에서 모두 관리할 수 있습니다. 
