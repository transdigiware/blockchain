---

copyright:
  years: 2019

lastupdated: "2019-05-31"

keywords: getting started tutorials, create a CA, enroll, register, create an MSP, wallet, create a peer, create ordering service, Raft, join a network, system channel

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

# Associar-se a um tutorial de rede
{: #ibp-console-join-network}

O {{site.data.keyword.blockchainfull}} Platform é uma oferta de blockchain-as-a-service que permite que você desenvolva, implemente e opere aplicativos e redes de blockchain. É possível obter mais informações sobre os componentes de blockchain e como eles trabalham juntos visitando a [Visão geral do componente Blockchain](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview). Este tutorial é a segunda parte na [série de tutoriais de rede de amostra](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network-sample-tutorial) e descreve como criar nós no console do {{site.data.keyword.blockchainfull_notm}} Platform e conectá-los a um consórcio de blockchain hospedado em outro cluster.
{:shortdesc}

No caso de estar usando a versão de avaliação beta do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, é provável que alguns painéis em seu console não correspondam à documentação atual, que é mantida atualizada com a instância de serviço geralmente disponível (GA). Se tiver uma instância de serviço beta e desejar obter os benefícios de todas as funcionalidades mais recentes, será aconselhável neste momento provisionar uma instância de serviço GA seguindo as instruções de [Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks).
{: important}

**Público-alvo:** este tópico é projetado para operadores de rede que são responsáveis por criar, monitorar e gerenciar a rede de blockchain.  

Se ainda não tiver usado o console do {{site.data.keyword.blockchainfull_notm}} Platform para implementar componentes em um cluster Kubernetes usando o serviço de Kubernetes do {{site.data.keyword.cloud_notm}}, consulte [Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks), se estiver usando um cluster do {{site.data.keyword.cloud_notm}} ou [Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain/get-started-console-icp.html#get-started-console-icp), se estiver usando o {{site.data.keyword.cloud_notm}} Private para implementar em um provedor em nuvem diferente do {{site.data.keyword.cloud_notm}}. Observe que o console em si não reside em seu cluster. Ele é uma ferramenta que pode ser usada para implementar componentes em seu cluster.

Independentemente de você implementar componentes em um cluster Kubernetes pago ou grátis, preste muita atenção aos recursos à sua disposição quando optar por implementar nós e criar canais. É sua responsabilidade gerenciar seu cluster Kubernetes e implementar recursos adicionais, se necessário. Embora os componentes sejam implementados com êxito em um cluster grátis do {{site.data.keyword.cloud_notm}}, quanto mais componentes forem incluídos, mais lentamente seus componentes serão executados. Para obter mais informações sobre dimensionamentos de componente e como o console interage com seu cluster do serviço de Kubernetes do {{site.data.keyword.cloud_notm}}, consulte [Alocando recursos](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction). Se estiver usando o {{site.data.keyword.cloud_notm}} Private para implementar em um provedor em nuvem diferente, será necessário consultar a documentação desse provedor para saber como monitorar seus recursos lá.

## Amostra de séries do tutorial de rede
{: #ibp-console-join-network-structure}

Esta série de tutorial de três partes orienta você durante o processo de criação e interconexão de uma rede do Hyperledger Fabric relativamente simples e multinó, usando o console do {{site.data.keyword.blockchainfull_notm}} Platform para implementar uma rede em seu cluster Kubernetes e instalar e instanciar um contrato inteligente. Observe que, ao mesmo tempo que este tutorial mostrará como esse processo funciona com um cluster Kubernetes pago do {{site.data.keyword.cloud_notm}}, o mesmo fluxo básico se aplica aos clusters grátis, embora com algumas limitações (por exemplo, não é possível dimensionar ou redimensionar nós em um cluster grátis).

* O [tutorial Construir uma rede](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) orienta você durante o processo de hospedagem de uma rede criando um serviço de pedido e um peer.
* **Tutorial Associar uma rede** Este tutorial atual orienta você durante o processo de associação de uma rede existente, criando um peer e associando-o a um canal.
* [Implementar um contrato inteligente na rede](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts) fornece informações sobre como gravar um contrato inteligente e implementá-lo em sua rede.

É possível usar as etapas nestes tutoriais para construir uma rede com várias organizações em um cluster para propósitos de desenvolvimento e teste. Use o tutorial **Construir uma rede** se você desejar formar um consórcio de blockchain criando um serviço de pedido e incluindo organizações. Use o tutorial **Associar-se a uma rede** para conectar um peer à rede. Seguir os tutoriais com membros de consórcio diferentes permite criar uma rede de blockchain realmente **distribuída**.

Este tutorial é destinado a mostrar como associar um peer a uma rede **existente**. Ele presume que um serviço de pedido já tenha sido criado em seu cluster ou em outro cluster do {{site.data.keyword.blockchainfull_notm}} Platform. Se você não tiver uma rede existente para se associar, visite o [tutorial Construir uma rede](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) para saber como criar uma. O tutorial **Associar-se a uma rede** o conduz pelas etapas para criar os componentes de blockchain `Org2` a seguir, destacados na caixa azul:
![Associar-se à estrutura de rede](../images/ib2-join-network.svg "Associar-se à estrutura de rede")  

Execute as etapas no tutorial **Associe-se à rede** para criar os componentes a seguir e concluir as ações a seguir:

* **Uma organização de peer** `Org2`  
  Crie a definição de Membership Services Provider (MSP) Org2 que define a organização `Org2`.
* ** Um peer **  ` Peer Org2 `   
  O livro-razão, `Ledger x` na ilustração acima, é mantido pelos peers distribuídos. O peer é implementado com o [Couch DB](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_as_state_database.html){: external} como o banco de dados de estado.
* **Uma Autoridade de Certificação (CA)** `Org2 CA`
  Uma CA é o nó que emite certificados para todos os administradores da organização, bem como para os nós associados a uma organização. Nós criaremos uma CA para a organização de peer `Org2`.
* **Associando-se a um canal**
O tutorial descreve como se associar ao canal que foi criado pelo [tutorial Construir uma rede](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network).

Em todo este tutorial, fornecemos **valores recomendados** para alguns dos campos no console. Isso permite que os nomes e as identidades sejam mais fáceis de reconhecer nas guias e nas listas suspensas. Esses valores não são obrigatórios, mas você os achará úteis, especialmente porque terá que lembrar de determinados valores, especialmente os IDs e segredos de usuários registrados, inseridos em uma etapa anterior. Se você esquecer esses valores, será necessário registrar usuários adicionais para seus administradores e componentes. Fornecemos uma tabela dos valores recomendados após cada tarefa e recomendamos que, se você não usar valores de amostra, registre os valores que usar em algum lugar conforme continuar o tutorial.
{:tip}

## Etapa um: criar uma organização peer e um peer
{: #ibp-console-join-network-create-ca-org2}

Para cada organização que você deseja criar com o console, é necessário implementar pelo menos uma CA. Uma CA é o nó que emite certificados para todos os participantes da rede (peers, serviços de solicitação, clientes, administradores e assim por diante). Esses certificados, que incluem um certificado de assinatura e uma chave privada, permitem que os participantes da rede se comuniquem, se autentiquem e, por fim, transacionem. Essas CAs criarão todas as identidades e certificados pertencentes à sua organização, além de definir a própria organização. É possível, então, usar essas identidades para implementar nós, criar identidades de administrador e enviar transações. Para obter mais informações sobre sua CA e as identidades que você precisará criar, consulte [Gerenciando identidades](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities).

Neste tutorial, criaremos uma organização. Portanto, precisaremos criar **uma CA**.

### Criando sua CA da organização do peer
{: #ibp-console-join-network-create-CA-org2CA}

Como parte deste tutorial, sua CA emite os certificados e as chaves privadas para seus usuários e nós. Essas identidades não são gerenciadas pela {{site.data.keyword.IBM_notm}} e as chaves não são armazenadas no console. Eles são armazenados somente no armazenamento local de seu navegador. Portanto, certifique-se de exportar suas identidades e o MSP de sua organização. Se você tentar acessar o console em uma máquina ou em um navegador diferente, será necessário importar essas identidades e definições de organização.
{:important}

Execute as etapas a seguir em seu console:  

1. Navegue para a guia **Nós** à esquerda e clique em **Incluir Autoridade de certificação**. Os painéis laterais permitirão que você customize a CA que deseja criar e a organização para a qual ela emitirá chaves.
2. Neste tutorial, estamos criando nós, portanto, certifique-se de que a opção para **Criar** uma Autoridade de certificação esteja selecionada. Em seguida, clique em **Avançar**.
3. Use o segundo painel lateral para fornecer à sua CA um **nome de exibição**. Nosso valor recomendado para essa CA é `Org2 CA`.
4. No próximo painel, forneça suas credenciais de administrador de CA especificando um **ID de registro do administrador de CA** de `admin` e um segredo de `adminpw`. Novamente, esses são **valores recomendados**.
5. Se estiver usando um cluster pago, você terá a oportunidade de configurar a alocação de recurso para o nó. Para os propósitos deste tutorial, aceite todos os padrões e clique em **Avançar**. Se desejar aprender mais sobre como alocar recursos no {{site.data.keyword.cloud_notm}} para seu nó, consulte esse tópico em [Alocando recursos](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-allocate-resources). Se estiver usando um cluster grátis, você verá a página **Resumo**.
6. Revise a página Resumo e, em seguida, clique em **Incluir autoridade de certificação**.

**Tarefa: criando a CA da organização de peer**

  | **Campo** | **Nome de exibição** | **ID de inscrição** | **Segredo** |
  | ------------------------- |-----------|-----------|-----------|
  | **Criar CA** | CA do Org2  | admin | adminpw |

*Figura 2. Criando a CA da organização de peer*  

Depois de implementar a CA, você a usará quando criar o MSP de sua organização, registrar usuários e criar seu ponto de entrada para uma rede, o **peer**.

Os usuários avançados podem já ter sua própria CA e não desejar criar uma nova no console. Se a sua CA existente puder emitir certificados no formato `X.509`, será possível usar certificados de sua própria CA de terceiro em vez de criar novos certificados aqui. Consulte este tópico em [Usando uma CA de terceiro com seu peer ou serviço de pedido](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-identities) para obter mais informações.

### Usando sua CA para registrar identidades
{: #ibp-console-join-network-use-CA-org2}

Cada nó ou aplicativo que você deseja criar precisa de certificados e chaves privadas para participar da rede de blockchain. Também é necessário criar identidades de administrador para esses nós e aplicativos para que seja possível gerenciá-los por meio do console. Nós criaremos uma CA e a usaremos para criar duas identidades:

* **Um administrador da organização**: essa identidade permite operar nós usando o console da plataforma.
* **Uma identidade peer**: essa é a identidade do próprio peer. Sempre que um peer executar uma ação (por exemplo, endossando uma transação), ele assinará usando seu certificado.

Dependendo de seu tipo de cluster, a implementação da CA poderá levar até dez minutos. Quando a CA for implementada pela primeira vez (ou quando estiver de outra forma indisponível), a caixa no quadro da CA ficará cinza. Quando a CA tiver sido implementada com êxito e estiver em execução, essa caixa ficará verde, indicando que está "Em execução" e pode ser usada para registrar identidades. Antes de continuar com as etapas abaixo para registrar identidades, deve-se aguardar até que o status da CA seja "Em execução".
{:important}

Depois que a CA estiver em execução, conforme indicado pela caixa verde no quadro, gere esses certificados concluindo as etapas a seguir:

1. Clique em `Org2 CA` e assegure-se de que a identidade `admin` criada para a CA esteja visível na tabela. Em seguida, clique no botão **Registrar usuário**.
2. Primeiro, registraremos o administrador da organização, que podemos fazer fornecendo um **ID de inscrição** de `org2admin` e um **segredo** de `org2adminpw`. Em seguida, configure o `Type` dessa identidade como `client` (as identidades do administrador devem sempre ser registradas como `client`, enquanto as identidades do nó devem sempre ser registradas usando o tipo `peer`). O campo de afiliação é para usuários avançados e não é uma parte do tutorial, portanto, clique na caixa que indica **Usar afiliação raiz**. Se desejar saber mais como as afiliações são usadas pela CA do Fabric, consulte este tópico em [Registrando uma nova identidade](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html#registering-a-new-identity){: external}. Por enquanto, selecione qualquer afiliação da lista (por exemplo, `Org1`). Além disso, ignore o campo **Máximo de inscrições**. Se desejar saber mais sobre registros, consulte [Registrando identidades](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-register). Clique em **Avançar**.
4. Para o propósito deste tutorial, não precisamos usar **Incluir atributo**. Se desejar saber mais sobre atributos de identidade, consulte [Registrando identidades](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-register).
5. Depois que o administrador da organização tiver sido registrado, repita esse mesmo processo para a identidade do peer (usando também o `Org2 CA`). Para obter a identidade de peer, forneça um ID de inscrição de `peer2` e um segredo de `peer2pw`. Essa é uma identidade de nó, portanto, selecione `peer` como o **Tipo**. Clique na caixa que indica **Usar afiliação raiz** e ignore **Máximo de inscrições**. Em seguida, no próximo painel, não designe quaisquer **Atributos**, como antes.

Registrar essas identidades com a CA é apenas a primeira etapa na **criação** de uma identidade. Você não conseguirá usar essas identidades até que tenham sido **registradas**. Para a identidade `org2admin`, isso ocorrerá durante a criação do MSP, que será visto na próxima etapa. No caso do peer, isso acontece durante a criação do peer.
{:note}

**Tarefa: registrar usuários**

  |  **Campo** | **Descrição** | **ID de inscrição** | **Segredo** |
  | ------------------------- |-----------|-----------|-----------|-----------|
  | **Registrar usuários** |  Administrador do Org2 | org2admin | org2adminpw |
  | | Identidade do peer |  peer2 | peer2pw |

*Figura 3. Usando sua CA para registrar usuários*  

### Criando a organização peer-to-peer MSP
{: #ibp-console-join-network-create-peers-org2}

Agora que criamos a CA do peer e a usamos para **registrar** nossas identidades de organização, precisamos criar uma definição formal da organização de peer, que é conhecida como definição de Membership Services Provider (MSP). Muitos peers podem pertencer a uma organização. **Você não precisa criar uma nova organização toda vez que cria um peer.** Como essa é a primeira vez que passamos pelo tutorial, criaremos o ID do MSP para essa organização. Durante o processo de criação do MSP, nós vamos gerar certificados para a identidade `org2admin` e incluí-los em nossa Carteira eletrônica.

1. Navegue para a guia **Organizações** na navegação esquerda e clique em **Criar definição do MSP**.
2. Forneça ao seu MSP o nome de exibição `Org2 MSP` e um ID do MSP de `org2msp`. Se desejar especificar seu próprio ID do MSP nesse campo, certifique-se de seguir as especificações sobre as limitações para esse nome na dica de ferramenta.
3. Em **Detalhes da Autoridade de certificação raiz**, especifique a CA usada para registrar as identidades na etapa anterior. Se essa for sua primeira vez neste tutorial, você deverá ver somente um: `Org2 CA`.
4. Os campos **ID de registro** e **Segredo de registro** abaixo disso serão preenchidos automaticamente com o ID e o segredo de registro do primeiro usuário criado com sua CA: `admin` e `adminpw`. No entanto, o uso dessa identidade faria com que sua organização se tornasse a mesma identidade que sua identidade de CA, o que, por motivos de segurança, não é recomendado. Em vez disso, selecione o ID de inscrição que você criou para seu administrador da organização na lista suspensa, `org2admin`, e insira seu segredo associado, `org2adminpw`. Em seguida, forneça a essa identidade um nome de exibição, `Org2 Admin`.
5. Clique no botão **Gerar** para registrar essa identidade como o administrador de sua organização e exporte a identidade para a Carteira eletrônica, na qual será usada ao criar o peer e os canais.
6. Clique em **Exportar** para exportar os certificados de administrador para o sistema de arquivos. Como dissemos acima, essa identidade não é armazenada no cluster ou gerenciada pelo {{site.data.keyword.IBM_notm}}. É armazenado apenas no armazenamento do navegador local. Se mudar os navegadores, será necessário importar essa identidade para sua Carteira eletrônica para poder administrar o peer.
7. Clique em **Criar definição do MSP**.

**Tarefa: criar a definição do MSP da organização de peer**

  |  | **Nome de exibição** | **ID do MSP** | **ID de inscrição**  | **Segredo** |
  | ------------------------- |-----------|-----------|-----------|-----------|
  | **Criar organização** | Org2 MSP | org2msp |||
  | **CA raiz** | CA do Org2 ||||
  | **Certificado do administrador da organização** | |  | org2admin | org2adminpw |
  | **Identidade** | Administrador da Org2 |||||

  *Figura 4. Criar a definição do MSP da organização de peer*  

Depois de ter criado o MSP, você deverá ser capaz de ver o administrador da organização peer em sua **Carteira eletrônica**, a qual pode ser acessada clicando na **Carteira eletrônica** na navegação esquerda.

**Tarefa: verificar sua carteira eletrônica**

  | **Campo** |  **Nome de exibição** | **Descrição** |
  | ------------------------- |-----------|----------|
  | **Identidade** | Administrador da Org2 | Identidade do Org2 |

  *Figura 5. Verificar sua Carteira eletrônica*  

Para obter mais informações sobre MSPs, consulte [Gerenciando organizações](/docs/services/blockchain/howto/ibp-console-organizations.html#ibp-console-organizations).

A exportação de sua identidade do administrador da organização é importante porque você é responsável por gerenciar e proteger esses certificados.
{:important}

### Criando um peer
{: #ibp-console-join-network-peer-create}

Depois de [criar uma CA](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network-create-CA-org2CA), usá-la para registrar identidades e criar o [MSP da organização de peer](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network-create-peers-org2), você estará pronto para criar um peer.

#### Qual função os peers executam?
{: #ibp-console-join-network-peer-role}

É importante lembrar-se de que as próprias organizações não mantêm livros-razão. Os peers executam. As organizações também usam peers para assinar propostas de transação e aprovar atualizações de configuração do canal. Como ter pelo menos dois peers em um canal os torna altamente disponíveis, ter pelo menos dois peers associados a um canal é considerado uma melhor prática para implementações de nível de produção. Neste tutorial, mostraremos apenas o processo para criar um único peer.

De uma perspectiva de alocação de recurso, é possível associar os mesmos peers a múltiplos canais. O design do peer assegura que os dados de um canal não possam passar para outro por meio do peer. No entanto, como o peer armazenará um livro-razão separado para cada canal, será necessário assegurar-se de que o peer tenha energia de processamento e armazenamento suficientes para manipular a transação e o carregamento de dados.

#### Implementando seu peer
{: #ibp-console-join-network-deploy-peer-role}

Use seu console para executar as etapas a seguir:

1. Na página **Nós**, clique em **Incluir peer**.
2. Certifique-se de que a opção para **Criar** um peer esteja selecionada. Em seguida, clique em **Avançar**.
3. Forneça a seu peer um **Nome de exibição** de `Peer Org2`. Para o propósito deste tutorial, não escolha usar uma CA externa para seu peer e, no caso de desejar obter mais informações, consulte [Usando certificados de uma CA externa](#ibp-console-build-network-third-party-ca). Clique em **Avançar**.
4. Na próxima tela, selecione `Org2 CA`, pois essa é a CA que você usou para registrar a identidade de peer. Selecione o **ID de inscrição** para a identidade de peer que você criou para seu peer na lista suspensa, `peer2`, e insira seu **segredo** associado, `peer2pw`. Em seguida, selecione `Org2 MSP` na lista suspensa e clique em **Avançar**.
5. O próximo painel lateral solicita informações de CA do TLS. Quando você criou a CA, um TLSCA foi criado junto a ela. Essa CA é usada para criar certificados para a camada de comunicação segura para os nós. Portanto, selecione o **ID de inscrição** para a identidade de peer que você criou para seu peer na lista suspensa, `peer2`, e insira o **segredo** associado, `peer2pw`. O **Nome do host da CSR do TLS** é uma opção disponível para usuários avançados que desejam especificar um nome de domínio customizado que pode ser usado para direcionar o terminal de peer. Os nomes de domínio customizado não fazem parte deste tutorial, portanto, deixe o **Nome do host do CSR do TLS** em branco por enquanto.
6. O próximo painel lateral solicita que você **Associe uma identidade** para torná-la o administrador de seu peer. Para o propósito deste tutorial, torne seu administrador da organização, `Org2 Admin`, o administrador de seu peer também. É possível registrar e inscrever uma identidade diferente com o `Org2 CA` e tornar essa identidade o administrador de seu peer, mas este tutorial usa a identidade `Org2 Admin`.
7. Se estiver usando um cluster pago, no próximo painel, você terá a oportunidade de configurar a alocação de recurso para o nó. Para os propósitos desse tutorial, é possível aceitar todos os padrões e clicar em **Avançar**. Se desejar aprender mais sobre como alocar recursos no {{site.data.keyword.cloud_notm}} para seu nó, consulte esse tópico em [Alocando recursos](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-allocate-resources). Se estiver usando um cluster {{site.data.keyword.cloud_notm}} grátis, você verá a página **Resumo**.
8. Revise o resumo e clique em **Incluir peer**.

**Tarefa: implementando um peer**

  |  | **Nome de exibição** | **ID do MSP** | **ID de inscrição** | **Segredo** |
  | ------------------------- |-----------|-----------|-----------|-----------|
  | **Criar peer** | Peer Org2 | org2msp |||
  | **CA** | CA do Org2 ||||
  | **Identidade do peer** | |  | peer2 | peer2pw |
  | **Certificado de administrador** | org2msp ||||
  | **CA TLS** | CA do Org2 ||||
  | **ID de CA TLS** | || peer2 | peer2pw |
  | **Associar identidade** | Administrador da Org2 |||||

  *Figura 6. Implementando um Período*  

Em um cenário de produção, recomenda-se implementar três peers em cada canal. Isso é para permitir que um peer fique inativo (por exemplo, durante um ciclo de manutenção) e ainda mantenha peers altamente disponíveis. Para implementar mais de um peer para uma organização, use a mesma CA usada para registrar sua primeira identidade peer. Neste tutorial, isso seria `Org2 CA`. Em seguida, registre uma nova identidade peer usando um ID e um segredo de registro distintos. Por exemplo, `org2secondpeer` e `org2secondpeerpw`. Em seguida, ao criar o peer, forneça esse ID e segredo de registro. Como esse peer ainda está associado ao Org2, escolha `Org2 CA`, `Org2 MSP` e `Org2 Admin` nas listas suspensas. Você pode escolher fornecer a esse novo peer um administrador diferente, que pode ser registrado e inscrito com `Org2 CA`, mas isso é opcional. Esta série de tutoriais mostrará apenas o processo para criar um único peer para cada organização peer.
{:tip}

## Etapa dois: associar-se ao consórcio hospedado pelo serviço de pedido
{: #ibp-console-join-network-add-org2}

Conforme observamos anteriormente, uma organização peer deve ser conhecida do serviço de solicitação antes que possa criar ou se associar a um canal (isso também é conhecido como associação ao "consórcio", a lista de organizações conhecidas do serviço de solicitação). Isso é porque os canais são, em um nível técnico, **caminhos de sistema de mensagens** entre peers por meio do serviço de solicitação. Assim como um peer pode ser associado a vários canais sem que as informações passem de um canal para outro, um serviço de solicitação também pode ter vários canais em execução por meio dele sem expor dados às organizações em outros canais.

Como somente os administradores de serviço de pedido podem incluir organizações de peer no consórcio, é necessário:

* **Ser** o administrador do serviço de pedido. Nesse caso, é possível incluir a organização de peer que você criou para o consórcio diretamente.
* **Enviar** informações do MSP para o administrador do serviço de pedido, que incluirão a sua organização em seu consórcio.

Na próxima etapa, mostraremos como incluir sua organização de peer em um consórcio hospedado por um serviço de pedido em outra instância de serviço do {{site.data.keyword.blockchainfull_notm}} Platform. O tutorial supõe que você tenha criado o serviço de pedido e que possa seguir cada passo. Se o serviço de pedido foi criado por outra pessoa, assegure-se de que o administrador do serviço de pedido siga as etapas marcadas com a caixa de dica azul na parte superior.

Se você seguiu o tutorial Construir uma rede ou se o seu console já inclui o serviço de pedido, será possível ir adiante para [Incluir a organização de peer no serviço de pedido](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network-add-org2-local). É possível, então, continuar com a [Etapa três: incluir a organização de peer em um canal existente](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network-add-channel).

### Exportar as informações de sua organização
{: #ibp-console-join-network-add-org2-remote}

É necessário enviar sua definição do MSP da organização para o administrador do serviço de pedido e ser incluído no consórcio usando as etapas abaixo. Para seguir estas etapas, é necessário ser o administrador da **organização peer**, o que significa que você tem a identidade do administrador da organização peer em sua Carteira eletrônica:

1. Navegue para a guia **Organizações**. É possível ver as organizações disponíveis para exportação serem listadas em **Organizações disponíveis**. Clique no botão **download** dentro do tile da organização para fazer download do arquivo de configuração JSON que representa o MSP da organização de peer.
2. Envie esse arquivo para o administrador do serviço de pedido em uma operação fora da banda.

### Importar a definição de organização
{: #ibp-console-join-network-import-remote-msp}

Esta etapa precisa ser concluída por um administrador de serviço de solicitação.
{:tip}

O administrador do serviço de pedido precisa importar esse arquivo JSON para incluir sua organização em seu console:

- Navegue para a guia **Organizações**, clique no botão **Importar definição do MSP** e selecione o arquivo JSON que representa a definição do MSP da organização de peer. É possível deixar a caixa de seleção `I have an administrator identity for the MSP definition` desmarcada, pois a identidade do administrador não é necessária aqui.

### Inclua a organização de peer no serviço de pedido
{: #ibp-console-join-network-add-org2-local}

Esta etapa precisa ser concluída por um administrador de serviço de solicitação.
{:tip}

Use estas etapas para incluir a organização de peer no consórcio hospedado pelo serviço de pedido. Somente os administradores de serviço de pedido podem incluir organizações de peer no consórcio. Deve-se ter a identidade do administrador da organização de serviço de pedido em sua carteira eletrônica para executar esta tarefa.

1. Navegue para a guia **Nós**.
2. Role para baixo até o serviço de pedido que você deseja usar e clique nele para abri-lo.
3. Em **Membros do consórcio**, clique em **Incluir organização**.
4. Na lista suspensa, selecione `Org2 MSP`, pois esse é o MSP que representa `Org2`.
5. Clique em  ** Incluir organização **.

Se você seguiu o tutorial **Construir uma rede** ou se o seu console já inclui o serviço de pedido e um canal, é possível ignorar a [Etapa três: Incluir a organização de peer em um canal existente](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network-add-channel).

### Exportar o serviço de pedido
{: #ibp-console-join-network-export-ordering-service}

Essa etapa precisa ser concluída pelo administrador do serviço de pedido.
{:tip}

Conclua as etapas a seguir para **Exportar** o serviço de pedido antes que ele possa ser importado pela organização de peer:

1. Navegue para o serviço de pedido dentro da guia **Nós**. Clique na seta **Download** abaixo do nome do serviço de pedido para fazer download de um arquivo de configuração JSON.
2. Envie esse arquivo para a organização de peer em uma operação fora da banda. O administrador da organização de peer pode, então, usar o arquivo de configuração para incluir o serviço de pedido no console.

### Importar o serviço de pedido de outro cluster
{: #ibp-console-join-network-import-remote-orderer}

Conclua as etapas a seguir para **importar** o serviço de pedido em seu console:

1. Navegue para a página **Nós** e clique em **Incluir serviço de pedido**.
2. Clique na opção para **Importar um serviço de pedido existente**.
3. Selecione o **Local do serviço** no qual o serviço de pedido foi implementado e clique no botão **Incluir arquivo** para selecionar o JSON que representa o serviço de pedido.
4. Quando você for solicitado a associar uma identidade, selecione a identidade de organização do peer. Neste tutorial, isso seria `Org2 Admin`. Clique em **Incluir serviço de pedido**.

Quando esse processo estiver concluído, `Org2` será capaz de criar um canal hospedado no `Ordering Service`. Para criar um novo canal, em vez de se associar a um canal existente, continue com [Criando um canal](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network-create-channel).

## Etapa três: incluir a organização de peer em um canal existente
{: #ibp-console-join-network-add-channel}

Essa etapa precisa ser concluída por um administrador do Org1.
{:tip}

Agora, o administrador do solicitador pode incluir a organização de peer no canal:
1. Navegue para a guia **Canais** e clique em `channel1`.
2. Clique no ícone **Configurações** para atualizar o canal e incluir a organização de peer no canal.
3. Na seção **Organizações**, abra a lista suspensa `Selecionar um membro do canal` e selecione o MSP da organização de peer, `Org2 MSP`.
4. Clique em **Incluir** e, em seguida, designe permissões a essa organização. Recomendamos que você os torne um `Operator` para que eles possam atualizar o canal.
5. Na lista suspensa **MSP do atualizador de canal** (sob o título **Organização atualizadora de canal**), assegure-se de que `Org1 MSP` esteja selecionado.
6. Na lista suspensa **Identidade**, assegure-se de que `Org1 Admin` esteja selecionado.
7. Quando você estiver pronto, clique em **Enviar proposta**.

Depois que o administrador do serviço de pedido tiver associado a sua organização de peer ao canal, será possível associar-se aos seus canais de peers que são hospedados pelo serviço de pedido.

## Etapa quatro: associar seu peer ao canal
{: #ibp-console-join-network-join-peer-org2}

Estamos quase acabando. Seu peer pode agora ser associado a um canal existente. É necessário obter o `channel name`, fora da banda, de uma organização que é um membro do canal. No tutorial **Construir uma rede**, criamos um canal chamado `channel1`. Se você ainda não estiver lá, navegue para a guia **Canais** na navegação esquerda.

Execute as etapas a seguir em seu console:

1. Clique no botão **Associar-se ao canal** para ativar os painéis laterais.
2. Selecione seu serviço de pedido denominado `Ordering Service` e clique em **Avançar**.
3. Insira o nome do canal ao qual você deseja se associar, `channel1` e clique em **Avançar**.
4. Selecione quais peers você deseja associar ao canal. Para propósitos deste tutorial, clique em `Peer Org2`.
5. Clique em  ** Associar canal **.

Se você planeja aproveitar os recursos [Dados privados](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html){: external} ou [Descoberta de serviço](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external} do Hyperledger Fabric, deve-se configurar peers de âncora em suas organizações na guia **Canais**. Para obter mais informações sobre como configurar peers de âncora para dados privados usando a guia **Canais** no console, consulte [Dados privados](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data).

## Criando um canal
{: #ibp-console-join-network-create-channel}

Como agora a sua organização é um membro do consórcio, você também tem a habilidade de criar um novo canal. Primeiro, será necessário importar a definição do MSP de outros membros do canal. Use as etapas abaixo para criar um canal com dois membros: o `Org1` que foi criado no [tutorial Construir uma rede](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) e o `Org2` que foi criado nas etapas acima.

### Exportar as informações de sua organização
{: #ibp-console-join-network-add-org2-export-info}

Essa etapa precisa ser concluída por um administrador do Org1.
{:tip}

`Org1` precisa enviar a definição do MSP da organização antes que a inclusão no canal possa ser feita. É necessário ser um membro do consórcio hospedado pelo serviço de pedido antes de poder ser incluído no canal. Para seguir estas etapas, é necessário ser o administrador da **organização peer**, o que significa que você tem a identidade do administrador da organização peer em sua Carteira eletrônica:

1. Navegue para a guia **Organizações**. É possível ver as organizações disponíveis para exportação serem listadas em **Organizações disponíveis**. Clique no botão **download** dentro do tile da organização para fazer download do arquivo de configuração JSON que representa o MSP da organização de peer.
2. Envie esse arquivo para o membro do consórcio que irá criar o canal em uma operação fora da banda.

### Importar a definição de organização
{: #ibp-console-join-network-create-channel-import-msp}

Em seguida, é necessário importar a definição do MSP de `Org1` em seu console:

1. Navegue para a guia **Organizações**, clique no botão **Importar definição do MSP** e selecione o arquivo JSON que representa a definição do MSP da organização de peer.

#### Criando o canal
{: #ibp-console-join-network-channels-create}

Execute as etapas a seguir em seu console:

1. Clique em  ** Criar canal **. Um painel lateral será aberto.
2. Dê ao canal um **nome**, `channel2`. Anote esse valor, pois você precisará compartilhá-lo com qualquer pessoa que desejar associar esse canal.
3. Selecione `Ordering Service` na lista suspensa.
4. Escolha as **Organizações** que serão uma parte desse canal. Como temos duas organizações, selecione e inclua `Org1 MSP (org1msp)` e, em seguida, `Org2 MSP (org2msp)`. Torne ambas as organizações pelo menos **Operador**. Nota: não use o `Ordering Service MSP` aqui.
5. Escolha uma **Política de atualização de canal** para o canal. Essa é a política que ditará quantas organizações terão que aprovar atualizações para a configuração do canal. É possível selecionar `1 out of 2`. À medida que você incluir organizações no canal, será necessário mudar essa política para refletir as necessidades de seu caso de uso. Um padrão sensato consiste em usar a maioria das organizações. Por exemplo, `3 of 5`.
6. Especifique quaisquer limitações de **Controle de acesso** que você deseja fazer. Nota: essa é uma **opção avançada**. Se você configurar o acesso a um recurso para uma organização específica, isso restringirá o acesso a esse recurso para cada organização. Por exemplo, se o acesso padrão a um recurso específico for o `Leitores` de todas as organizações e esse acesso for mudado para o `Admin` de `Org2`, então **somente** o administrador do Org2 terá acesso a esse recurso. Como o acesso a determinados recursos é fundamental para a operação suave de um canal, é altamente recomendável tomar decisões de controle de acesso com cuidado. Se decidir limitar o acesso a um recurso, certifique-se de que o acesso a esse recurso seja incluído, conforme necessário, para cada organização.
7. Selecione a **Organização do criador de canal**. Como você está operando o tutorial como `Org2`e possui os certificados `Org2 Admin` em sua carteira eletrônica do console, selecione `Org2 MSP` na lista suspensa. Da mesma forma, escolha `Org2 Admin` como a identidade que está criando o canal.

Quando estiver pronto, clique em **Criar canal**. Você é levado de volta para a guia Canais e é possível ver um ladrilho pendente do canal recém-criado.

** Tarefa: criar um canal **

  |  **Campo** | **Nome** |
  | ------------------------- |-----------|
  | **Nome de canal** | channel2 |
  | **Serviço de solicitação** | Serviço de Solicitação |
  | **Organizações** | Org2 MSP |
  | **Política de atualização de canal** | 1 de 2 |
  | **Lista de controle de acesso** | Nenhum |
  | **Organização do criador de canal** | Org2 MSP |
  | **Identidade** | Administrador da Org2|

*Figura 7. Criar um canal*

## Próximas etapas
{: #ibp-console-join-network-next-steps}

Depois de ter associado seu peer a um canal, use as etapas a seguir para implementar um contrato inteligente e iniciar o envio de transações para o blockchain:

- [Implemente um contrato inteligente em sua rede](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts) usando o console.
- Depois de ter instalado e instanciado seu contrato inteligente, será possível [enviar transações usando o aplicativo cliente](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-connect-to-SDK).
- Use [a amostra de papel comercial](/docs/services/blockchain/howto/ibp-console-create-app.html#ibp-console-app-commercial-paper) para implementar um contrato inteligente de exemplo e enviar transações do código do aplicativo de amostra.
