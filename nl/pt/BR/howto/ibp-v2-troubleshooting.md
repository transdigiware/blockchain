---

copyright:
  years: 2019
lastupdated: "2019-07-16"

keywords: troubleshooting, debug, why, what does this mean, how can I, when I

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Detecção de problemas
{: #ibp-v2-troubleshooting}

Problemas gerais podem ocorrer ao usar o console para gerenciar nós, canais ou contratos inteligentes. Em muitos casos, é possível recuperar-se desses problemas seguindo algumas etapas simples.
{:shortdesc}

Este tópico descreve problemas comuns que podem ocorrer ao usar o console do {{site.data.keyword.blockchainfull_notm}} Platform.

- [Quando passo o mouse sobre o meu nó, o status é `Status unavailable`. O que isso significa?](#ibp-v2-troubleshooting-status-unavailable)
- [Quando passo o mouse sobre o meu nó, o status é `Status undetectable`. O que isso significa?](#ibp-v2-troubleshooting-status-undetectable)
- [Por que minhas operações de nó estão falhando após eu criar meu serviço de peer ou de pedido?](#ibp-console-build-network-troubleshoot-entry1)
- [Por que obtenho o erro `Unable to get system channel` ao abrir meu serviço de pedido?](#ibp-troubleshoot-ordering-service)
- [Por que meu peer falha ao ser iniciado?](#ibp-console-build-network-troubleshoot-entry2)
- [Por que minha instalação, instanciação ou upgrade do contrato inteligente falha?](#ibp-console-smart-contracts-troubleshoot-entry1)
- [Por que o contrato inteligente que eu instalei no peer não está listado na IU?](#ibp-console-build-network-troubleshoot-missing-sc)
- [Como posso visualizar os logs de contêiner do meu contrato inteligente?](#ibp-console-smart-contracts-troubleshoot-entry2)
- [Meu canal, contratos inteligentes e identidades desapareceram do console. Como posso obtê-los de volta?](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-troubleshooting#ibp-v2-troubleshooting-browser-storage)
- [Por que estou obtendo o erro `Unable to authenticate with the enroll ID and secret you provided` quando crio uma nova definição do MSP da organização?](#ibp-v2-troubleshooting-create-msp)
- [Por que estou obtendo o erro `An error occurred when updating channel` quando tento incluir uma organização em meu canal?](#ibp-v2-troubleshooting-update-channel)
- [Meu cluster Kubernetes do {{site.data.keyword.cloud_notm}} expirou. O que isso significa?](#ibp-v2-troubleshooting-cluster-expired)
- [Por que as transações que eu envio do VS Code estão falhando?](#ibp-v2-troubleshooting-anchor-peer)
- [Por que obtenho um erro 401 Desautorizado quando efetuo login no meu console?](#ibp-v2-troubleshooting-console-401)
- [Por que os nós que eu implemento no {{site.data.keyword.cloud_notm}} Private não processam transações e estão falhando ao verificar o funcionamento?](#ibp-v2-troubleshooting-healthchecks)
- [Por que minhas transações estão retornando um erro de política de endosso: a assinatura configurada não satisfez a política?](#ibp-v2-troubleshooting-endorsement-sig-failure)

## Quando passo o mouse sobre o meu nó, o status é `Status unavailable`. O que isso significa?
{: #ibp-v2-troubleshooting-status-unavailable}
{: troubleshoot}

O status do nó no bloco para a CA, o peer ou o nó do pedido é cinza, o que significa que o status do nó não está disponível. Idealmente, quando você passa o mouse sobre qualquer nó, o seu status deve ser `Running`.
{: tsSymptoms}

Esse problema poderá ocorrer caso o nó seja recém-criado e o processo de implementação não tenha sido concluído. Caso o nó seja uma CA, é provável que não esteja em execução.
Se o nó for um nó de peer ou de pedido, essa condição ocorrerá quando o verificador de funcionamento que é executado com relação aos nós de peer ou de pedido não puder entrar em contato com o nó.  A solicitação de status pode falhar com um erro de tempo limite porque o nó não respondeu dentro de um período de tempo específico, o que significa que ele ou a conectividade de rede pode estar inativa.
{: tsCauses}

Se esse for um novo nó, aguarde mais alguns minutos até que a implementação tenha sido concluída. Se não for um novo nó, [examine seus logs associados](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) para ver os erros e determinar a causa.
{: tsResolve}

## Quando passo o mouse sobre o meu nó, o status é `Status undetectable`. O que isso significa?
{: #ibp-v2-troubleshooting-status-undetectable}
{: troubleshoot}

O status do nó no bloco para o nó de peer ou de pedido é amarelo, o que significa que o status do nó não pode ser detectado. Idealmente, quando você passa o mouse sobre qualquer nó, o seu status deve ser `Running`.
{: tsSymptoms}

Essa condição ocorre somente em nós de peer e de pedido que foram *importados* para o console e o verificador de funcionamento não pode ser executado com relação ao nó. Esse status acontece porque um `operations_url` não foi especificado quando o nó foi importado. Uma URL de operações é necessária para que o verificador de funcionamento do nó seja executado. O nó em si é provavelmente `Running`, mas como a URL de operações não foi especificada, seu status não pode ser determinado.
{: tsCauses}

É possível resolver esse problema executando as seguintes etapas:
 1. Clique no quadro do nó para abri-lo.
 2. Clique no ícone **Configurações**.
 3. Clique em **Associar identidade**, visualize e anote a identidade associada a esse nó. Clique em **Cancelar** para fechar esse painel.
 4. Clique em **Exportar** para gerar um arquivo `JSON` para o nó.
 5. Edite o arquivo `JSON` gerado e insira o `operations_url` para o nó. O valor de `operations_url` depende de como ele foi configurado e de diversas configurações de rede. Esse valor precisa ser fornecido pelo administrador de rede que implementou o nó.
 6. Clique em **Excluir (Delete)**. Essa etapa remove o nó importado do console, mas não exclui o nó real.
 7. Na guia **Nós**, clique em **Incluir peer** ou **Incluir serviço de pedido** seguido por **Importar um peer existente** ou **Importar um serviço de pedido existente**.
 8. Clique em **Fazer upload de JSON** e navegue até o arquivo JSON que acabou de editar. Clique em **Avançar**.
 9. Associe a mesma identidade anotada na etapa três.
 10. Clique em **Incluir peer** ou **Incluir serviço de pedido**.

Agora, o verificador de funcionamento poderá ser executado com relação ao nó e relatar seu status.
{: tsResolve}

## Por que minhas operações de nó estão falhando após eu criar meu serviço de peer ou de pedido?
{: #ibp-console-build-network-troubleshoot-entry1}
{: troubleshoot}

Possivelmente, um erro ocorreu ao gerenciar um nó existente. Geralmente, quando isso acontece, é útil consultar os logs do nó.  

Por exemplo, quando você tenta operar o nó, a ação pode falhar.
{: tsSymptoms}

Depois de criar um novo serviço de peer ou de pedido, dependendo de sua configuração de armazenamento em cluster, pode demorar alguns minutos para que os nós estejam prontos para operação.
{: tsCauses}

Verifique o painel do Kubernetes e assegure-se de que o status do peer ou do nó seja `Running`. Em seguida, tente sua ação novamente. Se você ainda estiver tendo problemas após o nó estar ativo, [verifique os seus logs de nó](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) quanto a erros.  
{: tsResolve}

## Por que obtenho o erro `Unable to get system channel` ao abrir meu serviço de pedido?
{: #ibp-troubleshoot-ordering-service}
{: troubleshoot}

Depois de criar um serviço de pedido em seu console do {{site.data.keyword.cloud_notm}} Private, o status é `Running`. Mas quando você abre o serviço de pedido, vê o erro:

```
Não é possível obter o canal do sistema. Se você tiver associado uma identidade sem privilégio administrativo no nó de serviço de pedido, não será possível visualizar ou gerenciar os detalhes do serviço de pedido.
```

{: tsSymptoms}

Essa condição ocorre no console de blockchain em execução no {{site.data.keyword.cloud_notm}} Private. Em navegadores não Chrome, é necessário que você aceite um certificado para que o console se comunique corretamente com o nó.
{: tsCauses}

Há várias maneiras de resolver esse problema:
1. Em suas notas sobre a liberação do Helm, em que a URL do navegador do console é fornecida, há também uma nota para acessar uma URL e aceitar o certificado. Navegue para essa URL e aceite o certificado. Agora abra seu serviço de pedido. O erro não deve mais ocorrer.
2. Se você puder usar um navegador Chrome, abra seu console de blockhain no Chrome e abra seu serviço de pedido. O erro não ocorre. Observe que você precisará exportar suas identidades de sua carteira eletrônica no navegador não Chrome e, em seguida, importá-las para a carteira eletrônica no navegador Chrome para que tudo continue a funcionar.
{: tsResolve}

## Por que meu peer falha ao ser iniciado?
{: #ibp-console-build-network-troubleshoot-entry2}
{: troubleshoot}

É possível que esse erro tenha acontecido sob uma variedade de condições.

O log do peer inclui `2019-02-06 19:43:24.159 UTC [main] InitCmd -> ERRO 001 Cannot run peer because cannot init crypto, folder “/certs/msp” does not exist`
{: tsSymptoms}

- Esse erro pode ocorrer sob as condições a seguir:
  - Ao criar a definição do MSP da organização de serviço de pedido, você especificou um ID de inscrição e um segredo que correspondem a uma identidade do tipo `peer` e não `client`. Ele deve ser do tipo  ` client `.
  - Ao criar a definição do MSP da organização de serviço de pedido, você especificou um ID de inscrição e um segredo que não correspondem ao ID de inscrição ou ao segredo da identidade administrativa da organização correspondente.
  - Ao criar o serviço de peer ou de pedido, você especificou o ID de inscrição e o segredo de uma identidade que não é do tipo 'peer'.

- Abra seu nó de peer ou de CA do serviço de pedido e visualize as identidades registradas listadas na tabela **Usuários registrados**.
- Exclua o peer ou o serviço de pedido e recrie-o, sendo cuidadoso para especificar o ID de inscrição e o segredo corretos.
- Observe que, antes de criar o peer ou o serviço de pedido, é necessário criar um ID de administrador da organização, do tipo 'client'. Certifique-se de especificar o mesmo ID do ID de inscrição ao criar a definição do MSP da organização. Consulte essas instruções para [registrar identidades de peer](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1) e essas instruções para [registrar identidades de solicitador](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-orderer).
{: tsResolve}

## Por que minha instalação, instanciação ou upgrade do contrato inteligente falha?
{: #ibp-console-smart-contracts-troubleshoot-entry1}
{: troubleshoot}

É possível que tenha ocorrido um erro ao instalar, instanciar ou fazer upgrade de um contrato inteligente.  Por exemplo, ao tentar instalar um contrato inteligente em um peer, ele falha com o erro `An error occurred when installing smart contract on peer.`
{: tsSymptoms}

Você poderá receber esse erro se essa versão do contrato inteligente já existir no peer ou se o seu peer estiver sem recursos.
{: tsCauses}

- Abra o painel do Kubernetes e assegure-se de que o status do peer seja `Running`.  
- Abra o nó de peer e assegure-se de que a versão do contrato inteligente ainda não exista no peer e tente novamente com a versão adequada.
- Se você ainda estiver tendo problemas após o nó estar ativo, [verifique os seus logs de nó](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-node-logs) quanto a erros.  
{: tsResolve}

## Por que o contrato inteligente que eu instalei no peer não está listado na IU?
{: #ibp-console-build-network-troubleshoot-missing-sc}
{: troubleshoot}

Um contrato inteligente foi instalado em um peer, mas quando você clica na guia **Contratos inteligentes**, ele não é listado.
{: tsSymptoms}

Esse problema pode ocorrer quando outro usuário ou aplicativo instala o contrato inteligente no peer e você não tem a identidade de administrador de peer em sua carteira eletrônica do navegador.
{: tsCauses}

Para visualizar os contratos inteligentes instalados em um peer, é necessário que você seja um administrador de peer. Assegure-se de que a identidade do administrador de peer exista em sua carteira eletrônica do navegador. Se não existir, será necessário importá-la para a sua carteira eletrônica do console.
{: tsResolve}

## Como posso visualizar os logs de contêiner do meu contrato inteligente?
{: #ibp-console-smart-contracts-troubleshoot-entry2}
{: troubleshoot}

Talvez seja necessário visualizar os logs de contêiner do seu contrato inteligente, ou chaincode, para depurar um problema do contrato inteligente.
{: tsSymptoms}

Siga estas instruções para visualizar os logs do contêiner de contrato inteligente em:
- [{{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-container-logs).
- [{{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-console-icp-manage#console-icp-manage-container-logs).
{: tsResolve}

## Meu canal, contratos inteligentes e identidades desapareceram do console. Como posso obtê-los de volta?
{: #ibp-v2-troubleshooting-browser-storage}
{: troubleshoot}

Suas identidades de carteira eletrônica do console consistem em um certificado de assinatura e uma chave privada que permitem gerenciar os componentes de blockchain, mas eles são armazenados somente em seu armazenamento local do navegador. Você é responsável por proteger e gerenciar essas identidades. Recomendamos que você as exporte para o sistema de arquivos depois de criá-las. Sempre que você cria um novo nó, associa uma identidade de sua carteira eletrônica do console ao nó. O que permite o gerenciamento do nó é essa identidade do administrador. Quando você alterna os navegadores ou muda para um navegador em uma máquina diferente, essas identidades não estão mais em sua carteira eletrônica. Portanto, não é possível gerenciar os componentes.
{: tsSymptoms}

Um dos novos recursos do {{site.data.keyword.blockchainfull_notm}} Platform é que agora você é responsável por proteger e gerenciar seus certificados. Portanto, eles são persistidos apenas no armazenamento local do navegador para permitir que você gerencie o componente. Se estiver usando uma janela de navegador privada e, em seguida, alternar para uma janela de navegador não privada ou para outro navegador, as identidades criadas serão removidas da carteira eletrônica de seu console na nova sessão do navegador. Portanto, é necessário que você exporte as identidades da carteira eletrônica do console em sua sessão de navegador privada para seu sistema de arquivos. Em seguida, será possível importá-las para a sessão do navegador não privada, se elas forem necessárias. Caso contrário, não haverá formas de recuperá-las.
{: tsCauses}

- Sempre que você criar uma nova definição do MSP da organização, gere chaves para uma identidade que tenha permissão para administrar a organização. Portanto, durante esse processo, deve-se clicar nos botões **Gerar** e, em seguida, **Exportar** para armazenar a identidade gerada em sua carteira eletrônica do console e, em seguida, salvá-la em seu sistema de arquivos como um arquivo JSON.
- Para resolver esse problema em seu navegador, é necessário importar essas identidades e associá-las ao nó correspondente:
  - No navegador no qual você está tendo o problema, clique na guia **Carteira eletrônica**, seguido por **Incluir identidade** para importar o arquivo JSON em sua carteira eletrônica.
  - Clique em **Fazer upload de JSON** e navegue para o arquivo JSON que você exportou usando o botão **Incluir arquivos**.
  - Clique em **Enviar**.
  - Agora abra o nó de serviço de pedido ou de peer ao qual essa identidade foi associada originalmente e clique no ícone **Configurações**.
  - Clique no botão  ** Associar identidade ** .
  - Selecione a identidade que você acabou de importar para a sua carteira eletrônica do console na lista suspensa.
  - Clique em  ** Associar **.
- Repita esse processo para cada identidade que estava na carteira eletrônica do navegador original.
{: tsResolve}

## Por que estou obtendo o erro `Unable to authenticate with the enroll ID and secret you provided` quando eu crio uma nova definição do MSP da organização?
{: #ibp-v2-troubleshooting-create-msp}
{: troubleshoot}

Ao tentar criar uma nova definição do MSP da organização por meio da guia Organizações, você enfrenta o erro `Unable to authenticate with the enroll ID and secret you provided`.
{: tsSymptoms}

Esse erro ocorre quando o valor que você especificou para o segredo de inscrição não é válido para o ID de inscrição que você selecionou na seção `Gerar certificado de administrador da organização` do painel.
{: tsCauses}

Verifique se você selecionou o ID de inscrição de administrador da organização correto na lista suspensa de ID de inscrição e insira o valor correto para o segredo de inscrição.
{: tsResolve}

## Por que estou obtendo o erro `An error occurred when updating channel` quando tento incluir uma organização em meu canal?
{: #ibp-v2-troubleshooting-update-channel}
{: troubleshoot}

Quando você tenta incluir outra organização em um canal, a atualização falha com a mensagem `An error occurred when updating channel`.
{: tsSymptoms}

Esse erro ocorre quando o **ID do MSP do atualizador de canal** selecionado no painel **Atualizar canal** não é um administrador do canal.
{: tsCauses}

No painel **Atualizar canal**, role para baixo para o **ID do MSP do atualizador de canal** e selecione o ID do MSP que foi especificado quando o canal foi criado ou especifique o ID do MSP que é o administrador do canal.
{: tsResolve}

## Meu cluster Kubernetes do {{site.data.keyword.cloud_notm}} expirou. O que isso significa?
{: #ibp-v2-troubleshooting-cluster-expired}
{: troubleshoot}

Eu recebi um e-mail de que meu cluster de serviço {{site.data.keyword.IBM_notm}} Kubernetes está prestes a expirar ou seu status é `Expired`. Ou, após 30 dias, não será possível acessar o console.
{: tsSymptoms}

Os clusters Kubernetes gratuitos são válidos por apenas 30 dias.
{: tsCauses}

Não é possível migrar de um cluster gratuito para um pago. Após 30 dias, não será possível acessar o console e todos os seus nós e certificados serão excluídos. Consulte este tópico sobre [Expiração do cluster Kubernetes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-cluster-expiration) para obter informações sobre o que está acontecendo e o que pode ser feito.
{: tsResolve}

## Por que as transações que eu envio do VS Code estão falhando?
{: #ibp-v2-troubleshooting-anchor-peer}
{: troubleshoot}

As transações enviadas do VS Code falham com um erro semelhante a:
```
Error submitting transaction: No endorsement plan available for {"chaincodes":[{"name":"hello-world"}]}
```
{: tsSymptoms}

Esse erro ocorrerá se você estiver usando o recurso Fabric Service Discovery, mas não configurou nenhum peer de âncora em seu canal.
{: tsCauses}

Siga a etapa três do [tópico de dados privados](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) no tutorial Implementar um contrato inteligente para configurar seus peers de âncora.

## Por que obtenho um erro 401 Desautorizado quando efetuo login no meu console?
{: #ibp-v2-troubleshooting-console-401}
{: troubleshoot}

Quando tento efetuar login em meu console, não consigo acessar o console por meio do meu navegador. Se eu verifico os logs do navegador, posso localizar o erro 401 Desautorizado.
{: tsSymptoms}

A sessão do console do navegador atinge o tempo limite após **8 horas** de inatividade. Se uma sessão se tornar inativa, o console impedirá que o usuário inativo execute qualquer ação.
{: tsCauses}

Se a sua sessão se tornar inativa, será possível apenas tentar atualizar seu navegador. Se isso não funcionar, feche o navegador, incluindo **todas** as guias e janelas. Abra a URL novamente. Você deverá efetuar login.

Como uma boa prática, é necessário já ter armazenado os seus certificados e identidades no sistema de arquivos. Se você estiver usando uma janela incógnito, todos os certificados serão excluídos do armazenamento local do navegador quando você fechar o navegador. Depois de efetuar login novamente, será necessário reimportar suas identidades e certificados.
{: note}

## Por que os nós que eu implemento no {{site.data.keyword.cloud_notm}} Private não processam transações e estão falhando ao verificar o funcionamento?
{: #ibp-v2-troubleshooting-healthchecks}
{: troubleshoot}

Meu console afirma que meus nós de peers e de pedido ainda estão em execução. No entanto, minhas transações estão falhando. Quando executo uma verificação de vivacidade ou de prontidão nos pods do nó, a verificação afirma que os pods não estão funcionais.
{: tsSymptoms}

Se você tiver implementado, removido e atualizado nós em seu cluster várias vezes, talvez como resultado de teste, o Docker poderá falhar devido a um problema conhecido com o {{site.data.keyword.cloud_notm}} Private. Para obter mais informações, consulte esse problema na [documentação do {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/getting_started/known_issues.html#25626){: external}.
{: tsCauses}

Para resolver esse problema, remova os pods com falha e implemente seus nós novamente. Também é possível reiniciar o serviço do Docker no cluster.

## Por que minhas transações estão retornando um erro de política de endosso: a assinatura configurada não satisfez a política?
{: #ibp-v2-troubleshooting-endorsement-sig-failure}
{: troubleshoot}

Quando eu chamo um contrato inteligente para enviar uma transação, a transação retorna a falha de política de endosso a seguir:
```
erro retornado: erro de VSCC: falha de política de endosso, erro: assinatura configurada não satisfez a política
```
{: tsSymptoms}

Se você se associou recentemente a um canal e instalou o contrato inteligente, esse erro ocorrerá se você não tiver incluído sua organização na política de endosso. Como sua organização não está na lista de organizações que podem endossar uma transação por meio do contrato inteligente, o endosso de seus peers é rejeitado pelo canal. Se você encontrar esse problema, será possível mudar a política de endosso fazendo upgrade do contrato inteligente. Para obter mais informações, consulte [Especificando uma política de endosso](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse) e [Atualizando um contrato inteligente](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade).
{: tsCauses}
