---

copyright:
  years: 2019
lastupdated: "2019-07-10"

keywords: APIs, build a network, authentication, service credentials, API key, API endpoint, IAM access token, Fabric CA client, import a network, generate certificates

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

# Construindo uma rede com APIs
{: #ibp-v2-apis}

O {{site.data.keyword.blockchainfull}} Platform expõe APIs de RESTful para criar, importar, editar e excluir seus componentes de blockchain, bem como para gerenciar as configurações de criação de log, de notificações e do console. É possível usar as APIs e os SDKs correspondentes para desenvolver aplicativos que interagem com sua rede de blockchain.
{: shortdesc}

Este tutorial apresenta o fluxo genérico para construir uma rede de blockchain com APIs do {{site.data.keyword.blockchainfull_notm}} Platform. Para obter mais informações sobre cada API, consulte o [doc de referência de API do {{site.data.keyword.blockchainfull_notm}} Platform](/apidocs/blockchain){: external}.

Essas APIs são compatíveis com o {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} v0.1.77 ou mais recente somente. Para verificar a versão, navegue para `https://[your_console_url]/version.txt`, em que *`[your_console_url]`* é a URL de seu console do {{site.data.keyword.blockchainfull_notm}} Platform. Por exemplo: https://1ee1869ffa6496d6bc1ce4b-optools.bp01.blockchain.cloud.ibm.com/version.txt
{:note}

## Antes de iniciar
{: #ibp-v2-apis-prereq}

Você deve ter uma instância de serviço do {{site.data.keyword.blockchainfull_notm}} Platform on {{site.data.keyword.cloud_notm}} para que seja possível emitir chamadas da API para interagir com a rede. Se você ainda não tiver uma instância de serviço, siga as [Instruções de introdução](/docs/services/blockchain/reference?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks) para criar uma.

## Autenticação
{: #ibp-v2-apis-authentication}

Para usar as APIs para acessar sua rede de blockchain criada com o {{site.data.keyword.blockchainfull_notm}} Platform, seu aplicativo precisa ser capaz de autenticar com o {{site.data.keyword.cloud_notm}} e se conectar à sua instância de serviço.

### Recuperando credenciais de serviço
{: #ibp-v2-apis-retrieve-service-credentials}

Você precisa de uma credencial de autenticação básica para assegurar que você tenha acesso à sua instância de serviço do {{site.data.keyword.blockchainfull_notm}} Platform no {{site.data.keyword.cloud_notm}}.

1. Na [lista de recursos do {{site.data.keyword.cloud_notm}} ](https://cloud.ibm.com/resources){: external}, abra a instância de serviço do blockchain que você criou.
2. Clique em **Credenciais de serviço** no navegador esquerdo.
3. Clique no botão **Nova credencial** na página **Credenciais de serviço** para criar uma nova credencial.
  1. Forneça um nome à credencial, por exemplo, *UseAPIs*.
  2. É possível deixar o campo **Incluir parâmetro de configuração sequencial** em branco.
  3. Clique no botão **Incluir**.
4. Após a nova credencial ser criada, clique em **Visualizar credenciais** sob o cabeçalho **AÇÕES** dessa credencial. O conteúdo da credencial é semelhante a este exemplo:

  ```
  {
    "api_endpoint": "https://1088ac8563e44f5a92539d946733ad7e-optools.so01.blockchain.test.cloud.ibm.com"
    "apikey": "nvASKts6B6lMZGZUNTGVP7MLK2BujMnxz0plSPYaqc3R",
    "configtxlator": "https://1088ac8563e44f5a92539d946733ad7e-configtxlator.so01.blockchain.test.cloud.ibm.com",
    "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:staging:public:blockchain:us-south:a/9d46037caee397faa45c55e087d26baa:1088ac85-63e4-4f5a-9253-9d946733ad7e::",
    "iam_apikey_name": "auto-generated-apikey-c1981f5c-cff0-464f-9a78-ed2f52e24d1a",
    "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
    "iam_serviceid_crn": "crn:v1:staging:public:iam-identity::a/9d46037caee397faa45c55e087d26baa::serviceid:ServiceId-774190c5-9c37-4f68-8572-8d6e2aabbc7e",
  }
  ```

Na credencial de serviço, é possível localizar a **Chave de API** (`apikey`) e o **Terminal de API** (`api_endpoint`) que você precisa para recuperar um token de acesso do {{site.data.keyword.iamshort}} (IAM).

### Recuperando um token de acesso
{: #ibp-v2-apis-retrieve-token}

É possível autenticar com o {{site.data.keyword.blockchainfull_notm}} Platform, recuperando um token de acesso do IAM. Com um token de acesso, é possível trabalhar com a API do {{site.data.keyword.blockchainfull_notm}} Platform em nome de seu serviço ou aplicativo ou fora do {{site.data.keyword.cloud_notm}}, sem precisar compartilhar suas credenciais pessoais de usuário.  

Chame a API do {{site.data.keyword.iamshort}} para recuperar seu token de acesso.

```cURL
curl -X POST \
  "https://iam.cloud.ibm.com/identity/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Accept: application/json" \
  -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>" \
```
{: codeblock}

Na solicitação, substitua `<API_KEY>` pelo valor `apikey` de suas credenciais de serviço. O exemplo truncado a seguir mostra a saída do token:

```
{
"access_token": "eyJraWQiOiIyM...",
"refresh_token": "...",
"expires_in":3600,
"expiration":1555558683,
"scope":"ibm openid"}
```
{: screen}

Use o valor `access_token` integral, prefixado pelo tipo de token `Bearer`, para trabalhar programaticamente com sua rede de blockchain usando as APIs.

Os tokens de acesso são válidos por uma hora, mas é possível gerá-los novamente conforme necessário. Para manter o acesso ao serviço, atualize o token de acesso para sua chave de API em uma base regular chamando a API do {{site.data.keyword.iamshort}}.   
{: tip}

## Formando sua solicitação de API
{: #ibp-v2-apis-form-api-request}

Quando você fizer uma chamada da API para o serviço, estruture sua solicitação de API de acordo como provisionou inicialmente sua instância de serviço do {{site.data.keyword.blockchainfull_notm}} Platform.

Para construir sua solicitação, emparelhe um terminal de API com as credenciais de autenticação apropriadas no formato a seguir:

```cURL
curl -X <API method> \
    <API_endpoint>/<API> \
    -H 'authorization: Bearer <access_token>' \
    -H 'Content-Type: application/json' \
    -d '<request body>' \
```
{: codeblock}

Os comandos curl de exemplo são fornecidos para cada API no [doc de referência de API do {{site.data.keyword.blockchainfull_notm}} Platform](/apidocs/blockchain){: external}.

Além disso, é possível usar a função **Testar** no doc de Referência da API para testar suas chamadas da API antes de incluí-las em seus aplicativos. É necessário efetuar login no {{site.data.keyword.cloud_notm}} para usar a função **Testar**. É possível selecionar qualquer instância de serviço na lista suspensa. Todas as solicitações de API são enviadas para a rede especificada no terminal de API.

## Limitações
{: #ibp-v2-apis-limitations}

É possível importar apenas nós de CA, peer e de pedido existentes por meio de outras redes do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.

## Construindo uma rede usando APIs
{: #ibp-v2-apis-build-with-apis}

É possível usar APIs para criar componentes de blockchain em sua instância do {{site.data.keyword.blockchainfull_notm}} Platform. Use as etapas a seguir para construir uma rede de blockchain usando as APIs do {{site.data.keyword.blockchainfull_notm}}.

1. Crie uma Autoridade de Certificação (CA) chamando [`POST /ak/api/v1/kubernetes/components/ca`](/apidocs/blockchain?code=try#create-a-ca).

  Lembre-se de sua entrada e da resposta, pois elas serão necessárias mais tarde.
  {: tip}

  É necessário aguardar o início da CA. Isso pode levar alguns minutos, dependendo do ambiente. É possível chamar a API `GET <ca_url>/cainfo` para verificar seu status de CA. Você obterá erros repetidos, em seguida, um código de status `200`, o que significa que é possível continuar com a próxima etapa. Observe que essa chamada da API atinge o tempo limite após um minuto.

2. Use sua CA para registrar suas identidades de componente e de administrador e gerar os certificados necessários. É possível usar o cliente Fabric CA para concluir as etapas a seguir:

  - [Configure o cliente de CA do Fabric](#ibp-v2-apis-config-fabric-ca-client).
  - [Gerar certificados com seu administrador de CA](#ibp-v2-apis-enroll-ca-admin).
  - [Registre o novo componente com sua CA](#ibp-v2-apis-config-register-component).
  - Também é necessário [registrar um administrador da organização](#ibp-v2-apis-config-register-admin) e, em seguida, [gerar certificados para o administrador](#ibp-v2-apis-config-enroll-admin) dentro de uma pasta do MSP. Você não terá que concluir essa etapa se já tiver registrado sua identidade de administrador.
  - [Registre o novo componente com sua CA TLS](#ibp-v2-apis-config-register-component-tls).

  Também é possível concluir essas etapas usando o console do {{site.data.keyword.blockchainfull_notm}} Platform. Para obter mais informações, consulte [Criando e gerenciando identidades](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities).

3. [Crie uma definição do MSP para sua organização](#ibp-v2-apis-msp) chamando [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?#import-a-membership-service-provide-msp).

4. [Construa o arquivo de configuração](#ibp-v2-apis-config) que é necessário para criar um serviço de pedido ou peer. Deve-se construir um arquivo de configuração exclusivo para cada serviço de pedido ou peer que você deseja criar. Se você estiver implementando múltiplos nós de pedido, será necessário fornecer um arquivo de configuração para cada nó que você deseja criar.

5. Crie um serviço de pedido chamando [`POST /ak/api/v1/kubernetes/components/orderer`](/apidocs/blockchain?code=try#create-an-ordering-service).

6. Crie um peer chamando [`POST /ak/api/v1/kubernetes/components/peer`](/apidocs/blockchain?code=try#create-a-peer).

7. Se você desejar usar o console para operar seus componentes de blockchain, deve-se importar sua identidade de administrador para sua carteira eletrônica do console. Use a guia de carteira eletrônica para importar o certificado e a chave privada de seu administrador do nó para o console e criar uma identidade. Em seguida, é necessário usar o console para associar essa identidade aos componentes criados. Para obter mais informações, consulte [Importando uma identidade de administrador para o console do {{site.data.keyword.blockchainfull_notm}} Platform](#ibp-v2-apis-admin-console).

8. Depois de implementar sua rede, é possível usar os SDKs do Fabric, a CLI do Peer ou a IU do console para criar canais e instalar ou instanciar contratos inteligentes. Se for necessário criar programaticamente um canal, você deverá fornecer o nome do consórcio. Para o {{site.data.keyword.blockchainfull}} Platform, o nome do consórcio deve ser configurado como `SampleConsortium`.

A credencial de serviço que é usada para autenticação da API deve ter a função `Manager` no IAM para ser capaz de criar componentes. Consulte a tabela neste tópico sobre [funções do usuário](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove) para obter mais informações.
{: note}

### Criando um nó dentro de uma zona específica
{: #ibp-v2-apis-zone}

Se você estiver usando um cluster multizona, será possível usar as APIs para implementar um componente de blockchain em uma zona específica do {{site.data.keyword.cloud_notm}}. Isso permite que a sua rede mantenha a disponibilidade no caso de uma falha de zona. É possível usar as etapas a seguir para implementar um peer ou um nó de pedido em uma zona específica.

1. Localize as zonas nas quais os nós do trabalhador estão localizados. Navegue para a tela de visão geral de seu cluster multizona no [serviço Kubernetes do {{site.data.keyword.cloud_notm}} no {{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/kubernetes/clusters){: external}. Na tela de visão geral do cluster, clique em **Nós do trabalhador** para ver uma tabela de todos os nós do trabalhador em seu cluster. É possível localizar a zona em que cada nó do trabalhador localizado na coluna **Zona** da tabela.

  Também é possível localizar as zonas dos nós do trabalhador usando a CLI kubectl. Navegue para o painel **Acesso** e siga as instruções em **Obter acesso ao seu cluster** para se conectar ao seu cluster usando o {{site.data.keyword.cloud_notm}} e as ferramentas da CLI kubectl. Depois de conectado, use o comando `kubectl get nodes --show-labels` para obter a lista completa de nós e zonas de seu cluster. Você será capaz de localizar a zona em que cada nó do trabalhador está localizado após o campo `zone` na coluna `LABELS`.

2. Para criar um nó dentro de uma zona específica, forneça o nome da zona para as chamadas API [Criar um serviço de pedido](/apidocs/blockchain?code=try#create-an-ordering-service) ou [Criar um peer](/apidocs/blockchain?code=try#create-an-ordering-service) usando o campo de zona do corpo da solicitação. A política antiafinidade do console do {{site.data.keyword.blockchainfull_notm}} Platform implementará automaticamente o seu componente para diferentes nós do trabalhador dentro de cada zona com base nos recursos disponíveis.

## Importar uma rede usando APIs
{: #ibp-v2-apis-import-with-apis}

Também é possível usar as APIs para importar os componentes do {{site.data.keyword.blockchainfull_notm}} criados usando as APIs ou o console do {{site.data.keyword.blockchainfull_notm}} Platform em outra instância de serviço do {{site.data.keyword.blockchainfull_notm}} Platform.

1. Importe uma autoridade de certificação chamando [`POST /ak/api/v1/components/ca`](/apidocs/blockchain?code=try#import-a-ca).

  Lembre-se de sua entrada e da resposta, pois elas serão necessárias mais tarde.
  {: tip}

  É necessário aguardar o início da CA. Isso pode levar alguns minutos, dependendo do ambiente. É possível chamar [`GET /components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) para verificar o status da CA. Você obterá erros repetidos antes de obter um código de status `200` para acessar a próxima etapa. Observe que essa chamada da API atinge o tempo limite em um minuto.

2. Importe uma definição do MSP da organização, chamando [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?code=try#import-an-msp).

3. Importe um serviço de pedido chamando [`POST /ak/api/v1/components/orderer`](/apidocs/blockchain?code=try#import-a-ordering-service).

4. Importe um peer chamando [`POST /ak/api/v1/components/peer`](/apidocs/blockchain?code=try#import-a-peer).

5. Se você planeja usar o console do {{site.data.keyword.blockchainfull_notm}} Platform para operar seus componentes de blockchain, deve-se importar as identidades de administrador do componente para a sua carteira eletrônica do console. Para obter mais informações, consulte [Importando uma identidade de administrador para o console do {{site.data.keyword.blockchainfull_notm}} Platform](#ibp-v2-apis-admin-console).

6. Depois de implementar sua rede, é possível usar os SDKs do Fabric, a CLI do Peer ou a IU do console para criar canais e instalar ou instanciar contratos inteligentes. Se for necessário criar programaticamente um canal, você deverá fornecer o nome do consórcio. Para o {{site.data.keyword.blockchainfull}} Platform, o nome do consórcio deve ser configurado como `SampleConsortium`.

A credencial de serviço que é usada para autenticação da API deve ter a função `Writer` no IAM para ser capaz de importar componentes. Consulte a tabela neste tópico sobre [funções do usuário](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove) para obter mais informações.
{: note}

## Operando sua CA usando o cliente Fabric CA
{: #ibp-v2-apis-config-fabric-ca-client}

É possível usar o cliente Fabric CA para operar suas autoridades de certificação. Execute os comandos do cliente Fabric CA a seguir para registrar suas identidades de componente e administrador e gerar os certificados necessários.

### Configurar o cliente de CA do Fabric
{: #ibp-v2-apis-setup-fabric-ca-client}

1. Faça download do [cliente de CA do Fabric](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#fabric-ca-client){: external} para seu sistema de arquivos local.

  A maneira mais fácil de obter o cliente Fabric CA é fazer download de todos os binários da ferramenta Fabric diretamente. Navegue para um diretório no qual gostaria de fazer download dos binários com sua linha de comandos e busque-os executando o comando [Curl](https://hyperledger-fabric.readthedocs.io/en/release-1.4/prereqs.html#install-curl){: external} a seguir.

  ```
  curl -sSL http://bit.ly/2ysbOFE | bash -s 1.4.0 1.4.0 -d -s
  ```
  {:codeblock}

  Esse comando instala os binários em um diretório `bin/`.

2. Configure o caminho `PATH` para o diretório no qual você transferiu por download os binários da ferramenta Fabric:

  ```
  export PATH=$PATH:<full/path/to/fabric-client/bin>
  ```
  {:codeblock}

  Por exemplo, se instalasse os binários em seu diretório inicial, você configuraria seu `PATH` como:

  ```
  export PATH=$PATH:$HOME/bin
  ```
  {:codeblock}

3. Crie a pasta na qual armazenará os certificados de seu administrador de CA.

  ```
  mkdir -p $HOME/fabric-ca-client/ca-admin
  ```
  {:codeblock}

4. Configure o valor da variável de ambiente `$FABRIC_CA_CLIENT_HOME` para que seja o caminho em que o cliente de CA armazenará os certificados MSP gerados. Assegure-se de remover o material de configuração que pode ser criado por tentativas anteriores. Se você não executou o comando `enroll` antes, a pasta `msp` e o arquivo `.yaml` não existem.

  ```
  export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-client/ca-admin
  rm -rf $FABRIC_CA_CLIENT_HOME/fabric-ca-client-config.yaml
  rm -rf $FABRIC_CA_CLIENT_HOME/msp
  ```
  {:codeblock}

5. Recupere o certificado TLS de sua CA para ser usado pelo cliente Fabric CA. Se você estiver usando o console do {{site.data.keyword.blockchainfull_notm}} Platform, abra a CA e clique em **Configurações** e procure o certificado no formato base64 no campo **Certificado TLS**. Se você estiver usando as APIs, será possível chamar [`GET /ak/api/v1/components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) e localizar o certificado TLS da CA no campo `"PEM"`. Se você criou a CA usando a API `Create a Fabric CA`, também é possível localizar o certificado TLS no corpo de resposta.

  É necessário converter o certificado do base64 no formato PEM para usá-lo para se comunicar com sua CA. Insira a sequência codificada em base64 do certificado TLS no comando a seguir. Assegure-se de que você esteja em seu diretório `$HOME/fabric-ca-client`.

  ```
  cd $HOME/fabric-ca-client
  mkdir catls
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  echo <base64_string> | base64 --decode $FLAG > tls.pem
  ```
  {:codeblock}

### Gerar certificados com seu administrador de CA
{: #ibp-v2-apis-enroll-ca-admin}

Uma identidade de **administrador de CA** foi registrada automaticamente para você quando criou sua CA. Agora é possível usar esse nome de administrador e senha para emitir um comando `enroll` com o cliente de CA do Fabric para gerar uma pasta MSP com certificados que podem, então, ser usados para registrar outras identidades do nó de peer ou de pedido.

1. Assegure-se de concluir as etapas para [configurar o cliente Fabric CA](#ibp-v2-apis-config-fabric-ca-client) e que `$FABRIC_CA_CLIENT_HOME` esteja configurado no diretório no qual você deseja armazenar seus certificados de administrador de CA.

  ```
  echo $FABRIC_CA_CLIENT_HOME
  $HOME/fabric-ca-client/ca-admin
  ```
  {:codeblock}

2. Execute o comando a seguir para gerar certificados:

  ```
  fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <ca_name>  --tls.certfiles  <ca_tls_cert_path>
  ```
  {:codeblock}

  O comando `<enroll_id>` e `<enroll_password>` são o `enroll_id` e `enroll_secret` especificados quando você criou a CA. Use a **URL do terminal da autoridade de certificação** de seu console do {{site.data.keyword.blockchainfull_notm}} Platform ou o `"ca_url"` retornado por sua chamada da API como o valor para `<ca_url_with_port>`. Deixe fora o `http://` no início. O `<ca_name>` é o **Nome da CA** de seu console ou o `"ca_name"` retornado pelas APIs.

  O `<ca_tls_cert_path>` é o caminho completo de seu certificado TLS da CA.

  Uma chamada real pode ser semelhante ao comando de exemplo a seguir:

  ```
  fabric-ca-client enroll -u https://admin:adminpw@9.30.94.174:30167 --caname ca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```  
  {:codeblock}

**Dica:** se o valor da URL de inscrição, que é o valor de parâmetro `-u`, contiver um caractere especial, será necessário codificar o caractere especial ou circundar a URL com as aspas simples. Por exemplo, `!` torna-se `%21` ou o comando é semelhante a:

  ```
  ./fabric-ca-client enroll -u 'https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241' --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname ca
  ```
  {:codeblock}

  O comando `enroll` gera um conjunto completo de certificados, que é conhecido como uma pasta Membership Service Provider (MSP), que está localizada dentro do diretório no qual você configura o caminho `$HOME` para seu cliente Fabric CA. Por exemplo, `$HOME/fabric-ca-client/ca-admin`. Para obter mais informações sobre MSPs e o que a pasta MSP contém, consulte [Membership Service Providers](/docs/services/blockchain?topic=blockchain-managing-certificates#managing-certificates-msp).

  Se o comando `enroll` falhar, consulte o [tópico de Resolução de Problemas](#ibp-v2-apis-config-troubleshooting) para obter as possíveis causas.

  É possível executar um comando de árvore para verificar se você concluiu todas as etapas de pré-requisito. Navegue para o diretório no qual você armazenou seus certificados. Um comando de árvore deve gerar um resultado semelhante à estrutura a seguir:

  ```
  cd $HOME/fabric-ca-client
tree
.
  ├── ca-admin
  │   ├── fabric-ca-client-config.yaml
  │   └── msp
  │       ├── cacerts
  │       │   └── 9-30-250-70-30395-SampleOrgCA.pem
  │       ├── IssuerPublicKey
  │       ├── IssuerRevocationPublicKey
  │       ├── keystore
  │       │   └── 2a97952445b38a6e0a14db134645981b74a3f93992d9ddac54cb4b4e19cdf525_sk
  │       ├── signcerts
  │       │   └── cert.pem
  │       └── user
  └── catls
      └── tls.pem    
  ```

### Registrando a identidade do componente com a CA
{: #ibp-v2-apis-config-register-component}

Primeiro, é necessário registrar uma identidade de componente com sua CA. Seu componente usará essa identidade para gerar os certificados que ele precisa quando é implementado.

1. [Gere certificados com seu administrador de CA](#ibp-v2-apis-enroll-ca-admin) usando o cliente Fabric CA. Use esses certificados de administrador para emitir os comandos a seguir. Assegure-se de que `$FABRIC_CA_CLIENT_HOME` esteja configurado como `$HOME/fabric-ca-client/ca-admin`.

  ```
  echo $FABRIC_CA_CLIENT_HOME
  $HOME/fabric-ca-client/ca-admin
  ```

2. Emita o comando a seguir para localizar sua afiliação e seu nome da organização.
  ```
  fabric-ca-client affiliation list --caname <ca_name> --tls.certfiles <ca_tls_cert_path>
  ```
  {:codeblock}

  Seu comando pode ser semelhante ao exemplo a seguir:
  ```
  fabric-ca-client affiliation list --caname ca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```

  Você verá informações semelhantes ao exemplo a seguir:
  ```
  affiliation: org1 affiliation: org1.department1
  ```

  Anote o segundo valor de **afiliação**, por exemplo, `org1.department1`. Você precisará usar esse valor no comando abaixo.

3. Execute o comando a seguir para registrar o nó de pedido ou peer.

  ```
  fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type <component_type> --id.secret <secret> --tls.certfiles <ca_tls_cert_path>
  ```
  {:codeblock}

  Crie um nome e uma senha para o componente e, em seguida, use-os para substituir `name` e `secret`.  Tome nota destas informações. Configure `--id.type` como `orderer` se você estiver implementando um nó de pedido ou configure-o para `peer` se você estiver implementando um peer. O comando pode ser semelhante ao exemplo a seguir:

  ```
  fabric-ca-client register --caname ca --id.affiliation org1.department1 --id.name peer1 --id.secret peer1pw --id.type peer --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
  ```

  É necessário salvar o `"enrollid"` e o `"enrollsecret"` do comando acima para o momento em que você criar seu arquivo de configuração.
  {: important}

  É possível registrar uma identidade apenas uma vez. Se você tiver algum problema, tente um comando com um novo nome do usuário e senha. Como uma medida de segurança, use cada identidade e o enrollID e segredo associados para implementar somente um peer. Embora seja possível usar uma identidade de **administrador** para vários componentes (essa é a nossa estratégia de implementação recomendada), não reutilize IDs de peer e senhas.

  Quando o comando for concluído com êxito, você verá informações semelhantes ao exemplo a seguir:

  ```
  2018/06/18 16:53:00 [INFO] Configuration file location: /fabric-ca-platform/admin/fabric-ca-client-config.yaml
  2018/06/18 16:53:00 [INFO] TLS Enabled
  2018/06/18 16:53:00 [INFO] TLS Enabled
  Password: peerpw
  ```

### Registrando seu administrador da organização
{: #ibp-v2-apis-config-register-admin}

Também é necessário criar uma identidade de administrador que possa ser usada para operar sua rede. Você usará essa identidade para operar componentes específicos, como a instalação de um contrato inteligente em seu peer. Também é possível usar essa identidade como um administrador de sua organização e usá-la para criar e editar canais.  

É necessário registrar essa nova identidade com sua CA e usá-la para gerar uma pasta do MSP. É possível tornar essa identidade um administrador da organização, incluindo seu signCert em seu MSP da organização. Você também precisará incluir o signCert em seu arquivo de configuração para que ele possa ser feito com o certificado de administrador do nó de pedido ou peer durante a implementação. É necessário criar somente uma identidade de administrador para sua organização. Como resultado, é necessário concluir essas etapas somente uma vez. É possível usar o signCert que você gerou para implementar muitos peers ou nós de pedido.

Assegure-se de que seu `$FABRIC_CA_CLIENT_HOME` esteja configurado com o caminho para o MSP de seu Administrador de CA.

```
echo $FABRIC_CA_CLIENT_HOME
$HOME/fabric-ca-client/ca-admin
```
{:codeblock}

Registre a identidade de administrador do componente com a CA executando o comando a seguir:

```
fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type client --id.secret <password> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Crie uma nova identidade de usuário `name` e `secret` para o administrador. Certifique-se de usar valores diferentes da identidade do peer ou do nó de pedido que você acabou de registrar. O comando é semelhante ao exemplo a seguir:

```
fabric-ca-client register --caname ca --id.name peeradmin --id.affiliation org1.department1 --id.type client --id.secret peeradminpw --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

Tome nota destas informações. Você usará esse `name` e `secret` para gerar a pasta MSP usando o comando `enroll`.
{: important}

### Gerando a pasta do Membership Service Provider (MSP) do administrador
{: #ibp-v2-apis-config-enroll-admin}

Após o registro do administrador do componente, é possível gerar certificados executando o comando a seguir:

```
fabric-ca-client enroll -u https://<peer_admin_enroll_id>:<peer_admin_enroll_password>@<ca_url_with_port> --caname <ca_name> -M $HOME/path/to/peer-admin/msp --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Por exemplo:

```
fabric-ca-client enroll -u https://peeradmin:peeradminpw@9.30.94.174:30167 --caname ca -M $HOME/fabric-ca-client/peer-admin/msp --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```
{:codeblock}

Depois que esse comando for concluído com êxito, ele gerará uma nova pasta MSP no diretório que você especificou usando o sinalizador `-M`. Esse diretório contém os certificados que você precisa usar para operar seus componentes. É possível verificar se o comando enroll funcionou usando um comando de árvore. Navegue para o diretório no qual você armazenou seus certificados. Um comando de árvore deve gerar um resultado semelhante à estrutura a seguir:


```
cd $HOME/fabric-ca-client
tree
.
├── ca-admin
│   ├── fabric-ca-client-config.yaml
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── IssuerPublicKey
│       ├── IssuerRevocationPublicKey
│       ├── keystore
│       │   └── 2a97952445b38a6e0a14db134645981b74a3f93992d9ddac54cb4b4e19cdf525_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
├── catls
│   └── tls.pem
├── orderer-admin
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── keystore
│       │   └── dfe06060490bf62e2bd709433fc747ff28cdbb1e040682c5d47a4e8598db4f2e_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
├── peer-admin
│   └── msp
│       ├── cacerts
│       │   └── 9-30-250-70-30395-SampleOrgCA.pem
│       ├── keystore
│       │   └── 24f76217c5c7e641ee29b068712181294e427cbee4dbfce230a1a98b53489cbe_sk
│       ├── signcerts
│       │   └── cert.pem
│       └── user
└── tlsca-admin
    ├── fabric-ca-client-config.yaml
    └── msp
        ├── cacerts
        │   └── 9-30-250-70-30395-tlsca.pem
        ├── IssuerPublicKey
        ├── IssuerRevocationPublicKey
        ├── keystore
        │   └── 45a7838b1a91ddfe3d4d22a5a7f2639b868493bcce594af3e3ceb9c07899d117_sk
        ├── signcerts
        │   └── cert.pem
        └── user
```

Será necessário retornar a essa pasta ao criar sua definição do MSP da organização e arquivos de configuração.
{: important}

### Registrando a identidade do componente com a CA do TLS
{: #ibp-v2-apis-config-register-component-tls}

Quando você criou sua CA, uma CA TLS foi implementada junto com sua CA padrão. Também é necessário registrar o nó de pedido ou peer com a sua CA do TLS. Para fazer isso, primeiro você precisará se inscrever usando o administrador da CA TLS. Mude `$FABRIC_CA_CLIENT_HOME` para um diretório no qual você deseja armazenar seus certificados de administrador de CA do TLS.

```
cd $HOME/fabric-ca-client
mkdir tlsca-admin
export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca-client/tlsca-admin
```
{:codeblock}

Execute o comando abaixo para se inscrever como seu administrador com relação à CA do TLS. O ID de inscrição e a senha de seu administrador de CA TLS são os mesmos que de sua CA padrão. Como resultado, o comando a seguir é o mesmo que você usou para se inscrever como seu [administrador de CA](#ibp-v2-apis-enroll-ca-admin) somente com o nome de sua CA TLS. Seu nome da CA TLS é o valor de **Nome da CA TLS** no painel **configurações** de CA em seu console ou o valor do `"tlsca_name"` retornado pela API `Create a CA`.

```
fabric-ca-client enroll -u https://<enroll_id>:<enroll_password>@<ca_url_with_port> --caname <tls_ca_name> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Uma chamada real pode ser semelhante ao exemplo a seguir:

```
fabric-ca-client enroll -u https://admin:adminpw@9.30.94.174:30167 --caname tlsca --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

Depois de se inscrever, você terá os certificados necessários para registrar seu componente com a CA do TLS. Execute o comando a seguir para registrar o nó de pedido ou peer:

```
fabric-ca-client register --caname <ca_name> --id.name <name> --id.affiliation org1.department1 --id.type peer --id.secret <password> --tls.certfiles <ca_tls_cert_path>
```
{:codeblock}

Esse comando é semelhante ao que você usou para registrar a identidade do componente com a CA, exceto que é necessário usar o nome da CA TLS. Se você estiver implementando um nó de pedido em vez de um peer, configure `--id.type` para `orderer` em vez de `peer`. Deve-se fornecer a essa identidade um nome de usuário e senha diferentes daqueles que você usou com relação à sua CA padrão. Um registro real pode ser semelhante ao comando a seguir:

```
fabric-ca-client register --caname tlsca --id.affiliation org1.department1 --id.name peertls --id.secret peertlspw --id.type peer --tls.certfiles $HOME/fabric-ca-client/catls/tls.pem
```

É necessário salvar o `"enrollid"` e o `"enrollsecret"` do comando acima para o momento em que você criar seu arquivo de configuração.
{: important}

### Detecção de problemas
{: #ibp-v2-apis-config-troubleshooting}

#### **Problema:** erro ao executar o comando `enroll`
{: #ibp-v2-apis-config-enroll-error1}

Ao executar o comando de inscrição do cliente Fabric CA, é possível que o comando falhe com o erro a seguir:

```
Error: Failed to read config file at '/Users/chandra/fabric-ca-client/ca-admin/fabric-ca-client-config.yaml': While parsing config: yaml: line 42: mapping values are not allowed in this context
```
{:codeblock}

**Solução:**

Esse erro pode ocorrer quando o cliente Fabric CA tenta se inscrever, mas não pode se conectar à sua CA. Isso poderá acontecer se:   

- Seu comando `enroll` contém um `https://` extra no parâmetro `-u`.
- O nome da autoridade de certificação está incorreto.
- O nome do usuário ou senha está incorreto.

Revise os parâmetros especificados em seu comando `enroll` e assegure que nenhuma dessas condições existam.

#### **Problema:** erro com a URL da CA ao executar o comando `enroll`
{: #ibp-v2-apis-config-enroll-error2}

O comando de inscrição do cliente Fabric CA poderá falhar se a URL de inscrição, o valor de parâmetro `-u`, contiver um caractere especial. Por exemplo, o comando a seguir com o ID de inscrição e a senha de `admin:C25A06287!0`,

```
./fabric-ca-client enroll -u https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241 --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname PeerOrg1CA
```

falhará e produzirá o erro a seguir:

```
!pw@9.12.19.115: evento não localizado
```

#### **Solução:**
{: #ibp-v2-apis-config-enroll-error2-solution}

É necessário codificar o caractere especial ou circundar a URL com as aspas simples. Por exemplo, `!` torna-se `%21` ou o comando é semelhante a:

```
./fabric-ca-client enroll -u 'https://admin:C25A06287!0@ash-zbc07c.4.secure.blockchain.ibm.com:21241' --tls.certfiles $HOME/fabric-ca-remote/cert.pem --caname PeerOrg1CA
```

## Criando uma definição do MSP da organização
{: #ibp-v2-apis-msp}

É possível usar as APIs para criar uma definição do MSP da organização, chamando [`POST /ak/api/v1/components/msp`](/apidocs/blockchain?code=try#import-an-msp). Esse MSP contém certificados que definem sua organização em um consórcio de blockchain, bem como os certificados de administrador que podem ser usados para operar sua rede. Se você seguiu a etapa acima, já gerou os certificados necessários para criar um MSP da organização. Use as etapas a seguir para concluir o corpo da solicitação da chamada da API.

1. Insira `msp` como o `type` de solicitação.

2. Selecione um ID do MSP para sua organização. O ID do MSP é o nome formal de sua organização dentro do consórcio. O ID do MSP usado para criar o MSP da organização precisa ser o mesmo que você usa para implementar seus peers.

3. Selecione um nome de exibição para seu MSP.

4. É necessário fornecer o signCert, no formato base64, de seu administrador da organização que você registrou e inscreveu usando o cliente Fabric CA.  

  Navegue para o diretório do MSP que foi criado quando você [gerou certificados usando o administrador da organização](#ibp-v2-apis-config-enroll-admin).
  ```
  cd $HOME/fabric-ca-client/peer-admin/msp
  ```
  Neste diretório MSP, abra o arquivo signCert do novo usuário e converta-o para o formato base64 usando os comandos a seguir:

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-peer-admin>/msp/signcerts/cert.pem | base64 $FLAG
  ```
  {:codeblock}

  **Nota:** é importante que a sequência gerada usando o comando acima seja formatada como uma única linha. Isso deve ser semelhante a:

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdLZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpFVk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFXTmxjblF1WTI5dE1TQXdIZ1lEVlFRREV4ZEVhV2RwUTJWeWRDQkhiRzlpWVd3Z1VtOXZkQ0JEDQpRVEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJBWVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBaMmxEWlhKMElGTklRVElnDQpVMlZqZFhKbElGTmxjblpsY2lC
  ```
  Não assim:

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdL
  ZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpF
  Vk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFX
  VEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJB
  WVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBa
  ```

  Por exemplo:

  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/fabric-ca-client/peer-admin/msp/signcerts/cert.pem | base64 $FLAG
  ```
  {:codeblock}

  Esse comando imprime uma sequência semelhante ao exemplo a seguir:

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNuRENDQWtPZ0F3SUJBZ0lVTXF5VDhUdnlwY3lYR2sxNXRRY3hxa1RpTG9Nd0NnWUlLb1pJemowRUF3SXcKYURFTTlEKaFhTTzRTWjJ2ZHBPL1NQZWtSRUNJQ3hjUmZVSWlkWHFYWGswUGN1OHF2aCtWSkhGeHBLUnQ3dStHZDMzalNSLwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
  ```

  Forneça essa sequência para o campo `admins` da solicitação de API.

5. Também é necessário fornecer o certificado raiz de sua CA. Esse certificado foi criado quando você [gerou certificados usando seu administrador de CA](#ibp-v2-apis-enroll-ca-admin).

  Navegue para o diretório do MSP do administrador de CA.
  ```
  cd $HOME/fabric-ca-client/ca-admin/msp
  ```
  Nesse diretório, abra a pasta cacerts e converta o certificado dentro do formato base64 usando os comandos a seguir:
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/<path-to-ca-admin>/msp/cacerts/<ca_root_cert>.pem | base64 $FLAG
  ```
  {:codeblock}

  Isso imprimirá o certificado raiz como uma sequência codificada em base64.

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGekNDQWIyZ0F3SUJBZ0lVQmZnZzcvVnIrL25OVEFNQlQ4UUtHL00wQU8wd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU1TURVd016RXpNamt3TUZvWERUTTBNRFF5T1RFek1qa3dNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUVXMUtvN2lWeVE2VWkwdDVqbU5KaWVuSUwKR3pNM1BDWHlhL2VSQ0NWMmFQb0dTZ1lrVUg2UWN5RjAzbFlMZFU4Y0drNTQ0alViVC9KT1lYeVgzTWc4bHFORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZDK2lJR0NSb2Zvb3FsVkZoU3dOMmk2MXNJaVBNQW9HQ0NxR1NNNDlCQU1DQTBnQU1FVUNJUURTYW9RL1E0QzkKbFl1VGNhVXVHb3d6YmhUZHBuN2F3S2lHN1Nvd2lSQXVld0lnUWlyM3RNR3IvYWo2aU5lRXJFN2NyOVowQ0gvTwp3QnNQcWd4RVR3MjVqZUU9Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
  ```

  Forneça essa sequência para o campo `root_certs` da solicitação de API.

6. Também será necessário fornecer o certificado raiz de sua CA TLS. O certificado raiz TLS permite que seus peers participem em gossip em um canal.

  Navegue para o diretório do MSP que foi gerado quando você [inscreveu seu administrador de CA TLS](#ibp-v2-apis-config-register-component-tls).
  ```
  cd $HOME/fabric-ca-client/tlsca-admin/msp
  ```
  Nesse diretório, abra a pasta cacerts e converta o certificado dentro do formato base64 usando os comandos a seguir:
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  cat $HOME/<path-to-tlsca-admin>/msp/cacerts/<tls_root_cert>.pem | base64 $FLAG
  ```
  {:codeblock}

  Isso imprimirá o certificado raiz da CA TLS como uma sequência codificada em base64.

  ```
  LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNHRENDQWI2Z0F3SUJBZ0lVWUVQWnprNXV2b3dobEtacG5JMXplODdIQUlnd0NnWUlLb1pJemowRUF3SXcKWFRFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVE0d0RBWURWUVFERXdWMGJITmpZVEFlCkZ3MHhPVEExTURNeE16STVNREJhRncwek5EQTBNamt4TXpJNU1EQmFNRjB4Q3pBSkJnTlZCQVlUQWxWVE1SY3cKRlFZRFZRUUlFdzVPYjNKMGFDQkRZWEp2YkdsdVlURVVNQklHQTFVRUNoTUxTSGx3WlhKc1pXUm5aWEl4RHpBTgpCZ05WQkFzVEJrWmhZbkpwWXpFT01Bd0dBMVVFQXhNRmRHeHpZMkV3V1RBVEJnY3Foa2pPUFFJQkJnZ3Foa2pPClBRTUJCd05DQUFRdSs2UnZWd2w5T2dDVlAraEVxbjVxdExRVG9LWkw4a1lic0pOeU1JbERoc3hlNWx6cW1zQkoKbTk2eUR2TVV6OSsxL2pzb1M4M1JqMVAwc3M2TnJNb3FvMXd3V2pBT0JnTlZIUThCQWY4RUJBTUNBUVl3RWdZRApWUjBUQVFIL0JBZ3dCZ0VCL3dJQkFUQWRCZ05WSFE0RUZnUVVnUEc4anJEK1BxVjdoelc3WDlsbTFrMS91WjR3CkZRWURWUjBSQkE0d0RJY0VxVGJESW9jRUNwbnBkVEFLQmdncWhrak9QUVFEQWdOSUFEQkZBaUVBenk3cHJZaVMKQmlDVWdYeWRkY09WMm9mZmtqaEI0N091QXFjQWNqZS9SWkVDSUdKZFgzZ1ErTDRIN3duY1RoZkwrenU1ejV1UApGUWhXTmlNS3hQWEYrZnYwCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
  ```

  Forneça essa sequência para o campo `tls_root_certs` da solicitação de API.

## Criando um arquivo de configuração
{: #ibp-v2-apis-config}

É necessário concluir um arquivo de configuração para criar um peer ou nó de pedido usando as APIs. Esse arquivo é fornecido para a API como o objeto `config` no corpo da solicitação da chamada da API. Se você estiver criando múltiplos nós de pedido, será necessário fornecer um arquivo de configuração para cada nó que você deseja criar em uma matriz para a solicitação de API. Por exemplo, para um serviço de pedido de Raft de cinco nós, é necessário criar uma matriz de cinco arquivos de configuração. É possível fornecer o mesmo arquivo para cada nó, desde que o ID de inscrição que você forneça tenha um limite de inscrição suficientemente alto. É necessário implementar uma CA em sua instância de serviço do {{site.data.keyword.cloud_notm}} Platform e seguir as etapas para registrar e inscrever as identidades necessárias antes de concluir o arquivo.

O modelo para o arquivo de configuração pode ser localizado abaixo:
```
{
	"enrollment": {
		"component": {
			"cahost": "", 			"caport": "", 			"caname": "", 			"catls": {
				"cacert": ""
			}, 			"enrollid": "", 			"enrollsecret": "", 			"admincerts": [""]
		}, 		"tls": {
			"cahost": "", 			"caport": "", 			"caname": "", 			"catls": {
				"cacert": ""
			}, 			"enrollid": "", 			"enrollsecret": "", 			"csr": {
				"hosts": [""] }
		}
	}
}
```
{:codeblock}

Copie esse arquivo inteiro em um editor de texto no qual é possível editá-lo e salvá-lo em seu sistema de arquivos local como um arquivo JSON. Use as etapas abaixo para concluir esse arquivo de configuração e use-o para implementar um serviço de pedido ou peer.

### Recuperar as informações de conexão de CA
{: #ibp-v2-apis-config-connx-info}

Primeiro, é necessário fornecer as informações de conexão de sua CA no {{site.data.keyword.cloud_notm}} Platform. É possível usar a IU do console ou as APIs para obter as informações necessárias sobre sua CA.

**Se você estiver usando o console do {{site.data.keyword.blockchainfull_notm}} Platform:**
abra a CA em seu console e clique em **Configurações**, em seguida, no botão **Exportar** para exportar as informações de CA para um arquivo JSON. É possível usar os valores desse arquivo para concluir seu arquivo de configuração.

**Se você estiver usando as APIs:**,
será possível chamar [`GET /ak/api/v1/components`](https://test.cloud.ibm.com/apidocs/blockchain?code=try#get-all-components) para obter as informações de conexão de sua CA. Se você criou a CA usando a API `Create a Fabric CA`, também é possível localizar as informações necessárias no corpo de resposta.

- Os valores `"cahost"` e `"caport"` são visíveis no campo `ca_url` no corpo de resposta ou no arquivo JSON de CA que você exportou.  Por exemplo, se seu `ca_url` for https://9.30.94.174:30167, o valor do `"cahost"` seria `9.30.94.174` e o `"caport"` seria `30167`.
- O `"caname"` é o nome da CA que foi especificado quando você implementou a CA. Esse é o valor do campo `ca_name` no corpo de resposta ou no arquivo JSON exportado.
- O `"cacert"` é o certificado TLS codificado em base64 de sua CA. Esse é o valor do campo `pem` no corpo de resposta ou no arquivo JSON exportado.

- Os campos na seção `"tls"` abaixo requerem as mesmas informações que você transmitiu para as seções do componente acima, exceto que é necessário usar o valor do nome da instância TLS da CA que foi especificado durante a implementação da CA. Substitua `caname` pelo valor de `tlsca_name` no corpo de resposta ou no arquivo JSON exportado. Use o mesmo certificado TLS para o valor de `"cacert"`.
  ```
  "tls": {
    "cahost": "",
    "caport": "",
    "caname": "",
    "catls": {
      "cacert": ""
  ```
  {:codeblock}

### Fornecer o ID e o segredo de inscrição do componente

1. Cole o `name` e o `secret` que você usou para [registrar seu componente com sua CA padrão](#ibp-v2-apis-config-register-component) como o `"enrollid"` e `"enrollsecret"` no arquivo de configuração, sob a seção `"component"`:

  ```
  "component": {...
    },
    "enrollid": "peer1",
    "enrollsecret": "peer1pw",
  ```
2. Cole o `name` e o `secret` que você usou para [registrar seu componente com sua CA tls](#ibp-v2-apis-config-register-component-tls) como o `"enrollid"` e `"enrollsecret"` no arquivo de configuração, sob a seção `"tls"`:

  ```
  "tls": {...
    },
    "enrollid": "peertls",
    "enrollsecret": "peertlspw",
  ```

### Fornecer o signCert de seu administrador da organização

Navegue para o diretório do MSP que foi criado quando você [gerou certificados usando o administrador da organização](#ibp-v2-apis-config-enroll-admin).
```
cd $HOME/fabric-ca-client/peer-admin/msp
```
Neste diretório MSP, abra o arquivo signCert do novo usuário e converta-o para o formato base64 usando os comandos a seguir:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/<path-to-peer-admin>/msp/signcerts/cert.pem | base64 $FLAG
```
{:codeblock}

**Nota:** é importante que a sequência gerada usando o comando acima seja formatada como uma única linha. Isso deve ser semelhante a:

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdLZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpFVk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFXTmxjblF1WTI5dE1TQXdIZ1lEVlFRREV4ZEVhV2RwUTJWeWRDQkhiRzlpWVd3Z1VtOXZkQ0JEDQpRVEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJBWVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBaMmxEWlhKMElGTklRVElnDQpVMlZqZFhKbElGTmxjblpsY2lC
```
Não assim:

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tDQpNSUlFbERDQ0EzeWdBd0lCQWdJUUFmMmo2MjdL
ZGNpSVE0dHlTOCs4a1RBTkJna3Foa2lHOXcwQkFRc0ZBREJoDQpNUXN3Q1FZRFZRUUdFd0pWVXpF
Vk1CTUdBMVVFQ2hNTVJHbG5hVU5sY25RZ1NXNWpNUmt3RndZRFZRUUxFeEIzDQpkM2N1WkdsbmFX
VEFlRncweE16QXpNRGd4TWpBd01EQmFGdzB5TXpBek1EZ3hNakF3TURCYU1FMHhDekFKQmdOVkJB
WVRBbFZUDQpNUlV3RXdZRFZRUUtFd3hFYVdkcFEyVnlkQ0JKYm1NeEp6QWxCZ05WQkFNVEhrUnBa
```

Por exemplo:

```
export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
cat $HOME/fabric-ca-client/peer-admin/msp/signcerts/cert.pem | base64 $FLAG
```
{:codeblock}

Esse comando imprime uma sequência semelhante ao exemplo a seguir:

```
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNuRENDQWtPZ0F3SUJBZ0lVTXF5VDhUdnlwY3lYR2sxNXRRY3hxa1RpTG9Nd0NnWUlLb1pJemowRUF3SXcKYURFTTlEKaFhTTzRTWjJ2ZHBPL1NQZWtSRUNJQ3hjUmZVSWlkWHFYWGswUGN1OHF2aCtWSkhGeHBLUnQ3dStHZDMzalNSLwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
```

Copie essa sequência para o campo `"admincerts"` na seção do componente no arquivo de configuração.

### Hosts de CSR (Solicitação de Assinatura de Certificado)

Você tem a opção de fornecer um domínio customizado para seu componente usando a seção `"csr"` do arquivo do componente.

```
"csr": {
  "hosts": [""]
  }
```
{:codeblock}

Essa seção é fornecida para que os usuários avançados especifiquem um nome de host customizado que pode ser usado para direcionar o terminal de peer. A maioria dos usuários pode deixar essa seção em branco.

### Concluindo o arquivo de configuração
{: #ibp-v2-apis-config-file}

Depois de concluir todas as etapas acima, seu arquivo de configuração atualizado poderá ser semelhante ao exemplo a seguir:


```
{
    "enrollment": {
        "component": {
            "cahost": "9.30.20.70",
            "caport": "32129",
            "caname": "ca",
            "catls": {
                "cacert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVTmlUbkdTandSdU1JVXhpWGcwMGZZWXhPSndJd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJ0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE5qRTJNVEF3TUZvWERUTXpNVEV4TWpFMk1UQXdNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUU1dlBucDJUNTdkY2hTOGRLNExsMFJRZEEKd284RmJsMzBPcnBGdWFHUld5TFl4eGcxcVFTemhUY3hTcGtHZjh3a1FzVDVFb01lSWcrRytldjBOU01FUTZORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQW9HQ0NxR1NNNDlCQU1DQTBjQU1FUUNJR0FCRmNSdXhtSkIKY3c4OTJJOXhPS3YxVmYyT0JHZUh5N2pFQzRBRm5najFBaUJqdHFvdjBXMXdxZjhwcGttYkxIQkJoWW1vS3ZqRwo4bDQyeVQ5bWYxWVQrZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
            },
            "enrollid": "peer1",
            "enrollsecret": "peer1pw",
            "admincerts": [
                "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMzRENDQW9PZ0F3SUJBZ0lVS2Vib0drZEJqenM5bllRbkVQTWp0VG56YTVrd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE9URTNNRE13TUZvWERURTVNVEV4T1RFM01EZ3dNRm93ZmpFTE1BaKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApjbXhsWkdkbGNqRXVNQXNHQTFVRUN4TUVkWE5sY2pBTEJnTlZCQXNUQkc5eVp6RXdFZ1lEVlFRTEV3dGtaWEJoCmNuUnRaVzUwTVRFUU1BNEdBMVVFQXhNSFlXUnRhVzR4Y0RCWk1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUgKQTBJQUJLbjUwdEU5TmpZb0RFNDBqalh6RUJ2T2c3Y3RtOElRd0laMnRkS29iNEwwVVhKdSs1Tkt5S2dyUk9vbApWcjBmQmg5cWZWMjl4Nms0T2dmMFNiVklBZWlqZ2ZRd2dmRXdEZ1lEVlIwUEFRSC9CQVFEQWdlQU1Bd0dBMVVkCkV3RUIvd1FDTUFBd0hRWURWUjBPQkJZRUZOYWFkV0VzcGp2Smk1akpiVktiS2M3ZU1wUmhNQjhHQTFVZEl3UVkKTUJhQUZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQ2NHQTFVZEVRUWdNQjZDSEdOb1lXNWtjbUZ6TFcxaQpjQzV5WVd4bGFXZG9MbWxpYlM1amIyMHdhQVlJS2dNRUJRWUhDQUVFWEhzaVlYUjBjbk1pT25zaWFHWXVRV1ptCmFXeHBZWFJwYjI0aU9pSnZjbWN4TG1SbGNHRnlkRzFsYm5ReElpd2lhR1l1Ulc1eWIyeHNiV1Z1ZEVsRUlqb2kKWVdSdGFXNHhjQ0lzSW1obUxsUjVjR1VpT2lKMWMyVnlJbjE5TUFvR0NDcUdTTTQ5QkFNQ0EwY0FNRVFDSURGeApDYzE1bDZUZ1dqYnhSZzlmNjczOGV0K0NZZ1I3VVpGR200VHdoQk5jQWlBNmtUMFFwbDV6WnBUN3BzM0dySXlVCmEydDRHSTQ5ZTdjUm5PMmdrSzl6Z3c9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg=="
            ]
        },
        "tls": {
            "cahost": "9.30.20.70",
            "caport": "32129",
            "caname": "tlsca",
            "catls": {
                "cacert": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUNGakNDQWIyZ0F3SUJBZ0lVTmlUbkdTandSdU1JVXhpWGcwMGZZWXhPSndJd0NnWUlLb1pJemowRUF3SXcKYURFTE1Ba0dBMVVFQmhNQ1ZWTXhGekFWQmdOVkJBZ1REazV2Y25Sb0lFTmhjbTlzYVc1aE1SUXdFZ1lEVlFRSwpFd3RJZVhCbGNteGxaR2RsY2pFUE1BMEdBMVVFQ3hNR1JtRmljbWxqTVJrd0Z3WURWUVFERXhCbVlXSnlhV010ClkyRXRjMlZ5ZG1WeU1CNFhEVEU0TVRFeE5qRTJNVEF3TUZvWERUTXpNVEV4TWpFMk1UQXdNRm93YURFTE1Ba0cKQTFVRUJoTUNWVk14RnpBVkJnTlZCQWdURGs1dmNuUm9JRU5oY205c2FXNWhNUlF3RWdZRFZRUUtFd3RJZVhCbApXhsWkdkbGNqRVBNQTBHQTFVRUN4TUdSbUZpY21sak1Sa3dGd1lEVlFRREV4Qm1ZV0p5YVdNdFkyRXRjMlZ5CmRtVnlNRmt3RXdZSEtvWkl6ajBDQVFZSUtvWkl6ajBEQVFjRFFnQUU1dlBucDJUNTdkY2hTOGRLNExsMFJRZEEKd284RmJsMzBPcnBGdWFHUld5TFl4eGcxcVFTemhUY3hTcGtHZjh3a1FzVDVFb01lSWcrRytldjBOU01FUTZORgpNRU13RGdZRFZSMFBBUUgvQkFRREFnRUdNQklHQTFVZEV3RUIvd1FJTUFZQkFmOENBUUV3SFFZRFZSME9CQllFCkZMd2d1N0J3Uk9lQ2hzV2hWQWptMTdmalh1eVBNQW9HQ0NxR1NNNDlCQU1DQTBjQU1FUUNJR0FCRmNSdXhtSkIKY3c4OTJJOXhPS3YxVmYyT0JHZUh5N2pFQzRBRm5najFBaUJqdHFvdjBXMXdxZjhwcGttYkxIQkJoWW1vS3ZqRwo4bDQyeVQ5bWYxWVQrZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K"
            },
            "enrollid": "peertls",
            "enrollsecret": "peertlspw",
            "csr": {
                "hosts": [
                ]
            }
        }
    }
}
```
{:codeblock}

É possível deixar os outros campos em branco. Depois de concluir esse arquivo, é possível passá-lo como o campo `config` para o corpo da solicitação da API `Criar um serviço de pedido` ou `Criar um peer`.

### Importando uma identidade de administrador para o console do {{site.data.keyword.blockchainfull_notm}} Platform
{: #ibp-v2-apis-admin-console}

Se você desejar usar o console do {{site.data.keyword.blockchainfull_notm}} Platform para operar seus componentes de blockchain, deve-se importar sua identidade de administrador para a sua carteira eletrônica do console. Abra o painel de carteira eletrônica em seu console. Clique no botão **Incluir identidade** na tela de visão geral. Clicar nesse botão abrirá um painel lateral que permite incluir o certificado de assinatura e a chave privada de uma identidade diretamente no console.
- **Nome:** insira um nome de exibição para a identidade que é usada somente para sua referência.
- **Certificado:** faça upload do certificado de assinatura de seu administrador. Se você seguiu as instruções acima, será possível localizar essa chave na pasta `$HOME/fabric-ca-client/peer-admin/msp/signcerts/`.
- **Chave privada:** faça upload de sua chave privada de administradores. Se você seguiu as instruções acima, será possível localizar essa chave na pasta `$HOME/fabric-ca-client/peer-admin/msp/keystore/`.

Depois de importar sua identidade de administrador, será possível associar essa identidade aos componentes que você criou. É possível, então, usar o console para operar sua rede.
