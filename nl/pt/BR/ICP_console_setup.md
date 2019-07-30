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

O gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform requer que as políticas específicas de segurança e acesso sejam ligadas ao namespace de destino antes da instalação. Fornecemos os arquivos YAML que definem as políticas nas etapas abaixo. É possível salvar esses arquivos em seu sistema local e, em seguida, ligá-los ao seu namespace usando a CLI do {{site.data.keyword.cloud_notm}} Private. Siga as etapas abaixo antes de implementar o gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform.

1. Salve o arquivo abaixo que define o PodSecurityPolicy do {{site.data.keyword.blockchainfull_notm}} Platform como `ibm-blockchain-platform-psp.yaml` em seu sistema local:

    ```
    apiVersion: extensions/v1beta1 kind: PodSecurityPolicy metadata: name: ibm-blockchain-platform-psp spec: hostIPC: false hostNetwork: false hostPID: false privileged: true allowPrivilegeEscalation: true readOnlyRootFilesystem: false seLinux: rule: RunAsAny supplementalGroups: rule: RunAsAny runAsUser: rule: RunAsAny fsGroup: rule: RunAsAny requiredDropCapabilities:
      - TODAS as allowedCapabilities:
      - NET_BIND_SERVICE
      - CHOWN
      - DAC_OVERRIDE
      - SETGID
      - SETUID
      - Volumes FOWNER:
      - '*'
    ```
    {:codeblock}

2. Salve o arquivo abaixo que define o ClusterRole necessário para o PodSecurityPolicy como `ibm-blockchain-platform-clusterrole.yaml`:

    ```
    apiVersion: rbac.authorization.k8s.io/v1 kind: ClusterRole metadata: annotations: name: ibm-blockchain-platform-clusterrole rules:
    - apiGroups:
      - resourceNames de extensões:
      - recursos de ibm-blockchain-platform-psp:
      - verbos de podsecuritypolicies:
      - use
    - apiGroups:
      - "*"
      recursos:
      - pods
      - services
      - nós de extremidades
      - persistentvolumeclaims
      - persistentvolumes
      - events
      - configmaps
      - segredos
      - ingressos
      - funções
      - Roleligações
      - verbos serviceaccounts:
      - '*'
    - apiGroups:
      - recursos apiextensions.k8s.io:
      - persistentvolumeclaims
      - persistentvolumes
      - verbos customresourcedefinitions:
      - '*'
    - apiGroups:
      - Recursos do ibp.com:
      - '*'
      - ibpservices
      - ibpcas
      - ibppeers
      - ibpfabproxies
      - verbos ibporderers:
      - '*'
    - apiGroups:
      - Recursos do ibp.com:
      - '*'
      verbos:
      - '*'
    - apiGroups:
      - recursos de aplicativos:
      - implementações de produção
      - daemonsets
      - replicasets
      - verbos statefulsets:
      - '*'
    ```
    {:codeblock}

3. Salve o arquivo abaixo que define o ClusterRoleBinding como `ibm-blockchain-platform-clusterrolebinding.yaml`. Se você decidir mudar o nome do ServiceAccount no arquivo abaixo, será necessário fornecer o nome para o campo `Nome da conta do serviço` na seção **Todos os parâmetros** da página de configuração ao implementar o gráfico do Helm.

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

Depois de ter salvado os arquivos YAML PodSecurityPolicy, ClusterRole e ClusterRoleBinding em seu sistema local, um administrador de cluster precisará usar a CLI do {{site.data.keyword.cloud_notm}} Private para ligar as políticas ao seu namespace.

1. Efetue login em seu cluster do {{site.data.keyword.cloud_notm}} Private e selecione o namespace de destino de sua implementação.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```

2. Efetue login no registro de imagem do docker para o seu cluster:

  ```
  docker login <cluster_CA_domain>:8500
  ```
   {:codeblock}

3. Use os comandos a seguir para aplicar as políticas em seu namespace de destino:

  ```
  kubectl apply -f ibm-blockchain-platform-psp.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrole.yaml
  kubectl apply -f ibm-blockchain-platform-clusterrolebinding.yaml
  ```
  {:codeblock}

4. Depois de aplicar as políticas, deve-se conceder à sua conta de serviço o nível necessário de permissões para implementar o seu console. Execute o comando a seguir com o nome de seu namespace de destino:

  ```
  kubectl -n <namespace> create rolebinding ibm-blockchain-platform-clusterrole-rolebinding --clusterrole=ibm-blockchain-platform-clusterrole --group=system:serviceaccounts:<namespace>
  ```
