---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform, release, new features

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:external: target="_blank" .external}

# o quê há de novo
{: #whats-new}

## 18 de junho de 2019
{: #whats-new-6-18-2019}

O {{site.data.keyword.blockchainfull}} Platform for Multicloud, que permite o uso da segunda geração do {{site.data.keyword.blockchainfull_notm}} Platform em sua própria infraestrutura e o provedor em nuvem Kubernetes de sua escolha, torna-se geralmente disponível. Essa liberação permite que o console da interface com o usuário seja usado para implementar e gerenciar componentes de blockchain em seu próprio ambiente usando o {{site.data.keyword.cloud_notm}} Private. O {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud usa o código base do Hyperledger Fabric v1.4.1 e suporta a implementação no {{site.data.keyword.cloud_notm}} Private v3.2.

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

- Mais informações sobre o {{site.data.keyword.blockchainfull_notm}} Platform estão disponíveis em [Sobre o {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto/ibp-console.html#ibp-console-overview).
- Instruções sobre como implementar a liberação em seu provedor em nuvem escolhido usando o {{site.data.keyword.cloud_notm}} Private está disponível em [Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks).
- Tutoriais atualizados para usar o {{site.data.keyword.blockchainfull_notm}} Platform estão disponíveis na subseção **Console do {{site.data.keyword.blockchainfull_notm}} Platform** na categoria **COMO FAZER**.
  * O [tutorial Construir uma rede](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) orientará você durante o processo de hospedagem de uma rede criando CAs, um serviço de pedido e um peer.
  * O [tutorial Associar uma rede](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network) explica como associar uma rede existente criando um peer e associando-o a um canal.
  * [Implementar um contrato inteligente na rede](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts) fornece informações sobre como gravar um contrato inteligente e implementá-lo em sua rede.
- A oferta {{site.data.keyword.blockchainfull_notm}} Platform baseia-se no Hyperledger Fabric v1.4.1 e suporta gossip de ponto a ponto, descoberta de serviço e dados privados. Visite este [tópico](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data) para saber como configurar dados privados em sua rede.

Para obter mais informações sobre o Hyperledger Fabric v1.4.1, consulte a [Documentação do Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}. Para obter mais informações sobre o {{site.data.keyword.cloud_notm}} Private, consulte a [documentação do {{site.data.keyword.cloud_notm}} Private v3.2](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

Se você ainda estiver usando o {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private v1.0.1 ou v1.0.2, consulte a [documentação do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain-icp-102/ibp_for_icp_deployment_guide.html).

## 31 de maio de 2019
{: #whats-new-5-31-2019}

O {{site.data.keyword.blockchainfull_notm}} Platform de segunda geração, que permite implementar, operar e monitorar sua rede de blockchain, se torna geralmente disponível. Essa liberação inclui um novo console da interface com o usuário que pode ser usado para implementar e gerenciar componentes de blockchain em seu próprio cluster {{site.data.keyword.IBM_notm}} Kubernetes Service no {{site.data.keyword.cloud_notm}}.

Essa liberação do {{site.data.keyword.blockchainfull_notm}} Platform inclui os recursos-chave a seguir:

**DESENVOLVA ---- Experiência de desenvolvedor integrada**
- **Codifique facilmente** seus contratos inteligentes em Node.js, Golang ou Java, grave aplicativos clientes usando a nova extensão {{site.data.keyword.blockchainfull_notm}} VS Code, aproveite a **integração do SDK** com o console e aprenda com nossos tutoriais e amostras valiosos.
- **DevOps simplificado** permite mover de desenvolvimento para testar para produção em um único ambiente, escalando para cima seus recursos do Kubernetes para incluir mais componentes.
- **Recursos-chave atualizados do Fabric**. Aproveite os recursos mais recentes do Hyperledger Fabric v1.4.1:
  -  [Serviço de solicitação de Raft](https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#raft){: external}
  - **Integração de serviço do {{site.data.keyword.cloud_notm}}.** Aproveite os serviços integrados do {{site.data.keyword.cloud_notm}}, como o {{site.data.keyword.cloud_notm}} Kubernetes Service Dashboard, o {{site.data.keyword.IBM_notm}} Log Analysis with LogDNA e o {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM).
  - [Coleções de **Dados privados**](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data) que fornecem maior privacidade de dados, assegurando que os dados do livro-razão sejam compartilhados somente com o peers autorizados por meio do protocolo gossip.
  - [Descoberta de serviço](https://hyperledger-fabric.readthedocs.io/en/release-1.4/discovery-overview.html){: external}, permitindo que você descubra e atualize dinamicamente como seu aplicativo interage com sua rede.
  - [Listas de controle de acesso ao canal](https://hyperledger-fabric.readthedocs.io/en/release-1.4/access_control.html){: external} que permitem o controle adicional da governança de seus canais e contratos inteligentes.

**OPERE --- Controle total de suas implementações**
- ** Implemente somente os componentes necessários **. Conectar um peer a múltiplos canais e redes ou hospede um serviço de solicitação ao qual os parceiros de negócios podem se conectar.
- ** Manter o controle completo de suas identidades **. Armazene e gerencie as chaves que são usadas para administrar seus nós sem armazenar suas chaves privadas no {{site.data.keyword.cloud_notm}}.
- **Operação centralizada**. O console do {{site.data.keyword.blockchainfull_notm}} Platform permite implementar e gerenciar todas as suas organizações e nós em **um console central** sem ter que depender da {{site.data.keyword.IBM_notm}} ou de outros fornecedores para gerenciar seus solicitadores ou Autoridades de certificação. Também é possível incluir ou remover membros de um consórcio de blockchain, criar e associar canais e instalar e instanciar contratos inteligentes por meio de seu console.
- **Hospedar ou se associar a uma rede**. Implemente peers hospedados em seu cluster para múltiplos canais em múltiplas nuvens ou convide outras organizações para associar seu consórcio ou canais enquanto as organizações gerenciam seus nós de forma independente entre as infraestruturas.
- **Gerencie o acesso** dos usuários que podem administrar ou monitorar seus nós.
- **Acesso direto aos logs** de seus nós por meio do serviço {{site.data.keyword.IBM_notm}} Kubernetes. Use o serviço {{site.data.keyword.cloud_notm}} Log Analysis ou um serviço de terceiro para extrair e analisar seus logs.
- **Interaja diretamente com seus pods** usando o Painel do Kubernetes. Use o executável em seus pods e contêineres para executar comandos e atualizar certificados por meio da linha de comandos.
- **Coleção de assinatura dinâmica** que permite melhor controle sobre a governança colaborativa em configurações de canal.

**CRESÇA --- Escalabilidade e flexibilidade**
- **Escolha seu cálculo**. Você tem a flexibilidade de decidir a quantia de CPU, de memória e de armazenamento que você deseja provisionar em seu cluster Kubernetes. Para obter mais informações, consulte [Como o {{site.data.keyword.cloud_notm}} Kubernetes Service interage com o console](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction).
- **Escale** para cima e para baixo os recursos em seu cluster Kubernetes, pagando somente pelo que você precisa. Para obter mais informações, consulte [Precificação](/docs/services/blockchain/howto/pricing.html#ibp-pricing).
- **Recuperação de desastre e alta disponibilidade de multizona.** Essa opção duplica a sua implementação do Kubernetes entre zonas, ativando a alta disponibilidade (HA) de seus componentes e a recuperação de desastre (DR).
- **Execute em qualquer lugar** (instruções em breve). Graças ao **código base unificado** do console do {{site.data.keyword.blockchainfull_notm}} Platform, é possível executar seus componentes em nuvens públicas do {{site.data.keyword.cloud_notm}}, do {{site.data.keyword.cloud_notm}} Private e de terceiros.

- Mais informações sobre o {{site.data.keyword.blockchainfull_notm}} Platform estão disponíveis em [Sobre o {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto/ibp-console.html#ibp-console-overview).
- Instruções sobre como implementar a liberação em um cluster do {{site.data.keyword.IBM_notm}} Kubernetes Service estão disponíveis em [Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks).
- Novos tutoriais para usar o {{site.data.keyword.blockchainfull_notm}} Platform estão disponíveis na subseção **{{site.data.keyword.blockchainfull_notm}} Platform 2.0** na categoria **COMO FAZER**.
  * [Construir um tutorial de rede](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) fornece orientação durante o processo de hospedagem de uma rede ao criar um solicitador e um peer.
  * O [tutorial Associar uma rede](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network) explica como associar uma rede existente criando um peer e associando-o a um canal.
  * [Implementar um contrato inteligente na rede](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts) fornece informações sobre como gravar um contrato inteligente e implementá-lo em sua rede.
- A oferta {{site.data.keyword.blockchainfull_notm}} Platform baseia-se no Hyperledger Fabric v1.4.1 e suporta gossip de ponto a ponto, descoberta de serviço e dados privados. Visite este [tópico](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data) para saber como configurar dados privados em sua rede.

## 9 de maio de 2019
{: #whats-new-5-09-2019}

O {{site.data.keyword.blockchainfull_notm}} libera a versão 1.0.0 do {{site.data.keyword.blockchainfull_notm}} Platform Visual Studio (VS) Code Extension. Esse kit de ferramentas do desenvolvedor contém os recursos chave a seguir:

**Uma interface com o usuário reconfigurada para navegação mais simples**

**Tutoriais pré-criativos e detalhados que cobrem como:**
- Desenvolver um contrato inteligente
- Provisionar uma instância de serviço do {{site.data.keyword.blockchainfull_notm}} no {{site.data.keyword.cloud_notm}}
- Implementar e transacionar com sua instância de serviço do {{site.data.keyword.blockchainfull_notm}} Platform

**Todos os recursos populares de liberações anteriores, incluindo:**
- Capacidade de depurar contratos inteligentes
- Código de amostra
- Recurso de carteira eletrônica atualizado

Para obter mais informações, consulte [Desenvolvendo contratos inteligentes com a extensão do Visual Studio Code](/docs/services/blockchain/vscode-extension.html "Desenvolvendo contratos inteligentes com a extensão do Visual Studio Code").

## 23 de abril de 2019
{: #whats-new-4-23-2019}

Liberações do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private v1.0.2, que usa o código base do Hyperledger Fabric v1.4.0 e suporta a implementação no {{site.data.keyword.cloud_notm}} Private v3.1.2.

Se desejar fazer upgrade do seu {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private existente para a v1.0.2, consulte [Fazendo upgrade do gráfico do Helm no {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/helm_install_icp.html#helm-install-upgrading).

Para obter mais informações sobre o Hyperledger Fabric v1.4.0, consulte a [Documentação do Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}. Para obter mais informações sobre o {{site.data.keyword.cloud_notm}} Private, consulte a [documentação do {{site.data.keyword.cloud_notm}} Private v3.1.2](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.2/kc_welcome_containers.html){: external}.

## 8 de fevereiro de 2019
{: #whats-new-2-08-2019}

O {{site.data.keyword.blockchainfull_notm}} Platform libera um beta 2.0 grátis, a próxima geração de soluções do {{site.data.keyword.blockchainfull_notm}} Platform que permite implementar, operar e monitorar sua rede de blockchain. O beta 2.0 grátis do {{site.data.keyword.blockchainfull_notm}} Platform inclui um novo console da interface com o usuário, que pode ser usado para implementar e gerenciar componentes de blockchain em seu próprio cluster do {{site.data.keyword.IBM_notm}} Kubernetes Service no {{site.data.keyword.cloud_notm}}. O beta 2.0 grátis do {{site.data.keyword.blockchainfull_notm}} Platform permitirá a você:

**Construir sua rede mais rápido e mais fácil dentro de uma experiência contínua**

*	Integração suave entre o desenvolvimento de contrato inteligente (VS Code) e o gerenciamento de rede
*	O DevOps simplificado permite mover do desenvolvimento para teste para produção por meio de um único console
*	Suporte para gravação de contratos inteligentes em linguagens Javascript, Java e Go

**Operar e controlar redes com controle total**

*	Implemente somente os componentes de blockchain necessários (peer, serviço de pedido, Autoridade de Certificação)
*	O console projetado novamente permite gerenciar componentes de rede em um local, não importa onde eles estejam implementados
*	Manter controle completo de suas identidades, livro-razão e contratos inteligentes

**Aumente redes distribuídas com facilidade com a flexibilidade de multinuvem recém-ativada**

*	Conecte-se a nós em execução em qualquer ambiente (nuvens no local, públicas, híbridas)
*	Conecte facilmente um único peer a múltiplas redes de segmentos de mercado
*	Comece pequeno, pague conforme crescer por aquilo que você usar sem investimento antecipado e faça upgrade facilmente por meio do Kubernetes

- Mais informações sobre o {{site.data.keyword.blockchainfull_notm}} Platform free 2.0 beta estão disponíveis em [Sobre o {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto/ibp-console.html#ibp-console-overview).
- Instruções sobre como implementar a liberação free 2.0 beta em um cluster do {{site.data.keyword.IBM_notm}} Kubernetes Service estão disponíveis em [Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform 2.0](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks).
- Novos tutoriais para usar o beta 2.0 grátis do {{site.data.keyword.blockchainfull_notm}} Platform estão disponíveis na subseção **{{site.data.keyword.blockchainfull_notm}} Platform 2.0** sob a categoria **COMO**.
  * [Construir um tutorial de rede](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network) fornece orientação durante o processo de hospedagem de uma rede ao criar um solicitador e um peer.
  * O [tutorial Associar-se a uma rede](/docs/services/blockchain/howto/ibp-console-join-network.html#ibp-console-join-network) explica como se associar a uma rede existente criando um peer e associando-o a um canal.
  * [Implementar um contrato inteligente na rede](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts) fornece informações sobre como gravar um contrato inteligente e implementá-lo em sua rede.
- A oferta beta 2.0 grátis do {{site.data.keyword.blockchainfull_notm}} Platform é baseada no Hyperledger Fabric v1.4 e suporta gossip de ponto a ponto, descoberta de serviço e dados privados. Visite este [tópico](/docs/services/blockchain/howto/ibp-console-smart-contracts.html#ibp-console-smart-contracts-private-data) para saber como configurar dados privados em sua rede.

- A extensão do {{site.data.keyword.blockchainfull_notm}} Visual Studio Code está disponível por meio do Mercado do Visual Studio Code. Os desenvolvedores podem usar a extensão para criar, testar e implementar contratos inteligentes em uma instância do Hyperledger Fabric. A extensão é compatível com o Hyperledger Fabric 1.3 e superior. Para obter informações sobre como usar a extensão do VS Code, veja [Ferramentas para contratos inteligentes](/docs/services/blockchain/vscode-extension.html#develop-vscode).  

O índice é aprimorado agrupando todos os tópicos de introdução juntos em uma seção chamada **Tutoriais de introdução**. Além disso, cada uma das ofertas do {{site.data.keyword.blockchainfull_notm}} Platform é descrita nas subseções **Sobre o {{site.data.keyword.blockchainfull_notm}} Platform** que estão agora contidas sob a seção **APRENDER**.

**Nota:** os créditos de nuvem grátis não estão mais disponíveis para usuários do Starter Plan.

## 7 de dezembro de 2018
{: #whats-new-12-07-2018}

Os tópicos *Operar rede do Starter Plan* e *Operar rede do Enterprise Plan* são combinados e substituídos por um único tutorial sobre [Usando o monitor de rede](/docs/services/blockchain/v10_dashboard.html#ibp-dashboard).

## 27 de novembro de 2018
{: #whats-new-11-27-2018}

O {{site.data.keyword.blockchainfull_notm}}  Platform libera o  {{site.data.keyword.blockchainfull_notm}}  Platform for  {{site.data.keyword.cloud_notm}}  Private. O {{site.data.keyword.cloud_notm}} Private é uma plataforma de aplicativo para desenvolver e gerenciar aplicativos conteinerizados baseados em Kubernetes e permite que os usuários implementem autoridades de certificação (CAs), solicitadores e peers no x86, LinuxONE e {{site.data.keyword.IBM_notm}} Z. O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private é baseado no Hyperledger Fabric v1.2.1 e é implementado usando gráficos do Helm do Kubernetes. Depois de instalar o gráfico do Helm, será possível localizá-lo como um serviço empacotado no Catálogo do {{site.data.keyword.cloud_notm}} Private. Observe que [esta oferta](/docs/services/blockchain/ibp-for-icp-about.html#ibp-icp-about) é mais adequada para usuários experientes do Fabric.

Para obter mais informações sobre o {{site.data.keyword.cloud_notm}} Private, consulte a [documentação do {{site.data.keyword.cloud_notm}} Private v3.1.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/kc_welcome_containers.html){: external}.

A liberação do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private também marca o término do programa {{site.data.keyword.blockchainfull_notm}} Platform Remote Peer (Beta). Ainda é possível implementar um peer no {{site.data.keyword.cloud_notm}} Private ou no AWS e conectá-lo a uma rede do Starter Plan ou do Enterprise Plan. Para obter mais informações, consulte [implementando um peer no Starter ou no Enterprise Plan](/docs/services/blockchain/howto/peer_deploy_ibp.html#ibp-peer-deploy) e [implementando um peer no AWS](/docs/services/blockchain/howto/remote_peer_aws.html#remote-peer-aws). Esses peers são considerados peers **distribuídos** em vez de peers remotos porque a implementação é gerenciada pelo cliente e, como resultado, não há nenhum local central a partir do qual ser remoto.

Essa liberação também apresenta algumas melhorias no índice da documentação. Se você for um usuário Starter ou Enterprise, seu conteúdo ainda poderá ser localizado nas seções **APRENDIZADO**, **INSTRUÇÕES**, **REFERÊNCIA** e **AJUDA** e os links ainda são os mesmos. Ele pode apenas estar em uma subseção diferente no índice.

## 13 de novembro de 2018
{: #whats-new-11-13-2018}

O {{site.data.keyword.blockchainfull_notm}} Platform libera o {{site.data.keyword.blockchainfull_notm}} Platform for Amazon Web Services (AWS).

O {{site.data.keyword.blockchainfull_notm}} Platform for AWS permite que você execute peers no AWS Cloud e conecte os peers às redes de blockchain existentes no {{site.data.keyword.cloud_notm}}. Seus peers no AWS podem alavancar o perfil de conexão e outros componentes de blockchain nas redes existentes, enquanto fornece mais flexibilidade para colocar seus peers com outros aplicativos fora do {{site.data.keyword.cloud_notm}}. Para obter mais informações, consulte [Sobre o {{site.data.keyword.blockchainfull_notm}} Platform for Amazon Web Services](/docs/services/blockchain/howto/remote_peer_aws.html#remote-peer-aws).
/docs/services/blockchain/howto/remote_peer_aws.html#remote-peer-aws

## 4 de outubro de 2018
{: #whats-new-10-04-2018}

O {{site.data.keyword.blockchainfull_notm}} Platform faz upgrade do Starter Plan a partir do Hyperledger Fabric v1.1.0 a v1.2.1. Para obter mais informações, veja [Sobre o Starter Plan](/docs/services/blockchain/starter_plan.html#starter-plan-about).

**Importante:** o Fabric v1.2.1 atualmente não é compatível com o beta Remote Peer, que usa um peer v1.1.0. As redes do Starter Plan, que foram criadas antes de 4 de outubro de 2018 e são baseadas no Fabric v1.1.0, não são impactadas e ainda podem ser usadas com o beta Remote Peer. O {{site.data.keyword.blockchainfull_notm}} Platform atualizará o beta Remote Peer para usar o peer v1.2.1, que será compatível com as novas redes Starter, que estão no nível do Fabric v1.2.1 e nas redes Enterprise, que estão no nível do Fabric v1.1.0. Até que o beta Remote Peer seja atualizado para o Fabric v1.2.1, não tente implementar um peer remoto v1.1.0 com uma nova rede Starter v1.2.1.

## 4 de setembro de 2018
{: #whats-new-9-04-2018}

O {{site.data.keyword.blockchainfull_notm}} Platform libera a oferta Beta do Remote Peer. A oferta do Remote Peer é baseada no Hyperledger Fabric v1.1.0. Usando o Remote Peer, é possível executar os nós de peer do {{site.data.keyword.blockchainfull_notm}} Platform em seu próprio ambiente de nuvem do {{site.data.keyword.cloud_notm}} Private ou Amazon Web Services (AWS). Para obter mais informações, consulte [Sobre peers remotos](/docs/services/blockchain/howto/remote_peer_aws.html#remote-peer-aws).

## 15 de junho de 2018
{: #whats-new-6-15-2018}

O {{site.data.keyword.blockchainfull_notm}} Platform libera a disponibilidade geral do Starter Plan. Para obter mais informações, veja [Sobre o Starter Plan](/docs/services/blockchain/starter_plan.html#starter-plan-about).

## 15 de maio de 2018
{: #whats-new-5-15-2018}

O {{site.data.keyword.blockchainfull_notm}} Platform faz upgrade do Enterprise Plan do Hyperledger Fabric v1.0.0 para a v1.1.0. Para obter mais informações, veja [Sobre o plano corporativo](/docs/services/blockchain/enterprise_plan.html#enterprise-plan-about).

## 18 de março de 2018
{: #whats-new-3-18-2018}

O {{site.data.keyword.blockchainfull_notm}} Platform libera o Starter Plan Beta. O Starter Plan oferece a você um ambiente de desenvolvimento e teste para aprender e praticar em uma rede do {{site.data.keyword.blockchainfull_notm}} Platform. Para obter mais informações, veja [Sobre o Starter Plan](/docs/services/blockchain/starter_plan.html#starter-plan-about).

## 11 de agosto de 2017
{: #whats-new-8-11-2017}

O {{site.data.keyword.blockchainfull_notm}} Platform substitui o
{{site.data.keyword.blockchainfull_notm}} on Cloud e libera o Enterprise Plan. Para obter mais informações, veja [Sobre o plano corporativo](/docs/services/blockchain/enterprise_plan.html#enterprise-plan-about).
