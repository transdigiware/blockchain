---

copyright:
  years: 2019
lastupdated: "2019-07-10"

keywords: import nodes, another console, import a CA, import a peer, import admin identities, import an ordering service node

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

# 노드 가져오기
{: #ibp-console-import-nodes}

콘솔에는 다른 {{site.data.keyword.blockchainfull}} Platform 콘솔을 사용하여 작성된 노드를 가져오는 옵션이 포함되어 있습니다.
{:shortdesc}  

**대상 독자:** 이 주제는 블록체인 네트워크를 작성, 모니터링 및 관리할 책임이 있는 네트워크 운영자를 위해 설계되었습니다.

## 노드를 가져오는 이유
{: #ibp-console-import-nodes-why}

콘솔에 이미 존재하는 노드와 함께 작동시켜야 하는 경우 다른 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔에서 CA, 순서 지정 서비스 및 피어를 가져오십시오. 예를 들어, 콘솔 외부 조직의
피어를 콘솔 내의 채널에 가입시키려고 할 때 피어 가져오기 옵션을 사용할 수 있습니다. {{site.data.keyword.blockchainfull_notm}} Platform 서비스는 다중 환경에 배치되므로 일부 서비스 인스턴스에는 CA 및 피어만 포함되고 다른 서비스 인스턴스에서 순서 지정 서비스를 호스팅할 수 있습니다. 콘솔에
이종 노드를 가져오면 이러한 분산 컴포넌트를 하나의 사용자 인터페이스에서 보고 조작하여 블록체인에서 함께 트랜잭션할 수 있습니다.

노드를 콘솔로 가져오면 다음과 같은 강력한 운영 기능 세트를 사용할 수 있게 됩니다.
- 컨소시엄에 새 조직 추가
- 새 채널 작성
- 채널에 피어 가입
- 배치 위치에 상관없이 피어에 스마트 계약 설치
- 채널에서 스마트 계약 인스턴스화
- 채널에서 스마트 계약 업그레이드

노드를 가져올 때 해당 연결 정보를 제공해야 합니다. 컴포넌트를 가져올 때 실제 컴포넌트 자체를 가져오지는 않습니다.
대신 연결 정보를 사용하여 콘솔에서 작동할 수 있도록 콘솔 내에 컴포넌트의 표시를 빌드합니다. 마찬가지로, 가져온 노드를 콘솔에서 삭제할 경우 노드 자체는 배치된 위치에서 계속 실행되므로 삭제되지 않습니다. 대신, 노드를 가져온 노드에서만 제거됩니다.  

콘솔에 노드를 가져온 후 노드의 **설정** 탭을 사용하여 연결 정보를 수정할 수도 있습니다.

노드를 콘솔에 가져오기 전에 노드가 작성된 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔에서 노드를 내보내야 합니다. 네트워크 운영자는 단순히 노드 정보를 콘솔에서 `JSON` 파일로 내보낼 수 있습니다.
{: note}

## 제한사항
{: #ibp-console-import-limitations}

- 스타터 플랜 또는 엔터프라이즈 플랜 네트워크에서는 노드를 가져올 수 없습니다.
- 가져올 모든 노드는 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 사용하여 배치해야 합니다.
- 콘솔로 가져온 노드는 패치할 수 없습니다.
- 배치된 클러스터에서 콘솔에 가져온 노드를 삭제할 수 없습니다. 콘솔에서만 노드를 제거할 수 있습니다.
- {{site.data.keyword.cloud_notm}} Private에 배치된 노드를 가져오는 경우, 컴포넌트에서 사용되는 gRPC 웹 프록시 포트가 외부적으로 콘솔에 노출되었는지 확인해야 합니다. 자세한 정보는 [{{site.data.keyword.cloud_notm}} Private에서 노드 가져오기](#ibp-console-import-icp)를 참조하십시오.
- 가져온 노드의 타일을 열면 Fabric 버전이 표시되지 않고 **사용량 및 정보** 탭이 사용 불가능합니다. 

## 여기에서 시작: 인증서 또는 인증 정보 수집
{: #ibp-console-import-start-here}

다른 {{site.data.keyword.blockchainfull_notm}} Platform 서비스 인스턴스에서 기존의 블록체인 컴포넌트를 가져오려면 먼저 컴포넌트의 관리자 ID를 콘솔 지갑으로 가져와야 합니다. ID를
콘솔 지갑으로 가져오면 노드 작동이 간소화됩니다.  노드가 배치된 네트워크 운영자는 지갑의 노드 관리자 ID를 [가져올](#ibp-console-import-nodes-admin-identities) 수 있는 JSON 파일로 내보내야 합니다.  일반적으로, 노드 관리자 ID는 조직 MSP 정의를 작성할 때 지정된 피어 또는 순서 지정자 조직입니다. 또한 노드가 있는 콘솔의 지갑에 이미 있어야 합니다.

### 관리자 ID를 콘솔 지갑으로 가져오기
{: #ibp-console-import-nodes-admin-identities}

각 {{site.data.keyword.blockchainfull_notm}} Platform 컴포넌트는 내부 컴포넌트 관리자의 자체 서명 인증서, 서명 인증서와 함께 배치됩니다. 관리자가 컴포넌트에 대해 조치를 수행하는 경우 이 서명 인증서가 트랜잭션에 첨부되고 컴포넌트 구성에 대해 유효성이 검증됩니다. 관리자가 키를
사용하여, 즉, 새 사용자를 등록하고 새 채널을 작성하거나 피어에서 스마트 계약을 설치하여 컴포넌트를
작동시킬 수 있습니다. 콘솔을 사용하여 다른 콘솔을 통해 작성된 순서 지정 서비스 또는 피어를 작동하려면 연관된 관리자 ID를 콘솔 지갑으로 업로드해야 합니다. 그런 다음 가져온 노드를 콘솔 지갑의 ID와 연관시킬 수 있습니다. 이러한 ID는
작성된 콘솔에서 내보내야 하며 노드를 가져올 때 필요합니다.

새 ID를 가져오려면 **지갑** 탭을 연 후 **ID 추가**를 클릭하십시오. **JSON 업로드**를
클릭하여 네트워크 운영자가 노드가 작성된 곳에서 내보낸 JSON ID 파일을 찾아보십시오.

**ID 추가** 패널을 완료한 후에 제출을 클릭하면
지갑 개요 화면에서 새 관리자 ID를 볼 수 있습니다. 이제 CA, 피어 또는 순서 지정 서비스 컴포넌트를 가져올 때 해당 ID를 참조할 수 있습니다.

## CA 가져오기
{: #ibp-console-import-ca}

CA 노드는 모든 네트워크 엔티티(피어, 순서 지정 서비스, 클라이언트 등)가 통신 및 인증을 수행하고 궁극적으로 거래할 수 있도록 해당 엔티티에 인증서를 발행하는 블록체인 컴포넌트입니다. 각 조직에는 신뢰의 루트 역할을 수행하는 자체 CA가 있습니다. 블록체인 컨소시엄에 가입하거나 빌드하여
조직을 추가해야 합니다. [블록체인 컴포넌트 개요](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-ca)에서
CA에 대해 자세히 볼 수 있습니다.  

CA를 콘솔로 가져오고자 하는 몇 가지 이유가 있습니다. CA를 가져온 후에는 **사용자 등록**을 클릭하여 피어 조직에 더 많은 피어 사용자를 등록하는 데 사용할 수 있습니다. 또는 CA를 사용하여 피어 또는 순서 지정 서비스에 대해 추가 관리자 ID를 작성할 수 있습니다.

CA를 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔로 가져와서 작동시키려면 네트워크 운영자가 이미 CA가 배치된 {{site.data.keyword.blockchainfull_notm}} Platform에서 CA를 내보낸 상태여야 합니다. CA를
가져오면 새로운 사용자를 등록하고 [ID를 등록](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-enroll)할 수 있습니다.

### 시작하기 전에
{: #ibp-console-import-ca-before-you-begin}

- CA의 관리자 ID JSON 파일을 콘솔 지갑으로 가져왔는지 확인하거나, CA를 처음 배치할 때 지정된 CA 및 TLS CA에 대한 관리자 등록 ID 및 시크릿이 있는지 확인하십시오.
- 작성된 콘솔에서 내보낸 CA JSON 파일이 사용 가능한지 확인하십시오.

### CA를 가져오는 방법  
{: #ibp-console-import-nodes-howto-ca}

CA 가져오기는 **노드** 탭에서 수행됩니다.
1. **인증 기관 추가**, **기존 인증 기관 가져오기**를 차례로 클릭한 후 **다음**을 클릭하십시오.
2. **인증 기관 위치** 드롭 다운 목록에서 CA가 처음 배치된 위치를 선택하십시오.
3. **파일 추가**를 클릭하여 CA가 처음 배치된 콘솔에서 내보낸 CA JSON 파일을 업로드하십시오.
4. 다음 패널에 CA 배치 시 사용된 등록 ID와 시크릿을 입력할 수 있습니다.
5. 끝으로, CA 배치 시 사용된 TLS CA에 대한 ID와 시크릿을 입력할 수 있습니다. CA를 배치하면 CA와 TLS CA가 함께 배치됩니다. 네트워크 운영자는 두 CA에 대해 동일한 등록 ID와 시크릿을 사용하거나, CA 배치 시 CA 및 TLS CA에 대해 고유한 등록 ID와 시크릿을 지정할 수 있습니다.

CA를 콘솔에 가져온 후에 CA를 사용하여 새 ID를 작성하고 필요한 인증서를 생성하여 컴포넌트를 작동시키고 트랜잭션을 네트워크에
제출할 수 있습니다. 자세한 정보를 보려면 [인증 기관 관리](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-manage-ca)를 참조하십시오.

## 순서 지정 서비스 가져오기
{: #ibp-console-import-orderer}

순서 지정 서비스는 네트워크 구성원의 트랜잭션을 수집하고 트랜잭션의 순서를 지정하며 트랜잭션을 블록으로 번들화합니다. 순서 지정 서비스는 블록체인 컨소시엄의 공통 바인딩이며 다른 조직이 가입할 컨소시엄을 설립하는 경우에 배치되어야 합니다. [블록체인 컴포넌트 개요](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-orderer)에서 순서 지정 서비스에 대해 자세히 알아볼 수 있습니다.

순서 지정 서비스를 콘솔로 가져오면 피어에 대한 새 채널을 작성하여 개인적으로 거래할 수 있습니다.

### 시작하기 전에
{: #ibp-console-import-orderer-before-you-begin}

순서 지정 서비스를 가져오려면 먼저 다음 정보를 수집해야 합니다.

- [순서 지정 서비스의 관리자 ID JSON 파일](#ibp-console-import-nodes-admin-identities)을 콘솔 지갑으로 가져왔는지 확인하십시오.
- 작성된 콘솔에서 내보낸 순서 지정 서비스 JSON 파일이 사용 가능한지 확인하십시오.

### 순서 지정 서비스를 가져오는 방법
순서 지정 서비스 가져오기는 **노드** 탭에서 수행됩니다.
1. **순서 지정 서비스 추가**, **기존 순서 지정 서비스 가져오기**를 차례로 클릭한 후 **다음**을 클릭하십시오.
2. **인증 기관 위치** 드롭 다운 목록에서 순서 지정 서비스가 처음 배치된 위치를 선택하십시오.
3. **파일 추가**를 클릭하여 순서 지정 서비스가 처음 배치된 콘솔에서 내보낸 순서 지정 서비스 JSON 파일을 업로드하십시오. 5개 노드 Raft 순서 지정 서비스인 경우 파일의 모든 5개 순서 지정 서비스에 대한 연결 정보가 포함된 하나의 파일이 있어야 합니다.
4. **기존 ID**를 클릭한 후 콘솔 지갑으로 가져온 순서 지정 서비스 관리자 ID를 선택하여 순서 지정 서비스의 관리자 ID를 설정하십시오.

순서 지정 서비스를 콘솔로 가져온 후에 새 채널을 작성할 때 새 조직 구성원을 추가하고 순서 지정 서비스를 선택할 수 있습니다.

## 피어 가져오기
{: #ibp-console-import-peer}

피어 노드는 원장을 유지보수하고 원장에 대한 조회 및 업데이트 조작을 수행하기 위해 스마트 계약을 실행하는 블록체인 컴포넌트입니다. 조직
구성원은 피어를 소유하고 유지보수합니다.  컨소시엄에 가입하는 각 조직은 최소 한 개의 피어를 배치해야 하며, 고가용성(HA)의 경우 최소 두 개의 피어를 배치해야 합니다. [블록체인 컴포넌트 개요](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview-peer)에서
피어에 대해 자세히 볼 수 있습니다.

피어를 콘솔로 가져온 후에 피어에 스마트 계약을 설치하고 블록체인의 다른 채널에 피어를 가입시킬 수 있습니다.

**참고:** 피어 조직에 더 많은 피어를 추가하거나 피어에 대한 추가 관리자 ID를 작성해야 하는 경우,
피어의 CA를 가져와 해당 CA를 사용하여 조작을 수행해야 합니다.

### 시작하기 전에
{: #ibp-console-import-peer-before-you-begin}

피어를 가져오려면 먼저 다음 정보를 수집해야 합니다.

- [피어의 관리자 ID JSON 파일](#ibp-console-import-nodes-admin-identities)을 콘솔 지갑으로 가져왔는지 확인하십시오.
- 작성된 콘솔에서 내보낸 피어 JSON 파일이 사용 가능한지 확인하십시오.

### 피어를 가져오는 방법
{: #ibp-console-import-peer-howto}

피어 가져오기는 **노드** 탭에서 수행됩니다.
1. **피어 추가**, **기존 피어 가져오기**를 차례로 클릭한 후 **다음**을 클릭하십시오.
2. **인증 기관 위치** 드롭 다운 목록에서 피어가 처음 배치된 위치를 선택하십시오.
3. **파일 추가**를 클릭하여 피어가 처음 배치된 콘솔에서 내보낸 피어 JSON 파일을 업로드하십시오.
4. **기존 ID**를 클릭한 후 콘솔 지갑으로 가져온 피어 관리자 ID를 선택하여 피어의 관리자 ID를 설정하십시오.  

피어를 콘솔로 가져온 후에 피어에 스마트 계약을 설치하고 블록체인의 채널에 피어를 가입시킬 수 있습니다.

## 조직 MSP 정의 가져오기
{: #ibp-console-import-msp}

MSP가 다른 {{site.data.keyword.blockchainfull_notm}} Platform 서비스 인스턴스 콘솔을 사용하여 배치되었으며 MSP 정의를 사용하는 채널을 작성하거나 업데이트하려는 경우 조직 MSP 정의를 가져와야 합니다. 또한 다른 콘솔에 배치된 조직 MSP에 속하는 피어 또는 순서 지정 서비스를 작성하려는 경우 MSP 및 연관된 MSP 관리자 ID를 가져와야 합니다.  MSP를 작성한 네트워크 운영자는 조직 MSP 정의와 해당 관리자 ID를 JSON 파일로 내보내서 사용자와 공유해야 합니다.

- 조직에 속하는 피어 또는 순서 지정 서비스를 작성하려는 경우 연관된 MSP 관리자 ID를 지갑으로 가져오십시오.
- 조직 MSP 정의 가져오기는 **조직** 탭에서 수행됩니다.
- **MSP 정의 가져오기**를 클릭하여 JSON 파일을 업로드하십시오.
- (선택사항) MSP 관리자 ID를 지갑으로 가져온 경우 `MSP 정의에 대한 관리자 ID 있음` 선택란을 선택하십시오. 이 옵션을 선택하지 않으면 나중에 피어 또는 순서 지정 서비스를 작성하려고 할 때 이 조직의 MSP 정의가 MSP 드롭 다운 목록에 나열되지 않습니다.

## {{site.data.keyword.cloud_notm}} Private에서 노드 가져오기
{: #ibp-console-import-icp}

{{site.data.keyword.cloud_notm}} Private에 작성된 노드를 다른 {{site.data.keyword.cloud_notm}} Private 클러스터 또는 {{site.data.keyword.cloud_notm}}에 배치된 콘솔로 가져올 수 있습니다. 그러나 노드의 gRPC URL에서 사용된 포트가 클러스터 외부에서 노출되었는지 확인해야 합니다. 방화벽 뒤에 {{site.data.keyword.cloud_notm}} Private를 배치하는 경우, 노드와 통신하도록 클러스터 외부의 콘솔을 허용하려면 화이트리스트를 사용하여 패스스루를 사용으로 설정해야 합니다.

예를 들어 아래의 {{site.data.keyword.cloud_notm}} Private에서 내보낸 피어의 JSON 파일을 찾을 수 있습니다. 다른 콘솔에서 피어와 통신하려면 `grpcwp_url` 포트(이 예에서는 포트 32403)가 외부 트래픽에 열려 있는지 확인해야 합니다.

```
{
    "name": "peer",
    "grpcwp_url": "https://9.30.252.107:32403", \\ensure that port 32403 is externally exposed
    "api_url": "grpcs://9.30.252.107:30891",
    "operations_url": "https://9.30.252.107:30222",
    "type": "fabric-peer",
    "msp_id": "org1msp",
    "pem": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWI2Z0F3SUJBZ0lVUi9zMGxGTG5ZNmdWRmV1Mlg5ajkrY3JDZFBrd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEEyTVRBeE9USXhNREJhRncwek5EQTJNRFl4T1RJeE1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFUYUtyN2srUHNYeXFkWkdXUHlJUXlGMGQxUkFFdmdCYlpkVnlsc3hReWZOcUdZS0FZV3A0SFUKVUVaVHVVNmtiRXN5Qi9aOVJQWEY0WVNGbW8reTVmSkhvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVUrcnBNb2dRc3dDTnZMQzJKNmp2cElQOExwaE13CkZRWURWUjBSQkE0d0RJY0VDUjc4YTRjRXJCRE5DakFLQmdncWhrak9QUVFEQWdOSEFEQkVBaUJGWmpMWU9XZUMKLy92L2RNMHdYNUxZT3NCaHFFNnNQZ1BSWWppOTZqT093QUlnZEppZDU0WmxjR2h0R3dEY3ZoZE02RVlBVFpQNwpmS29IMDZ3ZFhpK3VzVXM9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
}
```
