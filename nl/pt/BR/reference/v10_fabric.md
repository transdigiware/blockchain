---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: Hyperledger Fabric, confidential channels, Membership Service Provider, Linux Foundation, SDKs, modular architecture, permissioned network

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Hyperledger Fabric
{: #hyperledger-fabric}

A rede do {{site.data.keyword.blockchainfull}} é construída na pilha do Hyperledger Fabric, um dos projetos de blockchain dentro do Hyperledger Project da Linux Foundation. Ela é uma rede "com permissão" na qual todos os usuários e componentes têm identidades conhecidas. A lógica de assinatura/verificação é implementada em cada ponto de contato de comunicação e as transações são consentidas por meio de uma série de verificações de endosso e validação. Nesse sentido, isso difere grandemente das implementações de blockchain tradicionais que promovem o anonimato e são forçadas a depender de criptomoedas e obrigações de cálculo intenso para validar transações.
{:shortdesc}

O Hyperledger Fabric oferece uma arquitetura modular para estender a escalabilidade e o desempenho. Este tópico apresenta alguns componentes chave no Hyperledger Fabric. Para obter uma introdução completa do Hyperledger Fabric, consulte a [Documentação do Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}.

## Peers
{: #hyperledger-fabric-peer}

Em um nível físico, uma rede de blockchain é composta principalmente por nós de peer (ou, simplesmente, peers). Peers são os elementos fundamentais da rede porque hospedam livros-razão e contratos inteligentes (que estão contidos em ["chaincode"](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/chaincodenamespace.html){: external}. Mais precisamente, o peer hospeda **instâncias** do livro-razão e **instâncias** de contratos inteligentes. Como os contratos inteligentes e os livros-razão são usados para encapsular os processos compartilhados e as informações compartilhadas em uma rede, respectivamente, esses aspectos de um peer fazem deles um bom ponto de partida para entender o que uma rede do Fabric realmente faz.

Para saber mais sobre os peers especificamente, consulte [este documento focando apenas nos peers](https://hyperledger-fabric.readthedocs.io/en/release-1.4/peers/peers.html){: external} na documentação da comunidade do Fabric.

## Autoridade de certificação
{: #hyperledger-fabric-certificate-authority}

Como uma plataforma para redes de blockchain **com permissão**, o Hyperledger Fabric inclui um componente de **Autoridade de Certificação (CA)** modular para gerenciar as identidades de rede de todas as organizações de membros e seus usuários. O requisito para uma identidade com permissão para cada usuário permite o controle baseado em ACL na atividade de rede e garante que cada transação seja basicamente rastreável para um usuário registrado.
* A CA emite um certificado raiz (**rootCert**) para cada **membro** (organização ou indivíduo) que está autorizado a se associar à rede.
* A autoridade de certificação também emite um certificado de inscrição (**eCert**) para cada componente de membro, aplicativo do lado do servidor e ocasionalmente usuário.
* Cada usuário inscrito também recebe uma alocação de certificados de transação (**tCerts**). Cada **tCert** autoriza uma transação de rede.

Este controle baseado em certificado sobre a associação de rede e ações permite que os membros restrinjam o acesso a canais, aplicativos e dados privados e confidenciais, por identidades do usuário específicas.

Para obter mais informações sobre o componente de autoridade de certificação do Hyperledger Fabric, consulte o [Guia do usuário de CA do Fabric](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/){: external}.

## Membership Service Provider
{: #hyperledger-fabric-membership-service-provider}

O Hyperledger Fabric inclui um componente **Membership Service Provider (MSP)** para oferecer uma abstração de todos os mecanismos de criptografia e protocolos atrás da emissão e validação de certificados, e autenticação do usuário. O MSP é instalado em cada peer de canal para assegurar que as solicitações de transação que são emitidas para o peer se originem de uma identidade do usuário autenticada e autorizada.

Para obter mais informações sobre o componente de provedor de serviços de associação do Hyperledger Fabric, consulte [Associação](https://hyperledger-fabric.readthedocs.io/en/release-1.4/membership/membership.html){: external} na [Documentação do Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}.

## Serviço de solicitação
{: #hyperledger-fabric-ordering-service}

Em outros blockchains distribuídos, como o Ethereum e o Bitcoin, não há autoridade central que peça transações e as envie para os peers. O Hyperledger Fabric, blockchain no qual o {{site.data.keyword.blockchainfull_notm}} Platform se baseia, funciona de forma diferente. Ele apresenta um nó chamado **solicitador**.

Solicitadores são os componentes principais em uma rede porque executam algumas funções essenciais:

- Eles literalmente **pedem** os blocos de transações que são enviados para os peers para serem gravados em seus livros-razão e esse processo é chamado de "pedido". Se essas transações fossem empacotadas e pedidas nos próprios peers, isso aumentaria a possibilidade de um peer gravar uma transação no livro-razão onde outro peer não gravaria, criando uma bifurcação de estado.
- Eles mantêm o **canal do sistema do solicitador**, o local em que o **consórcio**, a lista de organizações peer que tem permissão para criar canais, reside.
- Eles executam verificações de validação de identidade importantes. Por exemplo, se uma organização tentar criar um canal quando ela não for um membro do consórcio do solicitador, a solicitação será negada. Os solicitadores também validam com relação aos comportamentos em canais de transação, como as permissões para mudar uma configuração de canal.

O Hyperledger Fabric suporta atualmente implementações de serviço de pedido INDIVIDUAIS (um nó de pedido) e baseadas em Kafka. Para obter mais informações sobre o serviço de pedido do Hyperledger Fabric, consulte [Trazendo um serviço de pedido baseado em Kafka](https://hyperledger-fabric.readthedocs.io/en/release-1.4/kafka.html){: external} na [Documentação do Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}.

## Os SDKs do Fabric
{: #hyperledger-fabric-fabric-sdks}

Os Hyperledger Fabric SDKs permitem que os desenvolvedores de aplicativos construam aplicativos que interajam com uma rede de blockchain. Esses SDKs ajudam a facilitar os aplicativos a gerenciarem o ciclo de vida de canais e chaincode.

O Hyperledger Fabric fornece ambos, um SDK Node.js e SDK Java, e fornece as funções a seguir para interagir com a rede de blockchain:

* Registre e inscreva usuários
* Crie canais
* Associe peers a um canal
* Atualize a configuração do canal do sistema ou do canal do aplicativo
* Instalar chaincode em peers
* Instanciar chaincode em um canal
* Faça upgrade em um canal
* Chame as funções de chaincode para atualizar o livro-razão
* Consulte o livro-razão para transações, blocos ou chaves específicos
* Monitore eventos em um canal (por exemplo, confirmação bem-sucedida de uma transação)

Para obter mais informações sobre os SDKs do Fabric, consulte [SDKs do Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/fabric-sdks.html){: external} em [ Documentação do Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}.

## Fluxo de transação
{: #hyperledger-fabric-transaction-flow}

Para assegurar a consistência e a integridade de dados, o Hyperledger Fabric implementa múltiplos pontos de verificação em todo o fluxo de transação, incluindo autenticação de cliente, endosso, solicitação e confirmação no livro-razão.

A **Figura 1** mostra o fluxo de transação em uma rede de blockchain do Hyperledger Fabric:
![Fluxo de transação](../images/v10_txflow.svg "Fluxo de transação em uma rede Hyperledger Fabric")

Em uma rede do Hyperledger Fabric, o fluxo de dados para consultas e transações é iniciado por um aplicativo do lado do cliente enviando uma solicitação de transação a um peer em um canal. O fluxo inicial de dados na rede é comum para as consultas e transações:

1. Usando as APIs disponíveis no SDK, um aplicativo cliente assina e envia uma proposta de transação para os peers de endosso apropriados no canal especificado. Esta proposta de transação inicial é uma **solicitação** para endosso.
2. Cada peer no canal verifica a identidade e a autoridade do cliente de envio e (se válidas) executa o chaincode especificado com relação às entradas fornecidas. Com base nos resultados da transação e na política de endosso para o chaincode chamado, cada peer retorna uma resposta SIM ou NÃO assinada para o aplicativo. Cada resposta SIM assinada é um **endosso** da transação.

	Neste ponto no fluxo de transação, o processo diverge para consultas e transações. Se a proposta chamou uma função de consulta no chaincode, o aplicativo retorna os dados ao cliente. Se a proposta chamou uma função no chaincode para atualizar o livro-razão, o aplicativo continua com as etapas a seguir:
3. O aplicativo encaminha a transação, que inclui o conjunto de leitura/gravação e os endossos, para o **serviço de pedido**.
4. Em seguida, a transação é retransmitida para o serviço de pedido. Todos os peers de canal validam cada transação no bloco aplicando a Política de Validação específica do chaincode e executando uma Verificação de Versão de Controle de Simultaneidade.
	* Qualquer transação que falha no processo de validação é marcada como inválida no bloco e o bloco é anexado ao livro-razão do canal.
	* Todas as transações válidas atualizam o banco de dados do estado de acordo com os pares chave/valor modificados.

O **protocolo de disseminação de dados por boatos** transmite continuamente os dados de livro-razão no canal para garantir livros-razão sincronizados entre os peers. Para obter mais informações, consulte [Protocolo de disseminação de dados gossip](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html){: external} na [Documentação do Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}.

Para obter uma introdução passo a passo sobre o fluxo de transação, consulte [Fluxo de transação](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external} na documentação do [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}.
