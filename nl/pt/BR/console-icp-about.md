---

copyright:
years: 2018, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform, IBM Cloud Private, system requirements, Kubernetes Helm chart, behind a firewall

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

# Sobre o {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #console-icp-about}

O {{site.data.keyword.blockchainfull}} Platform pode ser implementado em nuvens públicas e privadas, como o {{site.data.keyword.cloud_notm}}, seu próprio data center e nuvens públicas de terceiros. Essa implementação multinuvem é possível por meio do {{site.data.keyword.cloud_notm}} Private, a plataforma de orquestração de contêiner baseada no Kubernetes da IBM. Essa liberação fornece uma interface com o usuário do console que pode ser usada para implementar e gerenciar os componentes do blockchain em um cluster do {{site.data.keyword.cloud_notm}} Private. O {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private usa a base de código do Hyperledger Fabric v1.4.1 e suporta a implementação no {{site.data.keyword.cloud_notm}} Private v3.2.
{:shortdesc}

Essa liberação do {{site.data.keyword.blockchainfull_notm}} Platform inclui os recursos-chave a seguir:

**DESENVOLVA ---- Experiência de desenvolvedor integrada**
- **Codifique facilmente** seus contratos inteligentes em Node.js, Golang ou Java, grave aplicativos clientes usando a nova extensão {{site.data.keyword.blockchainfull_notm}} VS Code, aproveite a **integração do SDK** com o console da interface com o usuário e aprenda com nossos detalhados tutoriais e amostras.
- **DevOps simplificado** permite mover de desenvolvimento para testar para produção em um único ambiente, escalando para cima seus recursos do Kubernetes para incluir mais componentes.
- **Integração de serviço do Kubernetes.** Aproveite serviços como Grafana e Prometheus para criação de log e Kibana para monitoramento.
- **Recursos-chave atualizados do Fabric**. Aproveite os recursos mais recentes do Hyperledger Fabric v1.4.1:
  - [Serviço de solicitação de Raft](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Coletas de dados privados](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data) que fornecem maior privacidade de dados, assegurando que os dados do livro-razão sejam compartilhados somente com peers autorizados por meio do protocolo gossip.
  - [Descoberta de serviço](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, permitindo que você descubra e atualize dinamicamente como seu aplicativo interage com sua rede.
  - [Listas de controle de acesso do canal](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} que permitem controle adicional da governança de seus canais e contratos inteligentes.

**OPERE --- Controle total de suas implementações**
- ** Implemente somente os componentes necessários **. Conectar um peer a múltiplos canais e redes ou hospede um serviço de solicitação ao qual os parceiros de negócios podem se conectar.
- ** Manter o controle completo de suas identidades **. Armazene e gerencie as chaves usadas para administrar seus nós em seu próprio ambiente seguro.
- **Operação unificada**. O console do {{site.data.keyword.blockchainfull_notm}} Platform permite implementar e gerenciar todas as suas organizações e nós em **um console** sem ter que depender da {{site.data.keyword.IBM_notm}} ou de outros fornecedores para gerenciar seus solicitadores ou Autoridade de certificação. Também é possível incluir ou remover membros de um consórcio de blockchain, criar e associar canais e instalar e instanciar contratos inteligentes por meio de seu console.
- **Hospedar ou se associar a uma rede**. Implemente peers hospedados em seu cluster para múltiplos canais em múltiplas nuvens ou convide outras organizações para associar seu consórcio ou canais enquanto as organizações gerenciam seus nós de forma independente entre as infraestruturas.
- **Gerencie o acesso** dos usuários que podem administrar ou monitorar seus nós.
- **Acesso direto aos logs** de seus nós por meio do serviço de Kubernetes. Use qualquer serviço de terceiro suportado para extrair e analisar seus logs.
- **Interaja diretamente com seus pods** usando o serviço de Kubernetes.
- **Coleção de assinatura dinâmica** que permite melhor controle sobre a governança colaborativa em configurações de canal.

**CRESÇA --- Escalabilidade e flexibilidade**
- **Escolha seu cálculo**. Você tem a flexibilidade de decidir a quantia de CPU, de memória e de armazenamento que você deseja provisionar em seu cluster Kubernetes. Para obter mais informações, consulte [Como o console interage com seu cluster Kubernetes](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction).
- **Escale** para cima e para baixo os recursos em seu cluster Kubernetes, pagando somente pelo que você precisa. Para obter mais informações, consulte [Precificação](/docs/services/blockchain/howto/pricing.html#ibp-pricing).
- **Recuperação de desastre e alta disponibilidade de multizona.** Essa capacidade duplica sua implementação do Kubernetes entre as zonas, permitindo alta disponibilidade (HA) de seus componentes e recuperação de desastre (DR).
- **Execute em qualquer lugar**. Graças ao **código base unificado** do console do {{site.data.keyword.blockchainfull_notm}} Platform, é possível executar seus componentes em qualquer ambiente suportado pelo {{site.data.keyword.cloud_notm}} Private.

O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private é um produto empacotado para o {{site.data.keyword.cloud_notm}} Private e é entregue como um gráfico do Helm do Kubernetes. Para obter mais informações sobre o {{site.data.keyword.cloud_notm}} Private, consulte a documentação do [{{site.data.keyword.cloud_notm}} Private versão 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## O que o  {{site.data.keyword.blockchainfull_notm}}  Platform for  {{site.data.keyword.cloud_notm}}  Private oferece

O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private permite que você instale o console do {{site.data.keyword.blockchainfull_notm}} Platform em uma implementação do {{site.data.keyword.cloud_notm}} Private. Então, é possível usar o console para criar todos os componentes fundamentais de um blockchain do Hyperledger Fabric, uma autoridade de certificação, um serviço de pedido e peers, em seu cluster local. Para obter mais informações sobre os blocos de construção de redes do Hyperledger Fabric, consulte a [visão geral do componente de blockchain](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview). Também é possível usar seu console para operar uma rede de várias nuvens distribuída, importando os nós implementados em outros clusters do {{site.data.keyword.cloud_notm}} Private ou no {{site.data.keyword.cloud_notm}}.

## É o {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private adequado para você

A execução do {{site.data.keyword.blockchainfull_notm}} Platform fora do {{site.data.keyword.cloud_notm}} fornece mais flexibilidade para aumentar ou associar uma rede de blockchain. Ela ajuda os iniciadores de rede a cultivarem suas redes, permitindo que novos membros se associem usando a plataforma de sua escolha. Isso permitirá que as organizações que estão interessadas em se associar às redes de blockchain coloquem seus peers com seus aplicativos existentes ou se integrem a seus sistemas de registro.

Os usuários dessa oferta gerenciarão sua própria segurança e infraestrutura. O {{site.data.keyword.cloud_notm}} não fornece esses serviços. Revise as [Considerações e limitações](#console-icp-about-considerations) na próxima seção antes de iniciar.

## Considerações e limitações
{: #console-icp-about-considerations}

Antes de iniciar, assegure-se de entender as limitações a seguir:

- Você é responsável pelo gerenciamento do monitoramento de funcionamento, da segurança, da criação de log e do uso de recurso de seus componentes de blockchain.
- O console pode ser usado apenas para criar e controlar componentes com base no Hyperledger Fabric v1.4.1 ou mais recente.
- É possível importar apenas nós que foram exportados de outros consoles do {{site.data.keyword.blockchainfull_notm}} Platform. Para poder operar um peer ou solicitador importado por meio do console, também é necessário importar a definição do MSP da organização do nó associado e a identidade do administrador para o seu console.

Para dar uma olhada nos ambientes suportados pelo {{site.data.keyword.cloud_notm}} Private, consulte [Ambientes suportados](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}.
{:important}

## Pré-requisitos do sistema
{: #console-icp-about-prerequisites}

O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private suporta os sistemas operacionais a seguir:
- Red Hat Enterprise Linux (RHEL) 7.3, 7.4, 7.5
- Ubuntu 18.04 LTS e 16.04 LTS
- SUSE Linux Enterprise Server (SLES) 12 SP3

O gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private foi validado para ser executado nos clusters do {{site.data.keyword.cloud_notm}} Private v3.2 no Ubuntu Linux usando os nós do trabalhador e o armazenamento auxiliar a seguir:

- **LinuxONE e {{site.data.keyword.IBM_notm}} Z**: z/VM e KVM, que estão usando NFS.
- **x86**: Linux de 64 bits que usa GlusterFS.

## Licença
{: #console-icp-about-license}

Consulte [Precificação para o {{site.data.keyword.blockchainfull_notm}} Platform para Multicloud](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) para obter mais informações sobre licenciamento e precificação.

## Instalando o  {{site.data.keyword.blockchainfull_notm}}  Platform for  {{site.data.keyword.cloud_notm}}  Private
{: #console-icp-about-install}

O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private é entregue como um arquivo de gráfico do Helm que pode ser instalado em um cluster local do {{site.data.keyword.cloud_notm}} Private. Depois de instalar o gráfico do Helm, é possível localizar o {{site.data.keyword.blockchainfull_notm}} Platform como um aplicativo no catálogo do {{site.data.keyword.cloud_notm}} Private. Para obter instruções sobre como instalar o gráfico do Helm e os pré-requisitos necessários, consulte [Instalando o {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#helm-console-install).

Se você for um novo usuário do {{site.data.keyword.cloud_notm}} Private e desejar informações e dicas sobre como instalar e implementar o {{site.data.keyword.cloud_notm}} Private, veja [Configurando o {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_console_setup.html#icp-console-setup).

É possível implementar o {{site.data.keyword.blockchainfull_notm}} Platform atrás de um firewall, sem ter acesso à Internet pública. O pacote de gráficos Helm transferidos por download inclui todas as imagens do Docker do componente Fabric que o {{site.data.keyword.blockchainfull_notm}} Platform usa, sem puxá-los do DockerHub durante a implementação.

## Considerações de segurança
{: #console-icp-about-security}

Como esses componentes são implementados em sua própria infraestrutura, você é responsável por gerenciar sua segurança. Isso inclui áreas importantes de segurança, como gerenciamento de chaves e criptografia de dados. Revise os tópicos a seguir ao considerar a segurança de seus componentes.

### Segurança de dados
{: #console-icp-about-security-data}

Os planos Starter e Enterprise do {{site.data.keyword.blockchainfull_notm}} Platform usam criptografia de disco inteiro que é baseada em [criptografia de chave simétrica](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external} para proteger todos os dados que as redes usam. Deve-se executar etapas semelhantes em seu próprio ambiente para proteger seus dados do peer.

Os dados em seu banco de dados de estado, se você usa LevelDB ou CouchDB, não são criptografados. É possível usar a criptografia de nível do aplicativo para proteger os dados em repouso em seu banco de dados de estado.

### Residência de dados
{: #console-icp-about-security-data-residency}

Os requisitos de residência de dados podem exigir que o processamento e armazenamento de todos os dados do livro-razão do blockchain permaneçam dentro da fronteira de um único país (ou dentro de algum outro limite definido). Para obter mais informações sobre como a residência de dados pode ser realizada, consulte [Residência de dados](#console-icp-about-data-residency).

### Gerenciamento de chave
{: #console-icp-about-security-key-management}

O gerenciamento de chave é um aspecto crítico da segurança. Se uma chave privada estiver comprometida ou perdida, agentes hostis poderão acessar seus dados e sua funcionalidade. A {{site.data.keyword.IBM_notm}} usa dispositivos físicos que são chamados de [Módulos de segurança de hardware](/docs/services/blockchain/glossary.html#glossary-hsm) (HSM) para armazenar as chaves privadas das redes do plano Enterprise do {{site.data.keyword.blockchainfull_notm}} Platform.

Quando você implementa um componente no {{site.data.keyword.cloud_notm}} Private, você é responsável por gerenciar suas chaves privadas. Embora o {{site.data.keyword.blockchainfull_notm}} Platform gere chaves privadas, essas chaves não são armazenadas no Platform. É essencial armazenar suas chaves com segurança para que elas não sejam comprometidas. É possível localizar a chave privada de seu componente na pasta de keystore do MSP de seu peer, no diretório `/mnt/crypto/peer/peer/msp/keystore/` dentro de seu componente. Para obter mais informações sobre os certificados dentro de seu peer, consulte a seção [Membership Services Provider](/docs/services/blockchain/certificates.html#managing-certificates-msp) do tutorial [Gerenciando certificados no {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/certificates.html#managing-certificates).

É possível usar o Key Escrow para recuperar chaves privadas perdidas. Isso precisa ser executado antes da perda de qualquer chave. Se uma chave privada não puder ser recuperada, será necessário obter novas chaves privadas registrando uma nova identidade com sua Autoridade de certificação. Também será necessário remover e substituir o seu signCert de qualquer canal ao qual tenha se associado.

### TLS
{: #console-icp-about-security-tls}

A [Segurança da Camada de Transporte](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} (TLS) é integrada ao modelo de confiança do Hyperledger Fabric. Todos os componentes Starter e Enterprise no {{site.data.keyword.blockchainfull_notm}} Platform usam TLS para se autenticar e se comunicar uns com os outros. Portanto, os componentes de rede no Platform precisam ser capazes de concluir um handshake TLS com seus peers. Uma implicação disso é que você precisa ativar o intermediário, usando a lista de aplicativos confiáveis, por exemplo, em seu firewall de aplicativos clientes para seu peer.

### Configuração do Provedor de Serviços de Associação
{: #console-icp-about-security-MSP}

Os componentes do {{site.data.keyword.blockchainfull_notm}} Platform consomem identidades via Membership Service Providers (MSPs). MSPs associam os certificados que as autoridades de certificação emitem com as funções de rede e canal. Para obter mais informações sobre como os MSPs trabalham com o peer, consulte [Membership Services Provides (MSPs)](/docs/services/blockchain/certificates.html#managing-certificates-msp).

### Segurança do aplicativo
{: #console-icp-about-security-appl}

Como todas as chamadas de chaincode são assinadas, o Fabric gerencia a segurança do aplicativo. Além disso, o Fabric também inclui verificações de nível de aplicativo baseadas na ACL.

## Residência de dados
{: #console-icp-about-data-residency}

Como as redes de blockchain são indiferentes para o tipo de dados que são processados, etapas extras devem ser tomadas algumas vezes para manter determinados tipos de dados seguros. O requisito mais comum sobre residência de dados está associado às leis dentro de certos países, que exigem que todos os dados que são processados e armazenados num sistema de TI devem permanecer dentro das fronteiras de um país específico. Da mesma forma, algumas empresas em indústrias altamente regulamentadas, como governo, assistência médica e serviços financeiros, requerem que os dados sejam armazenados inteiramente atrás de seu firewall. Portanto, para alcançar a residência de dados, todos os componentes da rede de blockchain devem fazer parte do mesmo [canal](/docs/services/blockchain/glossary.html#glossary-channel) e residirem na fronteira de um único país.

Para tratar dos requisitos de residência de dados, é importante entender a arquitetura do Hyperledger Fabric subjacente ao {{site.data.keyword.blockchainfull_notm}} Platform. A arquitetura é centrada em torno de três componentes-chave: um serviço de ordenação (composto de solicitadores), autoridades de certificação (CA) e peers. Um peer recebe atualizações de estado pedidas no formato de blocos do serviço de ordenação e mantém o estado e o livro-razão. Portanto, um peer e um serviço de ordenação têm um relacionamento direto. O livro-razão contém os valores mais recentes para todas as chaves e os dados que os logs de transações incluem.

Além disso, os aplicativos clientes usam os [SDKs do Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/getting_started.html){: external} para enviar transações para os peers e o serviço de ordenação. Essas transações incluem dados do [conjunto de leitura/gravação](https://hyperledger-fabric.readthedocs.io/en/release-1.4/readwrite.html){: external}, que contém os pares chave-valor no livro-razão.

Se a residência de dados no país for um requisito, o solicitador, os peers e os aplicativos clientes deverão residir no mesmo país. Quando uma rede do {{site.data.keyword.blockchainfull_notm}} Platform é criada no {{site.data.keyword.cloud_notm}}, você tem a opção de selecionar um local para a rede. <!--For a Starter Plan network, you can select from US South, United Kingdom, and Sydney. For an Enterprise Plan network, you can select from currently available locations, which include Dallas, Frankfurt, London, Sao Paulo, Tokyo, and Toronto. -->Para obter mais informações sobre regiões e locais, veja [Regiões e locais do {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/reference/ibp_regions.html#ibp-regions-locations).

### Um caso de uso para residência de dados

Considere uma rede do {{site.data.keyword.blockchainfull_notm}} Platform que inclua o solicitador e a CA juntamente com um consórcio de quatro organizações. As organizações possuem um ou mais nós de peer e todas as quatro organizações fazem parte de um único canal e todos os componentes da rede residem na região (por exemplo, Frankfurt), em que a rede do {{site.data.keyword.blockchainfull_notm}} Platform foi implementada. Por último, os aplicativos clientes que interagem com os peers também residem na Alemanha.

Nesse exemplo, a residência de dados é mantida.

![Residência de dados quando todos os componentes estão no mesmo país](images/remote_peer_data_res_1.png "Residência de dados quando todos os componentes estão no mesmo país")

Agora, vamos considerar as implicações quando um **peer distribuído** ingressa em uma das organizações. Um peer distribuído pode residir na mesma região que o restante da rede ou em qualquer lugar fora da região da rede do {{site.data.keyword.blockchainfull_notm}} Platform:

-	Se o peer residir no mesmo país que o restante da rede, a residência de dados será mantida. Todos os dados do livro-razão permanecem dentro da Alemanha como na **Figura 1** acima.
-	Se o peer residir em um país diferente (como os EUA, por exemplo), a residência de dados não será mais mantida porque os dados no livro-razão de peer são compartilhados fora da fronteira do país.

Para resolver esse problema, os **canais** podem ser usados para segregar dados para um subconjunto de peers na rede. Quando a rede do {{site.data.keyword.blockchainfull_notm}} Platform contém um peer distribuído e solicitadores além das fronteiras do país, os canais fornecem isolamento de dados do livro-razão das organizações com peers fora da fronteira do país.

**Nota:** os solicitadores estão sempre localizados na região do data center selecionada para hospedar a rede. Não é possível ter múltiplos solicitadores entre as fronteiras do país. Os peers, no entanto, podem estar localizados no data center ou em um local remoto fora do {{site.data.keyword.cloud_notm}}.

![Residência de dados quando os peers estão fora do país da região do {{site.data.keyword.blockchainfull_notm}} Platform](images/remote_peer_data_res_2.png "Residência de dados quando os peers estão fora do país da região do {{site.data.keyword.blockchainfull_notm}} Platform")

Na **Figura 2**, a residência de dados não é requerida para `OrgC` e `OrgD`. Na verdade, o `OrgD` agora inclui dois peers distribuídos, `OrgD-peer1` e `OrgD-peer2`, que residem nos *Estados Unidos*. Portanto, para que `OrgA`, `OrgB` e seus respectivos aplicativos clientes e peers que residem na Alemanha isolem os dados do livro-razão no canal `X`, um novo canal `Y` é criado para `OrgC` e `OrgD`.

Para um entendimento mais profundo do fluxo de dados na rede do {{site.data.keyword.blockchainfull_notm}} Platform, consulte a [Documentação do Fabric sobre o fluxo de transação](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html){: external}.

No futuro, a nova tecnologia no Hyperledger Fabric melhorará a capacidade de alcançar maior residência de dados usando as [coleções de dados privados](https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html){: external} e a prova de conhecimento zero.

- Uma coleta de dados privados assegura que os dados privados sejam compartilhados ponto a ponto (por meio do protocolo gossip) apenas para os peers autorizados a vê-los, por exemplo, peers que estão dentro das fronteiras do país. Os dados são armazenados em um banco de dados privado no peer. O serviço de ordenação não está envolvido aqui e não vê os dados privados. Um hash desses dados é gravado nos livros-razão de cada peer no canal. O hash que é usado para validação de estado serve como evidência da transação e pode ser usado para propósitos de auditoria. Os dados privados estão disponíveis para redes no {{site.data.keyword.blockchainfull_notm}} Platform que estão em execução no Fabric versão 1.2.1 ou mais recente. No entanto, o recurso de dados privados não está disponível para os peers em execução no {{site.data.keyword.cloud_notm}} Private, pois as redes do {{site.data.keyword.cloud_notm}} Private não usam atualmente o gossip.

- Uma prova de conhecimento zero (ZKP) permite que um “comprovador” assegure a um “verificador” que ele conhece um segredo sem revelar o próprio segredo.

É possível obter mais informações sobre essas tecnologias no White Paper sobre [Transações privadas e confidenciais com o Hyperledger Fabric](https://developer.ibm.com/tutorials/cl-blockchain-private-confidential-transactions-hyperledger-fabric-zero-knowledge-proof/){: external}.

## Obtendo Suporte
{: #console-icp-about-support}

Para obter mais informações sobre como obter suporte no {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, bem como recursos grátis de desenvolvedor de blockchain e fóruns de suporte que possam ser usados para solucionar problemas, consulte [Obtendo suporte](/docs/services/blockchain/ibmblockchain_support.html#blockchain-support).

Se você comprou uma licença do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private e deseja entrar em contato com o suporte ao cliente, consulte as informações sobre como [acessar a comunidade de suporte da {{site.data.keyword.IBM_notm}} e abrir um chamado de suporte](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}.
