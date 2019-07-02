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

# Administrando seu console
{: #console-icp-manage}

Depois de implementar seu console no {{site.data.keyword.cloud}} Private, é possível usar o console para incluir ou remover usuários do console, bem como APIs de acesso que permitem que você opere sua rede e governe seu console. Também é possível acessar e customizar os logs de seu console.
{:shortdesc}

## Gerenciando usuários por meio do console
{: #console-icp-manage-users}

O usuário que fornece o console do {{site.data.keyword.blockchainfull_notm}} Platform é considerado como o administrador do console. Então, o administrador pode incluir e conceder acesso para outros usuários ao console usando a guia **Usuários** no console. Todo usuário que acessa o console deve ter uma política de acesso designada com uma função de usuário definida. A política determina quais ações o usuário pode executar dentro do console. Por padrão, ao administrador do console é fornecida a função **Gerenciador** para o console. Outros usuários poderão ter as funções **Gerenciador**, **Gravador** ou **Leitor** designadas quando um gerenciador de console os incluir no console. Observe que os usuários também podem ser gerenciados com [APIs](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-users-apis).

| Função | Recursos |
|--------|----------|
| Gerente | Como um Gerenciador, você tem permissões que vão além da função Gravador. É possível fazer tudo que um Leitor e um Gravador podem fazer, bem como: <ul><li>Provisionar novos componentes usando o console ou as APIs.</li><li>Excluir os componentes provisionados usando o console ou as APIs.</li><li>Incluir/remover usuários e mudar as políticas de acesso de usuário.</li><li>Mudar os níveis de criação de log do console usando o console ou as APIs.</li><li>Reiniciar o console usando uma API.</li></ul> |
| Gravador | Como um Gravador, você tem permissões que vão além da função Leitor, incluindo: <ul><li>Importe componentes usando o console ou as APIs.</li><li>Remover os componentes importados usando o console ou as APIs.</li><li>Registrar usuários em uma CA.</li><li> Incluir ou remover notificações usando o console ou as APIs.</li></ul>  |
| Leitor | Como um leitor, é possível executar ações somente leitura, incluindo: <ul><li>Visualizar a IU do console.</li><li>Visualizar o log do console.</li><li>Exportar componentes.</li><li>Emitir qualquer API GET.</li></ul> | |

As permissões são acumulativas. Se você selecionar uma função **Gerenciador**, o usuário também será capaz de executar todas as ações de **Gravador** e **Leitor** e não será necessário verificar adicionalmente essas funções. Da mesma forma, um usuário com a função **Writer** será capaz de executar todas as ações da função **Reader**.

### Gerenciando a senha de um usuário
{: #console-icp-manage-user-pw}

A senha que foi armazenada dentro do [segredo de senha](/docs/services/blockchain/howto?topic=blockchain-console-deploy-icp#console-deploy-icp-password-secret) e, em seguida, transmitida para o console durante a configuração, torna-se a senha padrão para o console. Todos os usuários precisam usar essa senha ao efetuar login no console pela primeira vez. O gerenciador do console também pode especificar uma nova senha padrão. Na guia **Usuários** do console, clique no link **Atualizar configuração** sob o nome do bloco do seu serviço de autenticação. No painel direito, é possível visualizar ou atualizar a senha padrão para os novos usuários do console.

É necessário compartilhar com os usuários a senha padrão ou a senha padrão que você reconfigurou para que eles possam efetuar login no console. Eles deverão mudar a senha após o primeiro login.
{:note}

Os usuários com a função de gerenciador podem reconfigurar as senhas para outros usuários. Na guia **Usuários** do console, clique nos três pontos verticais no final da linha para um usuário específico e, em seguida, clique em **Reconfigurar senha**. O console reconfigurará a senha desse usuário para a senha padrão, o que é possível verificar e confirmar no painel à direita. Os usuários com qualquer função podem mudar sua própria senha a qualquer momento. Clique no ícone do avatar no canto superior direito e, em seguida, clique em **Mudar senha**.

Depois de incluir novos usuários no console, eles poderão não ser capazes de visualizar todos os nós, canais ou chaincode implementados por outros. Para trabalhar com esses componentes, cada usuário precisa importar as identidades associadas para sua própria carteira eletrônica do console. Para obter mais informações, consulte [Armazenando identidades em sua carteira eletrônica do console](/docs/services/blockchain/howto?topic=blockchain-ibp-console-identities#ibp-console-identities-wallet).
{:important}

### Modificando a função de um usuário
{: #console-icp-manage-reset-user-pw}

Um usuário com uma função de gerenciador pode atualizar as funções de outros usuários do console e fornecer a eles acesso a mais ou menos recursos do console. Na guia **Usuários** do console, clique nos três pontos verticais no término da linha do usuário específica e, em seguida, clique em **Atualizar usuário autenticado**. Selecione a nova função para o usuário no painel direito. A função do usuário será atualizada quando ele efetuar login no console na próxima vez.

### Excluindo um usuário do console
{: #console-icp-manage-icp-remove-user}

Se você for um usuário com uma função de gerenciador, será possível remover o acesso de um usuário ao console. Na guia **Usuários** do console, clique na caixa de seleção no início da linha do usuário específica. Em seguida, clique no ícone **Lixeira** na parte superior da tabela, ao lado do botão **Incluir novos usuários**. Depois de remover o usuário da tabela, o usuário não poderá efetuar login no console novamente.

## Usando as APIs do {{site.data.keyword.blockchainfull_notm}}
{: #console-icp-manage-apis}

O {{site.data.keyword.blockchainfull_notm}} Platform expõe as APIs de RESTful que permitem importar, editar e remover nós de blockchain do console. Também é possível usar as APIs para gerenciar as configurações, a criação de log e as notificações do console. Use as instruções abaixo para usar as APIs fornecidas por um console implementado no {{site.data.keyword.cloud_notm}} Private.

### Pré-requisitos
{: #console-icp-manage-prereqs}

Para usar as APIs, será necessário reunir as informações a seguir:

- O console **Terminal de API**. Essa é a URL que você usa para acessar o console usando o seu navegador. É possível localizar essa URL na seção **Notas:** da tela de visão geral da liberação do Helm.
- Um nome de usuário e uma senha que podem ser usados para acessar o console. Para criar uma chave de API, deve-se ter uma conta com uma [função de gerenciador](#console-icp-manage-users).

### Usando uma chave API
{: #console-icp-manage-create-api-key}

Cada console fornece seu próprio gerenciamento de identidade e de acesso. É possível usar o nome de usuário e a senha do console para gerar uma chave de API e o segredo que podem autorizar suas chamadas API.

Cada chave de API está associada a uma função que controla quais recursos o usuário tem permissão para executar. As chaves de API usam as mesmas funções de política de acesso que existem para os usuários que efetuam login no console usando um nome de usuário e senha. Consulte a tabela na seção [Gerenciando usuários](#console-icp-manage-users) para obter a lista de ações que cada função tem permissão para executar. Como somente funções de gerenciador podem criar chaves de API, um administrador do console precisa criar chaves de API para usuários leitores e gravadores. As chaves de API nunca expiram. Como resultado, o administrador do console deve excluir as chaves de API que não estão sendo usadas.

Há três APIs disponíveis para gerenciar suas chaves de API:
- [Crie uma chave de API](#console-icp-manage-create-api-key)
- [Visualizar chaves API](#console-icp-manage-view-api-keys)
- [Excluir chaves de API](#console-icp-manage-delete-api-keys)

Use a solicitação abaixo para criar uma chave de API e um segredo. Então, é possível usar essa chave e o segredo para usar as outras APIs. O segredo da API não é armazenado pelo console e precisa ser salvo pelo usuário.

#### Crie uma chave de API
{: #console-icp-manage-create-api-key-api}

| **Solicitação** |  |
|-------------|-----------|
| PATH | POST `<API_endpoint>`/ak/api/v1/permissions/keys |
| **Campos do corpo da solicitação** | |
| <ul><li>`funções`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` Pelo menos um valor é necessário </li><li>`string` opcional</li></ul>|
| **Campos do corpo de resposta** | |
| <ul><li>`api_key`</li><li>`description`</li><li>`funções`</li></ul>| <ul><li>`Sequência`</li><li>`string` Save: a chave não é armazenada</li><li>`["<role>"]`</li></ul>|
| Autorização necessária | gerenciador |

#### Exemplo de solicitação curl: criar chave de API
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

### Gerenciar as chaves da API do console
{: #console-icp-manage-api-keys}

Depois de ter criado uma chave de API e o segredo, será possível usar as APIs para visualizar ou remover as chaves criadas. As chaves e os segredos não expiram até que sejam excluídos.

#### Visualizar chaves API
{: #console-icp-manage-view-api-keys}

| **Solicitação** |  |
|-------------|-----------|
| Caminho | GET `<API_endpoint>`/ak/api/v1/permissions/keys |
| **Campos do corpo de resposta** | |
| <ul><li>`api_key`</li><li>`funções`</li><li>`ts_created`</li><li>`description`</li></ul>| <ul><li>`Sequência`</li><li>`["<role>"]`</li><li>`number` Registro de data e hora do Unix em milissegundos</li><li>`Sequência`</li></ul>|
| Autorização necessária | leitor |

#### Exemplo de solicitação curl: visualizar chaves da API
{: #console-icp-manage-view-api-key-example}
```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys \
  -u <api_key>:<api_secret>
```

#### Excluir chaves de API
{: #console-icp-manage-delete-api-keys}

| **Solicitação** |  |
|-------------|-----------|
| Caminho | DELETE `<API_endpoint>`/ak/api/v1/permissions/keys:`<api_key>` |
| Autorização necessária| gerenciador |

#### Exemplo de solicitação curl: excluir chave de API
{: #console-icp-manage-delete-api-keys-example}


```
curl -X DELETE \
  https://9.30.252.107:31212/ak/api/v1/permissions/keys/:<api_key> \
  -u <api_key>:<api_secret>
```

### Gerenciando usuários usando as APIs
{: #console-icp-manage-users-apis}


Também é possível usar as APIs para listar, incluir ou remover usuários que podem efetuar login no console ou acessar as APIs do console. Também é possível editar as funções de usuários para fazer upgrade de um nível de acesso de usuários. As APIs a seguir estão disponíveis:

- [Listar usuários](#console-icp-manage-list-users-api)
- [Editar usuários](#console-icp-manage-edit-users-api)
- [Incluir usuários](#console-icp-manage-add-users-api)
- [Remover usuários](#console-icp-manage-remove-users-api)

Para as APIs que governam os usuários e as configurações de seu console e gerenciam os nós de sua rede, é necessário incluir um sinalizador ``-K`` ou ``--insecure`` para efetuar bypass do requisito para um certificado TLS. Também é possível fazer download do certificado TLS por meio de seu console usando o navegador.

#### Listar usuários
{: #console-icp-manage-list-users-api}


| **Solicitação** |  |
|-------------|-----------|
| Caminho | GET `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campos do corpo de resposta** | |
| <ul><li>`uuids`</li><li>` E-mail                                  `</li><li>`funções`</li><li>`created`</li></ul>| <ul><li>ID do usuário de `string`</li><li>Endereço de e-mail de `string`</li><li>`["<role>"]`</li><li>`number` Registro de data e hora do Unix em milissegundos</li></ul>|
| Autorização necessária | leitor |

#### Exemplo de solicitação curl: listar usuários
{: #console-icp-manage-list-users-api-example}

```
curl -X GET \
  https://9.30.252.107:31212/ak/api/v1/permissions/users \
  -u <api_key>:<api_secret> \
  -K
```

#### Editar usuários
{: #console-icp-manage-edit-users-api}

| **Solicitação** |  |
|-------------|-----------|
| Caminho | PUT `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campos do corpo da solicitação** | |
| <ul><li>`usuários`</li><li>`funções`</li></ul> | <ul><li>ID do usuário de `string` </li><li>`["reader", "writer", "manager"]` Pelo menos um valor é necessário</li></ul> |
| **Campos do corpo de resposta** | |
| <ul><li>`uuids`</li></ul>| <ul><li>ID do usuário de `string`</li></ul>|
| Autorização| gerenciador |

#### Exemplo de solicitação curl: editar um usuário
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

#### Incluir usuários
{: #console-icp-manage-add-users-api}

| **Solicitação** |  |
|-------------|-----------|
| Caminho | POST `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campos do corpo da solicitação** | |
| <ul><li>`funções`</li><li>`description`</li></ul>| <ul><li>`["reader", "writer", "manager"]` Pelo menos um valor é necessário </li><li>`string` opcional</li></ul>|
| Autorização | gerenciador |

#### Exemplo de solicitação curl: incluir um usuário
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

#### Remover usuários
{: #console-icp-manage-remove-users-api}

| **Solicitação** |  |
|-------------|-----------|
| Caminho | DELETE `<API_endpoint>`/ak/api/v1/permissions/users |
| **Campos do corpo da solicitação** | |
| <ul><li>`usuários`</li></ul>| <ul><li>ID do usuário de `string`</li></ul>|
| **Campos do corpo de resposta** | |
| <ul><li>`uuids`</li></ul>| <ul><li>ID do usuário de `string`</li></ul>|
| Autorização | gerenciador |

#### Exemplo de solicitação curl: remover um usuário
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

### Use as APIs para gerenciar seus componentes

É possível visualizar o conjunto completo de APIs que estão disponíveis na [Referência de API do {{site.data.keyword.blockchainfull_notm}} Platform](https://test.cloud.ibm.com/apidocs/blockchain).

Como você está usando as APIs para se comunicar com o console no {{site.data.keyword.cloud_notm}} Private, é necessário usar a autorização fornecida pelo {{site.data.keyword.cloud_notm}} com a autenticação fornecida pelo console. Substitua o ``Bearer Auth`` na referência de API por ``-u <api_key>:<api_secret>``. Também é necessário incluir um sinalizador ``-K`` ou ``--insecure`` para o comando ou fazer download do certificado TLS do console usando o navegador. Não é possível usar a guia **Experimente** para testar as APIs para as redes no {{site.data.keyword.cloud_notm}} Private.

Como um exemplo, a chamada API abaixo retornará as informações sobre todos os seus componentes em execução em uma instância de serviço do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.
```
curl -X POST \
  https://d456fcd8ee0e4ddfb1ad9bf45986e546-optools.bp01.blockchain.cloud.ibm.com/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer eyJraWQ.....zJPsw
```
A mesma chamada API se pareceria com a solicitação abaixo para um console implementado no {{site.data.keyword.cloud_notm}} Private.
```
curl -X POST \
https://9.30.252.107:31212/ak/api/v1/components \
  -H 'Content-Type: application/json' \
  -u kO25ME32Nu8TikR_:buYImbg0co8SxneoBWzHueYwrf9Xhg5f \
  -K
```

É possível usar as APIs para criar nós no cluster em que seu console está implementado e para importar nós de outros clusters ou do {{site.data.keyword.cloud_notm}}. Para obter mais informações, visite [Construir uma rede usando APIs](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-build-with-apis) e [Importar uma rede usando APIs](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-apis#ibp-v2-apis-import-with-apis).

## Visualizando seus logs
{: #icp-console-manage-logs}

Quando você usa o console do {{site.data.keyword.blockchainfull_notm}} Platform, pode ser necessário visualizar logs para depurar um problema.

### Visualizando os logs do console
{: #console-icp-manage-console-logs}

Será possível acessar facilmente os logs do console se você precisar depurar problemas que encontrar ao usar o console ou operar seus nós. Também é possível configurar o nível de criação de log para aumentar ou diminuir a quantia de logs coletados pelo console. Os logs do console são coletados separadamente dos [logs do nó](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#console-icp-manage-node-logs), que são coletados pelo {{site.data.keyword.cloud_notm}} Private.

Navegue para a guia **Configurações** no navegador do console para mudar as configurações de criação de log. Os logs do console são coletados de duas origens separadas:

  * **Criação de log do cliente:** esses logs são coletados quando os comandos são enviados de seu navegador para o console.
  * **Criação de log do servidor:** esses logs são coletados quando o console envia comandos para os seus nós e por meio da implementação do console. Esses logs incluem a saída de criação de log do Hyperledger Fabric.

Configure a quantidade de logs coletados usando a lista suspensa em cada tipo de log. Por exemplo, **Erro** e **Aviso** coletam a menor quantia de logs, enquanto **Depuração** e **Todos** coletam o máximo.

É possível visualizar apenas os logs do console ao efetuar login como um administrador do console. Para visualizar os logs na guia **Configurações**, substitua a palavra `settings` na URL do navegador por `api/v1/logs`. Por exemplo, se a URL do console for `localhost:3001/settings`, será possível visualizar os logs navegando para `localhost:3001/api/v1/logs`. Os logs do cliente e do servidor são coletados em arquivos separados. Os logs mais recentes estão na parte superior da página.

### Visualizando os logs do nó
{: #console-icp-manage-node-logs}

Os logs de componente podem ser visualizados por meio da linha de comandos usando os [comandos da CLI kubectl](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} ou por meio do [Kibana](https://www.elastic.co/products/kibana){: external}, que está incluído em seu cluster do {{site.data.keyword.cloud_notm}} Private.

- Use o comando `kubectl logs` para visualizar os logs do contêiner dentro do pod. Siga as instruções para [Instalar a CLI kubectl](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/manage_cluster/install_kubectl.html){: external} se você ainda não tiver feito isso. Se você não tiver certeza de seu nome do pod, execute o comando a seguir para visualizar sua lista de pods.

  ```
  kubectl get pods
  ```
  {:codeblock}

  Em seguida, execute o comando a seguir para recuperar os logs para o contêiner do nó que reside dentro do pod:

  ```
  kubectl logs -f <pod_name> -c <node>
  ```
  {:codeblock}

  Substitua `<pod_name>` pelo nome de seu pod por meio da saída de comando acima.  
  Substitua `<node>` por `ca`, `peer` ou `orderer` para visualizar os logs para seu nó.  
  Substitua `<node>` por `fluentd` para visualizar os logs para os seus contratos inteligentes.

  Para obter mais informações sobre o comando `kubectl logs`, consulte a [Documentação do Kubernetes](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs){: external}.

- Como alternativa, é possível [acessar os logs](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/troubleshoot/events.html){: external} por meio de seu console do {{site.data.keyword.cloud_notm}} Private, que abre os logs no Kibana.

  **Nota:** ao visualizar seus logs no Kibana, você pode receber a resposta `No results found`. Essa condição poderá ocorrer se o {{site.data.keyword.cloud_notm}} Private usar o endereço IP do nó do trabalhador como seu nome do host. Para resolver esse problema, remova o filtro que inicia com `node.hostname.keyword` na parte superior do painel e os logs se tornarão visíveis.

### Visualizando os logs do contêiner de contrato inteligente
{: #console-icp-manage-container-logs}

Se você encontrar problemas com seu contrato inteligente, será possível visualizar o contrato inteligente, o chaincode e os logs de contêiner para depurar um problema. É possível executar o comando a seguir para visualizar os logs do contêiner do contrato inteligente:

```
kubectl  logs -f <peer_ped> -c fluentd
```
{:codeblock}

Substitua `<peer_pod>` pelo nome do pod peer no qual o chaincode está em execução. Use o comando `kubectl get po` para obter a lista de pods em execução.

## Instalando correções para seus nós
{: #console-icp-manage-patch}

As imagens de docker subjacentes do {{site.data.keyword.IBM_notm}} Hyperledger Fabric para os nós de peer, da CA e do solicitador podem precisar ser atualizadas ao longo do tempo, por exemplo, com atualizações de segurança ou para uma nova liberação secundária do Fabric. É possível atualizar as imagens do Fabric ao fazer upgrade do gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform.

Depois de fazer upgrade de seu gráfico do Helm, será possível localizar o texto **Correção disponível** em um bloco do nó se uma atualização de componente estiver disponível. Essa correção pode ser instalada em um nó sempre que você estiver pronto. Essas correções são opcionais, mas recomendadas. Não é possível corrigir nós que foram importados para o console.

As correções são aplicadas a nós um por vez. Enquanto a correção está sendo aplicada, o nó está indisponível para processar solicitações ou transações. Portanto, para evitar qualquer interrupção de serviço, sempre que possível, é necessário assegurar que outro nó do mesmo tipo esteja disponível para processar solicitações. A instalação de correções em um nó leva aproximadamente um minuto para ser concluída e quando a atualização é concluída, o nó está pronto para processar solicitações.
{:note}

Para aplicar uma correção a um nó, abra o ladrilho do nó e clique no botão **Instalar correção**. Não é possível corrigir os nós que você importou para o console.
