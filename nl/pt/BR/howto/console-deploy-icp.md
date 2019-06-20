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

# Implementando o console do {{site.data.keyword.blockchainfull_notm}}
{: #console-deploy-icp}

Use as instruções a seguir para implementar o console do {{site.data.keyword.blockchainfull}} Platform em seu cluster local do {{site.data.keyword.cloud_notm}} Private depois de ter instalado o gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform.
{:shortdesc}

## Recursos necessários
{: ##console-deploy-icp-resources-required}

Assegure-se de que seu sistema {{site.data.keyword.cloud_notm}} Private atenda aos requisitos mínimos de recurso de hardware para o console e os componentes que você cria:

| **Componente** (todos os contêineres) | CPU  | Memória (GB) | Armazenamento (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Console** | 1.3 | 2.5 | 10 |
| **Peer** | 1,2 | 2.4 | 200 (inclui 100 GB para peer e 100 GB para CouchDB)|
| **CA** | 0,1 | 0,2  | 20 |
| **Solicitador** | 0,45 | 0,9 | 100 |

 **Notas:**
 - Um vCPU é um núcleo virtual que será designado a uma máquina virtual ou a um núcleo do processador físico se o servidor não for particionado para máquinas virtuais. É necessário considerar os requisitos de vCPU ao decidir o virtual processor core (VPC) de sua implementação no {{site.data.keyword.cloud_notm}} Private. VPC é uma unidade de medida para determinar o custo de licenciamento de produtos {{site.data.keyword.IBM_notm}}. Para obter mais informações sobre cenários para decidir o VPC, consulte [núcleo do processador virtual (VPC)](https://www.ibm.com/support/knowledgecenter/en/SS8JFY_9.2.0/com.ibm.lmt.doc/Inventory/overview/c_virtual_processor_core_licenses.html){: external}.
 - Uma vCPU é equivalente a uma CPU, que é equivalente a um VPC.

## Armazenamento
{: #console-deploy-icp-storage}

Assegure-se de que você tenha criado uma storageClass com a quantia suficiente de armazenamento auxiliar para seu console e os componentes de blockchain.

- É necessário criar uma nova classe de armazenamento com recursos suficientes para seu console e os componentes criados. A classe de armazenamento fornecida para o console durante a configuração também será usada para armazenar dados do componente.
- Se você usar as configurações padrão, o gráfico do Helm criará uma nova solicitação de volume persistente com o nome da liberação do Helm para os dados do console.
- Se o fornecimento dinâmico não for usado, os [Volumes persistentes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/){: external} deverão ser criados e configurados com rótulos que podem ser usados para refinar o processo de ligação da solicitação de volume persistente (PVC) do Kubernetes.

## Pré-requisitos para implementar o console
{: #console-deploy-icp-prerequisites}

1. Antes de poder implementar o console do {{site.data.keyword.blockchainfull_notm}} Platform, deve-se [instalar o {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup) e [instalar o gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/howto/console-helm-install.html#console_helm-install).

2. É necessário criar um novo namespace customizado para a sua implementação do {{site.data.keyword.blockchainfull_notm}} Platform. Seu namespace precisa usar o [PodSecurityPolicy necessário](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install-prereqs-pod-security-requirements). Se planeja criar várias redes de blockchain, por exemplo, para criar diferentes ambientes para desenvolvimento, preparação e produção, é necessário criar um namespace exclusivo para cada ambiente.

3. Recupere o valor do endereço IP do Proxy do cluster de sua CA no console do {{site.data.keyword.cloud_notm}} Private. **Nota:** você precisará ser um [Administrador de cluster](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/user_management/assign_role.html){: external} para acessar seu IP de proxy. Efetue login no cluster do {{site.data.keyword.cloud_notm}} Private. No painel de navegação à esquerda, clique em **Plataforma** e, em seguida, em **Nós** para visualizar os nós que estão definidos no cluster. Clique no nó com a função `proxy` e, em seguida, copie o valor do `IP do host` da tabela. **Importante:** salve esse valor e você o usará ao configurar o campo `Proxy IP` do gráfico do Helm.


Salve esse valor e você o usará quando configurar o campo `Proxy IP` do gráfico do Helm.
{:important}

4. Crie uma senha que será usada para efetuar login no console pela primeira vez e armazene-a em um objeto secreto no {{site.data.keyword.cloud_notm}} Private. É possível localizar as etapas para criar o segredo na [próxima seção](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp-password-secret).

## Criando um segredo de senha do console
{: #console-deploy-icp-password-secret}

Antes de poder acessar seu console, é necessário criar uma senha padrão que você usará para efetuar login no console pela primeira vez. Crie um [Segredo do Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/){: external} para armazenar a senha antes de implementar o console. Um segredo do Kubernetes permite que você proteja e compartilhe informações sem ter que passá-las diretamente para a implementação.

1. Crie uma senha e codifique-a no formato Base64. Execute o comando a seguir em uma janela do terminal e substitua o valor de `password` pelo valor que você deseja usar. **Salve a saída desse comando**.
  ```
  echo -n 'password' | base64
  ```
  {:code_block}

2. Efetue login no console do {{site.data.keyword.cloud_notm}} Private. No painel de navegação à esquerda, clique em **Configuração** e, em seguida, em **Segredos**. Clique no botão **Criar segredo** para abrir um painel pop que permite gerar um novo objeto de segredo.

3. Na guia **Geral**, preencha os campos a seguir:
  - **Nome:** forneça ao seu segredo um nome exclusivo dentro de seu cluster. Você usará esse nome ao implementar o console. O nome deve ser todo em minúsculas.
  - **Namespace:**  o espaço de nomes para incluir seu segredo. Selecione o `namespace` no qual você deseja implementar o console.
  - **Tipo:** insira o valor `generic`.

4. Deixe a guia **Anotações** em branco.

5. Na guia **Dados**, inclua o nome do usuário e a senha como pares chave-valor.
  1. No primeiro campo **Nome**, insira `password`.
  2. No primeiro campo **Valor**, insira o resultado de `echo -n 'password' | base64` da etapa 1.
  3. Clique em **Criar** para formar um novo objeto Segredo.

Forneça o nome do segredo para o campo `Nome do segredo da senha do administrador do console` da página de configuração ao implementa o console. Será necessário usar o valor que você codificou na etapa 1 para efetuar login no console pela primeira vez. Esse valor se tornará a senha padrão do console, a menos que seja mudado por um administrador do console. Para obter mais informações, consulte [Gerenciando usuários por meio do console](/docs/services/blockchain/howto/console-icp-manage.html#console-icp-manage-users).

## Criando o segredo do TLS (opcional)
{: #console-deploy-icp-tls-secret}

O console usa TLS para proteger a comunicação entre o seu console e os nós de blockchain. Por padrão, o console gera seus próprios certificados TLS. No entanto, você tem a opção de fornecer seu próprio certificado e chave TLS. Use as etapas abaixo para armazenar os certificados em um [segredo do Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret/). Então, é possível fornecer esses certificados para o console, fornecendo o nome do segredo para o campo de segredo do TLS durante a configuração.

1. Converta o certificado TLS e a chave privada para o formato Base64. É possível converter o conteúdo do arquivo de certificado ou chave em uma sequência Base64 executando o seguinte comando na máquina local:

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat <cert_or_key> | base64 $FLAG
  ```

  Execute esse comando para o certificado TLS e a chave TLS que o acompanha. **Salve** a saída.

2. Efetue login no console do {{site.data.keyword.cloud_notm}} Private. No painel de navegação à esquerda, clique em **Configuração** e, em seguida, em **Segredos**. Clique no botão **Criar segredo** para abrir um painel pop que permite gerar um novo objeto de segredo.

3. Na guia **Geral**, preencha os campos a seguir:
  - **Nome:** forneça ao seu segredo um nome exclusivo dentro de seu cluster. Você usará esse nome para configurar sua CA. O nome deve ser todo em minúsculas.
  - **Namespace:**  o espaço de nomes para incluir seu segredo. Selecione o `namespace` no qual você deseja implementar sua CA.
  - **Tipo:** insira o valor `kubernetes.io/tls`.

4. Deixe a guia **Anotações** em branco.

5. Na guia **Dados**, inclua o nome do usuário e a senha como pares chave-valor.
  1. No primeiro campo **Nome**, insira `tls.crt`.
  2. No primeiro campo **Valor**, insira a sequência de seu certificado TLS codificado em Base64 da etapa 1.
  3. Clique no botão **Incluir dados** para incluir um segundo par chave-valor.
  4. No primeiro campo **Nome**, insira `tls.key`.
  5. No primeiro campo **Valor**, insira a sequência da chave TLS codificada em Base64 da etapa 1.
  6. Clique em **Criar** para formar um novo objeto Segredo.

Forneça o nome do segredo que você cria para o campo `Segredo TLS` na seção Todos os parâmetros da página de configuração ao implementar seu console.

Segredos não removidos de seu cluster do {{site.data.keyword.cloud_notm}} Private quando você exclui sua liberação do Helm. Você é responsável por gerenciar a senha e os segredos de TLS em seu cluster do {{site.data.keyword.cloud_notm}} Private. Se você planeja implementar outro console no futuro, é possível reutilizar os segredos. Caso contrário, você será responsável por excluí-los de seu cluster do {{site.data.keyword.cloud_notm}} Private.
{:note}

## Configuração
{: #console-deploy-icp-configuration}

Depois de criar o objeto secreto TLS, é possível implementar o console usando as etapas a seguir.

1. Efetue login no console privado do {{site.data.keyword.cloud_notm}} Private e clique em **Catálogo** no canto superior direito.
2. Clique em `Blockchain` no painel de navegação à esquerda e localize o bloco rotulado `ibm-blockchain-platform-prod`. Clique no quadrado para abri-lo. É necessário ver um arquivo leia-me que inclui informações sobre a instalação e a configuração do gráfico do Helm.
3. Clique na guia **Configuração** na parte superior do painel ou clique no botão **Configurar** no canto inferior direito.
4. Especifique os valores para os [Parâmetros de segurança de pod e de configuração](#icp-peer-deploy-configuration-parms) e aceite o contrato de licença.
5. Navegue para a seção **Parâmetros**:
- É possível implementar o console usando apenas os [Parâmetros de iniciação rápida](#icp-peer-deploy-quickstart-parms). Use essa opção se você estiver experimentando ou iniciando.
- É possível usar a seção [Todos os parâmetros](#icp-peer-deploy-quickstart-parms) para customizar o acesso à rede, os recursos e o armazenamento usados pelo console. A seção **Todos os parâmetros** é recomendada apenas para usuários mais experientes do Kubernetes.
6. Clique em **Instalar**.

As tabelas a seguir descrevem os campos de parâmetro de configuração e seus valores padrão.

### Parâmetros de segurança de pod e de configuração
{: #icp-peer-deploy-configuration-parms}

|  Parâmetro     | Descrição    | Padrão  | Requerido |
| --------------|-----------------|-------|------- |
| `Helm release name`| Nome para identificar a implementação da liberação do Helm. Comece com uma letra minúscula e termine com qualquer caractere alfanumérico. O nome deve conter apenas hifens e caracteres alfanuméricos minúsculos. | Nenhum | Sim |
| ` Namespace de destino `| Especifique o namespace que você criou para o console e os componentes. O namespace deve incluir uma política `ibm-privileged-psp`. Caso contrário, [ligue uma PodSecurityPolicy](https://cloud.ibm.com/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-psp) ao seu namespace. | Nenhum | Sim |
| `Target Cluster`| Especifique os clusters nos quais você gostaria de implementar o recurso. Os clusters disponíveis são filtrados pelo namespace selecionado e os requisitos de versão do Kubernetes. | Nenhum | Sim |
| `Políticas de namespace de destino`| Pré-configurado para usar seu namespace de destino. Os valores devem incluir a política `ibm-privileged-psp` ou `ibm-blockchain-platform-psp`. | Nenhum | Sim |

### Parâmetros de iniciação rápida e todos os parâmetros
{: #icp-peer-deploy-quickstart-parms}

Use a seção Iniciação rápida se estiver experimentando ou iniciando. A seção Todos os parâmetros permite que você customize o acesso à rede, os recursos e o armazenamento e é recomendado somente para usuários experientes do Kubernetes.

**Clique na guia Todos os parâmetros para visualizar todos os parâmetros de configuração.**

|  Parâmetro     | Descrição    | Padrão  | Requerido |
| --------------|-----------------|-------|------- |
| **Configurações do console** | **Administração e implementação do console** | | |
| `Proxy IP` | Endereço IP do nó do proxy de seu cluster. | Nenhum| Sim |
| `Console administrator email` | E-mail usado para efetuar login no console. | Nenhum | Sim |
| `Console administrator password secret name` | Nome do segredo [criado para armazenar a senha](#console-deploy-icp-password-secret) que será usada para efetuar login no console. | Nenhum | Sim|
| **Definições de rede** | **Acesso à rede para o console** | | |
| `Console hostname` | Insira o mesmo valor que o IP do proxy. | Nenhum | Sim |
| `Console port` | Insira a porta que você gostaria de usar no intervalo de 31210 a 31220. Essa porta não pode ser usada por outro aplicativo. | Nenhum | Sim |
| **Configurações de armazenamento** | **Armazenamento a ser usado pelo console** | | |
| `Storage class name` | Insira o nome da classe de armazenamento a ser usada pelo console e os componentes que você criar. | Nenhum | Sim |
{: caption="Tabela 1. Parâmetros de iniciação rápida" caption-side="top"}
{: #simpletabtable1}
{: tab-title="Quickstart Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

|  Parâmetro     | Descrição    | Padrão  | Requerido |
| --------------|-----------------|-------|------- |
| `Architecture` | Selecione a arquitetura da plataforma de nuvem. (AMD64 ou S390x) | AMD64 | NÃO |
| **Configurações do console** | **Administração e implementação do console** | | |
| `Service account name` | Conta de serviço a ser usada pelo operador. | Nenhum | NÃO |
| `Proxy IP` | Endereço IP do nó do proxy em seu cluster. | Nenhum | Sim|
| `Console administrator email` | E-mail usado para efetuar login no console.  | Nenhum | Sim |
| `Console administrator password secret name` | Nome do segredo [criado para armazenar a senha](#console-deploy-icp-password-secret) que será usada para efetuar login no console. | Nenhum | Sim|
| **Configurações de imagem do Docker** | **Use essas configurações para customizar as imagens do Fabric cujo pull o console deve fazer** | | |
| `imagePullSecret name` | imagePullSecret a ser usado para fazer download de imagens. | `ibp-ibmregistry` | NÃO |
| **Definições de rede** | **Acesso à rede para o console** | | |
| `Console hostname` | Insira o mesmo valor que o IP do proxy. | Nenhum | Sim |
| `Console port` | Insira qualquer porta que queira usar no intervalo de 31210 a 31220. | Nenhum | Sim |
| `Proxy hostname` | Nome do host do servidor proxy. Insira o mesmo valor que o IP do proxy. | Nenhum | NÃO |
| `Proxy port` | Insira qualquer porta que queira usar no intervalo de 31210 a 31220. Essa porta não pode ser usada por outro aplicativo ou pelo console. | Nenhum | NÃO |
| `Configtxlator hostname` | Insira o mesmo valor que o IP do proxy. | Nenhum | NÃO |
| `Configtxlator port` | Insira qualquer porta que queira usar no intervalo de 31210 a 31220. Essa porta não pode ser usada por outro aplicativo ou pelo console. | Nenhum | NÃO |
| `TLS secret` | Nome do segredo que você [criou para armazenar os certificados TLS](#console-deploy-icp-tls-secret) que serão usados pelo console. | Nenhum | NÃO |
| **Persistência de dados**| **Ativar a persistência de dados e o fornecimento dinâmico** | | |
| `Enable data persistence`| Marque para ativar a capacidade de persistir dados após reinicializações ou falhas do cluster. Consulte [Armazenamento em Kubernetes](https://kubernetes.io/docs/concepts/storage/) para obter mais informações. *Se não configurados como true, todos os dados serão perdidos quando ocorrer uma reinicialização de pod ou failover.* | Ativado | NÃO |
| `Enable dynamic provisioning`| Marque para ativar o fornecimento dinâmico para volumes de armazenamento. | enabled | NÃO |
| **Configurações de armazenamento**| **Armazenamento de provisão para seu console e suas ferramentas** | | |
| `Volume claim size`| O tamanho da solicitação de volume persistente a ser fornecido. | 10 Gi  | NÃO |
| `Storage class name`| Nome da classe de armazenamento a ser usada pelo console e pelos componentes que você criar. | Nenhum | Sim |
| `Existing volume claim`| Especifique o nome de uma solicitação de volume existente e deixe todos os outros campos em branco. | Nenhum | NÃO |
| `Selector label`| [Rótulo do seletor](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) para seu PVC| Nenhum | NÃO |
| `Selector value`| [Valor do seletor](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) para seu PVC | Nenhum | NÃO |
| `Storage access mode`| Especifique o [modo de acesso](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes){: external} de armazenamento para o PVC.  | ReadWriteMany | NÃO |
| `PVC name`| Somente para nova solicitação. Insira um nome para seu novo PVC. | Padronizado para o nome da liberação. | NÃO |
| **Alocar recursos**| **Alocar recursos para o console** | | |
| `Opstools CPU limit` | Número máximo de CPUs a serem alocadas para o componente opstools. | 500 m | NÃO |
| `Opstools memory limit` | Quantia máxima de memória a ser alocada para o componente opstools. | 1000 Mi | NÃO |
| `Opstools CPU request` | Número mínimo de CPUs para alocar para o componente opstools. | 500 m | NÃO |
| `Opstools memory request` | Quantia mínima de memória a ser alocada para o componente opstools.| 1000 Mi | NÃO |
| `Configtxlator CPU limit` | Número máximo de CPUs a serem alocadas para a ferramenta configtxlator. | 25 m | NÃO |
| `Configtxlator memory limit` | Quantia máxima de memória a ser alocada para a ferramenta configtxlator. | 50 Mi | NÃO |
| `Configtxlator CPU request` | Número mínimo de CPUs para alocar para a ferramenta configtxlator.| 25 m | NÃO |
| `Configtxlator memory request` | Quantia mínima de memória a ser alocada para a ferramenta configtxlator.| 50 Mi | NÃO |
| `CouchDB CPU limit` | Número máximo de CPUs a serem alocadas para o CouchDB. | 500 m | NÃO |
| `CouchDB memory limit` | Quantia máxima de memória a ser alocada para o CouchDB. | 1000 Mi | NÃO |
| `CouchDB CPU request` | Número mínimo de CPUs a serem alocadas para o CouchDB.| 500 m | 500 m |
| `CouchDB memory request` | Quantia mínima de memória a ser alocada para o CouchDB.| 1000 Mi | NÃO |
| `Operator CPU limit` | Número máximo de CPUs a serem alocadas para o componente operator. | 100 m | NÃO |
| `Operator memory limit` | Quantia máxima de memória a ser alocada para o componente operator. | 200 Mi | NÃO |
| `Operator CPU request` | Número mínimo de CPUs a serem alocados para o componente operator. | 100 m | NÃO |
| `Operator memory request` | Quantia mínima de memória a ser alocada para o componente operator. | 200 Mi | NÃO |
| `Deployer CPU limit` | Número máximo de CPUs a serem alocadas para o componente deployer. | 100 m | NÃO |
| `Deployer memory limit` | Quantia máxima de memória a ser alocada para o componente deployer. | 200 Mi | NÃO |
| `Deployer CPU request` | Número mínimo de CPUs a serem alocados para o componente deployer. | 100 m | NÃO |
| `Deployer memory request` | Quantia mínima de memória a ser alocada para o componente deployer. | 200 Mi | NÃO |
{: caption="Tabela 2. Todos os parâmetros" caption-side="top"}
{: #simpletabtable2}
{: tab-title="All Parameters"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

### Usando a linha de comandos do Helm para instalar a liberação do Helm
{: #console-deploy-icp-helm-cli}

Como alternativa, é possível usar a CLI `helm` para instalar a liberação do Helm. Antes de executar o comando `helm install`, assegure-se de [incluir o repositório do Helm do seu cluster no ambiente da CLI do Helm](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/app_center/add_int_helm_repo_to_cli.html){: external}.

É possível configurar os parâmetros necessários para a instalação, criando um arquivo `yaml` e passando-o para o comando `helm install` a seguir.
```
helm install --name <helm_release_name>  <helm_chart> \
--version <helm_chart_version> \
--values <customvalues.yaml> \
--tls
```
{:codeblock}

Em que:
- `<helm_release name>` representa o nome que você deseja fornecer à liberação do helm.
- `<helm_chart>` representa o nome do gráfico do Helm que é importado para o catálogo.
- `<helm_chart_version>` representa a versão do gráfico do Helm que é importado para o catálogo.
- `<customvalues.yaml>` é o nome do arquivo yaml que contém os parâmetros de configuração.

Por exemplo:
```
helm install --name jnchart2 mycluster/ibm-blockchain-platform \
--version 1.1.0 \
--values console-s390x-values.yaml \
--tls
```

É possível criar um novo a

## Verificando a Instalação

Depois de concluir os parâmetros de configuração e clicar no botão **Instalar**, clique no botão **Visualizar liberação do Helm** para visualizar sua implementação. Se isso foi bem-sucedido, você verá o valor 1 nos campos `DESIRED`, `CURRENT`, `UP TO DATE` e `AVAILABLE` na tabela Implementação. Você pode precisar clicar em atualizar e aguardar que a tabela seja atualizada.

Você visualiza os detalhes de sua implementação navegando até a tela de visão geral **Implementação** e clicando no pod que foi criado. A implementação do gráfico do Helm cria cinco contêineres em seu cluster:
- **opstools**: a IU do console.
- **deployer**: uma ferramenta que permite que seu console se comunique com suas implementações.
- **configtxlator**: uma ferramenta usada pelo console para ler e criar atualizações de canal.
- **couchdb**: uma instância do CouchDB que armazena os dados de seu console, incluindo suas informações de autorização.
- **operator**: um operador do Kubernetes que gerencia as implementações de seu componente.

## Efetuando login no console

É possível usar seu navegador para acessar o console após a instalação. É possível localizar a URL na seção **Notas:** da tela de visão geral da liberação do Helm que é aberta após a implementação. Certifique-se de que você não esteja usando a versão ESR do Firefox. Se estiver, alterne para outro navegador, como Chrome, e tente novamente.

Em seu navegador, é necessário ser capaz de ver a tela de login do console:
- Para o **ID do usuário**, use o valor fornecido para o campo `E-mail do administrador do console` durante a configuração.
- Para a **Senha**, use o valor codificado e armazenado dentro do [segredo de senha](#console-deploy-icp-password-secret) e, em seguida, transmitido para o console durante a configuração. Essa senha se tornará a senha padrão para o console que todos os novos usuários usam para efetuar login no console. Depois de efetuar login pela primeira vez, será solicitado que você forneça uma nova senha que possa ser usada para efetuar login no console.

O administrador que forneceu o gráfico do Helm pode conceder a outros usuários acesso ao console e especificar quais operações podem ser executadas. Para obter mais informações, consulte [Gerenciando usuários por meio do console](/docs/services/blockchain/howto/ibp-console-import-nodes.html#console-icp-manage-users).

## Próximas etapas
{: #console-deploy-icp-next-steps}

Depois de acessar seu console, é possível visualizar a guia **nós** da IU do console. É possível usar essa tela para implementar componentes em seu cluster local. Visite o [tutorial Construindo uma rede](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) para começar a usar o console. Também é possível usar essa guia para operar nós que foram criados em outras nuvens. Para obter mais informações, consulte [Importando nós](/docs/services/blockchain/howto/ibp-console-import-nodes.html#ibp-console-import-nodes).

Para aprender a gerenciar os usuários que podem acessar o console, use as APIs do {{site.data.keyword.blockchainfull_notm}} Platform e para visualizar os logs de seus componentes do console e do blockchain, visite [Administrando seu console no {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/ibp-console-import-nodes.html#console-icp-manage).
