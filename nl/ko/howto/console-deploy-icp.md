---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, IBM Blockchain Platform console, deploy, resource requirements, storage, parameters

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

# {{site.data.keyword.blockchainfull_notm}} 콘솔 배치
{: #console-deploy-icp}

{{site.data.keyword.blockchainfull_notm}} Platform Helm 차트를 설치한 후 {{site.data.keyword.blockchainfull}} Platform 콘솔을 로컬 {{site.data.keyword.cloud_notm}} Private 클러스터에 배치하려면 다음 지시사항을 따르십시오.
{:shortdesc}

## 필수 리소스
{: ##console-deploy-icp-resources-required}

사용자의 {{site.data.keyword.cloud_notm}} Private 시스템이 콘솔 및 작성하는 컴포넌트에 대한 최소 하드웨어 리소스 요구사항을 충족하는지 확인하십시오.

| **컴포넌트**(모든 컨테이너) | CPU  | 메모리(GB) | 스토리지(GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **콘솔** | 1.3 | 2.5 |10 |
| **피어** | 1.2 | 2.4 | 200(피어용 100GB 및 CouchDB용 100GB 포함)|
| **CA** | 0.1 | 0.2  | 20 |
|**순서 지정자** | 0.45 | 0.9 | 100 |

 **참고:**
 - vCPU는 서버가 가상 머신에 대해 파티션되지 않은 경우 가상 머신 또는 실제 프로세서 코어에 지정되는 가상 코어입니다. {{site.data.keyword.cloud_notm}} Private에서 배치를 위해 가상 프로세서 코어(VPC)를 결정할 때 vCPU 요구사항을 고려해야 합니다. VPC는 {{site.data.keyword.IBM_notm}} 제품의 라이센싱 비용을 판별하는 측정 단위입니다. VPC 결정 시나리오에 대한 자세한 정보는 [가상 프로세서 코어(VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}를 참조하십시오. 
 - vCPU = 1개의 CPU = 1개의 VPC

## 스토리지
{: #console-deploy-icp-storage}

콘솔 및 블록체인 컴포넌트에 대한 백업 스토리지가 충분한 상태에서 storageClass를 작성했는지 확인하십시오. 

- 콘솔 및 작성하는 컴포넌트에 대한 리소스가 충분히 포함된 새 스토리지 클래스를 작성해야 합니다. 구성 중 콘솔에 제공하는 스토리지 클래스는 컴포넌트 데이터를 저장하는 데도 사용됩니다. 
- 기본 설정을 사용하는 경우 콘솔 데이터에 대해 이름이 Helm 릴리스인 지속적 볼륨 청구가 Helm 차트에서 새로 작성됩니다. 
- 동적 프로비저닝을 사용하지 않는 경우 [지속적 볼륨](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external}을 작성한 후 Kubernetes 지속적 볼륨 청구(PVC) 바인드 프로세스를 세분화하는 데 사용할 수 있는 레이블로 설정해야 합니다. 

## 콘솔 배치를 위한 전제조건
{: #console-deploy-icp-prerequisites}

1. {{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 설치하려면 먼저 [{{site.data.keyword.cloud_notm}} Private을 설치](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup)하고 [{{site.data.keyword.blockchainfull_notm}} Platform Helm 차트를 설치](/docs/services/blockchain/howto/console-helm-install.html#console_helm-install)해야 합니다. 

2. {{site.data.keyword.blockchainfull_notm}} Platform 배치용 사용자 정의 네임스페이스를 새로 작성해야 합니다. 네임스페이스는 [필요한 PodSecurityPolicy](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install-prereqs-pod-security-requirements)를 사용해야 합니다. 예를 들어 개발, 스테이징 및 프로덕션 목적의 여러 환경을 작성하기 위해 블록체인 네트워크를 여러 개 작성하려는 경우 고유한 네임스페이스를 환경별로 작성해야 합니다. 

3. {{site.data.keyword.cloud_notm}} Private 콘솔에서 CA의 클러스터 프록시 IP 주소 값을 검색하십시오. **참고:** 프록시 IP에 액세스하려면 [클러스터 관리자](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/user_management/assign_role.html){: external}여야 합니다. {{site.data.keyword.cloud_notm}} Private 클러스터에 로그인하십시오. 왼쪽 탐색 패널에서 **플랫폼**, **노드**를 차례로 클릭하여 클러스터에 정의되어 있는 노드를 표시하십시오. 역할이 `proxy`인 노드를 클릭한 후 테이블에서 `Host IP`의 값을 복사하십시오. **중요:** 이 값을 저장하십시오. 이 값은 Helm 차트의 `Proxy IP` 필드를 구성할 때 사용하게 됩니다.


이 값을 저장하십시오. 이 값은 Helm 차트의 `Proxy IP` 필드를 구성할 때 사용하게 됩니다.
{:important}

4. 콘솔에 처음 로그인할 때 사용할 비밀번호를 작성한 후 {{site.data.keyword.cloud_notm}} Private의 시크릿 오브젝트에 저장하십시오. 시크릿을 작성하는 단계가 [다음 섹션](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp-password-secret)에 있습니다.

## 콘솔 비밀번호 시크릿 작성
{: #console-deploy-icp-password-secret}

콘솔에 액세스하려면 먼저 콘솔에 처음 로그인할 때 사용할 기본 비밀번호를 작성해야 합니다. 콘솔을 배치하기 전에 비밀번호를 저장할 [Kubernetes 시크릿](https://kubernetes.io/docs/concepts/configuration/secret/){: external}을 작성하십시오. Kubernetes 시크릿을 사용하면 정보를 배치에 직접 전달하지 않고도 정보를 보호하고 공유할 수 있습니다.

1. 비밀번호를 작성하고 base64 형식으로 인코딩하십시오. 터미널에서 다음 명령을 실행하고 `password` 값을 사용하려는 값으로 대체하십시오. **이 명령의 출력을 저장하십시오**. 
  ```
  echo -n 'password' | base64
  ```
  {:code_block}

2. {{site.data.keyword.cloud_notm}} Private 콘솔에 로그인하십시오. 왼쪽 탐색 패널에서 **구성**, **시크릿**을 차례로 클릭하십시오. **시크릿 작성** 단추를 클릭하여 새 시크릿 오브젝트를 작성할 수 있는 팝업 패널을 여십시오.

3. **일반** 탭에서 다음 필드를 완료하십시오.
  - **이름:** 클러스터 내에서 시크릿에 고유한 이름을 지정하십시오. 이 이름은 콘솔을 배치할 때 사용됩니다. 이름은 모두 소문자여야 합니다.
  - **네임스페이스:** 시크릿을 추가할 네임스페이스입니다. 콘솔을 배치할 `namespace`를 선택하십시오. 
  - **유형:** `generic` 값을 입력하십시오. 

4. **어노테이션** 탭을 비워 두십시오.

5. **데이터** 탭에서 사용자 이름 및 비밀번호를 키 값 쌍으로 추가하십시오.
  1. 첫 번째 **이름** 필드에 `password`를 입력하십시오. 
  2. 첫 번째 **값** 필드에 1단계에서 생성된 `echo -n 'password' | base64`의 결과를 입력하십시오. 
  3. **작성**을 클릭하여 새 시크릿 오브젝트를 작성하십시오.

콘솔을 배치할 때 구성 페이지의 `Console administrator password secret name` 필드에 시크릿 이름을 제공하십시오. 콘솔에 처음 로그인하려면 1단계에서 인코딩한 값을 사용해야 합니다. 이 값은 콘솔 관리자가 변경하지 않는 한 콘솔의 기본 비밀번호가 됩니다. 자세한 정보는 [콘솔에서 사용자 관리](/docs/services/blockchain/howto/console-icp-manage.html#console-icp-manage-users)를 참조하십시오. 

## TLS 시크릿 작성(선택사항)
{: #console-deploy-icp-tls-secret}

콘솔은 TLS를 사용하여 콘솔과 블록체인 노드 간의 통신을 보안 설정합니다. 기본적으로 콘솔은 자체 TLS 인증서를 생성합니다. 그러나 사용자 고유의 TLS 인증서 및 키를 제공할 수도 있습니다. 인증서를 [Kubernetes 시크릿](https://kubernetes.io/docs/concepts/configuration/secret/)에 저장하려면 다음 단계를 수행하십시오. 그런 다음 구성 중에 시크릿 이름을TLS 시크릿 필드에 제공하여 이러한 인증서를 콘솔에 제공할 수 있습니다. 

1. TLS 인증서와 개인 키를 base64 형식으로 변환하십시오. 로컬 시스템에서 다음 명령을 실행하여 인증서 또는 키 파일의 컨텐츠를 base64 문자열로 변환할 수 있습니다. 

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat <cert_or_key> | base64 $FLAG
  ```

  이 명령은 TLS 인증서와 수반되는 TLS 키에 대해 실행하십시오. 출력을 **저장**하십시오. 

2. {{site.data.keyword.cloud_notm}} Private 콘솔에 로그인하십시오. 왼쪽 탐색 패널에서 **구성**, **시크릿**을 차례로 클릭하십시오. **시크릿 작성** 단추를 클릭하여 새 시크릿 오브젝트를 작성할 수 있는 팝업 패널을 여십시오.

3. **일반** 탭에서 다음 필드를 완료하십시오.
  - **이름:** 클러스터 내에서 시크릿에 고유한 이름을 지정하십시오. CA를 구성하는 데 이 이름을 사용합니다. 이름은 모두 소문자여야 합니다.
  - **네임스페이스:** 시크릿을 추가할 네임스페이스입니다. CA를 배치하려는 `namespace`를 선택하십시오.
  - **유형:** `kubernetes.io/tls` 값을 입력하십시오. 

4. **어노테이션** 탭을 비워 두십시오.

5. **데이터** 탭에서 사용자 이름 및 비밀번호를 키 값 쌍으로 추가하십시오.
  1. 첫 번째 **이름** 필드에 `tls.crt`를 입력하십시오. 
  2. 첫 번째 **값** 필드에 1단계에서 생성된 base64 인코딩 TLS 인증서의 문자열을 입력하십시오. 
  3. **데이터 추가** 단추를 클릭하여 두 번째 키 값 쌍을 추가하십시오.
  4. 첫 번째 **이름** 필드에 `tls.key`를 입력하십시오. 
  5. 첫 번째 **값** 필드에 1단계에서 생성된 base64 인코딩 TLS 키의 문자열을 입력하십시오. 
  6. **작성**을 클릭하여 새 시크릿 오브젝트를 작성하십시오.

콘솔을 배치할 때 구성 페이지의 모든 매개변수 섹션에 있는 `TLS secret` 필드에 시크릿 이름을 제공하십시오. 

Helm 릴리스를 삭제해도 시크릿은 {{site.data.keyword.cloud_notm}} Private 클러스터에서 제거되지 않습니다. {{site.data.keyword.cloud_notm}} Private 클러스터에서 비밀번호와 TLS 시크릿을 관리하는 것은 사용자의 몫입니다. 나중에 다른 콘솔을 배치하려는 경우 시크릿을 재사용할 수 있습니다. 그렇지 않은 경우 사용자가 {{site.data.keyword.cloud_notm}} Private 클러스터에서 시크릿을 삭제해야 합니다.
{:note}

## 구성
{: #console-deploy-icp-configuration}

TLS 시크릿 오브젝트를 작성한 후에는 다음 단계를 수행하여 콘솔을 배치할 수 있습니다. 

1. {{site.data.keyword.cloud_notm}} Private 콘솔에 로그인한 후 오른쪽 상단에서 **카탈로그**를 클릭하십시오. 
2. 왼쪽 탐색 패널에서 `Blockchain`을 클릭하여 레이블이 `ibm-blockchain-platform-prod`인 타일을 찾으십시오. 타일을 클릭하여 여십시오. Helm 차트 설치 및 구성에 대한 정보가 포함된 Readme 파일이 표시되어야 합니다. 
3. 패널 상단에 있는 **구성** 탭을 클릭하거나 오른쪽 하단에 있는 **구성** 단추를 클릭하십시오. 
4. [구성 및 팟(Pod) 보안 매개변수](#icp-peer-deploy-configuration-parms)의 값을 지정하고 라이센스 계약에 동의하십시오. 
5. **매개변수** 섹션으로 이동하십시오. 
- 콘솔은 [빠른 시작 매개변수](#icp-peer-deploy-quickstart-parms)를 사용해서만 배치할 수 있습니다. 실험하거나 시작하는 경우 이 옵션을 사용하십시오. 
- [모든 매개변수](#icp-peer-deploy-quickstart-parms) 섹션을 사용하여 콘솔에 사용되는 네트워크 액세스, 리소스 및 스토리지를 사용자 정의할 수 있습니다. **모든 매개변수** 섹션은 보다 숙련된 Kubernetes 사용자에게만 권장됩니다. 
6. **설치**를 클릭하십시오.

다음 표에서는 구성 매개변수 필드와 해당 기본값에 대해 설명합니다. 

### 구성 및 팟(Pod) 보안 매개변수
{: #icp-peer-deploy-configuration-parms}

|매개변수     |설명    | 기본값  | 필수 |
| --------------|-----------------|-------|------- |
| `Helm release name`| Helm 릴리스 배치를 식별하는 이름입니다. 소문자로 시작하여 영숫자로 끝나야 하며 하이픈과 소문자의 영숫자만 포함해야 합니다. | 없음 |예 |
| `Target namespace`| 콘솔 및 컴포넌트용으로 작성한 네임스페이스를 지정하십시오. 네임스페이스에는 `ibm-privileged-psp` 정책이 포함되어야 합니다. 그렇지 않으면 네임스페이스에 [PodSecurityPolicy를 바인드](https://cloud.ibm.com/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-psp)하십시오. | 없음 |예 |
| `Target Cluster`| 리소스를 배치할 클러스터를 지정하십시오. 선택한 네임스페이스와 Kubernetes 버전 요구사항을 기준으로 사용 가능한 클러스터가 필터링됩니다. | 없음 |예 |
| `Target namespace policies`| 대상 네임스페이스를 사용하도록 사전 구성되었습니다. 값에는 `ibm-privileged-psp` 또는 `ibm-blockchain-platform-psp` 정책이 포함되어야 합니다. | 없음 |예 |

### 빠른 시작 및 모든 매개변수
{: #icp-peer-deploy-quickstart-parms}

빠른 시작 섹션은 실험하거나 시작하는 경우에 사용하십시오. 모든 매개변수 섹션에서는 네트워크 액세스, 리소스 및 스토리지를 사용자 정의할 수 있으며, 이 섹션은 보다 숙련된 Kubernetes 사용자에게만 권장됩니다. 

**모든 매개변수 탭을 클릭하면 모든 구성 매개변수를 볼 수 있습니다. **

|매개변수     |설명    | 기본값  | 필수 |
| --------------|-----------------|-------|------- |
| **콘솔 설정** | **콘솔의 관리 및 배치** | | |
| `Proxy IP` | 클러스터의 프록시 노드 IP 주소입니다. |없음|예 |
| `Console administrator email` | 콘솔 로그인에 사용되는 이메일입니다. | 없음 |예 |
| `Console administrator password secret name` | 콘솔 로그인에 사용할 [비밀번호를 저장하기 위해 작성한](#console-deploy-icp-password-secret) 시크릿의 이름입니다. | 없음 |예|
| **네트워크 설정** | **콘솔에 대한 네트워크 액세스** | | |
| `Console hostname` | 프록시 IP와 동일한 값을 입력하십시오. | 없음 |예 |
| `Console port` | 31210 - 31220 범위에서 사용할 포트를 입력하십시오. 이 포트는 다른 애플리케이션에서 사용할 수 없습니다. | 없음 |예 |
| **스토리지 설정** | **콘솔에 사용할 스토리지** | | |
| `Storage class name` | 콘솔 및 작성하는 컴포넌트에 사용할 스토리지 클래스의 이름을 입력하십시오. | 없음 |예 |
{: caption="표 1. 빠른 시작 매개변수" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Quickstart Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|매개변수     |설명    | 기본값  | 필수 |
| --------------|-----------------|-------|------- |
| `Architecture` | 클라우드 플랫폼 아키텍처를 선택하십시오(AMD64 또는 S390x). | AMD64 |아니오 |
| **콘솔 설정** | **콘솔의 관리 및 배치** | | |
| `Service account name` | 운영자가 사용할 서비스 계정입니다. | 없음 |아니오 |
| `Proxy IP` | 클러스터의 프록시 노드 IP 주소입니다. | 없음 |예|
| `Console administrator email` | 콘솔 로그인에 사용되는 이메일입니다. | 없음 |예 |
| `Console administrator password secret name` | 콘솔 로그인에 사용할 [비밀번호를 저장하기 위해 작성한](#console-deploy-icp-password-secret) 시크릿의 이름입니다. | 없음 |예|
| **Docker 이미지 설정** | **콘솔에서 가져올 패브릭 이미지를 사용자 정의하려면 다음 설정을 사용하십시오. ** | | |
| `imagePullSecret name` | 이미지를 다운로드하는 데 사용할 imagePullSecret입니다. | `ibp-ibmregistry` |아니오 |
| **네트워크 설정** | **콘솔에 대한 네트워크 액세스** | | |
| `Console hostname` | 프록시 IP와 동일한 값을 입력하십시오. | 없음 |예 |
| `Console port` | 31210 - 31220 범위에서 사용할 포트를 입력하십시오. | 없음 |예 |
| `Proxy hostname` | 프록시 서버 호스트 이름입니다.  프록시 IP와 동일한 값을 입력하십시오. | 없음 |아니오 |
| `Proxy port` | 31210 - 31220 범위에서 사용할 포트를 입력하십시오. 이 포트는 다른 애플리케이션이나 콘솔에서 사용할 수 없습니다. | 없음 |아니오 |
| `Configtxlator hostname` | 프록시 IP와 동일한 값을 입력하십시오. | 없음 |아니오 |
| `Configtxlator port` | 31210 - 31220 범위에서 사용할 포트를 입력하십시오. 이 포트는 다른 애플리케이션이나 콘솔에서 사용할 수 없습니다. | 없음 |아니오 |
| `TLS secret` | 콘솔에서 사용할 [TLS 인증서를 저장하기 위해 작성한](#console-deploy-icp-tls-secret) 시크릿의 이름입니다. | 없음 |아니오 |
| **데이터 지속성**| **데이터 지속성과 동적 프로비저닝을 사용으로 설정합니다.** | | |
| `Enable data persistence`| 클러스터가 다시 시작되거나 실패한 후 데이터 지속 여부를 사용으로 설정하려면 선택하십시오. 자세한 정보는 [Kubernetes의 스토리지](https://kubernetes.io/docs/concepts/storage/)를 참조하십시오. *true로 설정하지 않은 경우 장애 복구 또는 팟(Pod) 다시 시작 시 모든 데이터가 유실됩니다.* | Enabled |아니오 |
| `Enable dynamic provisioning`| 스토리지 볼륨에 대한 동적 프로비저닝을 사용으로 설정할 경우 선택합니다. | enabled |아니오 |
| **스토리지 설정**| **콘솔 및 도구에 대한 스토리지를 프로비저닝합니다.** | | |
| `Volume claim size`| 프로비저닝할 지속적 볼륨 청구의 크기입니다. | 10Gi  |아니오 |
| `Storage class name`| 콘솔 및 작성하는 컴포넌트에 사용할 스토리지 클래스의 이름입니다. | 없음 |예 |
| `Existing volume claim`| 기존 볼륨 청구의 이름을 지정하고 다른 모든 필드는 공백으로 둡니다. | 없음 |아니오 |
| `Selector label`| PVC를 위한 [선택기 레이블](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)| 없음 |아니오 |
| `Selector value`| PVC를 위한 [선택기 값](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) | 없음 |아니오 |
| `Storage access mode`| PVC의 스토리지 [액세스 모드](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes){: external}를 지정하십시오. | ReadWriteMany |아니오 |
| `PVC name`| 새 청구에만 해당됩니다. 새 PVC 이름을 입력하십시오. | 릴리스 이름으로 기본 설정됩니다. |아니오 |
| **리소스 할당**| **콘솔에 리소스를 할당합니다. ** | | |
| `Opstools CPU limit` | opstools 컴포넌트에 할당할 최대 CPU 수입니다. | 500m |아니오 |
| `Opstools memory limit` | opstools 컴포넌트에 할당할 최대 메모리 양입니다. | 1000Mi |아니오 |
| `Opstools CPU request` | opstools 컴포넌트에 할당할 최소 CPU 수입니다. | 500m |아니오 |
| `Opstools memory request` | opstools 컴포넌트에 할당할 최소 메모리 양입니다. | 1000Mi |아니오 |
| `Configtxlator CPU limit` | configtxlator 도구에 할당할 최대 CPU 수입니다. | 25m |아니오 |
| `Configtxlator memory limit` | configtxlator 도구에 할당할 최대 메모리 양입니다. | 50Mi |아니오 |
| `Configtxlator CPU request` | configtxlator 도구에 할당할 최소 CPU 수입니다. | 25m |아니오 |
| `Configtxlator memory request` | configtxlator 도구에 할당할 최소 메모리 양입니다. | 50Mi |아니오 |
| `CouchDB CPU limit` | CouchDB에 할당할 최대 CPU 수입니다. | 500m |아니오 |
| `CouchDB memory limit` | CouchDB에 할당할 최대 메모리 크기입니다. | 1000Mi |아니오 |
| `CouchDB CPU request` | CouchDB에 할당할 최소 CPU 수입니다.| 500m | 500m |
| `CouchDB memory request` | CouchDB에 할당할 최소 메모리 크기입니다.| 1000Mi |아니오 |
| `Operator CPU limit` | operator 컴포넌트에 할당할 최대 CPU 수입니다. | 100m |아니오 |
| `Operator memory limit` | operator 컴포넌트에 할당할 최대 메모리 양입니다. | 200Mi |아니오 |
| `Operator CPU request` | operator 컴포넌트에 할당할 최소 CPU 수입니다. | 100m |아니오 |
| `Operator memory request` | operator 컴포넌트에 할당할 최소 메모리 양입니다. | 200Mi |아니오 |
| `Deployer CPU limit` | deployer 컴포넌트에 할당할 최대 CPU 수입니다. | 100m |아니오 |
| `Deployer memory limit` | deployer 컴포넌트에 할당할 최대 메모리 양입니다. | 200Mi |아니오 |
| `Deployer CPU request` | deployer 컴포넌트에 할당할 최소 CPU 수입니다. | 100m |아니오 |
| `Deployer memory request` | deployer 컴포넌트에 할당할 최소 메모리 양입니다. | 200Mi |아니오 |
{: caption="표 2. 모든 매개변수" caption-side="top"}
{: #simpletabtable2}
{: tab-title="All Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

### Helm 명령행을 사용하여 Helm 릴리스 설치
{: #console-deploy-icp-helm-cli}

또는 Helm CLI를 사용하여 `helm` 릴리스를 설치할 수 있습니다. `helm install` 명령을 실행하기 전에 [Helm CLI 환경에 클러스터의 Helm 저장소를 추가](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/app_center/add_int_helm_repo_to_cli.html){: external}하십시오. 

`yaml` 파일을 작성하고 이 파일을 `helm install` 명령에 전달하여 설치에 필요한 매개변수를 설정할 수 있습니다.
```
helm install --name <helm_release_name>  <helm_chart> \
--version <helm_chart_version> \
--values <customvalues.yaml> \
--tls
```
{:codeblock}

여기서,
- `<helm_release name>`은 helm 릴리스에 지정할 이름을 표시합니다.
- `<helm_chart>`는 카탈로그에 가져온 Helm 차트의 이름을 표시합니다.
- `<helm_chart_version>`은 카탈로그에 가져온 Helm 차트의 버전을 표시합니다.
- `<customvalues.yaml>`은 구성 매개변수가 포함된 yaml 파일의 이름입니다.

예를 들어, 다음과 같습니다.
```
helm install --name jnchart2 mycluster/ibm-blockchain-platform \
--version 1.1.0 \
--values console-s390x-values.yaml \
--tls
```

다운로드된 아카이브 파일에 포함된 `values.yaml`을 편집하여 새 `yaml` 파일을 작성할 수 있으며, 여기에는 기본 설정이 사용된 모든 필수 매개변수가 포함됩니다.

## 설치 확인

구성 매개변수를 완료하고 **설치** 단추를 클릭한 후 **Helm 릴리스 보기** 단추를 클릭하여 배치를 보십시오. 성공한 경우 배치 테이블의 `DESIRED`, `CURRENT`, `UP TO DATE` 및 `AVAILABLE` 필드에 값 1이 표시됩니다. 새로 고치기를 클릭하고 테이블이 업데이트될 때까지 기다려야 할 수 있습니다.

**배치** 개요 화면으로 이동하여 작성된 팟(Pod)을 클릭하면 배치 세부사항을 확인할 수 있습니다. Helm 차트 배치는 클러스터에 5개의 컨테이너를 작성합니다. 
- **opstools**: 콘솔 UI입니다. 
- **deployer**: 콘솔에서 배치와 통신할 수 있는 도구입니다. 
- **configtxlator**:콘솔에서 채널 업데이트를 읽고 작성하는 데 사용되는 도구입니다. 
- **couchdb**: 권한 정보를 포함한 콘솔 데이터를 저장하는 CouchDB의 인스턴스입니다. 
- **operator**: 컴포넌트 배치를 관리하는 Kubernetes 운영자입니다. 

## 콘솔에 로그인

설치 후 브라우저를 사용하여 콘솔에 액세스할 수 있습니다. URL은 배치 후 열리는 Helm 릴리스 개요 화면의 **참고:** 섹션에서 찾을 수 있습니다. Firefox의 ESR 버전을 사용하고 있지 않은지 확인하십시오. 사용 중인 경우, Chrome과 같은 다른 브라우저로 전환한 후 다시 시도하십시오.

브라우저에 콘솔 로그인 화면이 표시되어야 합니다. 
- 구성 중 `Console administrator email` 필드에 제공한 값을 **사용자 ID**로 사용하십시오. 
- 인코딩하여 [비밀번호 시크릿](#console-deploy-icp-password-secret)에 저장한 후 구성 중에 콘솔에 전달한 값을 **비밀번호**로 사용하십시오. 이 비밀번호는 모든 신규 사용자가 콘솔에 로그인할 때 사용하는 콘솔의 기본 비밀번호가 됩니다. 처음으로 로그인하면 콘솔 로그인에 사용할 수 있는 새 비밀번호를 제공하라는 메시지가 표시됩니다. 

Helm 차트를 프로비저닝한 관리자는 다른 사용자에게 콘솔 액세스 권한을 부여하고 이들이 수행할 수 있는 오퍼레이션을 지정할 수 있습니다. 자세한 정보는 [콘솔에서 사용자 관리](/docs/services/blockchain/howto/ibp-console-import-nodes.html#console-icp-manage-users)를 참조하십시오. 

## 다음 단계
{: #console-deploy-icp-next-steps}

콘솔에 액세스한 후에는 콘솔 UI의 **노드** 탭을 볼 수 있습니다. 이 화면에서 로컬 클러스터에 컴포넌트를 배치할 수 있습니다. 콘솔 사용을 시작하려면 [네트워크 빌드 튜토리얼](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network)을 방문하십시오. 이 탭에서는 다른 노드에 작성된 노드를 작동시킬 수도 있습니다. 자세한 정보는 [노드 가져오기](/docs/services/blockchain/howto/ibp-console-import-nodes.html#ibp-console-import-nodes)를 참조하십시오. 

콘솔에 액세스할 수 있는 사용자를 관리하는 방법을 알아보려면 {{site.data.keyword.blockchainfull_notm}} Platform API를 사용하여 콘솔 및 블록체인 컴포넌트의 로그를 확인한 후 [{{site.data.keyword.cloud_notm}} Private에서 콘솔 관리](/docs/services/blockchain/howto/ibp-console-import-nodes.html#console-icp-manage)를 방문하십시오. 
