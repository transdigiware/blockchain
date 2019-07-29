---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

keywords: IBM Blockchain Platform, IBM Cloud Private, AWS, Data residency, world state

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# Residência de dados
{: #console-icp-about-data-residency}

Como as redes de blockchain são indiferentes para o tipo de dados que são processados, etapas extras devem ser tomadas algumas vezes para manter determinados tipos de dados seguros. O requisito mais comum sobre residência de dados está associado às leis dentro de certos países, que exigem que todos os dados que são processados e armazenados num sistema de TI devem permanecer dentro das fronteiras de um país específico. Da mesma forma, algumas empresas em indústrias altamente regulamentadas, como governo, assistência médica e serviços financeiros, requerem que os dados sejam armazenados inteiramente atrás de seu firewall.

As redes de blockchain permitem que múltiplas organizações usem um livro-razão distribuído para transacionar e compartilhar dados de uma maneira confiável e segura. No entanto, isso implica que os dados podem ser distribuídos entre os nós da rede e as regiões em que esses nós residem. As organizações podem usar várias opções para segregar dados do restante da rede e alcançar a residência dos dados:
1. [Coletas de dados privados em um canal compartilhado](#console-icp-about-data-residency-fabric)
2. [Coletas de dados privados em um canal separado](#console-icp-about-data-residency-use-case)
3. [Um canal separado com todos os nós no canal dentro de um único país](#console-icp-about-data-residency-use-case-channel)

Cada abordagem fornece um nível aumentado de isolamento e proteção para os seus dados, mas requer esforço adicional para implementar e gerenciar. Para ajudá-lo a entender como cada opção pode ser usada para alcançar a residência dos dados, fornecemos uma visão geral de como os dados são compartilhados dentro de uma rede do Hyperledger Fabric. Em seguida, apresentamos um caso de uso de exemplo para ilustrar como as organizações dentro de um consórcio de blockchain usariam cada opção para segregar seus dados e evitar que eles saiam de sua região.

## Como os dados são compartilhados dentro de uma rede do {{site.data.keyword.blockchainfull_notm}} Platform
{: #console-icp-about-data-residency-fabric}

A arquitetura do Hyperledger Fabric que está subjacente ao {{site.data.keyword.blockchainfull_notm}} Platform é centrada em torno de três componentes principais: um serviço de pedido (composto de nós de pedido), autoridades de certificação (CA) e peers. Além disso, as organizações enviam transações para esses nós por meio de aplicativos clientes usando os [SDKs do Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external}. Ao considerar a residência dos dados, é importante entender como esses componentes interagem com os dados e como os armazenam.

**Peers** são usados por membros do consórcio para armazenar o [reporte](https://hyperledger-fabric.readthedocs.io/en/release-1.4/ledger/ledger.html){: external} de blockchain. O reporte de blockchain consiste em duas partes. A primeira é o estado mundial, que armazena o valor mais recente para todos os dados no livro-razão em pares chave-valor. A segunda é o registro de blockchain de cada transação. Os peers recebem atualizações de estado no formato de novos blocos por meio do serviço de pedido. Então, eles usam os bloqueios e o estado mundial para confirmar as transações, atualizar o estado mundial e incluir o log de transações no blockchain. O serviço de pedido estabelece o pedido de transações para todos os peers no [consórcio](/docs/services/blockchain?topic=blockchain-glossary#glossary-consortium) e armazena uma cópia da parte de blockchain do livro-razão.

**Canais** são um mecanismo para a transmissão de dados dentro de uma rede. Não é possível participar de uma rede de blockchain sem se juntar a um canal. Os canais podem ser usados como membros da rede para criar a separação lógica entre os aplicativos de negócios e até mesmo para impulsionar o desempenho limitando o tráfego. Eles também podem ser usados por subconjuntos de organizações no consórcio para transacionar privadamente e segregar dados.

Os peers mantêm um livro-razão separado para cada canal ao qual se associam. Somente as organizações que são membros do canal podem se associar a seus peers para o canal e receber atualizações de livro-razão do serviço de pedido. Como resultado, cada canal é ligado a um serviço de pedido, que armazena a parte do blockchain de cada livro-razão do canal que ele mantém. Os aplicativos clientes enviam transações para os peers e o serviço de pedido de um determinado canal. Essas transações são incluídas no log de transações dentro do blockchain e incluem um [conjunto de leitura/gravação](https://hyperledger-fabric.readthedocs.io/en/release-1.4/readwrite.html){: external}, que é incluído nos pares chave-valor no estado mundial.

Se a residência dos dados no país for um requisito, será necessário considerar a localização de seus peers, o serviço de pedido e os aplicativos clientes. Também será necessário conhecer a localização dos peers que pertencem a outras organizações em seus canais.  Se você estiver usando o {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, será possível localizar a lista de [Regiões e localizações do {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/reference?topic=blockchain-ibp-regions-locations#ibp-regions-locations) nos quais você e os membros de seu consórcio podem implementar seus componentes.

## Um caso de uso para residência de dados
{: #console-icp-about-data-residency-use-case}

Podemos usar um consórcio de exemplo para ilustrar como os dados são distribuídos no {{site.data.keyword.blockchainfull_notm}} Platform e explorar como os membros poderiam alcançar a residência dos dados. A figura abaixo contém um consórcio com um serviço de pedido e quatro organizações. Cada organização tem um nó peer. Duas organizações, Org A e Org B, e o serviço de pedido estão localizados nos Estados Unidos. As outras duas organizações, Org C e Org D, estão localizadas na Alemanha. Todas as quatro organizações são membros e associaram seus peers ao canal X.

![Um consórcio de exemplo](images/data_res_use_case.svg "Um consórcio de exemplo")

Cada peer unido ao canal X armazena uma cópia do livro-razão do canal, exibido na **Figura 1** como livro-razão X. Como os peers dos Estados Unidos e da Alemanha são associados ao canal, os dados no livro-razão do canal residem nas duas geografias. A parte de blockchain do livro-razão também é armazenada pelo serviço de pedido que está localizado nos Estados Unidos.

Se duas organizações no consórcio criarem um segundo canal, o Canal Y, um segundo livro-razão será criado e armazenado nos peers de membros do canal. Somente as organizações que se associaram ao canal terão uma cópia dos dados do canal.

![Incluindo um segundo canal](images/data_res_use_case_channel.svg "Incluindo um segundo canal")


Na **Figura 2**, Org B e Org D se associaram o canal Y. Os peers da Org B e da Org D agora armazenam uma cópia do livro-razão Y, além do livro-razão X. Como o mesmo serviço de pedido foi usado para criar o canal X e o canal Y, o serviço de pedido agora tem uma cópia da parte de blockchain dos dois livros-razão do canal. Em ambas, a **Figura 1** e a **Figura 2**, os dados que estão sendo criados por aplicativos na Alemanha são armazenados nos Estados Unidos, o que é indesejável se a residência dos dados for necessária.

Podemos usar o exemplo acima para explorar as opções que as organizações têm para alcançar a residência dos dados. Suponha que a regulamentação na Alemanha requeira que alguns dos dados criados pela Org C e pela Org D permaneçam dentro do país. As organizações na Alemanha podem usar todas as três opções para evitar que os dados sejam armazenados nos Estados Unidos.

## Opção um: coletas de dados privados em um canal compartilhado
{: #console-icp-about-data-residency-use-case-private-data}

A Org C e a Org D podem usar o [recurso de dados privados](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#what-is-a-private-data-collection "O que é uma coleta de dados privados?"){: external} do Hyperledger Fabric para evitar que os dados sejam distribuídos para todas as organizações no canal. As coletas de dados privados permitem que as organizações compartilhem dados de estado ponto a ponto (por meio do protocolo Gossip) com outras organizações que estão autorizadas a ler a coleta. Os dados são armazenados em um banco de dados privado separado no peer. O serviço de pedido não está envolvido e não vê os dados privados. Somente um hash dos dados na coleta é incluído no livro-razão do canal e é armazenado nos peers de outros membros do canal e no serviço de pedido. Isso permite que as organizações verifiquem dados privados se quiserem tornar públicos os detalhes da transação. Para saber mais, visite o artigo de conceito de [Dados privados](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html#private-data "Private data"){: external} na documentação do Fabric.

![Dados privados](images/data_res_private_data.svg "legenda três")

Na **Figura 3**, a Org C e a Org D criaram uma coleta de dados privados, a coleta OrgC-OrgD, que permite que as organizações transacionem sem precisar compartilhar dados com a Org A, a Org B ou o serviço de pedido. Os dados de estado do valor da chave dentro dessa coleta são armazenados somente nos peers da Org C e da Org D e não saem da Alemanha. Porém, um hash dos dados dentro da coleção é armazenado no livro-razão X e compartilhado com o canal mais amplo. Isso significa que um hash dos dados na coleta OrgC-OrgD é armazenado nos peers e no serviço de pedido nos Estados Unidos.

Ao considerar dados privados, é importante entender a diferença entre os **dados em hash** e os **dados criptografados**. A criptografia usa uma função de duas maneiras para transformar dados em um formulário que oculta seu valor original, mas pode ser convertido de volta para o estado original. Como um exemplo, quando os dados são enviados por meio de uma rede que é segura usando TLS, os dados são criptografados usando um certificado TLS. Então, eles são enviados pela rede como texto criptográfico e, em seguida, decriptografados pelo destinatário. O texto criptografado contém todos os dados originais e pode ser decriptografado usando uma chave privada. No entanto, o hashing é uma função única que usa dados para criar uma sequência exclusiva de números e letras. Os dados em hash não podem ser convertidos de volta para o formulário original usando o hash. Para verificar os dados que criaram o hash, um destinatário precisa criar um novo hash dos dados originais usando a mesma função de hash e verificar se os valores de hash correspondem. Um terceiro não pode usar o hash sem uma cópia dos dados originais.  

É importante estar ciente de que com essa opção, enquanto as Orgs A e B não podem ver os dados reais do livro-razão porque eles estão em hash, eles ainda são capazes de ver que as Orgs C e D estão transacionando e podem ver o volume de transações que estão ocorrendo entre eles.

Também considere que os dados dentro de uma coleta de dados privados podem ser eliminados dos peers que os armazenam. Enquanto os dados são armazenados em um canal para sempre, as coletas permitem que os membros especifiquem quantos blocos são confirmados em um canal antes que os dados privados [sejam eliminados](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private_data_tutorial.html#pd-purge){: external}. Quando os dados são removidos da coleta de dados privados, o hash no canal não pode mais ser usado para verificar a transação que o criou. Na rede de exemplo na **Figura 3**, a Org C e a Org D podem usar uma política `block to live` para assegurar que qualquer dado que não precise persistir para sempre seja removido da rede inteiramente dentro de um período de tempo especificado.

## Opção dois: coletas de dados privados em um canal separado
{: #console-icp-about-data-residency-use-case-private-data-channel}

A Org C e a Org D também podem usar coletas de dados privados no contexto de um canal separado para fornecer isolamento adicional para os seus dados. Criar um novo canal, canal Y nesse caso, assegura que o hash dos dados privados seja compartilhado apenas com o serviço de pedido, sem ser compartilhado com outros membros do consórcio e armazenado em seus peers.

![Usando dados privados com um canal separado](images/data_res_private_data_channel.svg "Usando dados privados com um canal separado")

Na **Figura 4**, a Org C e a Org D formaram um novo canal, o canal Y, que não contém nenhum membro nos Estados Unidos. Como resultado, os hashes de dados na coleta OrgC-OrgD são armazenados no livro-razão Y em vez do livro-razão X e não são armazenados nos peers nos Estados Unidos. Como o serviço de pedido está localizado nos Estados Unidos, um hash dos dados criados na Alemanha ainda sai do país.

Criar um canal separado pode evitar que os detalhes da transação sejam compartilhados com outras organizações no consórcio. Se as organizações na Alemanha usavam um canal compartilhado, a Org A e a Org B seriam capazes de ver o número de hashes de transação que são confirmados para o livro-razão do canal pela coleta de dados privados. Isso poderia fornecer a essas organizações a visibilidade para o fato de que as Orgs C e D estão transacionando e o volume de transações gerado entre elas. Considere que a formação de um novo canal requer sobrecarga de gerenciamento adicional para criar e atualizar. Formar um novo canal também torna mais difícil para a Org C e a Org D compartilhar dados com a Org A e a Org B.

## Opção três: um canal com todos os componentes em um país
{: #console-icp-about-data-residency-use-case-channel}

A Org C e a Org D também podem criar um canal com toda a infraestrutura dentro do mesmo país. Isso requer que os peers associados ao canal, os aplicativos e o serviço de pedido residam todos dentro da mesma região. Nesse cenário, nenhum dos dados armazenados no livro-razão do canal deixará a região e será armazenado fora do país.

![Criando um canal separado](images/data_res_separate_channel.svg "Criando um canal separado")

Na **Figura 5**, a Org C e a Org D criaram um novo canal para dados que não devem sair da Alemanha. Isso requer a criação de um novo serviço de pedido localizado na Alemanha para assegurar que a cópia do solicitador do livro-razão do canal esteja armazenada dentro do país. Como nesse caso o serviço de pedido, OrgC-peer, e o OrgD-peer estão localizados na Alemanha, Org C e Org D agora podem manter os dados públicos no canal se quiserem ou ainda podem decidir usar as coletas de dados privados para evitar que os dados da transação inteira sejam armazenados no serviço de pedido.

A criação de um canal com todos os componentes em um país assegura que todos os dados residam em uma região, incluindo os pares chave-valor, o log de transações de blockchain e os hashes de qualquer dado privado. No entanto, essa opção requer a sobrecarga para manter um novo canal e os custos associados à manutenção do serviço de pedido.

## Material de referência
{: #console-icp-about-data-residency-reference}

Para um entendimento mais profundo do fluxo de dados na rede do {{site.data.keyword.blockchainfull_notm}} Platform, consulte a [Documentação do Fabric no fluxo de transação](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}.

No futuro, a prova de conhecimento zero melhorará a capacidade de alcançar mais a residência dos dados no Hyperledger Fabric. Um Zero-Knowledge Proof (ZKP) permite que um "provador" assegure a um "verificador" que conhece um segredo sem revelar o próprio segredo. É uma maneira de mostrar que você sabe algo que satisfaz uma instrução sem mostrar o que você sabe.

É possível obter mais informações sobre coletas de dados privados e a prova de conhecimento zero no White Paper sobre [Transações privadas e confidenciais com o Hyperledger Fabric](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}.
