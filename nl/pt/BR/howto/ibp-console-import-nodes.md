---

copyright:
  years: 2019
lastupdated: "2019-05-31"

keywords: import nodes, another console, import a CA, import a peer, import admin identities, import an ordering service node

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

# Importando nós
{: #ibp-console-import-nodes}

O console inclui a opção para importar nós que são criados usando outro console do {{site.data.keyword.blockchainfull}} Platform.
{:shortdesc}  

**Público-alvo:** este tópico é projetado para operadores de rede que são responsáveis por criar, monitorar e gerenciar a rede de blockchain.

## Por que importar um nó?
{: #ibp-console-import-nodes-why}

Importe CAs, serviços de pedido e peers de outros consoles do {{site.data.keyword.blockchainfull_notm}} Platform quando for necessário operá-los ao lado de nós que já existem em seu console. Por exemplo, é possível usar a opção de peer de importação quando você deseja associar peers de organizações fora de seu console a canais em seu console. Como os serviços do {{site.data.keyword.blockchainfull_notm}} Platform são implementados em múltiplos ambientes, é provável que algumas instâncias de serviço incluam apenas CAs e peers, enquanto outras hospedem os serviços de pedido. A importação dos nós distintos para o console fornece uma maneira de visualizar e operar esses componentes distribuídos por meio de uma única interface com o usuário para que eles possam transacionar juntos no blockchain.

Depois de importar os nós no console, um conjunto robusto de capacidade operacional se torna disponível, por exemplo:
- Incluir novas organizações no consórcio
- Criar novos canais
- Associar peers aos canais
- Instalar contratos inteligentes em peers, independentemente de onde eles foram implementados
- Instanciar contratos inteligentes em canais
- Fazer upgrade de contratos inteligentes em canais

Ao importar um nó, é necessário que você forneça suas informações de conexão. Ao importar um componente, você não importa o próprio componente físico; em vez disso, ele usa as informações de conexão para construir uma representação do componente no console para que ele possa ser operado por meio do console. Da mesma forma, quando você exclui um nó importado do console, o próprio nó, que ainda está em execução no local em que ele foi implementado, não é excluído. Em vez disso, ele é removido apenas do console no qual foi importado.  

Após a importação de um nó para o console, também é possível modificar suas informações de conexão usando a guia **Configurações** do nó.

Antes de poder importar os nós para o console, eles precisam ser exportados do console do {{site.data.keyword.blockchainfull_notm}} Platform no qual eles foram criados. O operador de rede pode simplesmente exportar as informações do nó por meio do console para um arquivo `JSON`.
{: note}

## Limitações
{: #ibp-console-import-limitations}

- Não é possível importar nós de redes do plano Starter ou do plano Enterprise.
- Todos os nós a serem importados devem ter sido implementados usando o console do {{site.data.keyword.blockchainfull_notm}} Platform.

## Iniciar aqui: Reunindo Certificados ou Credenciais
{: #ibp-console-import-start-here}

Antes de poder importar um componente de blockchain existente de outra instância de serviço do {{site.data.keyword.blockchainfull_notm}} Platform, é necessário importar a identidade de administrador do componente para a carteira eletrônica do console. A importação da identidade para a carteira eletrônica do console aperfeiçoa a operação do nó.  O operador de rede no qual o nó foi implementado deve exportar a identidade de administrador do nó da carteira eletrônica para um arquivo JSON que você pode [importar](#ibp-console-import-nodes-admin-identities).  Geralmente, a identidade do administrador do nó é o administrador da organização do peer ou do solicitador que foi especificado quando a definição do MSP da organização foi criada. Ele já deve estar na carteira eletrônica do console no qual o nó reside.

### Importando identidades de administrador para a carteira eletrônica do console
{: #ibp-console-import-nodes-admin-identities}

Cada componente do {{site.data.keyword.blockchainfull_notm}} Platform é implementado contendo seu próprio certificado de assinatura, o certificado de assinatura, de um administrador de componente. Quando as ações são executadas com relação ao componente pelo administrador, esse certificado de assinatura é anexado à transação e validado com relação à configuração do componente. As chaves permitem que o administrador opere seus componentes, como registrar novos usuários, criar um novo canal ou instalar contratos inteligentes em peers. Se você desejar usar o console para operar um serviço de pedido ou peer que foi criado usando outro console, será necessário fazer upload da identidade de administrador associada para a carteira eletrônica do console. Em seguida, é possível associar o nó importado à identidade na carteira eletrônica do console. Essas identidades devem ser exportadas do console no qual elas foram criadas e são necessárias quando o nó é importado.

Para importar uma nova identidade, abra a guia **Carteira eletrônica** e clique em **Incluir identidade**. Clique em **Fazer upload de JSON** para procurar o arquivo de identidade JSON que foi exportado pelo operador de rede de onde o nó foi criado.

Depois de concluir o painel **Incluir identidade** e clicar em enviar, será possível visualizar a nova identidade de administrador na tela de visão geral da carteira eletrônica. Agora é possível referir-se a essas identidades ao importar os componentes de CA, peer ou serviço de pedido.

## Importando uma CA
{: #ibp-console-import-ca}

Um nó de CA é o componente de blockchain que emite certificados para todas as entidades de rede (peers, serviços de pedido, clientes e assim por diante) para que essas entidades possam se comunicar, autenticar e, finalmente, transacionar. Cada organização tem a sua própria CA que age como a sua raiz de confiança. Será necessário incluir suas organizações se você estiver se associando ou construindo um consórcio de blockchain. É possível saber mais sobre CAs na [visão geral de componentes de blockchain](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview-ca).  

Há várias razões pelas quais você pode desejar importar uma CA para seu console. Depois de importar a CA, é possível usá-la para registrar mais usuários de peer para a organização de peer clicando em **Registrar usuário**. Ou, é possível usar a CA para criar identidades administrativas adicionais para um peer ou um serviço de pedido.

Para importar uma CA para o console do {{site.data.keyword.blockchainfull_notm}} Platform e operá-la, o operador de rede já deve ter exportado a CA do {{site.data.keyword.blockchainfull_notm}} Platform no qual ela foi implementada. A importação de uma CA permite registrar novos usuários e [inscrever identidades](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-enroll).

### Antes de iniciar
{: #ibp-console-import-ca-before-you-begin}

- Assegure-se de que você já tenha importado o arquivo JSON de identidade do administrador da CA para a carteira eletrônica do console ou que tenha o ID de inscrição e o segredo do administrador para a CA e a CA do TLS que foram especificados quando a CA foi originalmente implementada.
- Assegure-se de que o arquivo JSON de CA que foi exportado do console no qual ele foi criado esteja disponível.

### Como Importar uma CA  
{: #ibp-console-import-nodes-howto-ca}

A importação de uma CA é executada na guia **Nós**.
1. Clique em **Incluir autoridade de certificação**, seguida por **Importar uma autoridade de certificação existente** e clique em **Avançar**.
2. Selecione o local no qual a CA foi originalmente implementada por meio da lista suspensa **Localização da autoridade de certificação**.
3. Clique em **Incluir arquivo** para fazer upload do arquivo JSON da CA que foi exportado do console no qual foi originalmente implementado.
4. No próximo painel, é possível inserir o ID de inscrição e o segredo que foi usado quando a CA foi implementada.
5. Por último, insira o ID de inscrição e o segredo para a CA do TLS que foi usada quando a CA foi implementada. Ao implementar uma CA, uma CA e uma CA do TLS são implementadas juntas. O operador de rede pode usar o mesmo ID de inscrição e o segredo para ambas ou pode especificar IDs de inscrição e segredos exclusivo para a CA e a CA do TLS quando uma CA é implementada.

Após a importação da CA para o console, é possível usar sua CA para criar novas identidades e gerar os certificados necessários para operar seus componentes e enviar transações para a rede. Para saber mais, veja [Gerenciando autoridades de certificação](/docs/services/blockchain/howto/ibp-console-identities.html#ibp-console-identities-manage-ca).

## Importando um serviço de pedido
{: #ibp-console-import-orderer}

Um serviço de pedido é o componente de blockchain que coleta transações de membros da rede, pede as transações e as empacota em blocos. Ele é a ligação comum de consórcios de blockchain e precisará ser implementado se você estiver fundando um consórcio ao qual outras organizações se associarão. É possível aprender mais sobre os serviços de pedido na [visão geral de componentes de blockchain](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview-orderer).

A importação de um serviço de pedido para o console permite criar novos canais para os peers para transacionarem privativamente.

### Antes de iniciar
{: #ibp-console-import-orderer-before-you-begin}

Antes de poder importar um serviço de pedido, é necessário reunir as informações a seguir:

- Assegure-se de que você já tenha [importado o arquivo JSON de identidade de administrador do serviço](#ibp-console-import-nodes-admin-identities) em sua carteira eletrônica do console.
- Assegure-se de que o arquivo JSON do serviço de pedido que foi exportado do console no qual ele foi criado esteja disponível.

### Como importar um serviço de pedido
A importação de um serviço de pedido é executada por meio da guia **Nós**.
1. Clique no **Incluir serviço de pedido**, seguido por **Importar um serviço de pedido existente** e clique em **Avançar**.
2. Selecione a localização na qual o serviço de pedido foi originalmente implementado por meio da lista suspensa **Localização da autoridade de certificação**.
3. Clique em **Incluir arquivo** para fazer upload do arquivo JSON do serviço de pedido que foi exportado do console no qual foi originalmente implementado. Observe que, se esse era um serviço de pedido Raft de cinco nós, é necessário ter um único arquivo com as informações de conexão para todos os cinco nós do pedido no arquivo.
4. Configure a identidade do administrador para o serviço de pedido clicando em **Identidade existente** e selecionando a identidade do administrador do serviço de pedido que você importou para a carteira eletrônica do console.

Depois de ter importado o serviço de pedido para o console, será possível incluir novos membros da organização e selecionar o serviço de pedido ao criar novos canais.

## Importando um Período
{: #ibp-console-import-peer}

Um nó de peer é o componente de blockchain que mantém um livro-razão e executa um contrato inteligente para executar operações de consulta e atualização no livro-razão. Os membros da organização possuem e mantêm os peers.  Cada organização que se associa a um consórcio deve implementar pelo menos um peer e minimamente dois para alta disponibilidade (HA). É possível aprender mais sobre os peers na [visão geral de componentes de blockchain](/docs/services/blockchain/blockchain_component_overview.html#blockchain-component-overview-peer).

Após a importação de um peer para o console, é possível instalar contratos inteligentes no peer e associar o peer a outros canais em seu blockchain.

**Nota:** se você precisar incluir mais peers na organização de peer ou criar identidades de administrador adicionais para um peer, será necessário importar a CA do peer e, em seguida, usar essa CA para executar essas operações.

### Antes de iniciar
{: #ibp-console-import-peer-before-you-begin}

Antes de poder importar um peer, é necessário reunir as informações a seguir:

- Assegure-se de que você já tenha [importado o arquivo JSON de identidade de administrador do peer](#ibp-console-import-nodes-admin-identities) em sua carteira eletrônica do console.
- Assegure-se de que o arquivo JSON do peer que foi exportado do console no qual ele foi criado esteja disponível.

### Como Importar um Período
{: #ibp-console-import-peer-howto}

A importação de um peer é executada por meio da guia **Nós**.
1. Clique em **Incluir peer**, seguido por **Importar um peer existente** e, em seguida, clique em **Avançar**.
2. Selecione a localização na qual o peer foi originalmente implementado por meio da lista suspensa **Localização da autoridade de certificação**.
3. Clique em **Incluir arquivo** para fazer upload do arquivo JSON de peer que foi exportado do console no qual foi originalmente implementado.
4. Configure a identidade do administrador para o peer clicando em **Identidade existente** e selecionando a identidade do administrador de peer que você importou para a carteira eletrônica do console.  

Após a importação do peer para o console, é possível instalar contratos inteligentes no peer e associar o peer aos canais em seu blockchain.

## Importando uma definição do MSP da organização
{: #ibp-console-import-msp}

É necessário importar uma definição do MSP da organização se o MSP foi implementado usando outro console da instância de serviço do {{site.data.keyword.blockchainfull_notm}} Platform e você planeja criar ou atualizar um canal que usa essa definição do MSP. Além disso, se você planejar criar um serviço de peer ou pedido que faz parte de um MSP da organização que foi implementado em outro console, será necessário importar o MSP e a identidade do administrador do MSP associada.  O operador de rede que criou o MSP precisa exportar a definição do MSP da organização e a identidade do administrador correspondente para os arquivos JSON e compartilhá-las com você.

- Se você planeja criar um serviço de peer ou pedido que faz parte da organização, importe a identidade do administrador do MSP associada para sua carteira eletrônica.
- A importação da definição do MSP da organização é executada por meio da guia **Organizações**.
- Clique em **Importar definição do MSP** para fazer upload do arquivo JSON
- (Opcional) Se você importou a identidade do administrador do MSP em sua carteira eletrônica, marque a caixa de seleção `I have an administrator identity for the MSP definition`. Se você não selecionar essa opção, quando posteriormente tentar criar um peer ou um serviço de pedido, essa definição do MSP da organização não será listada na lista suspensa do MSP.
