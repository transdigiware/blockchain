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

O {{site.data.keyword.blockchainfull}} Platform pode ser implementado em nuvens públicas e privadas, como o {{site.data.keyword.cloud_notm}}, seu próprio data center e nuvens públicas de terceiros. Essa implementação multinuvem é possível por meio do {{site.data.keyword.cloud_notm}} Private, a plataforma de orquestração de contêiner baseada no Kubernetes da IBM. Essa liberação fornece uma interface com o usuário do console de blockchain que pode ser usada para implementar e gerenciar componentes de blockchain em um cluster do {{site.data.keyword.cloud_notm}} Private. O {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}} Private usa o código base do Hyperledger Fabric v1.4.1 e suporta a implementação no {{site.data.keyword.cloud_notm}} Private v3.2.0.
{:shortdesc}

## O que o  {{site.data.keyword.blockchainfull_notm}}  Platform for  {{site.data.keyword.cloud_notm}}  Private oferece
{: #console-icp-about-offers}

É possível usar essa oferta para instalar o console do {{site.data.keyword.blockchainfull_notm}} Platform em uma implementação do {{site.data.keyword.cloud_notm}} Private. Então, é possível usar o console para criar todos os componentes fundamentais de um blockchain do Hyperledger Fabric, uma autoridade de certificação, um serviço de pedido e peers, em seu cluster local. Para obter mais informações sobre os blocos de construção de redes do Hyperledger Fabric, consulte a [visão geral do componente de blockchain](/docs/services/blockchain?topic=blockchain-blockchain-component-overview#blockchain-component-overview). Também é possível usar seu console para operar uma rede de várias nuvens distribuída, importando os nós implementados em outros clusters do {{site.data.keyword.cloud_notm}} Private ou no {{site.data.keyword.cloud_notm}}.

Essa liberação do {{site.data.keyword.blockchainfull_notm}} Platform inclui os recursos-chave a seguir:

**DESENVOLVA ---- Experiência de desenvolvedor integrada**
- **Codifique facilmente** seus contratos inteligentes em Node.js, Golang ou Java, grave aplicativos clientes usando a nova extensão {{site.data.keyword.blockchainfull_notm}} VS Code, aproveite a **integração do SDK** com o console da interface com o usuário e aprenda com nossos detalhados tutoriais e amostras.
- **DevOps simplificado** permite mover de desenvolvimento para testar para produção em um único ambiente, escalando para cima seus recursos do Kubernetes para incluir mais componentes.
- **Integração de serviço do Kubernetes.** Aproveite serviços como Grafana e Prometheus para criação de log e Kibana para monitoramento.
- **Recursos-chave atualizados do Fabric**. Aproveite os recursos mais recentes do Hyperledger Fabric v1.4.1:
  - [Serviço de pedido do Raft](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - [Coletas de dados privados](/docs/services/blockchain/howto?topic=blockchain-ibp-console-smart-contracts#ibp-console-smart-contracts-private-data) que fornecem maior privacidade de dados, assegurando que os dados do livro-razão sejam compartilhados somente com peers autorizados por meio do protocolo gossip.
  - [Descoberta de serviço](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, permitindo que você descubra e atualize dinamicamente como seu aplicativo interage com sua rede.
  - [Listas de controle de acesso do canal](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} que permitem controle adicional da governança de seus canais e contratos inteligentes.

**OPERE --- Controle total de suas implementações**
- ** Implemente somente os componentes necessários **. Conectar um peer a múltiplos canais e redes ou hospede um serviço de pedido ao qual os parceiros de negócios podem se conectar.
- ** Manter o controle completo de suas identidades **. Armazene e gerencie as chaves usadas para administrar seus nós em seu próprio ambiente seguro.
- **Operação unificada**. O console do {{site.data.keyword.blockchainfull_notm}} Platform permite que você implemente e gerencie todas as suas organizações e nós em **um console** sem ter que contar com o {{site.data.keyword.IBM_notm}} ou outros fornecedores para gerenciar seus nós de pedido ou autoridade de certificação. Também é possível incluir ou remover membros de um consórcio de blockchain, criar e associar canais e instalar e instanciar contratos inteligentes por meio de seu console.
- **Hospedar ou se associar a uma rede**. Implemente peers hospedados em seu cluster para múltiplos canais em múltiplas nuvens ou convide outras organizações para associar seu consórcio ou canais enquanto as organizações gerenciam seus nós de forma independente entre as infraestruturas.
- **Gerencie o acesso** dos usuários que podem administrar ou monitorar seus nós.
- **Acesso direto aos logs** de seus nós por meio do serviço de Kubernetes. Use qualquer serviço de terceiro suportado para extrair e analisar seus logs.
- **Interaja diretamente com seus pods** usando o serviço de Kubernetes.
- **Coleção de assinatura dinâmica** que permite melhor controle sobre a governança colaborativa em configurações de canal.

**CRESÇA --- Escalabilidade e flexibilidade**
- **Escolha seu cálculo**. Você tem a flexibilidade de decidir a quantia de CPU, de memória e de armazenamento que você deseja provisionar em seu cluster Kubernetes. Para obter mais informações, consulte [Como o console interage com seu cluster Kubernetes](/docs/services/blockchain/howto?topic=blockchain-ibp-console-govern#ibp-console-govern-iks-console-interaction).
- **Escale** para cima e para baixo os recursos em seu cluster Kubernetes, pagando somente pelo que você precisa. Para obter mais informações, consulte [Precificação](/docs/services/blockchain/howto?topic=blockchain-ibp-pricing#ibp-pricing).
- **Recuperação de desastre e alta disponibilidade de multizona.** Essa capacidade duplica sua implementação do Kubernetes entre as zonas, permitindo alta disponibilidade (HA) de seus componentes e recuperação de desastre (DR).
- **Execute em qualquer lugar**. Graças ao **código base unificado** do console do {{site.data.keyword.blockchainfull_notm}} Platform, é possível executar seus componentes em qualquer ambiente suportado pelo {{site.data.keyword.cloud_notm}} Private.

O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private é um produto empacotado para o {{site.data.keyword.cloud_notm}} Private e é entregue como um gráfico do Helm do Kubernetes. Para obter mais informações sobre o {{site.data.keyword.cloud_notm}} Private, consulte a documentação do [{{site.data.keyword.cloud_notm}} Private versão 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## É o {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private adequado para você
{: #console-icp-about-suitable}

A execução do {{site.data.keyword.blockchainfull_notm}} Platform fora do {{site.data.keyword.cloud_notm}} fornece mais flexibilidade para aumentar ou associar uma rede de blockchain. Ela ajuda os iniciadores de rede a cultivarem suas redes, permitindo que novos membros se associem usando a plataforma de sua escolha. Isso permitirá que as organizações que estão interessadas em se associar às redes de blockchain coloquem seus peers com seus aplicativos existentes ou se integrem a seus sistemas de registro.

Os usuários dessa oferta gerenciarão sua própria segurança e infraestrutura. O {{site.data.keyword.cloud_notm}} não fornece esses serviços. Revise as [Considerações e limitações](#console-icp-about-considerations) na próxima seção antes de iniciar.

## Considerações e limitações
{: #console-icp-about-considerations}

Antes de iniciar, assegure-se de entender as limitações a seguir:

- Você é responsável pelo gerenciamento do monitoramento de funcionamento, da segurança, da criação de log e do uso de recurso de seus componentes de blockchain.
- O console pode ser usado apenas para criar e controlar componentes com base no Hyperledger Fabric v1.4.1 ou mais recente.
- É possível implementar apenas um namespace Kubernetes do peer do console. Se planeja criar várias redes de blockchain, por exemplo, para criar diferentes ambientes para desenvolvimento, preparação e produção, é necessário criar um namespace exclusivo para cada ambiente.
- É possível importar apenas nós que foram exportados de outros consoles do {{site.data.keyword.blockchainfull_notm}} Platform. Para poder operar um peer importado ou um nó de pedido por meio do console, também é necessário importar a definição do MSP da organização do nó associado e a identidade do administrador para o seu console.
- O {{site.data.keyword.blockchainfull_notm}} Platform é suportado somente no {{site.data.keyword.cloud_notm}} Private v3.2 Enterprise Edition.  O {{site.data.keyword.cloud_notm}} Private v3.2 Community Edition não é suportado.
- Não é possível usar o {{site.data.keyword.IBM_notm}} Multicloud Manager para instalar o gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform.

Para dar uma olhada nos ambientes suportados pelo {{site.data.keyword.cloud_notm}} Private, consulte [Ambientes suportados](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_environments/environments_overview.html){: external}.
{:important}

## Pré-requisitos do sistema
{: #console-icp-about-prerequisites}

Consulte [Requisitos do sistema](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.2.0/supported_system_config/system_reqs.html){: external} para obter a lista de pré-requisitos de hardware e de software para o {{site.data.keyword.cloud_notm}} Private. Observe, no entanto, que o {{site.data.keyword.blockchainfull_notm}} Platform não é suportado em sistemas Linux on Power (ppc64le) POWER8.

O gráfico do Helm do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private foi validado para ser executado nos clusters do {{site.data.keyword.cloud_notm}} Private v3.2 no Ubuntu Linux usando os nós do trabalhador e o armazenamento auxiliar a seguir:

- **LinuxONE e {{site.data.keyword.IBM_notm}} Z**: z/VM e KVM, que estão usando NFS.
- **x86**: Linux de 64 bits que usa GlusterFS.

## Licença
{: #console-icp-about-license}

Consulte [Precificação para o {{site.data.keyword.blockchainfull_notm}} Platform para Multicloud](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) para obter mais informações sobre licenciamento e precificação.

## Instalando o  {{site.data.keyword.blockchainfull_notm}}  Platform for  {{site.data.keyword.cloud_notm}}  Private
{: #console-icp-about-install}

O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private é entregue como um arquivo de gráfico do Helm que pode ser instalado em um cluster local do {{site.data.keyword.cloud_notm}} Private. Depois de instalar o gráfico do Helm, é possível localizar o {{site.data.keyword.blockchainfull_notm}} Platform como um aplicativo no catálogo do {{site.data.keyword.cloud_notm}} Private. Para obter instruções sobre como instalar o gráfico do Helm e os pré-requisitos necessários, consulte [Instalando o {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto?topic=blockchain-console-helm-install#helm-console-install).

Se você for um novo usuário do {{site.data.keyword.cloud_notm}} Private e desejar informações e dicas sobre como instalar e implementar o {{site.data.keyword.cloud_notm}} Private, veja [Configurando o {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup).

É possível implementar o {{site.data.keyword.blockchainfull_notm}} Platform atrás de um firewall, sem ter acesso à Internet pública. O pacote de gráficos Helm transferidos por download inclui todas as imagens do Docker do componente Fabric que o {{site.data.keyword.blockchainfull_notm}} Platform usa, sem puxá-los do DockerHub durante a implementação.

## Considerações de segurança
{: #console-icp-about-security}

Como esses componentes são implementados em sua própria infraestrutura, você é responsável por gerenciar sua segurança. Isso inclui áreas importantes de segurança, como gerenciamento de chaves e criptografia de dados. Revise os tópicos a seguir ao considerar a segurança de seus componentes.

### Segurança de dados
{: #console-icp-about-security-data}

O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} usa criptografia de disco inteiro que é baseada na [criptografia de chave simétrica](https://www.ibm.com/support/knowledgecenter/en/SSB23S_1.1.0.14/gtps7/s7symm.html){: external} para proteger todos os dados criados por cada instância de serviço. Deve-se executar etapas semelhantes em seu próprio ambiente para proteger seus dados do peer.

Os dados em seu banco de dados de estado, se você usa LevelDB ou CouchDB, não são criptografados. É possível usar a criptografia de nível do aplicativo para proteger os dados em repouso em seu banco de dados de estado.

<!--
### Data residency
{: #console-icp-about-security-data-residency}

Data residency requirements can mandate that the processing and storage of all blockchain ledger data remain within the border of a single country (or within some other defined boundary). For more information about how data residency can be accomplished, see [Data residency](#console-icp-about-data-residency).
-->

### Gerenciamento de chave
{: #console-icp-about-security-key-management}

O gerenciamento de chave é um aspecto crítico da segurança. Se uma chave privada estiver comprometida ou perdida, agentes hostis poderão acessar seus dados e sua funcionalidade. A {{site.data.keyword.IBM_notm}} usa dispositivos físicos que são chamados de [Módulos de segurança de hardware](/docs/services/blockchain?topic=blockchain-glossary#glossary-hsm) (HSM) para armazenar as chaves privadas das redes do plano Enterprise do {{site.data.keyword.blockchainfull_notm}} Platform.

Você é responsável por gerenciar suas chaves privadas. Embora seja possível usar a IU do console do {{site.data.keyword.blockchainfull_notm}} Platform para gerar chaves privadas, essas chaves não são armazenadas pelo console ou no {{site.data.keyword.cloud_notm}} Private. É essencial armazenar suas chaves com segurança para que elas não sejam comprometidas.

É possível usar o Key Escrow para recuperar chaves privadas perdidas. Isso precisa ser executado antes da perda de qualquer chave. Se uma chave privada não puder ser recuperada, será necessário obter novas chaves privadas registrando uma nova identidade com sua Autoridade de certificação. Também será necessário remover e substituir o seu signCert de qualquer canal ao qual tenha se associado.

### TLS
{: #console-icp-about-security-tls}

A [Segurança da Camada de Transporte](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_7.1.0/com.ibm.mq.doc/sy10660_.htm){: external} (TLS) é integrada ao modelo de confiança do Hyperledger Fabric. Todos os componentes do {{site.data.keyword.blockchainfull_notm}} Platform usam TLS para autenticar e se comunicar entre si. Portanto, os nós no {{site.data.keyword.cloud_notm}} Private precisam ser capazes de concluir um handshake TLS com outros componentes e seus aplicativos. Uma implicação disso é que você precisa ativar o intermediário usando a lista de aplicativos confiáveis, por exemplo, em seu firewall dos aplicativos de clientes para os seus nós.

### Segurança do aplicativo
{: #console-icp-about-security-appl}

Como todas as chamadas de chaincode são assinadas, o Fabric gerencia a segurança do aplicativo. Além disso, o Fabric também inclui verificações de nível de aplicativo baseadas na ACL.

## Obtendo Suporte
{: #console-icp-about-support}

Para obter mais informações sobre como obter suporte no {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, bem como recursos grátis de desenvolvedor de blockchain e fóruns de suporte que possam ser usados para solucionar problemas, consulte [Obtendo suporte](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support).

Se você comprou uma licença do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private e deseja entrar em contato com o suporte ao cliente, consulte as informações sobre como [acessar a comunidade de suporte da {{site.data.keyword.IBM_notm}} e abrir um chamado de suporte](https://www-01.ibm.com/support/docview.wss?uid=ibm10740041){: external}.
