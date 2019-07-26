---

copyright:
  years: 2019
lastupdated: "2019-07-10"

keywords: release note, latest changes, Hyperledger Fabric

subcollection: blockchain

---

{:note: .note}
{:important: .important}
{:tip: .tip}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:external: target="_blank" .external}

# 릴리스 정보
{: #release-notes-saas-20}

이 릴리스 정보(날짜로 그룹화됨)를 사용하여 Hyperledger Fabric v1.4.1을 기반으로 빌드된 {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}}의 최신 변경사항에 대해 알아보십시오.
{:shortdesc}

## 2019년 7월 10일
{: #07-10-2019}

**순서 지정 노드 패치`1.4.1-2`**  

이 패치는 순서 지정 노드가 다시 시작할 때 발생하는 수정사항을 수정합니다. 최초 블록은 순서 지정 노드가 순서 지정 서비스의 다른 노드와 통신하지 못하도록 업데이트됩니다. 이 [지시사항](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-patch)에 따라 순서 지정 서비스의 모든 기존 순서 지정 노드에 이 패치를 적용하여 각 순서 지정 노드를 업데이트할 수 있습니다. 작성하는 모든 새 순서 지정 노드에는 자동으로 이 수정사항이 포함됩니다. 

**앵커 피어 제거**  

이제 채널에서 앵커 피어를 제거할 수 있는 기능이 제공됩니다. 

**기타 버그 수정**  

버그 수정 및 번역 업데이트입니다. 

## 2019년 5월 24일
{: #05-24-2019}

**Raft 합의 프로토콜**  

: 프로덕션 네트워크에 권장되는 5개 노드 Raft 순서 지정 서비스를 이제 사용할 수 있습니다. 또한 개발 및 테스트 목적으로 단일 노드 Raft 순서 지정 서비스를 배치할 수 있습니다.

## 2019년 5월 9일
{: #05-09-2019}

**{{site.data.keyword.blockchainfull_notm}} Platform 콘솔 API 소개**

이제 API를 사용하여 피어, 순서 지정자 및 CA 노드를 프로비저닝, 편집 및 삭제할 수 있으므로 블록체인 네트워크 빌드를 스크립팅할 수 있습니다. API에 대해 자세히 알아보고 사용해 보려면 [{{site.data.keyword.cloud_notm}} API 문서 저장소](/apidocs/blockchain#introduction){: external}의 문서를 사용하십시오. 또한 API를 사용하여 네트워크를 빌드하는 방법에 대한 지시사항은 [API를 사용하여 네트워크 빌드](/docs/services/blockchain?topic=blockchain-ibp-v2-apis) 주제를 참조하십시오.  

**채널 통제**  

채널 통제 업데이트를 통해 정책을 재구성할 수 있습니다. 채널 업데이트에 서명해야 하는 채널 구성원을 제어할 수 있고 서명 콜렉션을 조정할 수도 있습니다.

## 2019년 4월 17일
{: #04-17-2019}

**노드 리소스의 크기를 지정하고 스케일링하는 기능**  

노드를 배치하면 이제 컨테이너에 CPU, 메모리 및 스토리지를 지정할 수 있는 기능이 제공됩니다(해당하는 경우). 사용 패턴에 따라 나중에 리소스를 위 또는 아래로 스케일링할 수 있습니다. 자세한 정보는 [리소스 할당](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-allocate-resources)을 참조하십시오.

**{{site.data.keyword.cloud_notm}} IAM(Identity and Access Management) 사용**  

IAM은 UI에서 수행할 수 있는 조치를 제한할 뿐만 아니라 콘솔 UI에 액세스할 수 있는 사용자 액세스 권한을 제어하는 데 사용됩니다.  [콘솔에서 사용자 추가 및 제거](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove)하는 방법에 대한 정보는 이 주제를 참조하십시오.

## 2019년 4월 3일
{: #04-03-2019}

**외부 CA에 대한 지원**

피어 또는 순서 지정자 노드를 추가하는 경우 {{site.data.keyword.IBM_notm}}에서 호스팅되지 않는 외부 CA에서 인증서를 사용할 수 있는 옵션이 제공됩니다. 자세한 정보는 [피어 또는 순서 지정자로 외부 CA에서 인증서 사용](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-third-party-ca)에 대한 주제를 참조하십시오.

**순서 지정자 성능 조정**

순서 지정자 처리량 및 성능에 대한 제어를 강화하기 위해 콘솔에서 매개변수를 조정하는 새 순서 지정자를 사용할 수 있습니다. 매개변수 구성에 대한 지시사항은 [순서 지정자 조정](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-orderer-tuning)에 대한 주제를 참조하십시오.
