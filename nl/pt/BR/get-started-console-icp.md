---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

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

# Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud
{: #get-started-console-icp}

O {{site.data.keyword.blockchainfull}} Platform pode ser implementado em nuvens públicas e privadas, como o {{site.data.keyword.cloud_notm}}, seu próprio data center e nuvens públicas de terceiros. Essa implementação multinuvem é possível por meio do {{site.data.keyword.cloud_notm}} Private, a plataforma de orquestração de contêiner baseada em Kubernetes da {{site.data.keyword.IBM_notm}}. É possível usar essa oferta para instalar o console do {{site.data.keyword.blockchainfull_notm}} Platform em uma implementação do {{site.data.keyword.cloud_notm}} Private e, em seguida, usar o console para criar componentes do {{site.data.keyword.blockchainfull_notm}} em seu cluster local. Também é possível usar seu console para operar uma rede de várias nuvens distribuída, importando os nós implementados em outros clusters do {{site.data.keyword.cloud_notm}} Private ou no {{site.data.keyword.cloud_notm}}.
{:shortdesc}

O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private é um produto empacotado para o {{site.data.keyword.cloud_notm}} Private. Para obter mais informações sobre o {{site.data.keyword.cloud_notm}} Private, consulte a documentação do [{{site.data.keyword.cloud_notm}} Private versão 3.2.0](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/kc_welcome_containers.html){: external}.

## Etapa um: configurar um cluster do Kubernetes no {{site.data.keyword.cloud_notm}} Private
{: #get-started-console-icp-step-one-set-up-k8s-on-icp}

A primeira etapa é configurar um cluster do Kubernetes no {{site.data.keyword.cloud_notm}} Private na plataforma de nuvem de sua escolha.
É possível visitar [Configurando o {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/ICP_setup.html#icp-setup) para obter recomendações e orientação.

Se você estiver construindo uma rede que será usada em produção, será necessário configurar sua implementação de cluster e sua rede de blockchain para alta disponibilidade:

- Para obter recomendações sobre como configurar seu cluster, visite [Implementar alta disponibilidade no {{site.data.keyword.cloud_notm}} Private](https://www.ibm.com/cloud/garage/practices/manage/high-availability-ibm-cloud-private){: external}.
- Quando você estiver pronto para começar a construir sua rede, visite [Considerações de alta disponibilidade para peers](/docs/services/blockchain/ibp-console-ha.html#ibp-console-ha-peers) e [Considerações de alta disponibilidade para os serviços de pedido](/docs/services/blockchain/ibp-console-ha.html#ibp-console-ha-ordering-service).

## Etapa dois: instalar o {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-icp-step-two-deploy-console}

O {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private é entregue como um gráfico do Helm que pode ser transferido por download por meio do Passport Advantage (PPA). Para saber como instalar o gráfico do Helm em seu cluster local, visite [Instalando o {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} Private](/docs/services/blockchain/howto/console-helm-install.html#console-helm-install).

## Etapa três: Implementar o console do {{site.data.keyword.blockchainfull_notm}} Platform
{: #get-started-console-icp-step-three-deploy-console}

Depois de ter instalado o gráfico do Helm, é possível clicar no bloco do aplicativo {{site.data.keyword.blockchainfull_notm}} Platform na página Catálogo para instalar um console do {{site.data.keyword.blockchainfull_notm}} Platform em seu cluster local. Para aprender a configurar o console, bem como os recursos que são necessários por seus componentes de blockchain, consulte [Implementando o console do {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain/howto/console-deploy-icp.html#console-deploy-icp).

## Etapa quatro: incluir usuários no console como o administrador
{: #get-started-console-icp-step-four-add-console-admin}

O administrador do console pode efetuar login no console usando o endereço de e-mail e a senha que foram fornecidos durante a implementação. A senha que foi fornecida torna-se a senha padrão do console e é usada por todos os novos usuários para efetuar login no console pela primeira vez. O administrador pode, então, incluir novos usuários no console, permitindo que outros efetuem login e comecem a trabalhar com os nós do {{site.data.keyword.blockchainfull_notm}}. O administrador também pode configurar uma nova senha padrão. Para saber mais, consulte [Gerenciando usuários por meio do console](/docs/services/blockchain/howto/console-icp-manage.html#console-icp-manage-users).

## Etapa cinco: usar o console para criar seus componentes
{: #get-started-console-icp-build-network}

Depois de ter implementado o console, é possível usá-lo para criar, operar e controlar os componentes do {{site.data.keyword.blockchainfull_notm}} em seu cluster local. Para começar a usar a IU do console, visite o [tutorial Construindo uma rede](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network).

## Etapa seis: conectar redes entre nuvens
{: #get-started-console-icp-import-nodes}

É possível usar o console para operar componentes que estão em execução em outros clusters do {{site.data.keyword.cloud_notm}} Private ou no {{site.data.keyword.cloud_notm}}. Primeiro, será necessário exportar as informações do componente para um arquivo JSON por meio do console no qual o componente foi originalmente implementado. Em seguida, é possível importar o arquivo JSON do nó para o console que é implementado em seu cluster local e gerenciar os componentes entre nuvens. Para obter mais informações, consulte [Importando nós](/docs/services/blockchain/howto/ibp-console-import-nodes.html#ibp-console-import-nodes).
