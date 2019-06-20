---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: IBM Blockchain Platform offerings, IBM Cloud Private, AWS, VS code extension, IBM Cloud

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

# Introdução ao  {{site.data.keyword.blockchainfull_notm}}  Platform
{: #get-started-ibp}

O {{site.data.keyword.blockchainfull}} Platform fornece uma oferta blockchain-as-a-service (BaaS) de pilha integral e gerenciada que permite implementar componentes de blockchain em ambientes de sua escolha. O ambiente pode ser {{site.data.keyword.cloud_notm}}, no local por meio do {{site.data.keyword.cloud_notm}} Private e nuvens de terceiros, como Amazon Web Services (AWS). Neste tutorial, conduziremos você pelo processo geral para configurar uma rede básica de blockchain com o {{site.data.keyword.blockchainfull_notm}} Platform.
{:shortdesc}

Antes de usar uma oferta do {{site.data.keyword.blockchainfull_notm}} Platform, leia as informações técnicas e de suporte na seção [Renúncia de responsabilidade](/docs/services/blockchain/needtoknow.html#disclaimer).
{: important}


## Antes de iniciar
{: #get-started-ibp-prereqs}

O {{site.data.keyword.blockchainfull_notm}} Platform fornece diferentes ofertas para ajudar todos os tipos de usuários a iniciar em sua jornada de blockchain e mover seus aplicativos para a produção. É necessário decidir o seu ambiente e a oferta que você usará. A Figura 1 lista os recursos, o público-alvo, a precificação e as plataformas de nuvem para diferentes ofertas. Para obter mais informações sobre cada oferta, clique na oferta na tabela a seguir.

| **Ofertas** | **O que está incluído** | **Política de faturamento** | **Plataforma de nuvem** |
| ------------------------- |-----------|-----------|-----------|-----------|
| [**{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**](/docs/services/blockchain?topic=blockchain-develop-vscode#develop-vscode) | Os desenvolvedores podem começar com o IDE que fornece um explorador e comandos acessíveis por meio da paleta de comandos, para desenvolver contratos inteligentes rapidamente. | Grátis | É executado em sua máquina local |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**](/docs/services/blockchain/howto/ibp-console.html#ibp-console-overview) | O console e as APIs do {{site.data.keyword.blockchainfull_notm}} Platform que podem ser usados para implementar e gerenciar componentes de blockchain em seu cluster {{site.data.keyword.cloud_notm}} Kubernetes | [Precificação de VPC $0,29 USD/VPC-hora](/docs/services/blockchain/howto/pricing-saas.html) | {{site.data.keyword.cloud_notm}} |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**](/docs/services/blockchain/ibp-for-icp-about.html#ibp-icp-about) | Console do {{site.data.keyword.blockchainfull_notm}} Platform implementado em um cluster do {{site.data.keyword.cloud_notm}} Private usando um gráfico do Helm do Kubernetes | [Precificação de VPC](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) | {{site.data.keyword.cloud_notm}} Private |
| [**{{site.data.keyword.blockchainfull_notm}} Platform for AWS**](/docs/services/blockchain/howto/remote_peer.html#remote-peer-aws-about) | Modelo de iniciação rápida do AWS para implementar peers remotos que estão fora do {{site.data.keyword.cloud_notm}} | Grátis | AWS |

*Figura 1. {{site.data.keyword.blockchainfull_notm}} Ofertas de plataforma *


## Etapa um: Obter uma oferta
{: #get-started-ibp-step1}

Assegure-se de que você tenha a conta de nuvem ou a licença do PPA para obter uma oferta do {{site.data.keyword.blockchainfull_notm}} Platform.

* **{{site.data.keyword.blockchainfull_notm}} Platform Extension for VS Code**

  Essa extensão do VS Code está disponível gratuitamente no [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} e pode ser usada para desenvolver, depurar e testar contratos inteligentes para implementação eventual no {{site.data.keyword.blockchainfull_notm}}.

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  Essa oferta está disponível no [{{site.data.keyword.cloud_notm}} painel do Catálogo](https://cloud.ibm.com/catalog){: external} do {{site.data.keyword.cloud_notm}}.

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  Essa oferta é entregue como um gráfico do Helm implementável por meio do [Passport Advantage On-line](https://www.ibm.com/software/passportadvantage/pao_customer.html).

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  Essa oferta está disponível na AWS e é possível implementar um peer de blockchain na AWS usando o [Modelo de iniciação rápida](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}.

## Etapa dois: Implementar o  {{site.data.keyword.blockchainfull_notm}}  Platform
{: #get-started-ibp-step2}

* **{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**

  Efetue login no {{site.data.keyword.cloud_notm}} e crie uma instância de serviço com a oferta. Siga o assistente para concluir a configuração inicial para sua rede. Para obter mais informações, consulte [Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Kubernetes Service](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks).

* **{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**

  Antes de implementar uma rede, é necessário instalar o gráfico do Helm em um cluster do {{site.data.keyword.cloud_notm}} Private. Em seguida, é possível implementar o console do {{site.data.keyword.blockchainfull_notm}} Platform e usá-lo para implementar e operar componentes de blockchain em seu cluster local. Para obter mais informações, consulte [Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain/get-started-console-icp.html#get-started-console-icp).

* **{{site.data.keyword.blockchainfull_notm}} Platform for AWS**

  Essa oferta está disponível na AWS e é possível implementar um peer de blockchain na AWS usando o [Modelo de iniciação rápida](https://aws.amazon.com/quickstart/architecture/ibm-blockchain-platform/){: external}.

## Próximas etapas
{: #get-started-ibp-next-steps}

Depois de implementar o {{site.data.keyword.blockchainfull_notm}} Platform no ambiente de sua escolha, é possível configurar a rede com o consórcio, os nós, os canais, os contratos inteligentes e iniciar as transações nela. Para obter mais informações, veja os tópicos da tarefa sob a seção **COMO** na navegação à esquerda desta documentação.

## Obtendo Suporte
{: #get-started-ibp-getting-support}

O {{site.data.keyword.IBM_notm}} oferece várias opções de suporte em soluções de blockchain implementadas pela {{site.data.keyword.IBM_notm}}. Para obter mais informações sobre o suporte do {{site.data.keyword.blockchainfull_notm}} Platform, veja [Obtendo suporte](/docs/services/blockchain/ibmblockchain_support.html#blockchain-support).
