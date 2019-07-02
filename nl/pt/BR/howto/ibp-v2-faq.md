---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}

# Perguntas Frequentes
{: #ibp-v2-faq}

**Geral**   

- [Qual é o valor de usar o {{site.data.keyword.blockchainfull_notm}} Platform sobre o Hyperledger Fabric nativo? ](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [Qual versão do Hyperledger Fabric está sendo usada com o {{site.data.keyword.blockchainfull_notm}} Platform? ](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [Qual banco de dados os peers usam para seu livro-razão?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [Quais linguagens são suportadas para contratos inteligentes?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [Você suporta o uso de certificados de autoridades de certificação (CAs) não IBM?](#ibp-v2-faq-v2-external-certs)  

**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**  

- [Posso fazer upgrade da V1.0 para o novo {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-v2-IBP-Overview-1-5)
- [O que acontece quando eu excluo meu serviço do {{site.data.keyword.blockchainfull_notm}} Platform? ](#ibp-v2-faq-v2-IBP-Overview-1-8)
- [Quais regiões estão disponíveis para o serviço de blockchain em execução no {{site.data.keyword.cloud_notm}}? ](#ibp-v2-faq-v2-IBP-Overview-1-9)
- [Posso usar meu cluster do {{site.data.keyword.cloud_notm}} Kubernetes Service existente?](#ibp-v2-faq-v2-Infrastructure-4-2)
- [Nós temos acesso aos serviços de criação de log? Quais logs estão disponíveis para mim?](#ibp-v2-faq-v2-Logging-and-Monitoring-11-6)  

**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**    

- [Quais são os benefícios do {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-benefits)
- [Quais ambientes o {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud suporta?](#ibp-v2-faq-icp-environments)
- [Qual é o modelo de precificação para o {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-pricing)
- [Quais serviços eu preciso instalar para usar o {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-services)
- [Como posso estimar os requisitos de dimensionamento do {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud para meus ambientes de desenvolvimento, de teste e de produção?](#ibp-v2-faq-icp-sizing)
- [O que acontece com meus componentes de blockchain ao excluir minha liberação do Helm?](#ibp-v2-faq-icp-delete)

## Qual é o valor de usar o {{site.data.keyword.blockchainfull_notm}} Platform sobre o Hyperledger Fabric nativo?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

O {{site.data.keyword.blockchainfull_notm}} Platform permite que os clientes implementem facilmente uma rede de blockchain customizada. É possível usar uma IU do console intuitiva para implementar rapidamente sua rede, instalar e instanciar facilmente contratos inteligentes e monitorar suas transações.

## Qual versão do Hyperledger Fabric está sendo usada com o {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}

O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} e o {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud usam o Hyperledger Fabric v1.4.1

## Qual banco de dados os peers usam para seu livro-razão?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}

Todos os peers que são implementados com o {{site.data.keyword.blockchainfull_notm}} Platform usam o CouchDB como banco de dados para o livro-razão.

## Quais linguagens são suportadas para contratos inteligentes?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}

O {{site.data.keyword.blockchainfull_notm}} Platform suporta contratos inteligentes que são gravados em Go e Node.js. O novo modelo de programação do Hyperledger Fabric suporta atualmente somente Node.js, mas mais estão chegando em breve. Se você estiver interessado em preservar o seu código de aplicativo existente ou usar SDKs do Fabric para linguagens diferentes do Node.js, ainda será possível se conectar à sua rede do {{site.data.keyword.blockchainfull_notm}} Platform usando as APIs do Fabric SDK de nível inferior.

## Você suporta o uso de certificados de autoridades de certificação não IBM?
{: #ibp-v2-faq-v2-external-certs}
{: faq}

Sim, é possível trazer seus próprios certificados, desde que eles sejam emitidos por uma CA que seja compatível com X.509.

## Posso fazer upgrade da V1.0 para o novo {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-IBP-Overview-1-5}
{: faq}

Não para o Starter Plan. Não é possível fazer upgrade do Starter Plan para o novo {{site.data.keyword.blockchainfull_notm}} Platform.
Para o Enterprise Plan, você será capaz de fazer upgrade para o novo {{site.data.keyword.blockchainfull_notm}} Platform no futuro. Você receberá um e-mail para o proprietário da conta da equipe do {{site.data.keyword.blockchainfull_notm}} Platform para ajudar.

## O que acontece quando eu excluo meu serviço do {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-IBP-Overview-1-8}
{: faq}

Quando você exclui uma instância de serviço do {{site.data.keyword.blockchainfull_notm}} Platform, todas as CAs de blockchain, peers e nós de pedido são excluídas juntamente com seu armazenamento associado.

## Quais regiões estão disponíveis para o serviço de blockchain em execução no {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-v2-IBP-Overview-1-9}
{: faq}

As regiões disponíveis para o {{site.data.keyword.blockchainfull_notm}} Platform estão listadas em [Locais do {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain?topic=blockchain-ibp-regions-locations). Observe que se deve criar um cluster {{site.data.keyword.cloud_notm}} Kubernetes Service na mesma região que o serviço de blockchain para reconhecer o cluster. As regiões adicionais estarão disponíveis em breve.

## Posso usar meu cluster do {{site.data.keyword.cloud_notm}} Kubernetes Service existente?
{: #ibp-v2-faq-v2-Infrastructure-4-2}
{: faq}

Seu cluster do Kubernetes existente funcionará com o {{site.data.keyword.blockchainfull_notm}} Platform desde que satisfaça as condições a seguir:
- Ele está executando o Kubernetes v1.11 ou uma versão estável superior.
- Há recursos disponíveis suficientes no cluster.

## Nós temos acesso aos serviços de criação de log? Quais logs estão disponíveis para mim?
{: #ibp-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

Com o {{site.data.keyword.blockchainfull_notm}} Platform, agora é possível acessar diretamente seu peer, a CA e os logs de nó de pedido por meio do painel do Kubernetes. É recomendável aproveitar o serviço {{site.data.keyword.cloud_notm}} LogDNA que permite que você analise facilmente os logs em tempo real.

## Quais são os benefícios do {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-benefits}
{: faq}

Consulte este tópico em [O que o {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud oferece](/docs/services/blockchain?topic=blockchain-console-icp-about#what-ibm-blockchain-platform-for-ibm-cloud-private-offers).

## Quais ambientes o {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud suporta?
{: #ibp-v2-faq-icp-environments}
{: faq}

O {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud suporta todos os ambientes que o {{site.data.keyword.cloud_notm}} Private v3.2 suporta. Consulte [Plataformas e sistemas operacionais suportados](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/supported_system_config/supported_os.html){: external} para obter mais informações.

## Qual é o modelo de precificação para o {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-pricing}
{: faq}

O licenciamento do {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud [](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) é baseado na quantia de CPU (VPC) disponibilizada para o produto. Para obter detalhes, [entre em contato com a IBM](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}.

## Quais serviços eu preciso instalar para usar o {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-services}
{: faq}

É necessário instalar o {{site.data.keyword.cloud_notm}} Private v3.2.

## Como posso estimar os requisitos de dimensionamento do {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud para meus ambientes de desenvolvimento, teste e produção?
{: #ibp-v2-faq-icp-sizing}
{: faq}

Depois de entender quantas autoridades de certificação, peers e nós de pedido são necessários, é possível examinar a [tabela de alocações de recursos padrão](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-resources) para obter uma estimativa aproximada das CPUs (VPCs) necessárias para a sua rede.

## O que acontece com meus componentes de blockchain ao excluir minha liberação do Helm?
{: #ibp-v2-faq-icp-delete}
{: faq}

Quando você exclui uma liberação do Helm de seu cluster do {{site.data.keyword.cloud_notm}} Private, os componentes de blockchain associados não são excluídos. Para remover adequadamente uma liberação do Helm de seu cluster, é necessário excluir todos os seus componentes usando o console de blockchain ou as APIs de blockchain primeiro. Em seguida, será possível excluir o gráfico do Helm.  
