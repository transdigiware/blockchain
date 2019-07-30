---

copyright:
  years: 2019
lastupdated: "2019-07-10"

keywords: smart contract, private data, private data collection, anchor peer

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

# Tutorial de implementação de um contrato inteligente na rede
{: #ibp-console-smart-contracts}

Um contrato inteligente é o código, às vezes referido como chaincode, que permite ler e atualizar dados no livro-razão de blockchain. Um contrato inteligente pode transformar a lógica de negócios em um programa executável acordado e verificado por todos os membros de uma rede de blockchain. Este tutorial é a terceira parte na [série de tutorial de rede de amostra](#ibp-console-smart-contracts-structure) e descreve como implementar contratos inteligentes para iniciar transações na rede de blockchain.
{:shortdesc}

No caso de estar usando a versão de avaliação beta do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, é provável que alguns painéis em seu console não correspondam à documentação atual, que é mantida atualizada com a instância de serviço geralmente disponível (GA). Para obter os benefícios de todas as funcionalidades mais recentes, é aconselhável neste momento provisionar uma nova instância de serviço GA seguindo as instruções de [Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).
{: important}

**Público-alvo:** este tópico é projetado para operadores de rede que são responsáveis por criar, monitorar e gerenciar a rede de blockchain. Além disso, os desenvolvedores de aplicativos podem estar interessados nas seções que mencionam como criar um contrato inteligente.

## Amostra de séries do tutorial de rede
{: #ibp-console-smart-contracts-structure}

Esta série de tutorial de três partes orienta você durante o processo de criação e interconexão de uma rede do Hyperledger Fabric relativamente simples e multinó, usando o console do {{site.data.keyword.blockchainfull_notm}} Platform para implementar uma rede em seu cluster Kubernetes e instalar e instanciar um contrato inteligente.

* [Construir um tutorial de rede](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network) fornece orientação durante o processo de hospedagem de uma rede ao criar um solicitador e um peer.
* [Associar-se a um tutorial de rede](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network) fornece orientação durante o processo de junção de uma rede existente ao criar um peer e associá-lo a um canal.
* **Implementar um contrato inteligente na rede** Esse tutorial atual fornece informações sobre como gravar um contrato inteligente e implementá-lo em sua rede.

É possível usar as etapas nestes tutoriais para construir uma rede com várias organizações em um cluster para propósitos de desenvolvimento e teste. Use o tutorial **Construir uma rede** se você desejar formar um consórcio de blockchain criando um nó do solicitador e incluindo organizações. Use o tutorial **Associar-se a uma rede** para conectar um peer à rede. Seguir os tutoriais com membros de consórcio diferentes permite criar uma rede de blockchain realmente **distribuída**.  

Este tutorial final é destinado a mostrar como criar e empacotar um contrato inteligente, como instalar o contrato inteligente em um peer e como instanciar o contrato inteligente em um canal.  

Os contratos inteligentes são instalados em peers e, em seguida, instanciados em canais. **Todos os membros que desejam enviar transações ou ler dados usando um contrato inteligente precisam instalar o contrato em seu peer.** Um contrato inteligente é definido por seu nome e versão. Portanto, tanto o nome quanto a versão do contrato instalado precisam ser consistentes em todos os peers no canal que planejam executar o contrato inteligente.

Depois que um contrato inteligente é instalado nos peers, um único membro de rede instancia o contrato no canal. O membro de rede precisa ter se associado ao canal para executar essa ação. A instanciação atualiza o livro-razão com os dados iniciais usados pelo contrato inteligente e, em seguida, inicia os contêineres de contrato inteligente nos peers associados ao canal que têm o contrato instalado. Os peers poderão então usar os contêineres em execução para transacionar.  
- **Somente um membro de rede precisa instanciar um contrato inteligente.**
- **Se um peer com um contrato inteligente instalado se associar a um canal no qual a mesma versão de contrato inteligente já tiver sido instanciada, o contêiner de contrato inteligente será iniciado automaticamente.**  

Neste tutorial, vamos percorrer as etapas para usar o console do {{site.data.keyword.blockchainfull_notm}} Platform para gerenciar a implementação de contratos inteligentes em sua rede:

- [ Instale contratos inteligentes em seus peers ](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-install).
- [Instancie-os em canais](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-instantiate).
- [ Especificar políticas de aprovação ](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse).
- [Faça upgrade das políticas e do código de contrato inteligente](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade).


## Antes de iniciar

Antes de poder instalar um contrato inteligente, assegure-se de que você tenha as coisas a seguir prontas.

- Deve-se [construir uma rede ](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network) ou [associar-se a uma rede](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network) usando o console do {{site.data.keyword.blockchainfull_notm}} Platform.
- Como os contratos inteligentes são instalados em peers, assegure-se de que você [inclua peers](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-peer-org1) em seu console.  
- Contratos inteligentes são instanciados em um canal. Portanto, deve-se [criar um canal](/docs/services/blockchain/howto?topic=blockchain-ibp-console-build-network#ibp-console-build-network-create-channel) ou [associar-se a um canal](/docs/services/blockchain/howto?topic=blockchain-ibp-console-join-network#ibp-console-join-network-join-peer-org2) usando seu console.

## Etapa um: Gravar um contrato inteligente

O console do {{site.data.keyword.blockchainfull_notm}} gerencia a *implementação* de contratos inteligentes em vez de desenvolvimento. Se você estiver interessado em desenvolver contratos inteligentes, será possível começar a usar os tutoriais fornecidos pela comunidade do Hyperledger Fabric e o conjunto de ferramentas fornecido pela {{site.data.keyword.IBM_notm}}.

- Para saber como os contratos inteligentes podem ser usados para realizar transações entre múltiplas partes, consulte o [tópico Desenvolvendo aplicativos](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/developing_applications.html){: external} na Documentação do Hyperledger Fabric.
- Quando você estiver pronto para começar a construir contratos inteligentes, será possível usar a [extensão Visual Studio Code do {{site.data.keyword.blockchainfull_notm}}](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} para iniciar a construção de seu próprio projeto de contrato inteligente. Também é possível usar essa extensão para [se conectar diretamente à sua rede por meio do código do Visual Studio](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-vscode) e explorar os tutoriais sequenciais.
- Para obter um tutorial rápido sobre o desenvolvimento de contratos inteligentes, consulte [Desenvolver um contrato inteligente com a extensão do VS Code do IBM Blockchain Platform](https://developer.ibm.com/tutorials/ibm-blockchain-platform-vscode-smart-contract/){: external}.
- Para obter um tutorial de ponta a ponta mais detalhado sobre como usar um aplicativo para interagir com contratos inteligentes, consulte [Tutorial de papel comercial do Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/tutorial/commercial_paper.html){: external}.
- Para aprender sobre como incorporar os mecanismos de controle de acesso em seu contrato inteligente, consulte [Chaincode para desenvolvedores](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode4ade.html#chaincode-access-control){: external}.
- Quando você estiver pronto para instalar, o contrato inteligente deverá ser empacotado no [formato .cds](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode4noah.html#packaging){: external} para que ele possa ser instalado nos peers. Para obter mais informações, veja [Empacotando contratos inteligentes](/docs/services/blockchain?topic=blockchain-develop-vscode#packaging-a-smart-contract). Como alternativa, é possível usar os [comandos da CLI de peer](https://hyperledger-fabric.readthedocs.io/en/release-1.4/commands/peerchaincode.html#peer-chaincode-package){: external} para construir o pacote.
<!-- Update the tutorial link to release1-4 when it is published -->


## Etapa dois: Instalar um contrato inteligente
{: #ibp-console-smart-contracts-install}

Use seu console para executar estas etapas:

1. Clique na guia **Contratos inteligentes** para instalar um ou mais contratos inteligentes.
2. Clique em **Instalar contrato inteligente** para fazer upload do arquivo de pacote de contrato inteligente no [formato .cds](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode4noah.html#packaging){: external}. _O arquivo do pacote de contrato inteligente deve ter menos de 4 MB de tamanho._ É possível usar a extensão do {{site.data.keyword.blockchainfull_notm}} Visual Studio Code para [criar um pacote de contrato inteligente](/docs/services/blockchain?topic=blockchain-develop-vscode#packaging-a-smart-contract). Ao instalar o pacote por meio da guia **Contratos inteligentes**, é possível selecionar um ou mais nós de peer nos quais instalar os contratos inteligentes.

Se somente um peer existir no console, o contrato inteligente será instalado nele. Você não é solicitado a selecionar um peer no qual instalar o contrato inteligente. É possível navegar para a guia de nós e clicar em um peer que seja gerenciado por seu console para visualizar a lista de contratos inteligentes que foram instalados em um peer individual.

É possível retornar à guia **Contratos inteligentes** mais tarde para instalar o contrato inteligente em peers adicionais, mesmo após o contrato inteligente ter sido instanciado no canal. Se a versão do contrato inteligente que você instalar for a mesma que a versão instanciada, esses peers adicionais poderão processar transações exatamente como os peers existentes.
{:tip}

**Esse console não pode ser usado para instalar os arquivos `.bna` do Hyperledger Composer.**

<!-- Instead, `.bna` files must be installed on a peer by using the [`Composer` CLI commands]("peer chaincode" "peer chaincode").  -->

## Etapa três: Instanciar um contrato inteligente
{: #ibp-console-smart-contracts-instantiate}

Contratos inteligentes são instanciados em um canal. Qualquer membro do console com peers associados a um canal pode instanciar um contrato inteligente e especificar a [política de endosso](/docs/services/blockchain?topic=blockchain-glossary#glossary-endorsement-policy) associada.

Use seu console para executar estas etapas:

1. Na guia de contratos inteligentes, localize o contrato inteligente na lista instalada em seus peers e clique em **Instanciar** no menu overflow no lado direito da linha.
2. No painel lateral que se abre, selecione um canal no qual instanciar o contrato inteligente. É possível selecionar o canal, denominado `channel1`, que você criou. Em seguida, clique em  ** Avançar **.
3. Especifique a [política de endosso para o contrato inteligente](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-endorse), descrito na seção a seguir. Quando diversas organizações são membros do canal, você tem a oportunidade de escolher quantas organizações forem necessárias para aprovar as transações do contrato inteligente.
4. Também é necessário selecionar os membros da organização a serem incluídos na política de aprovação. Se estiver seguindo adiante no tutorial, isso será `org1msp` e possivelmente `org2msp` se você tiver concluído os tutoriais **Construir uma rede** e **Associar-se a uma rede**.
5. No painel Selecionar peer, selecione um peer na lista suspensa que seja de uma organização membro do canal.
6. Se seu contrato inteligente inclui coletas de dados privados do Fabric, é necessário fazer upload do arquivo JSON da configuração de coleta associado, caso contrário, você pode ignorar esta etapa. Consulte este tópico para obter mais informações sobre usar [dados privados](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data).
7. No último painel, você é solicitado a especificar o nome da função de inicialização do contrato inteligente, juntamente com os argumentos associados para passar para essa função.

É possível visualizar todos os contratos inteligentes que foram instanciados em um canal, clicando no ícone de canal na navegação à esquerda, selecionando um canal na tabela e, em seguida, clicando na guia **Detalhes do canal**.

Esteja ciente de que se você usar um cluster de serviço grátis do {{site.data.keyword.cloud_notm}} Kubernetes, a instanciação poderá levar significativamente mais tempo do que em um cluster pago. Dependendo do número de peers que você implementou em seu cluster, isso pode levar vários minutos.

A combinação de **instalação e instanciação** é um recurso poderoso porque permite que um peer use um único contrato inteligente em vários canais. Os peers podem desejar se associar a múltiplos canais que usam o mesmo contrato inteligente, mas com conjuntos diferentes de membros de rede capazes de acessar os dados. Um peer poderá instalar o contrato inteligente uma vez e, em seguida, usar o mesmo contêiner de contrato inteligente em qualquer canal no qual ele tenha sido instanciado. Essa abordagem leve economiza espaço de cálculo e armazenamento, além de ajudar a escalar sua rede.

## Etapa quatro: enviar transações usando seus aplicativos clientes
{: #ibp-console-smart-contracts-connect-to-SDK}

Depois que um contrato inteligente tiver sido instanciado em um canal, será possível usar um aplicativo cliente para transacionar com outros membros de sua rede. Os aplicativos podem chamar a lógica de negócios contida em contratos inteligentes para criar, transferir ou atualizar ativos no livro-razão do blockchain.

### Conectar com SDK
{: #ibp-console-smart-contracts-connect-to-SDK-panel}
A guia **Contratos inteligentes** contém as informações necessárias para se conectar a um contrato inteligente instanciado por meio de um aplicativo do cliente. Próximo a cada contrato inteligente instanciado, navegue para o menu overflow. Clique no botão **Conectar a seu SDK**. Isso abre um painel lateral que fornece as informações necessárias para se conectar a esse contrato inteligente: o nome do contrato, o nome do canal e seu perfil de conexão. Para obter mais informações, consulte  [ Criando aplicativos ](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app).

## Especificando uma política de endosso
{: #ibp-console-smart-contracts-endorse}

Cada contrato inteligente deve ter uma política de endosso, que é especificada durante a instanciação. A política de endosso especifica o conjunto de organizações, os peers, em um canal que pode executar o contrato inteligente e validar independentemente a saída de transação. Por exemplo, uma política de endosso pode especificar que uma transação será incluída no livro-razão somente se a maioria dos membros no canal endossar a transação. A organização que instancia o contrato inteligente pode selecionar entre os membros que instalaram o contrato inteligente para se tornarem validadores e configura a política de endosso para todos os membros do canal. É possível atualizar sua política de endosso seguindo as etapas para [atualizar seu contrato inteligente](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-upgrade).

Quando você segue as etapas para [instanciar um contrato inteligente](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-instantiate), é possível usar o painel lateral para configurar a política de endosso de um contrato depois de selecionar o canal. Use esse painel para especificar uma política simples, selecionando os peers que precisam endossar a transação na lista de peers que instalaram o contrato inteligente no canal. É possível usar esse método para especificar uma política de endosso de todos os membros do canal, a maioria deles, um único membro ou um simples +1 evitando que os membros assinem automaticamente. A política de endosso padrão é configurada como `1 of x`, o que significa que somente um único membro é necessário para endossar uma transação de contrato inteligente.

Clique no botão **Avançado** se você desejar especificar uma política no formato JSON. É possível usar esse método para especificar políticas de endosso mais complicadas, como requerer que um determinado membro do canal tenha que validar uma transação, junto com a maioria de outros membros. É possível localizar [exemplos adicionais de políticas de endosso avançado](https://hyperledger-fabric.readthedocs.io/en/release-1.4/arch-deep-dive.html#example-endorsement-policies){: external} na Documentação do Hyperledger Fabric. Para obter mais informações sobre como gravar políticas de endosso em JSON, consulte [Documentação do Hyperledger Fabric Node SDK](https://fabric-sdk-node.github.io/global.html#ChaincodeInstantiateUpgradeRequest){: external}.

As políticas de endosso não são atualizadas automaticamente quando novas organizações se associam ao canal e instalam um chaincode. Por exemplo, se a política de endosso precisar de duas de cinco organizações para endossar uma transação, a política não será atualizada para requerer duas de seis organizações quando uma nova organização se associar ao canal. Em vez disso, a nova organização não será listada na política e eles não serão capazes de endossar transações. É possível incluir outra organização em uma política de endosso fazendo upgrade do chaincode relevante e atualizando a política.

## Fazendo upgrade de um contrato inteligente
{: #ibp-console-smart-contracts-upgrade}

É possível fazer upgrade de um contrato inteligente para mudar seu código, política de aprovação ou coleta de dados privados enquanto mantém seu relacionamento com os ativos no livro-razão. Há uma variedade de razões pelas quais você pode desejar fazer upgrade de um contrato inteligente instanciado.
1. É possível fazer upgrade do contrato inteligente para incluir ou remover a funcionalidade de seu código e iterar sobre a lógica de sua rede de negócios.
2. Sempre que um novo membro é incluído em um canal, a política de endosso dos contratos inteligentes instanciados *deve* ser atualizada para incluir o novo membro do canal. Para trabalhar com o novo membro do canal, o contrato inteligente deve ser reempacotado com um novo número de versão e instanciado no canal, mesmo se o contrato inteligente em si não tiver mudanças. Caso contrário, o endosso de transação não poderá ser bem-sucedido.
3. Quando uma coleta de dados privados é mudada, por exemplo, uma organização é incluída ou removida, é necessário fazer upgrade de seu contrato inteligente. Ou essa ação deverá ser usada sempre que uma nova coleta de dados privados for incluída no arquivo JSON de configuração de coleta.
4. Os argumentos de inicialização de contrato inteligente foram mudados.

{:important}
Antes de fazer upgrade de um contrato inteligente instanciado, a nova versão do contrato inteligente deve ser instalada em todos os peers no canal que estão executando o nível anterior do contrato inteligente.

### Como fazer upgrade de um contrato inteligente
{: #ibp-console-smart-contracts-upgrade-howto}

Para fazer upgrade de um contrato inteligente, instale o código atualizado especificando o mesmo nome de contrato inteligente, mas usando e um novo número de versão. Se você tiver instalado uma versão mais recente de um contrato inteligente em qualquer peer no canal, observe que a versão original agora tem o botão `Upgrade Available` próximo a ela na tabela **Contratos inteligentes instanciados**.

{:important}
Quando um novo membro que executará o contrato inteligente se associa ao canal, é obrigatório atualizar o contrato inteligente instalando uma nova versão em todos os peers e instanciando-a no canal com uma política de endosso modificada que inclui o novo membro.

- Navegue para a guia **Contratos Inteligentes** à esquerda.
- Role para baixo até a tabela **Contratos inteligentes instanciados**.
- Na tabela **Contratos inteligentes instanciados**, localize a versão e o canal do contrato inteligente que você deseja fazer upgrade. Eles devem ter o rótulo **Upgrade disponível** próximo a eles.
- Clique no **menu overflow** no lado direito da linha de contrato inteligente e clique em **Upgrade** para executar as etapas a seguir:  

 1. Selecione a versão do contrato inteligente que você deseja fazer upgrade no canal na lista suspensa.
 2. Atualize a política de endosso, incluindo ou removendo membros do canal. Também é possível clicar em **Avançado** para colar em uma nova sequência formatada JSON, que modifica a política existente.
 3. No painel Selecionar peer, é necessário selecionar um peer que possa aprovar a proposta para fazer upgrade do contrato inteligente. Portanto, é necessário selecionar um peer na lista suspensa que seja de uma organização que foi um membro do canal antes de o contrato inteligente ter sido instanciado pela última vez no canal.
 4. Se desejar associar um arquivo de configuração de coleta de dados privados ao contrato inteligente, será possível fazer upload do arquivo JSON. Ou, se desejar atualizar uma configuração de coleta existente, será possível fazer upload do arquivo JSON.   
 Se o contrato inteligente foi instanciado anteriormente com um arquivo de configuração de coleta, **deve-se** fazer upload novamente da versão anterior ou de uma nova versão do arquivo de configuração de coleta durante essa etapa.  
 5. (Opcional) Modifique os valores de argumento de inicialização de contrato inteligente se os parâmetros tiverem sido mudados. Se você não tiver certeza sobre isso, verifique com seu desenvolvedor de contrato inteligente. Se eles não tiverem mudado, esse campo poderá ser deixado em branco.

Depois de fazer upgrade do contrato inteligente, você mudará a versão do contrato que é instanciada no canal e mudará o contêiner de contrato inteligente para todos os peers que instalaram a nova versão. Se estiver usando coletas de dados privados, certifique-se de ter configurado peers de âncora no canal.

### Considerações sobre quando você faz upgrade de contratos inteligentes
{: #ibp-console-smart-contracts-upgrade-considerations}

1. Eu preciso instalar a nova versão do contrato inteligente em todos os meus peers?  

  Quando um contrato inteligente é chamado em um peer, ele tenta executar a versão que está instanciada no canal. Se uma versão anterior estiver em execução no peer, isso resultará em um erro. Portanto, antes de fazer upgrade de um contrato inteligente em um canal, *a melhor prática é instalar a versão mais recente do contrato inteligente em todos os peers que estão executando a versão anterior.*  

2. Uma versão de contrato inteligente subsequente ainda pode processar dados no livro-razão de uma versão anterior do contrato inteligente?  

  Sim. Contanto que o novo código de contrato inteligente trate os dados de uma maneira compatível (incluindo novos campos JSON e não mudando a semântica ou o tipo de campos existentes), qualquer versão subsequente do contrato inteligente poderá ser executada com relação aos dados de uma versão anterior.

3. O que acontecerá com meus peers se eu esquecer de fazer upgrade deles para a versão mais recente antes de atualizar o contrato inteligente no canal?  

  Depois de atualizar o canal para usar a nova versão do contrato inteligente, se ainda houver peers no canal executando a versão anterior, eles não estarão mais aptos a endossar transações para o contrato inteligente. Além disso, você se arrisca a não ter endossos suficientes para que as transações sejam confirmadas no livro-razão, dependendo de como a política de endosso de contrato inteligente está definida. No entanto, é possível instalar a nova versão do contrato inteligente nesses peers mais tarde e eles serão novamente capazes de endossar transações, atualizando-se efetivamente.

4. O que acontece quando eu removo uma organização da minha coleta de dados privados?

   Os peers nessa organização continuarão a armazenar dados na coleta de dados privados até que seu livro-razão atinja o bloco que remove sua associação da coleção. Após isso ocorrer, os peers não receberão dados privados em nenhuma transação futura e os _clientes_ dessa organização não poderão mais consultar os dados privados por meio de chaincode de nenhum peer.

## Dados privados
{: #ibp-console-smart-contracts-private-data}

Os dados privados são um recurso de redes do Hyperledger Fabric na versão 1.2 ou superior e são usados para manter informações confidenciais privadas de outros membros de organização **em um canal**. A privacidade de dados é obtida por meio do uso de [coleções de dados privados](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#what-is-a-private-data-collection "What is a private data collection?"){: external}. Por exemplo, vários atacadistas e um conjunto de agricultores podem ser associados a um único canal. Se um agricultor e um atacadista desejam transacionar privadamente, eles podem criar um canal para esse propósito. Mas eles também podem decidir criar uma coleta de dados privados no contrato inteligente que governa suas interações de negócios para manter a privacidade sobre aspectos sensíveis da venda, como o preço, sem precisar criar um canal secundário. Para saber mais sobre quando usar dados privados dentro de um blockchain, visite o artigo de conceito de [Dados privados](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#private-data "Private data"){: external} na documentação do Fabric.

Para usar dados privados com o {{site.data.keyword.blockchainfull_notm}} Platform, as três condições a seguir devem ser atendidas:  
1. **Defina a coleta de dados privados.** Um arquivo de coleta de dados privados pode ser incluído em seu contrato inteligente. Em seguida, no tempo de execução, seu aplicativo cliente pode usar as APIs de chaincode específicas de dados privados para inserir e recuperar dados da coleta. Para obter mais informações sobre como usar coleções de dados privados com o seu contrato inteligente, consulte o tutorial do Fabric SDK em [Usando dados privados](https://fabric-sdk-node.github.io/tutorial-private-data.html){: external} na documentação do Fabric SDK.  

2. **Instale e instancie o contrato inteligente.** Depois que a coleta de dados privados do contrato inteligente tiver sido definida, será necessário instalar o contrato inteligente nos peers que forem membros do canal. Quando você instanciar o contrato inteligente no canal usando o console, será necessário fazer upload do arquivo JSON de configuração da coleta. Para obter mais informações sobre como [criar um arquivo JSON de definição de coleção](https://fabric-sdk-node.github.io/tutorial-private-data.html){: external}, consulte o tópico da documentação do Fabric SDK.

  Em vez de usar o console para instalar e instanciar seu contrato inteligente com um arquivo de configuração de coleta, também será possível usar o SDK do Fabric. Essas instruções também estão disponíveis em [Como usar dados privados](https://fabric-sdk-node.github.io/release-1.4/tutorial-private-data.html){: external} na documentação do Node SDK.  

  **Nota:** um cliente precisa ser um administrador de seu peer para instalar ou instanciar um contrato inteligente usando o SDK. Portanto, é necessário fazer download dos certificados da identidade de administrador do peer por meio de sua carteira eletrônica do console e passar o certificado de assinatura e a chave privada do administrador do peer diretamente para o SDK em vez de criar uma identidade do aplicativo. Para obter um exemplo de como passar um par de chaves para o SDK, veja [Conectando-se à sua rede usando APIs do SDK do Fabric de nível inferior](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-low-level).  


3. **Configure peers de âncora.** Como o [gossip](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html){: external} organizacional cruzado deve ser ativado para que os dados privados funcionem, um peer âncora deve existir para cada organização na coleta. Consulte essas informações de [como configurar peers de âncora](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-channels-anchor-peers) em sua rede.

Seu canal agora está configurado para usar dados privados.
