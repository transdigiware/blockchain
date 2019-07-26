---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-16"


keywords: IBM Cloud Private, data storage CA, cluster ICP, configuration

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

# {{site.data.keyword.cloud_notm}} Private 설정
{: #icp-console-setup}

{{site.data.keyword.cloud_notm}} Private에 {{site.data.keyword.blockchainfull}} Platform 컴포넌트를 배치하고 블록체인 네트워크를 빌드하려면 먼저 사용자 환경에서 {{site.data.keyword.cloud_notm}} Private을 설정해야 합니다.
{:shortdesc}

## 전제조건
{: #icp-console-setup-prerequisites}

다음 전제조건을 완료한 후 {{site.data.keyword.cloud_notm}} Private을 설치할 환경을 준비하십시오.

### Docker
{{site.data.keyword.cloud_notm}} Private을 사용하려면 Docker가 설치되어 있어야 합니다. [{{site.data.keyword.cloud_notm}} Private 설치](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/install.html){: external}에 나오는 관련 지시사항에 따라 Docker를 설치하십시오.

### {{site.data.keyword.cloud_notm}} Private 설정
{{site.data.keyword.cloud_notm}} Private을 설치하기 전에 {{site.data.keyword.cloud_notm}} Private 설치에 필요한 노드를 준비하기 위해 다음과 같은 팁이 유용합니다. 추가 {{site.data.keyword.cloud_notm}} Private 전제조건은 [{{site.data.keyword.cloud_notm}} Private 문서](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep.html){: external}에서 찾을 수 있습니다.

#### `vm.max_map_count` 설정 업데이트
{{site.data.keyword.cloud_notm}} Private은 로깅 및 측정을 위해 Elastic Search를 사용합니다. Elastic Search의 경우 메모리 부족 예외를 방지하기 위해 `vm.max_map_count` 시스템 특성이 구성되어 있어야 합니다. {{site.data.keyword.cloud_notm}} Private을 설치하기 전에 [Elastic Search 구성 지시사항](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html){: external}을 참조하여 각 노드에 이 특성을 구성하십시오. 다음 명령을 사용하여 영구적으로 이 특성을 설정할 수 있습니다.

```
sysctl -w vm.max_map_count=262144; sysctl vm.max_map_count
echo "vm.max_map_count=262144” | tee -a /etc/sysctl.conf
```
{:codeblock}

#### 클러스터에 있는 각각의 노드에 `/etc/hosts` 파일 구성

- {{site.data.keyword.cloud_notm}} Private은 [Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/){: external}를 사용하여 컨테이너화된 애플리케이션을 관리합니다. 각각의 노드에 있는 `/etc/hosts`에 호스트 이름이 구성되어 있지 않은 경우 Kubernetes 도메인 이름 서버(DNS)가 실패합니다. 각 노드에 있는 `/etc/hosts` 파일에 [클러스터에 있는 각 노드의 IP 주소, 호스트 이름 및 단축 이름을 삽입](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep_cluster.html){: external}하십시오.

- [IPv6는 {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#ipv6){: external}에서 지원되지 않습니다. {{site.data.keyword.cloud_notm}} Private 클러스터에서 DNS 서비스 관련 문제점을 방지하려면 각각의 노드에 있는 `/etc/hosts` 파일에서 다음 행의 시작 부분에 `#` 부호를 사용하여 해당 행을 주석 처리함으로써 IPv6 설정을 사용 안함으로 설정하십시오.
  ```
  #::1  localhost ip6-localhost ip6-loopback
  ```
  {:codeblock}

## 필수 리소스
{: #icp-console-setup-resources}

사용자의 {{site.data.keyword.cloud_notm}} Private 시스템이 콘솔 및 작성하는 컴포넌트에 대한 최소 하드웨어 리소스 요구사항을 충족하는지 확인하십시오. 필요한 vCPU/CPU 수는 인프라, 네트워크 디자인 및 성능 요구사항에 따라 달라질 수 있습니다. 대략적인 vCPU/CPU 요구사항은 할당량이 {{site.data.keyword.cloud_notm}} Private에서 약간 다르더라도 {{site.data.keyword.cloud_notm}}에 대한 [기본 리소스 할당 표](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-default)를 조사하여 작성할 수 있습니다. 실제 리소스 할당은 노드를 배치할 때 블록체인 콘솔에서 볼 수 있습니다.

**참고:**  

- vCPU는 서버가 가상 머신에 대해 파티션되지 않은 경우 가상 머신 또는 실제 프로세서 코어에 지정되는 가상 코어입니다. {{site.data.keyword.cloud_notm}} Private에서 배치를 위해 가상 프로세서 코어(VPC)를 결정할 때 vCPU 요구사항을 고려해야 합니다. VPC는 {{site.data.keyword.IBM_notm}} 제품의 라이센싱 비용을 판별하는 측정 단위입니다. VPC 결정 시나리오에 대한 자세한 정보는 [가상 프로세서 코어(VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}를 참조하십시오.
- vCPU = 1개의 CPU = 1개의 VPC

### 스토리지 고려사항
{: #icp-console-setup-storage-considerations}

{{site.data.keyword.blockchainfull_notm}} Helm 차트는 동적 프로비저닝을 사용하여 작성하는 블록체인 컴포넌트 및 콘솔에서 사용할 스토리지를 프로비저닝합니다. 콘솔을 배치하기 전에 콘솔 및 컴포넌트에 대한 충분한 양의 백업 스토리지가 있는 storageClass를 작성해야 합니다. 구성 중에 작성한 storageClass의 이름을 제공해야 합니다.

- 기본 설정을 사용하는 경우 콘솔 데이터에 대해 이름이 Helm 릴리스인 지속적 볼륨 청구가 Helm 차트에서 새로 작성됩니다.
- NFS v2/v3 지속적 볼륨을 사용하는 경우 NFS 파일 시스템이 존재하는 호스트 시스템에서 **NFSv2/v3 파일 시스템 잠금을 위한 NFS 상태 모니터** 모듈(**rpc-statd**)을 사용으로 설정해야 합니다. 이 모듈은 NFS 파일 시스템에서 다른 프로세스가 보유한 파일에 대한 독점적 잠금을 확인할 수 있도록 해줍니다. 이 모듈을 사용으로 설정하려면 다음 명령을 실행하십시오.

  ```
  sudo systemctl enable rpc-statd
  ```
  {:codeblock}

  ```
  sudo systemctl start rpc-statd
  ```
  {:codeblock}

## {{site.data.keyword.cloud_notm}} Private 설치
{: #icp-console-setup-install}

사용자 환경에 {{site.data.keyword.cloud_notm}} Private을 설치 및 설정하려면 다음 단계를 완료하십시오.

1. [{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/kc_welcome_containers.html){: external} 클러스터를 v3.2.0에 설치하십시오.

2. {{site.data.keyword.cloud_notm}} Private CLI [3.2.0](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_cli.html){: external}을 설치하여 Helm 차트를 설치하십시오.

3. {{site.data.keyword.blockchainfull_notm}} Platform 배치용 사용자 정의 네임스페이스를 새로 작성하십시오. 네임스페이스당 하나의 Helm 차트만 배치할 수 있으므로 콘솔의 다중 인스턴스가 동일한 클러스터에서 실행되도록 하려면 별도의 네임스페이스를 사용해야 합니다.

4. 대상 네임스페이스에 대한 보안 및 액세스 정책을 설정하십시오. 지시사항은 [다음 절](#icp-console-setup-psp)에서 제공됩니다.

{{site.data.keyword.cloud_notm}} Private을 설치하고 팟(Pod) 보안 정책을 대상 네임스페이스에 바인드한 후에도 계속 {{site.data.keyword.cloud_notm}} Private 클러스터로 [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private Helm 차트를 가져올](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install) 수 있습니다.

## PodSecurityPolicy 요구사항
{: #icp-console-setup-psp}

{{site.data.keyword.blockchainfull_notm}} Platform Helm 차트를 사용하려면 설치 전에 특정 보안 및 액세스 정책을 대상 네임스페이스에 바인드해야 합니다. 아래 단계에 있는 정책을 정의하는 YAML 파일을 제공합니다. 이 파일을 로컬 시스템에 저장한 후 {{site.data.keyword.cloud_notm}} Private CLI를 사용하여 네임스페이스에 바인드할 수 있습니다. {{site.data.keyword.blockchainfull_notm}} Platform Helm 차트를 배치하기 전에 아래의 단계를 따르십시오. 

1. {{site.data.keyword.blockchainfull_notm}} Platform PodSecurityPolicy를 정의하는 아래 파일을 로컬 시스템의 `ibm-blockchain-platform-psp.yaml`로 저장하십시오. 

    ```
    apiVersion: extensions/v1beta1
    kind: PodSecurityPolicy
    metadata:
      name: ibm-blockchain-platform-psp
    spec:
      hostIPC: false
      hostNetwork: false
      hostPID: false
      privileged: true
      allowPrivilegeEscalation: true
      readOnlyRootFilesystem: false
      seLinux:
        rule: RunAsAny
      supplementalGroups:
        rule: RunAsAny
      runAsUser:
        rule: RunAsAny
      fsGroup:
        rule: RunAsAny
      requiredDropCapabilities:
      - ALL
      allowedCapabilities:
      - NET_BIND_SERVICE
      - CHOWN
      - DAC_OVERRIDE
      - SETGID
      - SETUID
      - FOWNER
      volumes:
      - '*'
    ```
    {:codeblock}

2. PodSecurityPolicy에 필요한 ClusterRole을 정의하는 아래 파일을 `ibm-blockchain-platform-clusterrole.yaml`로 저장하십시오.

    ```
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      annotations:
      name: ibm-blockchain-platform-clusterrole
    rules:
    - apiGroups:
      - extensions
      resourceNames:
      - ibm-blockchain-platform-psp
      resources:
      - podsecuritypolicies
      verbs:
      - use
    - apiGroups:
      - "*"
      resources:
      - pods
      - services
      - endpoints
      - persistentvolumeclaims
      - persistentvolumes
      - events
      - configmaps
      - secrets
      - ingresses
      - roles
      - rolebindings
      - serviceaccounts
      verbs:
      - '*'
    - apiGroups:
      - apiextensions.k8s.io
      resources:
      - persistentvolumeclaims
      - persistentvolumes
      - customresourcedefinitions
      verbs:
      - '*'
    - apiGroups:
      - ibp.com
      resources:
      - '*'
      - ibpservices
      - ibpcas
      - ibppeers
      - ibpfabproxies
      - ibporderers
      verbs:
      - '*'
    - apiGroups:
      - ibp.com
      resources:
      - '*'
      verbs:
      - '*'
    - apiGroups:
      - apps
      resources:
      - deployments
      - daemonsets
      - replicasets
      - statefulsets
      verbs:
      - '*'
    ```
    {:codeblock}

3. ClusterRoleBinding을 정의하는 아래 파일을 로컬 시스템에 `ibm-blockchain-platform-clusterrolebinding.yaml`로 저장하십시오. 아래 파일에서 ServiceAccount 이름을 변경하려는 경우, Helm 차트를 배치할 때 구성 페이지의 **모든 매개변수** 섹션에 있는 `서비스 계정 이름` 필드에 이름을 제공해야 합니다. 

  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRoleBinding
  metadata:
   name: ibm-blockchain-platform-clusterrolebinding
  roleRef:
   apiGroup: rbac.authorization.k8s.io
   kind: ClusterRole
   name: ibm-blockchain-platform-clusterrole
  subjects:
  - kind: ServiceAccount
    name: default
    namespace: default
  ```
  {:codeblock}

PodSecurityPolicy, ClusterRole 및 ClusterRoleBinding YAML 파일을 로컬 시스템에 저장하면 클러스터 관리자는 {{site.data.keyword.cloud_notm}} Private CLI를 사용하여 정책을 네임스페이스에 바인드해야 합니다. 

1. {{site.data.keyword.cloud_notm}} Private 클러스터에 로그인하고 배치의 대상 네임스페이스를 선택하십시오.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```

2. 클러스터의 Docker 이미지 레지스트리에 로그인하십시오. 

  ```
  docker login <cluster_CA_domain>:8500
  ```
   {:codeblock}

3. 다음 명령을 사용하여 정책을 대상 네임스페이스에 적용하십시오. 

  ```
  kubectl apply -f ibm-blockchain-platform-psp.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrole.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrolebinding.yaml
  ```
  {:codeblock}

4. 정책을 적용한 후 서비스 계정에 필요한 권한 레벨을 부여하여 콘솔을 배치해야 합니다. 대상 네임스페이스의 이름을 사용하여 다음 명령을 실행하십시오. 

  ```
  kubectl -n <namespace> create rolebinding ibm-blockchain-platform-clusterrole-rolebinding --clusterrole=ibm-blockchain-platform-clusterrole --group=system:serviceaccounts:<namespace>
  ```
