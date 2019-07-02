---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: HA, highly availability, multiregion

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

# Configurando implementações de alta disponibilidade (HA) multiregion
{: #ibp-console-hadr-mr}

A configuração de HA multiregion fornece o mais alto grau de cobertura de HA possível. A implementação de peers em múltiplas regiões geográficas assegura que, se qualquer região se tornar indisponível, os peers em outras regiões poderão continuar a transacionar. Observe que o suporte de HA multiregion para CAs e o serviço de pedido não está atualmente disponível.

## Visão geral
{: #ibp-console-hadr-overview}

Ao configurar o suporte de HA multiregion para peers, você executa as tarefas a seguir:
- Crie múltiplas instâncias de serviço no {{site.data.keyword.cloud}} que estejam ligadas a um cluster Kubernetes em uma região diferente.
- Crie seus nós de blockchain em regiões diferentes.
- Use a funcionalidade de exportação/importação de nó para gerenciar os nós por meio de um único console.

## Etapas de configuração
{: #ibp-console-hadr-config}

Para configurar o HA multiregion ao criar peers redundantes para cada organização, conclua as etapas a seguir ao configurar sua rede de blockchain:

1. Crie três clusters Kubernetes do {{site.data.keyword.cloud_notm}} nas regiões que você preferir. Esses clusters podem estar localizados em qualquer região que você desejar, embora para um alto desempenho eles devam ser relativamente próximos. Por exemplo, as regiões Costa Leste dos EUA, Costa Oeste dos EUA e Canadá são melhores do que as regiões Costa Oeste dos EUA, Londres e Tóquio.
2. Implemente uma nova instância da {{site.data.keyword.blockchainfull_notm}} Platform e vincule-a ao cluster na primeira região. Em seguida, implemente outra instância do {{site.data.keyword.blockchainfull_notm}} Platform e vincule-a ao cluster na segunda região. Repita essas etapas para vincular uma terceira instância de serviço para o cluster na terceira região. Quando você tiver concluído, terá três instâncias separadas do {{site.data.keyword.blockchainfull_notm}} Platform vinculadas a três clusters separados, cada uma delas em uma região diferente, e três consoles separados.

Este tutorial supõe que um serviço de pedido existe com um canal definido ao qual os peers podem se associar.
{: important}

### Etapa um: criar a CA e os metadados da organização do peer no cluster um
{: #ibp-console-hadr-peerCA}

1. Implemente uma CA no primeiro cluster seguindo as instruções no tutorial de construção de rede para [Criando a CA de sua organização do peer](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-CA-org1CA). Anote os valores do **ID de inscrição** e do **segredo** da CA, pois será necessário fornecer esses valores ao importar a CA para outros clusters.
2. Depois que o indicador de status do bloco da CA estiver verde, `Em execução`, siga as instruções para [Registrar identidades com a sua CA](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-use-CA-org1). Nessas instruções, você cria duas identidades, uma para o peer e outra para a identidade do administrador da organização do peer.
3. [Crie a definição do MSP da organização do peer](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peers-org1) para a organização do peer no primeiro cluster.
4. [Crie um peer](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-peer-create) no primeiro cluster.
5. [Instale seu contrato inteligente](/docs/services/blockchain?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install) em seu peer.

Antes de poder instanciar o contrato inteligente no canal, é necessário seguir estas etapas para associar o peer ao canal no serviço de pedido:
- [Exporte a definição de organização do peer](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-add-org2-remote) e compartilhe-a com um administrador de serviço de pedido.
- O administrador do serviço de pedido precisa seguir as etapas para [importar a organização do peer](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-msp) e incluí-la no consórcio. Eles também precisarão concluir as etapas nessas instruções para exportar o serviço de pedido para um arquivo JSON.
- [Importe o arquivo JSON do serviço de pedido](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-import-remote-orderer) para seu console.
- [Associe seu peer ao canal](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2).
- Finalmente, se você usar a descoberta de serviço, os dados privados e o gossip de peer, o administrador do serviço de pedido precisará [configurar peers de âncora](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers) no canal. Para HA, é recomendado que você inclua cada peer redundante como um peer de âncora. Dessa forma, se um dos peers está indisponível, o gossip entre as organizações pode continuar.   

Agora que você associou o peer ao canal, é possível [instanciar o contrato inteligente](/docs/services/blockchain?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2) no canal.

### Etapa dois: exportar os metadados e as identidades do cluster um
{: #ibp-console-hadr-export-meta1}

1. Exporte a definição de CA para um arquivo JSON.
   - Abra a CA na guia **Nós**.
   - Clique no ícone de download para gerar o arquivo JSON da CA por meio de sua sessão do navegador.
2. Exporte a definição do MSP da organização do peer para um arquivo JSON.
   - Navegue para a definição do MSP na guia **Organizações**.
   - Clique no ícone de download no bloco.
3. Exporte a identidade do administrador da organização do peer por meio de sua carteira eletrônica.
   - Navegue para a guia **Carteira eletrônica**.
   - Clique na identidade do administrador da organização do peer e clique em **Exportar identidade**.
   - Um arquivo JSON que contém os certificados de administrador da organização é criado. Anote o nome do arquivo e proteja o arquivo porque ele será necessário quando você criar peers adicionais em seus outros clusters.

### Etapa três: importar os metadados e as identidades para os clusters dois e três
{: #ibp-console-hadr-import-meta23}

1. Nos clusters dois e três, [importe o arquivo JSON da CA](/docs/services/blockchain/howto?topic=blockchain-ibp-console-import-nodes#ibp-console-import-ca) que você exportou do cluster um.  
2. Depois de fazer upload do arquivo JSON, é necessário inserir o ID de inscrição e o segredo do administrador da CA e o ID de inscrição e o segredo do administrador da CA do TLS que você usou quando implementou a CA no cluster um.
2. Importe o arquivo JSON de definição do MSP da organização do peer que você exportou do cluster um.
   - Na guia **Organizações**, clique em **Importar definição do MSP**.
   - Clique em **Incluir arquivo** para fazer upload do arquivo JSON.
   - Clique em **Eu tenho uma identidade de administrador para a definição do MSP** para permitir que você use essa definição do MSP quando criar novos peers em outras zonas.
   - Clique em **Importar definição do MSP**.
3. Importe o arquivo JSON de identidade do administrador da organização que você exportou do cluster um para a carteira eletrônica do console nos clusters dois e três.

### Etapa quatro: criar novos peers no cluster dois e três e se associar a um canal
{: #ibp-console-hadr-create-new-peers}

1. Nos clusters dois e três, use a CA para registrar um novo usuário de peer, seguindo as mesmas etapas que você usou quando registrou a identidade de peer no cluster um. Você só precisa criar os usuários do peer. Não é necessário recriar a identidade do administrador da organização porque você a importará na próxima etapa.
2. Crie novos peers usando a CA que você importou do cluster um como a CA do peer e usando o MSP da organização do peer que você importou do cluster um como a definição do MSP da organização do peer.
3. Nos clusters dois e três, agora é possível repetir as etapas que você executou para associar os peers ao mesmo canal do peer no cluster um. 
4. Depois de instalar o contrato inteligente nesses peers redundantes, o livro-razão será sincronizado automaticamente entre todos os peers.
5. Novamente, se você planejar usar a descoberta de serviço, os dados privados e o gossip de peer, será necessário tornar cada peer redundante um peer de âncora.  

Sua rede está agora configurada de forma que uma falha em qualquer região única não afetará a carga de trabalho do peer.  

Para maximizar sua HA ainda mais, considere as opções adicionais a seguir:
- Exporte os peers que você criou nos clusters dois e três e importe-os para o console no cluster um. Em seguida, tudo pode ser gerenciado por meio de um único cluster.
- No entanto, o cluster um pode falhar. Portanto, para um controle adicional quanto a uma falha de cluster, é possível importar todos os seus peers em cada um dos três consoles. Em seguida, se o cluster que contém qualquer console falhar, tudo ainda poderá ser gerenciado dos consoles nos outros dois clusters.
