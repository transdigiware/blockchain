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

Assegure-se de que o seu sistema {{site.data.keyword.cloud_notm}} Private atenda aos requisitos mínimos de recurso de hardware para o console e os componentes que você cria. O número de vCPU/CPUs que são necessários pode variar, dependendo de sua infraestrutura, do design de rede e dos requisitos de desempenho. Uma aproximação de seus requisitos de vCPU/CPU pode ser feita examinando a [tabela de alocações de recursos padrão](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing#ibp-saas-pricing-default) para o {{site.data.keyword.cloud_notm}}, embora as alocações sejam um pouco diferentes no {{site.data.keyword.cloud_notm}} Private. As suas reais alocações de recurso ficam visíveis em seu console de blockchain quando você implementa um nó.

**Notas:**  

- Um vCPU é um núcleo virtual que será designado a uma máquina virtual ou a um núcleo do processador físico se o servidor não for particionado para máquinas virtuais. É necessário considerar os requisitos de vCPU ao decidir o virtual processor core (VPC) de sua implementação no {{site.data.keyword.cloud_notm}} Private. VPC é uma unidade de medida para determinar o custo de licenciamento de produtos {{site.data.keyword.IBM_notm}}. Para obter mais informações sobre cenários para decidir o VPC, consulte [núcleo do processador virtual (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
- Uma vCPU é equivalente a uma CPU, que é equivalente a um VPC.

### Considerações sobre armazenamento
{: #icp-console-setup-storage-considerations}

O gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} usa o fornecimento dinâmico para provisionar o armazenamento que será usado pelo console e pelos componentes de blockchain que você criará. Antes de implementar o console, é necessário criar uma storageClass com uma quantia suficiente de armazenamento auxiliar para o seu console e os seus componentes. É necessário fornecer o nome da storageClass que você criou durante a configuração.

- Se você usar as configurações padrão, o gráfico do Helm criará uma nova solicitação de volume persistente com o nome da liberação do Helm para os dados do console.
- Se você usar Volumes Persistentes do NFS v2/v3, deverá ativar o módulo **Monitor de status NFS para os bloqueios de sistema de arquivos NFSv2/v3**, que também é conhecido como **rpc-statd**, no sistema host no qual o sistema de arquivos NFS existe. Esse módulo permite que o sistema de arquivos NFS verifique bloqueios exclusivos em arquivos que outros processos retêm. Execute os comandos a seguir para ativar esse módulo:

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

3. Crie um novo namespace customizado para sua implementação do {{site.data.keyword.blockchainfull_notm}} Platform. Observe que é possível implementar apenas um gráfico do Helm por namespace. Portanto, se você desejar que múltiplas instâncias do console sejam executadas no mesmo cluster, certifique-se de usar namespaces separados.

4. Configure as políticas de segurança e acesso para o namespace de destino. Instruções são fornecidas na [próxima seção](#icp-console-setup-psp).

Depois de instalar o {{site.data.keyword.cloud_notm}} Private e ligar uma política de segurança de pod a um namespace de destino, será possível continuar a [importação do gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#console-helm-install) em seu cluster do {{site.data.keyword.cloud_notm}} Private.

## Requisitos de PodSecurityPolicy
{: #icp-console-setup-psp}

O gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform requer que as políticas específicas de segurança e acesso sejam ligadas ao namespace de destino antes da instalação. Use as etapas a seguir para configurar as políticas antes da configuração do Gráfico do Helm:

1. Escolha uma PodSecurityPolicy predefinida para o seu namespace ou peça ao seu administrador de cluster para criar uma PodSecurityPolicy customizada para você:
  - É possível usar a PodSecurityPolicy predefinida de [`ibm-privileged-psp`](https://ibm.biz/cpkspec-psp)
  - Também é possível criar usando o YAML abaixo de uma definição de PodSecurityPolicy customizada:

    ```
    apiVersion: extensions/v1beta1 kind: PodSecurityPolicy metadata: name: ibm-blockchain-platform-psp spec: hostIPC: false hostNetwork: false hostPID: false privileged: true allowPrivilegeEscalation: true readOnlyRootFilesystem: false seLinux: rule: RunAsAny supplementalGroups: rule: RunAsAny runAsUser: rule: RunAsAny fsGroup: rule: RunAsAny requiredDropCapabilities:
      - TODAS as allowedCapabilities:
      - NET_BIND_SERVICE
      - CHOWN
      - DAC_OVERRIDE
      - SETGID
      - Volumes de SETUID:
      - '*'
    ```
    {:codeblock}

2. Crie uma ClusterRole para a PodSecurityPolicy.
  - Se você tiver criado uma política de segurança customizada, será possível criar uma ClusterRole usando o arquivo do YAML abaixo:

    ```
    apiVersion: rbac.authorization.k8s.io/v1 kind: ClusterRole metadata: annotations: name: ibm-blockchain-platform-clusterrole rules:
    - apiGroups:
      - resourceNames de extensões:
      - recursos de ibm-blockchain-platform-psp:
      - verbos de podsecuritypolicies:
      - use
    - apiGroups:
      - ""
      recursos:
      - verbos de segredos:
      - create
      - excluir
      - get
      - listar
      - correção
      - atualização
      - observar
    ```
    {:codeblock}

  - Se você estiver usando uma PodSecurityPolicy predefinida, será necessário apenas criar uma ClusterRole usando a segunda seção de apiGroups:

    ```
    apiVersion: rbac.authorization.k8s.io/v1 kind: ClusterRole metadata: annotations: name: ibm-blockchain-platform-clusterrole rules:
      - apiGroups:
      - ""
      recursos:
      - verbos de segredos:
      - create
      - excluir
      - get
      - listar
      - correção
      - atualização
      - observar
    ```
    {:codeblock}

3. Crie uma ClusterRoleBinding customizada. Se você decidir mudar o nome da ServiceAccount no arquivo abaixo, será necessário fornecer o nome para o campo `Service account name` na Seção **Todos os parâmetros** da página Configuração ao implementar o gráfico do Helm.

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

É possível concluir as etapas a seguir para usar arquivos do YAML para ligar as políticas de segurança e acesso ao seu namespace:

1. Salve o arquivo do YAML em seu sistema local.

2. Efetue login em seu cluster do {{site.data.keyword.cloud_notm}} Private e selecione o namespace de destino de sua implementação.

  ```
  docker login <cluster_CA_domain>:8500
  ```
   {:codeblock}

3. Use o comando a seguir para aplicar a política ao namespace de destino:

  ```
  kubectl apply -f <filename>.yaml
  ```
   {:codeblock}
