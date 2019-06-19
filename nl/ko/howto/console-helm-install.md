---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Cloud Private, IBM Blockchain Platform, install, Helm chart, PodSecurityPolicy

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

# {{site.data.keyword.blockchainfull_notm}} Platform Helm 차트 설치
{: #console-helm-install}

{{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private은 로컬 {{site.data.keyword.cloud_notm}} Private 클러스터에 설치할 수 있는 Helm 차트로 제공됩니다. Helm 차트를 설치한 후에는 {{site.data.keyword.cloud_notm}} Private 카탈로그에서 애플리케이션으로 {{site.data.keyword.blockchainfull_notm}} Platform을 찾을 수 있습니다.

Helm 차트는 [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}을 통해 구매해야 합니다. 구매 시 {{site.data.keyword.blockchainfull_notm}} Platform을 위한 기술 지원이 포함됩니다.

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private을 설치하기 전에 [고려사항 및 제한사항](/docs/services/blockchain/console-icp-about.html#console-icp-about-considerations)을 검토하십시오. 가격 책정, 지원, 보안 및 데이터 상주 고려사항에 대한 자세한 정보는 [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private 정보](/docs/services/blockchain/console-icp-about.html#console-icp-about)를 참조하십시오. 

## Helm 차트를 설치하기 위한 전제조건
{: #console-helm-install-prereqs}

Helm 차트를 설치하기 전에 {{site.data.keyword.cloud_notm}} Private 클러스터를 구성하고 팟(Pod) 보안 정책에 바인드되는 새 대상 네임스페이스를 작성해야 합니다. [{{site.data.keyword.cloud_notm}} Private 클러스터 설정 및 구성](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup) 지시사항을 검토하십시오. 예를 들어 개발, 스테이징 및 프로덕션 목적의 여러 환경을 작성하기 위해 블록체인 네트워크를 여러 개 작성하려는 경우 고유한 네임스페이스를 환경별로 작성해야 합니다. 

### PodSecurityPolicy 요구사항
{: #console-helm-install-prereqs-pod-security-requirements}

{{site.data.keyword.blockchainfull_notm}} Platform Helm 차트를 사용하려면 설치 전에 PodSecurityPolicy를 대상 네임스페이스에 바인드해야 합니다. 사전 정의된 PodSecurityPolicy를 선택하거나 클러스터 관리자가 사용자를 위해 사용자 정의 PodSecurityPolicy를 작성하도록 하십시오.
- 사전 정의된 PodSecurityPolicy 이름: [`ibm-privileged-psp`](https://ibm.biz/cpkspec-psp)
- 사용자 정의 PodSecurityPolicy 정의:
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
    volumes:
    - '*'
  ```
- 사용자 정의 PodSecurityPolicy를 위한 사용자 정의 ClusterRole:
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
    - ""
    resources:
    - secrets
    verbs:
    - create
    - delete
    - get
    - list
    - patch
    - update
    - watch
  ```
- 사용자 정의 ClusterRole을 위한 사용자 정의 ClusterRoleBinding:
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

## Helm 차트를 {{site.data.keyword.cloud_notm}} Private에 가져오기
{: #console-helm-install-importing}

1. {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private의 Helm 차트를 [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}에서 다운로드하십시오. 

2. 아직 로그인하지 않은 경우 {{site.data.keyword.cloud_notm}} Private 클러스터에 로그인하십시오.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```
  {:codeblock}

3. [Docker CLI](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html)가 구성되어 있는지 확인하십시오. Docker CLI를 구성한 후 다음 명령을 사용하여 클러스터의 이미지 레지스트리에 액세스하십시오.
  ```
  docker login <cluster_CA_domain>:8500
  ```
  {:codeblock}

4. 다음 명령을 사용하여 {{site.data.keyword.cloud_notm}} Private에서 저장소의 이름을 찾아 Helm 차트를 업로드하십시오.
  ```
  cloudctl catalog repos
  ```
  {:codeblock}

  이 명령이 완료되면 클러스터에서 저장소의 목록을 볼 수 있습니다. 대상 저장소의 이름을 선택하고 이를 저장하십시오. 아래 명령에서 이 이름을 사용해야 합니다.

5. 명령행을 사용하여 Helm 차트를 가져오십시오. PPA에서 다운로드한 Helm 차트를 저장한 디렉토리에서 Helm 차트를 {{site.data.keyword.cloud_notm}} Private 클러스터에 가져오려면 {{site.data.keyword.cloud_notm}} Private CLI에서 다음 명령을 실행하십시오.

  ```
    cloudctl catalog load-archive --archive <archive-name> --registry <cluster_CA_domain>:8500 --repo <repo-name>
  ```
  {:codeblock}

  다음 값을 대체하십시오.
  - `<archive-name>`(다운로드한 `.tgz` 파일의 이름 포함)
  - `<cluster_CA_domain>:8500`({{site.data.keyword.cloud_notm}} Private 클러스터에 로그인하는 데 사용하는 도메인 포함)
  - `<repo-name>`(차트를 업로드할 Helm 저장소 포함). 저장소를 나열하려면 'cloudctl catalog repos'를 실행하십시오.

  이 명령이 완료되면 다음 정보와 비슷한 내용이 표시됩니다.

  ```  
  Loading Helm chart
  Loaded Helm chart

  Synch charts on repo: <repo-name>
  OK
  ```  
  </details>

{{site.data.keyword.cloud_notm}} Private 콘솔에서 **카탈로그** 단추를 클릭한 후 왼쪽 탐색 분할창에서 **블록체인**을 클릭하십시오. 가져오기에 성공한 경우 **ibm-blockchain-platform-prod** 타일이 {{site.data.keyword.cloud_notm}} Private Catalog 페이지에 표시됩니다. 

## 다음 단계
{: #console-helm-install-next-steps}

Helm 차트를 설치한 후 {{site.data.keyword.cloud_notm}} Private 카탈로그의 **ibm-blockchain-platform-prod** 타일을 사용하여 {{site.data.keyword.blockchainfull_notm}} Platform 콘솔을 배치할 수 있습니다. 배치용 대상 네임스페이스를 새로 작성한 후 구성 페이지를 완료하기 전에 {{site.data.keyword.blockchainfull_notm}} Platform 컴포넌트의 리소스가 충분한지 확인해야 합니다. 자세한 정보는 [{{site.data.keyword.cloud_notm}} Private에 {{site.data.keyword.blockchainfull_notm}} 콘솔 배치](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp)를 참조하십시오. 
