---

copyright:
  years: 2019
lastupdated: "2019-05-31"

keywords: release note, latest changes, Hyperledger Fabric

subcollection: blockchain

---

{:note: .note}
{:important: .important}
{:tip: .tip}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:external: target="_blank" .external}

# Notas sobre a liberação
{: #release-notes-saas-20}

Use estas notas sobre a liberação que são agrupadas por data para conhecer as mudanças mais recentes para o {{site.data.keyword.blockchainfull}} Platform for {{site.data.keyword.cloud_notm}}, que é construído no Hyperledger Fabric v1.4.1.
{:shortdesc}


## 24 de maio de 2019
{: #05-24-2019}

**Protocolo de consenso do Raft** O serviço de pedido Raft de cinco nós, recomendado para redes de produção, agora está disponível. Além disso, para propósitos de desenvolvimento e teste, é possível implementar um serviço de pedido Raft de nó único.

## 9 de maio de 2019
{: #05-09-2019}

**Introduzindo APIs do console do {{site.data.keyword.blockchainfull_notm}} Platform**

As APIs estão agora disponíveis para provisionar, editar e excluir nós de peer, solicitador e CA, tornando possível criar o script da construção de sua rede de blockchain. Use a documentação no [Repositório de documentação de API do{{site.data.keyword.cloud_notm}}](/apidocs/blockchain#introduction){: external} para saber mais sobre as APIs e experimente-as. Além disso, consulte o tópico em [Construindo uma rede com APIs](/docs/services/blockchain?topic=blockchain-ibp-v2-apis) para obter instruções sobre como usar as APIs para construir sua rede.  

**Controle de canal**  

As atualizações de controle de canal permitem que as políticas sejam reconfiguradas. Também é possível controlar quais membros do canal precisam assinar uma atualização de canal e é possível orquestrar a coleção de assinaturas.

## 17 de abril de 2019
{: #04-17-2019}

**Capacidade para dimensionar e escalar recursos de nó**  

Agora, ao implementar um nó, você tem a capacidade de especificar a quantidade de CPU, memória e armazenamento para seus contêineres, quando aplicável. É possível dimensionar seus recursos posteriormente, de acordo com os padrões de uso. Para obter mais informações, consulte [Alocando recursos](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-allocate-resources).

**Uso de {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)**  

O IAM é usado para controlar o acesso de usuário à IU do console, bem como restringir as ações que podem ser executadas nela.  Consulte este tópico para obter informações sobre como [incluir e remover usuários do console](/docs/services/blockchain?topic=blockchain-ibp-console-manage-console#ibp-console-manage-console-add-remove).

## 3 de abril de 2019
{: #04-03-2019}

**Suporte para CA externa**

Ao incluir um nó de peer ou solicitador, você tem a opção de usar certificados de uma CA externa não hospedada pela {{site.data.keyword.IBM_notm}}. Consulte este tópico em [Usando certificados de uma CA externa com seu peer ou solicitador](/docs/services/blockchain?topic=blockchain-ibp-console-build-network#ibp-console-build-network-third-party-ca) para obter mais informações.

**Ajustando o desempenho do solicitador**

Novos parâmetros de ajuste do solicitador estão disponíveis no console para oferecer a você mais controle sobre o rendimento e o desempenho do seu solicitador. Consulte este tópico em [Ajustando seu solicitador](/docs/services/blockchain?topic=blockchain-ibp-console-govern#ibp-console-govern-orderer-tuning) para obter instruções sobre como configurar os parâmetros.
