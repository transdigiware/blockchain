---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

# Configurando o  {{site.data.keyword.cloud_notm}}  Privado
{: #icp-console-setup}

Antes de implementar os componentes do {{site.data.keyword.blockchainfull}} Platform e construir a rede de blockchain no {{site.data.keyword.cloud_notm}} Private, é necessário configurar o {{site.data.keyword.cloud_notm}} Privado em seu próprio ambiente.
{:shortdesc}

## Pré-requisitos
{: #icp-console-setup-prerequisites}

Preencha os pré-requisitos a seguir e prepare seu ambiente para instalar o {{site.data.keyword.cloud_notm}} Private.

### Docker
O {{site.data.keyword.cloud_notm}} Private requer que o Docker esteja instalado. Siga as instruções relacionadas em [Instalando o {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/install.html){: external} para instalar o Docker.

### {{site.data.keyword.cloud_notm}}  Configurações privadas
Antes de instalar o {{site.data.keyword.cloud_notm}} Private, as dicas a seguir são úteis para preparar seus nós para a instalação do {{site.data.keyword.cloud_notm}} Private. Os pré-requisitos adicionais do {{site.data.keyword.cloud_notm}} Private podem ser localizados na [documentação do {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep.html){: external}.

#### Atualize a configuração de `vm.max_map_count`
O {{site.data.keyword.cloud_notm}} Private usa o Elastic Search para criação de log e medição. Para evitar exceções de falta de memória, o Elastic Search requer que a propriedade de sistema `vm.max_map_count` seja configurada. Antes de instalar o {{site.data.keyword.cloud_notm}} Private, consulte as [Instruções de configuração de procura elástica](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html){: external} para configurar essa propriedade em cada nó. É possível usar os comandos a seguir para configurar essa propriedade permanentemente:

```
sysctl -w vm.max_map_count=262144; sysctl vm.max_map_count
echo "vm.max_map_count=262144” | tee -a /etc/sysctl.conf
```
{:codeblock}

#### Configure o arquivo `/etc/hosts` em cada nó em seu cluster

- O {{site.data.keyword.cloud_notm}} Private usa [Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/){: external} para gerenciar aplicativos conteinerizados. O servidor de nomes de domínio (DNS) do Kubernetes falhará se os nomes de host não estiverem configurados no arquivo `/etc/hosts` em cada nó. [Insira o endereço IP, o nome do host e o nome abreviado de cada nó em seu cluster](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/installing/prep_cluster.html){: external} no arquivo `/etc/hosts` em cada nó.

- O [IPv6 não é suportado pelo {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#ipv6){: external}. Para evitar problemas com o serviço DNS em um cluster do {{site.data.keyword.cloud_notm}} Private, desative as configurações de IPv6 no arquivo `/etc/hosts` em cada nó, comentando a linha a seguir com um sinal `#` no início da linha:
  ```
  #::1  localhost ip6-localhost ip6-loopback
  ```
  {:codeblock}

## Recursos necessários
{: #icp-console-setup-resources}

Certifique-se de que seu sistema do {{site.data.keyword.cloud_notm}} Private atenda aos requisitos mínimos de recurso de hardware para cada componente de tempo de execução do Fabric:

| **Componente** (todos os contêineres) | vCPU  | Memória (GB) | Armazenamento (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Console**                    | 1.3            | 2.5                   | 10                     |
| **Peer**                       | 1,2            | 2.4                   | 200 (inclui 100 GB para peer e 100 GB para CouchDB)|
| **CA**                         | 0,1            | 0,2                   | 20                     |
| **Solicitador**                    | 0,45           | 0,9                   | 100                    |

 **Notas:**
 - Um vCPU é um núcleo virtual que será designado a uma máquina virtual ou a um núcleo do processador físico se o servidor não for particionado para máquinas virtuais. É necessário considerar os requisitos de vCPU ao decidir o virtual processor core (VPC) de sua implementação no {{site.data.keyword.cloud_notm}} Private. VPC é uma unidade de medida para determinar o custo de licenciamento de produtos {{site.data.keyword.IBM_notm}}. Para obter mais informações sobre cenários para decidir o VPC, consulte [núcleo do processador virtual (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
 - Uma vCPU é equivalente a uma CPU, que é equivalente a um VPC.

### Considerações sobre armazenamento
{: #icp-console-setup-storage-considerations}

* É necessário criar uma nova classe de armazenamento com recursos suficientes para seu console e os componentes criados. A classe de armazenamento fornecida para o console durante a configuração também será usada para armazenar dados do componente.
* Se você usar as configurações padrão, o gráfico do Helm criará uma nova solicitação de volume persistente com o nome de sua liberação do Helm para os dados do console.
* O [fornecimento dinâmico](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/){: external} está disponível somente para nós amd64 no {{site.data.keyword.cloud_notm}} Private. Portanto, se o seu cluster incluir uma combinação de nós do trabalhador s390x e amd64, o fornecimento dinâmico não poderá ser usado.
* Se o fornecimento dinâmico não for usado, os [Volumes persistentes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external} deverão ser criados e configurados com rótulos que podem ser usados para refinar o processo de ligação de solicitação de volume persistente (PVC) do Kubernetes.
* Se você usar Volumes Persistentes do NFS v2/v3, deverá ativar o módulo **Monitor de status NFS para os bloqueios de sistema de arquivos NFSv2/v3**, que também é conhecido como **rpc-statd**, no sistema host no qual o sistema de arquivos NFS existe. Esse módulo permite que o sistema de arquivos NFS verifique bloqueios exclusivos em arquivos que outros processos retêm. Execute os comandos a seguir para ativar esse módulo:

  ```
  sudo systemctl enable rpc-statd
  ```
  {:codeblock}

  ```
  sudo systemctl start rpc-statd
  ```
  {:codeblock}

## Instalando o  {{site.data.keyword.cloud_notm}}  Privado
{: #icp-console-setup-install}

Conclua as etapas a seguir para instalar e configurar o {{site.data.keyword.cloud_notm}} Private em seu ambiente.

1. Instale um cluster do [{{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/kc_welcome_containers.html){: external} na v3.2.0.

2. Instale a CLI do {{site.data.keyword.cloud_notm}} Private [3.2.0](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_cli.html){: external} para instalar o gráfico do Helm.

3. Configure a política de segurança do pod para o namespace de destino. Instruções são fornecidas na [próxima seção](#icp-setup-psp).

Depois de instalar o {{site.data.keyword.cloud_notm}} Private e ligar uma política de segurança de pod a um namespace de destino, será possível continuar a [importação do gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install) em seu cluster do {{site.data.keyword.cloud_notm}} Private.

## Requisitos de PodSecurityPolicy
{: #icp-console-setup-psp}

O gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform requer que um [PodSecurityPolicy](https://kubernetes.io/docs/concepts/policy/pod-security-policy/){: external} seja ligado ao namespace de destino antes da instalação. Escolha um PodSecurityPolicy predefinido ou peça que o administrador de cluster crie um PodSecurityPolicy customizado para você:
- Nome do PodSecurityPolicy predefinido: [`ibm-privileged-psp`](https://ibm.biz/cpkspec-psp)
- Definição de PodSecurityPolicy customizado:
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
  {:codeblock}
- ClusterRole customizado para o PodSecurityPolicy customizado:
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
  {:codeblock}

- ClusterRoleBinding customizado para o ClusterRole customizado:
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
