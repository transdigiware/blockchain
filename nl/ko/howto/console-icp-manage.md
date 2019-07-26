---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, IBM Blockchain Platform, administrate, add user, remove user, password, APIs, authentication, view logs

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}
{:curl: #curl .ph data-hd-programlang='curl'}

# 콘솔 관리
{: #console-icp-manage}

콘솔을 {{site.data.keyword.cloud}} Private에 배치한 후에는 콘솔을 사용하여 콘솔 사용자를 추가 또는 제거할 수 있고, 네트워크를 작동시키고 콘솔을 제어하는 데 사용할 수 있는 API에 액세스할 수 있습니다. 콘솔 로그에 액세스하고 콘솔 로그를 사용자 정의할 수도 있습니다.
{:shortdesc}

## 콘솔에서 사용자 관리
{: #console-icp-manage-users}

{{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 프로비저닝하는 사용자는 콘솔 관리자로 간주됩니다. 관리자는 콘솔에서 **사용자** 탭을 사용하여 다른 사용자를 추가하고 이 사용자에게 콘솔 액세스 권한을 부여할 수 있습니다. 콘솔에 액세스하는 모든 사용자에게는 사용자 역할이 정의된 액세스 정책을 지정해야 합니다. 정책은 사용자가 콘솔 내에서 수행할 수 있는 조치를 결정합니다. 기본적으로 콘솔 관리자에게는 콘솔에 대핸 **관리자** 역할이 부여됩니다. 콘솔 관리자가 **관리자**, **작성자** 또는 **독자** 역할을 콘솔에 추가하면 해당 역할을 사용하는 다른 사용자를 지정할 수 있습니다. [API](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-users-apis)로 사용자를 관리할 수도 있습니다.

| 역할 | 기능 |
|--------|----------|
| 관리자 | 관리자는 작성자 역할 이상의 권한이 있습니다. 독자 및 작성자도 수행할 수 있는 모든 작업을 수행할 수 있습니다. 관리자의 역할은 다음과 같습니다. <ul><li>콘솔 또는 API를 사용하여 새 컴포넌트를 프로비저닝합니다.</li><li>콘솔 또는 API를 사용하여 프로비저닝된 컴포넌트를 삭제합니다.</li><li>사용자를 추가/제거하고 사용자 액세스 정책을 변경합니다.</li><li>콘솔 또는 API를 사용하여 콘솔 로깅 레벨을 변경합니다.</li><li>API를 사용하여 콘솔을 다시 시작합니다.</li></ul> |
| 작성자 | 작성자는 독자 역할 이상의 권한이 있습니다. 작성자의 역할은 다음과 같습니다. <ul><li>콘솔 또는 API를 사용하여 컴포넌트를 가져옵니다.</li><li>콘솔 또는 API를 사용하여 가져온 컴포넌트를 제거합니다.</li><li>CA에 사용자를 등록합니다.</li><li> 콘솔 또는 API를 사용하여 알림을 추가하거나 제거합니다.</li></ul>  |
| 독자 | 독자는 읽기 전용 조치를 수행할 수 있습니다. 독자의 역할은 다음과 같습니다. <ul><li>콘솔 UI를 봅니다.</li><li>콘솔 로그를 봅니다.</li><li>컴포넌트를 내보냅니다.</li><li>GET API를 실행합니다.</li></ul> | |

권한은 누적됩니다. **관리자** 역할을 선택할 경우 해당 사용자는 모든 **작성자** 및 **독자** 조치도 수행할 수 있으므로 이러한 역할을 추가로 확인하지 않아도 됩니다. 이와 마찬가지로, **Writer** 역할이 있는 사용자는 **Reader** 역할의 모든 조치를 수행할 수 있습니다.

### 사용자 비밀번호 관리
{: #console-icp-manage-user-pw}

[비밀번호 시크릿](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret)에 저장된 후 구성 중에 콘솔에 전달된 비밀번호는 콘솔의 기본 비밀번호가 됩니다. 모든 사용자는 콘솔에 처음 로그인할 때 이 비밀번호를 사용해야 합니다. 콘솔 관리자가 기본 비밀번호를 새로 지정할 수도 있습니다. 콘솔의 **사용자** 탭에서 인증 서비스 타일 이름 아래에 있는 **구성 업데이트** 링크를 클릭하십시오. 오른쪽 패널에서 콘솔 신규 사용자에 대한 기본 비밀번호를 확인하거나 업데이트할 수 있습니다.

콘솔에 로그인할 수 있도록 기본 비밀번호 또는 사용자가 재설정하는 기본 비밀번호를 사용자와 공유해야 합니다. 사용자는 처음 로그인했을 때 비밀번호를 변경해야 합니다.
{:note}

관리자 역할의 사용자는 다른 사용자의 비밀번호를 재설정할 수 있습니다. 콘솔의 **사용자** 탭에서 특정 사용자 행의 끝에 있는 세 개의 세로 점을 클릭한 후 **비밀번호 재설정**을 클릭하십시오. 콘솔에서 사용자의 비밀번호가 기본 비밀번호로 재설정되며, 이 값은 오른쪽 패널에서 확인할 수 있습니다. 어떤 역할의 사용자든지 자신의 비밀번호는 원할 때 변경할 수 있습니다. 오른쪽 상단에 있는 아바타 아이콘을 클릭한 후 **비밀번호 변경**을 클릭하십시오.

새 사용자를 콘솔에 추가한 후 사용자는 기타 사용자가 배치하는 모든 노드, 채널 또는 체인코드를 볼 수 없을 수도 있습니다. 이 콘솔 관련 작업을 수행하려면 각 사용자는 연관된 ID를 고유한 콘솔 지갑으로 가져와야 합니다. 자세한 정보는 [콘솔 지갑에 ID 저장](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet)을 참조하십시오.
{:important}

### 사용자 역할 수정
{: #console-icp-manage-reset-user-pw}

관리자 역할의 사용자는 다른 콘솔 사용자의 역할을 업데이트하고 사용자가 더 많거나 적은 콘솔 기능에 액세스하도록 설정할 수 있습니다. 콘솔의 **사용자** 탭에서 특정 사용자 행의 끝에 있는 세 개의 세로 점을 클릭한 후 **인증된 사용자 업데이트**를 클릭하십시오. 오른쪽 패널에서 사용자의 새 역할을 선택하십시오. 다음에 사용자가 콘솔에 로그인할 때 사용자 역할이 업데이트됩니다.

### 콘솔에서 사용자 삭제
{: #console-icp-manage-icp-remove-user}

관리자 역할의 사용자인 경우 콘솔에 대한 사용자의 액세스 권한을 제거할 수 있습니다. 콘솔의 **사용자** 탭에서 특정 사용자 행의 시작 부분에 있는 선택란을 클릭하십시오. 그런 다음 **새 사용자 추가** 단추 옆에 있는 표의 맨 위에 있는 **휴지통** 아이콘을 클릭하십시오. 표에서 사용자를 제거하고 나면 콘솔에 다시 로그인할 수 없습니다.

## {{site.data.keyword.blockchainfull_notm}} API 사용
{: #console-icp-manage-apis}

{{site.data.keyword.blockchainfull_notm}} Platform은 콘솔에서 블록체인 노드를 가져오고, 편집 및 제거할 수 있는 RESTful API를 표시합니다. 또한 API를 사용하여 콘솔의 설정, 로깅 및 알림을 관리할 수도 있습니다. 다음 지시사항에 따라 {{site.data.keyword.cloud_notm}} Private에 배치된 콘솔에서 제공하는 API를 사용하십시오.

### 전제조건
{: #console-icp-manage-prereqs}

API를 사용하려면 다음 정보를 수집해야 합니다.

- 콘솔 **API 엔드포인트**. 브라우저를 사용하여 콘솔에 액세스하는 데 사용할 URL입니다. 이 URL은 Helm 릴리스 개요 화면의 **참고:** 섹션에서 찾을 수 있습니다.
- 콘솔에 액세스하는 데 사용할 수 있는 사용자 이름 및 비밀번호입니다. API 키를 작성하려면 [관리자 역할](#console-icp-manage-users)의 계정이 있어야 합니다.

### API 키 사용
{: #console-icp-manage-create-api-key}

콘솔마다 자체 ID 및 액세스 관리를 제공합니다. 콘솔 사용자 이름과 비밀번호를 사용하여 API 호출에 권한을 부여할 수 있는 API 키와 시크릿을 생성할 수 있습니다.

각 API 키는 사용자가 수행할 수 있는 기능을 제어하는 역할과 연관됩니다. API 키는 사용자 이름과 비밀번호를 사용하여 콘솔에 로그인하는 사용자의 역할과 동일한 액세스 정책 역할을 사용합니다. 각 역할이 수행할 수 있는 조치 목록은 [사용자 관리](#console-icp-manage-users) 섹션의 표를 참조하십시오. 관리자 역할만 API 키를 작성할 수 있으므로 콘솔 관리자는 독자 및 작성자 사용자용 API 키를 작성해야 합니다. API 키는 만료되지 않습니다. 그러므로 사용하지 않는 API 키는 콘솔 관리자가 삭제해야 합니다.

API 키를 관리하기 위해 3개의 API를 사용할 수 있습니다.
- [API 키 작성](#console-icp-manage-create-api-key)
- [API 키 보기](#console-icp-manage-view-api-keys)
- [API 키 삭제](#console-icp-manage-delete-api-keys)

API 키와 시크릿을 작성하려면 아래 요청을 사용하십시오. 그런 다음 이 키와 시크릿을 사용하여 다른 API를 사용할 수 있습니다. API 시크릿은 콘솔에 저장되지 않으므로 사용자가 저장해야 합니다.

#### API 키 작성
{: #console-icp-manage-create-api-key-api}

| **요청** |  |
|-------------|-----------|
| 경로 | POST `<API_endpoint>`/ak/api/v1/permissions/keys |
| **요청 본문 필드** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` 최소 한 개의 값이 필요합니다. </li><li>`string` 선택사항</li></ul>|
| **응답 본문 필드** | |
| <ul><li>`api_key`</li><li>`description`</li><li>`roles`</li></ul>| <ul><li>`string`</li><li>`string` 저장: 키가 저장되지 않습니다.</li><li>`["<role>"]`</li></ul>|
| 필요한 권한 | 관리자 |

#### curl 요청 예: API 키 작성
{: #console-icp-manage-create-api-key-api-example}
```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u user@email.com:password \
  -H 'Content-Type: application/json' \
  -d '{
     "roles": ["writer", "manager"],
     "description": "newkey"
     }'
```

### 콘솔 API 키 관리
{: #console-icp-manage-api-keys}

API 키와 시크릿을 작성한 후에는 API 키를 사용하여 작성한 키를 보거나 제거할 수 있습니다. 키와 시크릿은 삭제할 때까지 만료되지 않습니다.

#### API 키 보기
{: #console-icp-manage-view-api-keys}

| **요청** |  |
|-------------|-----------|
| 경로 | GET `<API_endpoint>`/ak/api/v1/permissions/keys |
| **응답 본문 필드** | |
| <ul><li>`api_key`</li><li>`roles`</li><li>`ts_created`</li><li>`description`</li></ul>| <ul><li>`string`</li><li>`["<role>"]`</li><li>`number` Unix 시간소인(밀리초)</li><li>`string`</li></ul>|
| 필요한 권한 | 읽기 권한 |

#### curl 요청 예: API 키 보기
{: #console-icp-manage-view-api-key-example}
```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u <api_key>:<api_secret>
```

#### API 키 삭제
{: #console-icp-manage-delete-api-keys}

| **요청** |  |
|-------------|-----------|
| 경로 | DELETE `<API_endpoint>`/ak/api/v1/permissions/keys:`<api_key>` |
| 필요한 권한| 관리자 |

#### curl 요청 예: API 키 삭제
{: #console-icp-manage-delete-api-keys-example}


```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys/:<api_key> \
  -u <api_key>:<api_secret>
```

### API를 사용하여 사용자 관리
{: #console-icp-manage-users-apis}


API를 사용하여 콘솔에 로그인하거나 콘솔 API에 액세스할 수 있는 사용자를 나열, 추가 또는 제거할 수도 있습니다. 사용자 역할을 편집하여 사용자의 액세스 레벨을 업그레이드할 수도 있습니다. 다음과 같은 API를 사용할 수 있습니다.

- [사용자 나열](#console-icp-manage-list-users-api)
- [사용자 편집](#console-icp-manage-edit-users-api)
- [사용자 추가](#console-icp-manage-add-users-api)
- [사용자 제거](#console-icp-manage-remove-users-api)

콘솔의 설정 및 사용자를 관리하고 네트워크의 노드를 관리하는 API의 경우, TLS 인증서에 대한 요구사항을 무시하도록 ``-K`` 또는 ``--insecure`` 플래그를 추가해야 합니다. 또한 브라우저를 사용하여 콘솔에서 TLS 인증서를 다운로드할 수도 있습니다.

#### 사용자 나열
{: #console-icp-manage-list-users-api}


| **요청** |  |
|-------------|-----------|
| 경로 | GET `<API_endpoint>`/ak/api/v1/permissions/users |
| **응답 본문 필드** | |
| <ul><li>`uuids`</li><li>`email`</li><li>`roles`</li><li>`created`</li></ul>| <ul><li>`string` 사용자 ID</li><li>`string` 이메일 주소</li><li>`["<role>"]`</li><li>`number` Unix 시간소인(밀리초)</li></ul>|
| 필요한 권한 | 읽기 권한 |

#### curl 요청 예: 사용자 나열
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K
```

#### 사용자 편집
{: #console-icp-manage-edit-users-api}

| **요청** |  |
|-------------|-----------|
| 경로 | PUT `<API_endpoint>`/ak/api/v1/permissions/users |
| **요청 본문 필드** | |
| <ul><li>`users`</li><li>`roles`</li></ul> | <ul><li>`string` 사용자 ID </li><li>`["reader", "writer", "manager"]` 최소 한 개의 값이 필요합니다.</li></ul> |
| **응답 본문 필드** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` 사용자 ID</li></ul>|
| 권한| 관리자 |

#### curl 요청 예: 사용자 편집
{: #console-icp-manage-edit-users-api-example}

```
curl -X PUT \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc": {
          "roles": ["reader", "writer", "manager"],
            }
          }
        }'
```

#### 사용자 추가
{: #console-icp-manage-add-users-api}

| **요청** |  |
|-------------|-----------|
| 경로 | POST `<API_endpoint>`/ak/api/v1/permissions/users |
| **요청 본문 필드** | |
| <ul><li>`roles`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` 최소 한 개의 값이 필요합니다. </li><li>`string` 선택사항</li></ul>|
| 권한 | 관리자 |

#### curl 요청 예: 사용자 추가
{: #console-icp-manage-add-users-api-example}

```
curl -X POST \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "users": {
          "someone@mail.com": {
          "roles": ["reader", "writer", "manager"]
            }
          }
        }'
```

#### 사용자 제거
{: #console-icp-manage-remove-users-api}

| **요청** |  |
|-------------|-----------|
| 경로 | DELETE `<API_endpoint>`/ak/api/v1/permissions/users |
| **요청 본문 필드** | |
| <ul><li>`users`</li></ul>| <ul><li>`string` 사용자 ID</li></ul>|
| **응답 본문 필드** | |
| <ul><li>`uuids`</li></ul>| <ul><li>`string` 사용자 ID</li></ul>|
| 권한 | 관리자 |

#### curl 요청 예: 사용자 제거
{: #console-icp-remove-add-users-api-example}

```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K \
  -H 'Content-Type: application/json' \
  -d '{
  "uuids": [
        "b26e67a3-8f4c-40e4-b5e2-6303ad2979fc",
        "19bd26a0-6053-491d-ada3-ad5bb741f034"
          ]
        }'
```

### API를 사용하여 컴포넌트 관리

[{{site.data.keyword.blockchainfull_notm}} Platform API 참조](https://test.cloud.ibm.com/apidocs/blockchain)에서는 사용할 수 있는 전체 API 세트를 볼 수 있습니다.

사용자는 API를 사용하여 {{site.data.keyword.cloud_notm}} Private에서 콘솔과 통신하기 때문에 {{site.data.keyword.cloud_notm}}에세 제공하는 권한을 콘솔에서 제공하는 인증과 함께 사용해야 합니다. API 참조의 ``Bearer Auth``를 ``-u <api_key>:<api_secret>``으로 대체하십시오. 또한 ``-K`` 또는 ``--insecure`` 플래그를 명령에 추가하거나 브라우저를 사용하여 콘솔 TLS 인증서를 다운로드해야 할 수 있습니다. **시험 사용** 탭에서는 {{site.data.keyword.cloud_notm}} Private의 네트워크용 API를 테스트할 수 없습니다.

예를 들어 아래의 API 호출은 {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}의 서비스 인스턴스에서 실행하는 모든 컴포넌트에 대한 정보를 리턴합니다.
```
curl -X POST \
  https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer eyJraWQ.....zJPsw
```
동일한 API 호출은 {{site.data.keyword.cloud_notm}} Private에 배치된 콘솔에 대한 아래의 요청과 유사합니다.
```
curl -X POST \
https://9.30.252.107:31212/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -u kO25ME32Nu8TikR_:buYImbg0co8SxneoBWzHueYwrf9Xhg5f \
  -K
```

API를 사용하여 콘솔이 배치된 클러스터에 노드를 작성하고 다른 클러스터 또는 {{site.data.keyword.cloud_notm}}에서 노드를 가져올 수 있습니다. 자세한 정보는 [API를 사용하여 네트워크 빌드](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-build-with-apis) 및 [API를 사용하여 네트워크 가져오기](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-import-with-apis)를 참조하십시오.

## 로그 보기
{: #icp-console-manage-logs}

{{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 사용하는 경우 문제를 디버깅하기 위해 로그를 확인해야 할 수도 있습니다.

### 콘솔 로그 보기
{: #console-icp-manage-console-logs}

콘솔을 사용하거나 노드를 작동할 때 발생하는 문제점을 디버그해야 하는 경우 쉽게 콘솔 로그에 액세스할 수 있습니다. 로깅 레벨을 설정하여 콘솔에서 수집하는 로그의 양을 늘리거나 줄일 수도 있습니다. 콘솔 로그는 {{site.data.keyword.cloud_notm}} Private에서 수집하는 [노드 로그](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#console-icp-manage-node-logs)와 별도로 수집됩니다.

콘솔 브라우저에서 **설정** 탭으로 이동하여 로깅 설정을 변경하십시오. 콘솔 로그는 두 개의 독립된 소스에서 수집됩니다.

  * **클라이언트 로깅:** 해당 로그는 사용자 브라우저에서 콘솔로 명령이 전송될 때 수집됩니다.
  * **서버 로깅:** 해당 로그는 콘솔이 콘솔 배치에서 노드로 명령을 전송할 때 수집됩니다. 이러한 로그에는 Hyperledger Fabric 로깅 출력이 포함되어 있습니다.

각 로그 유형 아래의 드롭 다운 목록을 사용하여 수집되는 로그의 양을 설정하십시오. 예를 들어,
**오류** 및 **경고**는
가장 적은 양의 로그를 수집하는 반면 **디버그** 및 **모두**는 가장 많은 양의 로그를 수집합니다.

콘솔 관리자로 로그인한 경우에만 콘솔 로그를 볼 수 있습니다. **설정** 탭에서
로그를 보려면 브라우저 URL에서 `settings`를 `api/v1/logs`로 대체하십시오. 예를 들어,
콘솔 URL이 `localhost:3001/settings`인 경우, `localhost:3001/api/v1/logs`로 이동하면
로그를 볼 수 있습니다. 클라이언트 및 서버 로그는 별도의 파일에 수집됩니다. 최신 로그는 페이지 맨 위에 있습니다.

### 노드 로그 보기
{: #console-icp-manage-node-logs}

컴포넌트 로그는 [kubectl CLI 명령](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external}을 사용하거나 {{site.data.keyword.cloud_notm}} Private 클러스터에 포함된 [Kibana](https://www.elastic.co/products/kibana){: external}를 통해 볼 수 있습니다.

- `kubectl logs` 명령을 실행하여 팟(Pod) 내부의 컨테이너 로그를 보십시오. 아직 실행하지 않은 경우 [kubectl cli 설치](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} 지침을 따르십시오. 팟(Pod) 이름이 확실하지 않은 경우 다음 명령을 실행하여 팟(Pod)의 목록을 보십시오.

  ```
  kubectl get pods
  ```
  {:codeblock}

  그런 후 다음 명령을 실행하여 팟(Pod) 내에 있는 노드 컨테이너의 로그를 검색하십시오.

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  `<pod_name>`을 위 명령 출력의 팟(Pod) 이름으로 대체하십시오.  
  노드 로그를 보려면 `<node>`를 `ca`, `peer` 또는 `orderer`로 대체하십시오.  
  스마트 계약의 로그를 보려면 `<node>`를 `fluentd`로 대체하십시오.

  `kubectl logs` 명령에 대한 자세한 정보는 [Kubernetes 문서](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}를 참조하십시오.

- 또는 {{site.data.keyword.cloud_notm}} Private 콘솔에서 [로그에 액세스](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/troubleshoot/events.html){: external}할 수 있습니다. 그러면 Kibana에서 로그가 열립니다.

  **참고:** Kibana에서 로그인이 표시되면 `No results found`의 응답을 수신할 수 있습니다. 이 상태는 {{site.data.keyword.cloud_notm}} Private이 호스트 이름으로 작업자 노드 IP 주소를 사용하는 경우 발생할 수 있습니다. 이 문제점을 해결하려면 패널 상단에서 `node.hostname.keyword`로 시작하는 필터를 제거하십시오. 그런 다음 로그가 표시됩니다.

### 스마트 계약 컨테이너 로그 보기
{: #console-icp-manage-container-logs}

스마트 계약 관련 문제가 발생할 경우 문제를 디버깅하기 위해 스마트 계약 또는 체인코드, 컨테이너 로그를 확인할 수 있습니다. 다음 명령을 실행하여 스마트 계약 컨테이너 로그를 확인할 수 있습니다.

```
kubectl  logs -f <peer_ped> -c fluentd
```
{:codeblock}

`<peer_pod>`을 체인코드가 실행 중인 피어 팟(Pod)의 이름으로 대체하십시오. `kubectl get po` 명령을 사용하여 실행 중인 팟(Pod)의 목록을 가져오십시오.

## 노드 패치 설치
{: #console-icp-manage-patch}

시간 경과에 따라 피어, CA 및 순서 지정자 노드에 대한 기본 {{site.data.keyword.IBM_notm}} Hyperledger Fabric Docker 이미지를 업데이트해야 할 수 있습니다(예: 보안 업데이트 사용 또는 새 Fabric 지점 릴리스로). {{site.data.keyword.blockchainfull_notm}} Platform helm 차트를 업그레이드할 때 Fabric 이미지를 업데이트할 수 있습니다.

Helm 차트를 업그레이드하고 나면 컴포넌트 업데이트를 사용할 수 있는 경우 노드 타일에서 **패치 사용 가능** 텍스트를 찾을 수 있습니다. 이 패치는 준비가 되면 언제든 노드에 설치할 수 있습니다. 이러한 패치는 선택사항이지만 권장됩니다. 콘솔로 가져온 노드는 패치할 수 없습니다.

패치는 한 번에 하나의 노드에만 적용됩니다. 패치가 적용되는 동안에는 노드를 사용하여 요청 또는 트랜잭션을 처리할 수 없습니다. 따라서 서비스 중단을 방지하려면 가능한 경우 동일한 유형의 다른 노드가 요청을 처리할 수 있는지 확인해야 합니다. 노드에 패치를 설치하는 작업을 완료하는 데 약 1분이 걸리고 업데이트가 완료되면 노드가 요청을 처리할 준비가 됩니다.
{:note}

노드에 패치를 적용하려면 노드 타일을 열고 **패치 설치** 단추를 클릭하십시오. 콘솔로 가져온 노드는 패치할 수 없습니다.
