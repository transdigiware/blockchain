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

# Instalando o gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform
{: #console-helm-install}

O {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private é entregue como um gráfico do Helm que pode ser instalado em um cluster local do {{site.data.keyword.cloud_notm}} Private. Depois de instalar o gráfico do Helm, é possível localizar o {{site.data.keyword.blockchainfull_notm}} Platform como um aplicativo no catálogo do {{site.data.keyword.cloud_notm}} Private.

O gráfico do Helm deve ser comprado por meio do [Passport Advantage on-line](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}. Mediante a compra, o suporte técnico para o {{site.data.keyword.blockchainfull_notm}} Platform é incluído.

Antes de instalar o {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private, revise as [Considerações e limitações](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about-considerations). Para obter mais informações sobre precificação, suporte e considerações sobre residência de dados e segurança, consulte [Sobre o {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-about#console-icp-about).

## Pré-requisitos para instalar o gráfico Helm
{: #console-helm-install-prereqs}

Antes de instalar o gráfico do Helm, deve-se ter configurado um cluster do {{site.data.keyword.cloud_notm}} Private e criado um novo namespace de destino que esteja ligado a uma política de segurança de pod. Revise as instruções para [definir e configurar um cluster do {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup). É possível implementar apenas um console por namespace. Se planeja criar várias redes de blockchain, por exemplo, para criar diferentes ambientes para desenvolvimento, preparação e produção, é necessário criar um namespace exclusivo para cada ambiente.

### Requisitos de PodSecurityPolicy
{: #console-helm-install-prereqs-pod-security-requirements}

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

## Importando o gráfico Helm para  {{site.data.keyword.cloud_notm}}  Privado
{: #console-helm-install-importing}

1. Faça download do gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private por meio do [Passport Advantage on-line](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}.

2. Caso ainda não tenha feito isso, efetue login em seu cluster do {{site.data.keyword.cloud_notm}} Private.

  ```
  cloudctl login -a https://<cluster_CA_domain>:8443 --skip-ssl-validation
  ```
  {:codeblock}

3. Assegure-se de que a [CLI do Docker](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/manage_images/configuring_docker_cli.html) esteja configurada. Depois de configurar a CLI do Docker, acesse o registro de imagem em seu cluster usando o comando a seguir:
  ```
  docker login <cluster_CA_domain>:8500
  ```
  {:codeblock}

4. Localize o nome do repositório no {{site.data.keyword.cloud_notm}} Private para fazer upload de seu gráfico do Helm usando o comando a seguir:
  ```
  cloudctl catalog repos
  ```
  {:codeblock}

  Quando esse comando for concluído com êxito, será possível ver uma lista de repositórios em seu cluster. Escolha o nome do repositório de destino e salve-o. É necessário usá-lo no comando a seguir.

5. Importe o gráfico Helm usando a linha de comandos. No diretório em que você armazenou o pacote de gráfico do Helm transferido por download do PPA, execute o comando a seguir na CLI do {{site.data.keyword.cloud_notm}} Private para importar o gráfico do Helm para seu cluster do {{site.data.keyword.cloud_notm}} Private.

  ```
  cloudctl catalog load-archive --archive <archive-name> --registry <cluster_CA_domain>:8500 --repo <repo-name>
  ```
  {:codeblock}

  Substitua os seguintes valores:
  - `<archive-name>` pelo nome do arquivo `.tgz` transferido por download.
  - `<cluster_CA_domain>:8500` com o domínio que você usa para efetuar login no seu cluster do {{site.data.keyword.cloud_notm}} Private.
  - `<repo-name>` com o repositório do helm no qual você deseja fazer upload do gráfico. Execute 'cloudctl catalog repos' para listar seus repositórios.

  Quando esse comando for concluído com êxito, você verá algo semelhante às informações a seguir:

  ```  
  Uploading Helm chart(s)
   Processing chart: charts/ibm-blockchain-platform-prod-1.1.0.tgz
   Updating chart values.yaml
   Uploading chart
  Loaded Helm chart
  OK

  Synch charts
  OK

  Archive finished processing
  ```  
  </details>

Clique no botão **Catálogo** no console do {{site.data.keyword.cloud_notm}} Private e, em seguida, clique em **Blockchain** no painel de navegação à esquerda. Se a importação foi bem-sucedida, o bloco **ibm-blockchain-platform-prod** estará visível na página Catálogo do {{site.data.keyword.cloud_notm}} Private.

## Próximas etapas
{: #console-helm-install-next-steps}

Depois de instalar o gráfico do Helm, é possível usar o bloco **ibm-blockchain-platform-prod** em seu Catálogo do {{site.data.keyword.cloud_notm}} Private para implementar o console do {{site.data.keyword.blockchainfull_notm}} Platform. É necessário criar um novo namespace de destino para a implementação e assegurar-se de que seu cluster tenha recursos suficientes para seus componentes do {{site.data.keyword.blockchainfull_notm}} Platform antes de concluir a página de configuração. Para obter mais informações, consulte [Implementando o console do {{site.data.keyword.blockchainfull_notm}} no {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp).
