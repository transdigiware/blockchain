---

copyright:
  years: 2019
lastupdated: "2019-05-31"

keywords: network components, IBM Cloud Kubernetes Service, allocate resources, batch timeout, channel update, reallocate resources

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

# Controlando componentes
{: #ibp-console-govern}

Após criar CAs, peers, solicitadores, organizações e canais, é possível usar o console para atualizar esses componentes.
{:shortdesc}

No caso de estar usando a versão de avaliação beta do {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}, é provável que alguns painéis em seu console não correspondam à documentação atual, que é mantida atualizada com a instância de serviço geralmente disponível (GA). Para obter os benefícios de todas as funcionalidades mais recentes, é aconselhável neste momento provisionar uma nova instância de serviço GA seguindo as instruções de [Introdução ao {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain/howto/ibp-v2-deploy-iks.html#ibp-v2-deploy-iks).

**Público-alvo:** este tópico é projetado para operadores de rede que são responsáveis por criar, monitorar e gerenciar a rede de blockchain.

## Como o console interage com o cluster do Kubernetes
{: #ibp-console-govern-iks-console-interaction}

É responsabilidade do operador de rede monitorar o uso de CPU, memória e armazenamento e garantir que os recursos adequados estejam disponíveis **antes** de tentar criar ou redimensionar um nó.
{:important}

Como a instância do console do {{site.data.keyword.blockchainfull_notm}} Platform e seu cluster do Kubernetes não se comunicam diretamente sobre os recursos que estão disponíveis, o processo para implementar ou redimensionar componentes usando o console deverá seguir este padrão:

1. **Dimensione a implementação desejada**. Os painéis **Alocação de recurso** para a CA, o peer e o solicitador no console oferecem alocações padrão de CPU, memória e armazenamento para cada nó. Talvez seja necessário ajustar esses valores de acordo com seu caso de uso. Se não tiver certeza, comece com alocações padrão e ajuste-as conforme entender suas necessidades. Da mesma forma, o painel **Realocação de recurso** exibe as alocações de recursos existentes.

  Para entender a quantidade necessária de armazenamento e cálculo em seu cluster, consulte o gráfico subsequente a essa lista, que contém os padrões atuais para o peer, o solicitador e a CA.

2. **Verifique se você tem recursos suficientes em seu cluster do Kubernetes**. Se você está usando um cluster do Kubernetes hospedado no {{site.data.keyword.cloud_notm}}, recomendamos usar a ferramenta [{{site.data.keyword.cloud_notm}} SysDig](https://www.ibm.com/cloud/sysdig){: external} em combinação com seu painel do {{site.data.keyword.cloud_notm}} Kubernetes. Se não tiver espaço suficiente para implementar ou redimensionar recursos, será necessário aumentar o tamanho de seu cluster do serviço {{site.data.keyword.cloud_notm}} Kubernetes. Para obter mais informações sobre como aumentar o tamanho de um cluster, consulte [Escalando clusters](/docs/containers?topic=containers-ca#ca){: external}. Se você estiver usando um provedor em nuvem diferente do {{site.data.keyword.cloud_notm}}, será necessário consultar sua documentação para aprender como escalar clusters. Se tiver espaço suficiente em seu cluster, será possível continuar com a etapa 3.
3. **Use o console para implementar ou redimensionar seu nó**. Se o seu pod do Kubernetes for grande o suficiente para acomodar o novo tamanho do nó, a realocação deverá prosseguir sem problemas. Se o nó do trabalhador no qual o pod está em execução estiver com insuficiência de recursos, será possível incluir um novo nó do trabalhador maior em seu cluster e, em seguida, excluir o existente.

| **Componente** (todos os contêineres) | CPU  | Memória (GB) | Armazenamento (GB) |
|--------------------------------|---------------|-----------------------|------------------------|
| **Peer**                       | 1,2           | 2.4                   | 200 (inclui 100 GB para peer e 100 GB para CouchDB)|
| **CA**                         | 0,1           | 0,2                   | 20                     |
| **Nó de pedido**              | 0,45          | 0,9                   | 100                    |

Se você planeja implementar um serviço de pedido Raft de cinco nós, observe que o total de sua implementação aumentará por um fator de cinco. Portanto, um total de 2,25 de CPU, 4,5 GB de memória e 500 GB de armazenamento para os cinco nós Raft. Isso torna o serviço de solicitação de cinco nós maior que um único nó de trabalhador do Kubernetes de 2 CPUs.
{:tip}

Para casos nos quais um usuário deseja minimizar os encargos sem desativar um nó completamente ou excluí-lo, é possível diminuir sua capacidade até um mínimo de 0.001 CPU (1 milliCPU). Observe que o nó não será funcional ao usar essa quantidade de CPU.

Embora as figuras neste tópico se esforcem para serem precisas, esteja ciente de que há momentos em que um nó pode não ser implementado mesmo quando parece que você tem espaço suficiente em seu cluster. Certifique-se de referenciar seu painel do Kubernetes para ver quando os componentes são implementados e para obter mensagens de erro quando eles não são. Nos casos em que um componente não é implementado por falta de recursos, mesmo se parecer que há espaço suficiente no cluster, é provável que você tenha que implementar recursos de cluster adicionais para o componente ser implementado.
{:important}

## Alocando recursos
{: #ibp-console-govern-allocate-resources}

Enquanto os usuários de um cluster gratuito **devem usar tamanhos padrão** para os contêineres associados a seus nós, os usuários de clusters pagos podem configurar esses valores durante a criação de seus nós.

O painel **Alocação de recurso** no console fornece valores padrão para os diversos campos envolvidos na criação de um nó. Esses valores são escolhidos porque representam uma boa maneira de começar. No entanto, cada caso de uso é diferente. Embora este tópico forneça orientações para maneiras de pensar sobre esses valores, cabe ao usuário o monitoramento de seus nós e a localização de dimensionamentos que funcionem para eles. Portanto, ao barrar situações em que os usuários têm certeza de que precisarão de valores diferentes dos padrões, uma estratégia prática é usar esses padrões no início e ajustá-los posteriormente. Para obter uma visão geral de desempenho e escala do Hyperledger Fabric, no qual o {{site.data.keyword.blockchainfull_notm}} Platform é baseado, consulte [Respondendo às suas perguntas sobre desempenho e escala do Hyperledger Fabric](https://www.ibm.com/blogs/blockchain/2019/01/answering-your-questions-on-hyperledger-fabric-performance-and-scale/){: external}.

Todos os contêineres associados a um nó têm **CPU** e **memória**, no entanto, determinados contêineres associados ao peer, ao solicitador e à CA também têm **armazenamento**. Para obter mais informações sobre armazenamento, consulte [Considerações de armazenamento persistente](/docs/services/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-console-storage).

Você é responsável por monitorar seu consumo de CPU, memória e armazenamento em seu cluster. Se você solicitar mais recursos para um nó de blockchain do que há disponível, o nó não será iniciado, mas os nós existentes não serão afetados. Se você estiver usando o {{site.data.keyword.cloud_notm}} como seu provedor em nuvem, a CPU e a memória podem ser mudadas usando o console e o painel do {{site.data.keyword.cloud_notm}} Kubernetes Service. No entanto, após um nó ter sido criado, o armazenamento poderá ser mudado posteriormente apenas usando a CLI do {{site.data.keyword.cloud_notm}}. Para obter informações sobre como aumentar a CPU, a memória e o armazenamento em outros provedores em nuvem, consulte a documentação desses provedores em nuvem.
{:note}

Cada nó tem um contêiner de proxy da web gRPC que autoinicializa a camada de comunicação entre o console e um nó. Esse contêiner tem valores de recurso fixos e é incluído no painel Alocação de recurso para fornecer uma estimativa exata de quanto espaço é necessário no seu cluster Kubernetes para que o nó seja implementado. Como os valores para esse contêiner não podem ser mudados, não discutiremos o proxy da web gRPC nas seções a seguir.

### Autoridades de certificação (CAs)
{: #ibp-console-govern-CA}

Diferentemente de peers e solicitadores, que estão ativamente envolvidos no processo de transação, as CAs estão envolvidas somente no registro, na inscrição de identidades e na criação de um MSP. Isso significa que requerem menos CPU e memória. Para sobrecarregar uma CA, um usuário precisaria fazê-lo por meio de solicitações (provavelmente usando APIs e um script) ou tendo emitido tantos certificados a ponto de gerar insuficiência de armazenamento. Embora, como sempre, esses valores representem as necessidades de um caso de uso específico, nenhuma dessas coisas deve acontecer em operações comuns.

A CA tem apenas um contêiner associado que pode ser ajustado:

* **A própria CA**: encapsula os processos internos da CA, como registrar e inscrever nós e usuários, bem como armazenar uma cópia de cada certificado emitido.

#### Dimensionando uma CA durante a criação
{: #ibp-console-govern-CA-sizing-creation}

A CA tem apenas um contêiner único com valores que precisam de atenção, pois os valores do servidor proxy da web gRPC não podem ser mudados.

| Recursos | Condição para aumentar |
|-----------------|-----------------------|
| **CPU e memória do contêiner da CA** | Quando você espera que sua CA receba um grande volume de registros e inscrições. |
| **Armazenamento da CA** | Quando você planeja usar essa CA para registrar um grande número de usuários e aplicativos. |

### Colegas
{: #ibp-console-govern-peers}

O peer tem três contêineres associados que podem ser ajustados:

- **O próprio peer**: encapsula os processos internos do peer (como validar transações) e o blockchain (em outras palavras, o histórico de transações) para todos os canais aos quais ele pertence. Observe que o armazenamento do peer também inclui os contratos inteligentes instalados nele.
- **CouchDB**: local de armazenamento dos bancos de dados de estado do peer. Lembre-se de que cada canal tem um banco de dados de estado distinto.
- **Contrato inteligente**: lembre-se de que, durante uma transação, o contrato inteligente relevante é "chamado" (em outras palavras, executado). Observe que todos os contratos inteligentes instalados no peer serão executados em um contêiner separado dentro do contêiner de seu contrato inteligente, conhecido como um contêiner do Docker-in-Docker.  

O peer também inclui um contêiner para o **Coletor de logs** que canaliza os logs do contêiner do contrato inteligente para o contêiner do peer. Semelhante ao contêiner de proxy da web gRPC, não é possível ajustar o cálculo para esse contêiner.

#### Dimensionando um peer durante a criação
{: #ibp-console-govern-peers-sizing-creation}

Conforme observamos em nossa seção em [Como o console interage com seu cluster do Kubernetes](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction), é recomendável usar os padrões para esses contêineres de peer e ajustá-los posteriormente quando se tornar evidente como eles estão sendo utilizados por seu caso de uso.

| Recursos | Condição para aumentar |
|-----------------|-----------------------|
| **CPU e memória do contêiner do peer** | Quando você antecipa imediatamente um alto rendimento de transações. |
| **Armazenamento do peer** | Quando você antecipa a instalação de muitos contratos inteligentes nesse peer e para associá-lo a muitos canais. Lembre-se de que esse armazenamento também será usado para armazenar contratos inteligentes de todos os canais aos quais o peer está associado. Tenha em mente que consideramos uma transação "pequena" quando ela está no intervalo de 10.000 bytes (10k). Como o armazenamento padrão é 100 G, isso significa que até 10 milhões de transações totais serão armazenadas no peer antes que ele precise ser expandido (pela praticidade, o número máximo será menor do que esse, pois as transações podem variar em tamanho e o número não inclui contratos inteligentes). Embora 100 G possa parecer muito mais armazenamento do que é necessário, tenha em mente que ele é relativamente barato e que seu processo de aumento é mais complexo (requer o uso da linha de comandos) do que aumentar a CPU ou a memória. |
| **CPU e memória do contêiner do CouchDB** | Quando você antecipa um alto volume de consultas com relação a um grande banco de dados de estado. Esse efeito pode ser um pouco mitigado por meio do uso de [índices](https://hyperledger-fabric.readthedocs.io/en/release-1.4/couchdb_as_state_database.html#couchdb-indexes){: external}. No entanto, os altos volumes podem sobrecarregar o CouchDB, o que pode causar tempos limite de consultas e transações. |
| **Armazenamento do CouchDB (dados de livro-razão)** | Quando você espera um alto rendimento em muitos canais e não pretende usar índices. No entanto, como o armazenamento do peer, o armazenamento padrão do CouchDB é de 100 G, o que é significativo. |
| **CPU e memória do contêiner do contrato inteligente** | Quando você espera um alto rendimento em um canal, especialmente em casos nos quais diversos contratos inteligentes serão chamados de uma vez.|

### Solicitando nós
{: #ibp-console-govern-ordering-nodes}

Como os nós de pedido não mantêm o banco de dados de estado nem os contratos inteligentes do host, eles requerem menos contêineres do que os peers. Mas eles hospedam o blockchain (o histórico de transação) porque é nele em que a configuração do canal é armazenada e o serviço de pedido deve conhecer a configuração do canal mais recente para executar sua função.

Semelhante à CA, um nó de pedido tem apenas um contêiner associado que pode ser ajustado (se você estiver implementando um serviço de pedido de cinco nós, cinco conjuntos separados de contêineres de nó de pedido):

* **O próprio nó de pedido**: encapsula os processos internos do solicitador (como a validação de transações) e o blockchain para todos os canais que hospeda.

#### Dimensionando um solicitador durante a criação
{: #ibp-console-govern-orderer-sizing-creation}

Conforme observamos em nossa seção em [Como o {{site.data.keyword.cloud_notm}} Kubernetes Service interage com o console](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-iks-console-interaction), é recomendável usar os padrões para esses contêineres de solicitador e ajustá-los posteriormente quando ficar evidente como eles estão sendo utilizados.

| Recursos | Condição para aumentar |
|-----------------|-----------------------|
| **CPU e memória do contêiner do nó de pedido** | Quando você antecipa imediatamente um alto rendimento de transações. |
| **Armazenamento do nó de pedido** | Quando você antecipa que esse nó de pedido fará parte de um serviço de pedido em vários canais. Lembre-se de que o serviço de pedido mantém uma cópia do blockchain para cada canal que hospeda. O armazenamento padrão de um nó de pedido é 100G, o mesmo que o contêiner para o próprio peer. |

Assegurar que seus nós de pedido tenham CPU e memória suficientes é considerada uma melhor prática. Se um serviço de pedido estiver sobrecarregado, ele poderá atingir tempos limites e iniciar o descarte de transações, o que requer que as transações sejam reenviadas. Isso causa um dano muito maior a uma rede do que um único peer que se esforça para concluir ações. Em uma configuração de serviço de solicitação Raft, um nó líder sobrecarregado pode parar o envio de mensagens de pulsação, acionando uma eleição de líder e uma suspensão temporária da solicitação da transação. Da mesma forma, um nó seguidor pode perder mensagens e tentar acionar uma eleição de líder desnecessária.
{:important}

## Realocando recursos
{: #ibp-console-govern-reallocate-resources}

Depois de redimensionar um nó, você poderá ver um atraso antes que isso entre em vigor porque os contêineres estão sendo reconstruídos.
{:important}

Como dissemos acima, se o seu provedor em nuvem é o {{site.data.keyword.cloud_notm}}, recomendamos usar a ferramenta [{{site.data.keyword.cloud_notm}} SysDig](https://www.ibm.com/cloud/sysdig){: external} em combinação com seu painel do {{site.data.keyword.cloud_notm}} Kubernetes para monitorar o uso de seu recurso Kubernetes. Se determinar que um nó do trabalhador está com insuficiência de recursos, será possível incluir um novo nó do trabalhador maior em seu cluster e, em seguida, excluir o existente.
{:note}

Embora seja necessário menos esforço para implementar recursos suficientes em seu cluster do Kubernetes desde o início e, portanto, ser possível implementar e expandir recursos sem precisar aumentar os recursos em seu cluster, quanto maior a implementação, mais isso custará. Os usuários precisam considerar suas opções cuidadosamente e reconhecer as trocas que estão fazendo, independentemente da opção que escolherem.

Se o seu provedor em nuvem for {{site.data.keyword.cloud_notm}} ou em um provedor em nuvem diferente (usando o {{site.data.keyword.cloud_notm}} Private), será possível escalar seu cluster manualmente, monitorando seus nós e incluindo mais nós ou nós maiores. Embora esse processo possa ser trabalhoso, ele tem a vantagem de permitir que o usuário sempre tenha certeza do que está sendo cobrado em sua conta de nuvem.

Se um usuário tiver sido implementado no {{site.data.keyword.cloud_notm}} usando o {{site.data.keyword.cloud_notm}} Kubernetes Service, ele também terá a capacidade de usar o **escalador automático** do {{site.data.keyword.cloud_notm}} Kubernetes Service. O dimensionador automático dimensionará os nós do trabalhador em resposta às solicitações de recursos e suas configurações de especificação de pod. Para obter mais informações sobre o escalador automático do {{site.data.keyword.cloud_notm}} Kubernetes Service e como configurá-lo, consulte [Escalando clusters](/docs/containers?topic=containers-ca#ca){: external} na documentação do {{site.data.keyword.cloud_notm}}. Observe que permitir que o dimensionador automático ajuste seus recursos resultará em encargos para a sua conta do serviço {{site.data.keyword.cloud_notm}} Kubernetes, que variarão de acordo com seu uso.

Para escalar manualmente no console, clique no nó que você deseja ajustar na página **Nós** e, em seguida, clique na guia **Uso**. É possível ver um botão chamado **Realocar**, que ativará uma guia **Alocação de recursos** que é muito semelhante à que você viu quando criou o nó. Se desejar diminuir a quantidade de recursos disponíveis, simplesmente forneça valores mais baixos e clique em **Realocar recursos** nessa guia e na página **Resumo** resultante.

Se você desejar aumentar a CPU e a memória para um nó, use a guia **Alocação de recurso** no console para aumentar os valores. A caixa branca na parte inferior da página incluirá os novos valores. Depois de clicar em **Realocar recursos**, a página **Resumo** converterá esse valor em uma quantia de **VPC**, que é usada para calcular sua conta. Em seguida, será necessário navegar para o cluster do Kubernetes para certificar-se de que seu cluster tenha recursos suficientes para essa realocação. Se tiver, será possível clicar em **Realocar recursos**. Se não houver recursos suficientes disponíveis, será necessário aumentar o tamanho de seu cluster.

O método usado para aumentar o armazenamento dependerá da classe de armazenamento escolhida para seu cluster. Consulte a documentação de seu provedor em nuvem para conhecer isso. No {{site.data.keyword.cloud_notm}}, esse tópico é chamado de [opções de armazenamento](/docs/containers?topic=containers-kube_concepts#kube_concepts){: external}. Observe que, no {{site.data.keyword.cloud_notm}}, se você estiver prestes a esgotar o armazenamento em seu peer ou solicitador, deverá implementar um novo peer ou solicitador com um sistema de arquivos maior e permitir que ele sincronize por meio de seus outros componentes nos mesmos canais.

No {{site.data.keyword.cloud_notm}}, a CPU e a memória podem ser aumentadas usando o console (se você tem recursos disponíveis em seu cluster do {{site.data.keyword.cloud_notm}} Kubernetes Service). No entanto, o armazenamento deve ser aumentado usando a CLI do {{site.data.keyword.cloud_notm}}. Para obter um tutorial sobre como fazer isso, consulte [Mudando o tamanho e o IOPS de seu dispositivo de armazenamento existente](/docs/containers?topic=containers-file_storage#file_change_storage_configuration){: external}. Se você estiver usando um provedor em nuvem diferente do {{site.data.keyword.cloud_notm}}, consulte a documentação desse provedor para o processo de aumento de CPU, memória e armazenamento.

## Atualizando uma definição do MSP da organização
{: #ibp-console-govern-update-msp}

Com o tempo, é possível localizar a necessidade de atualizar uma definição do MSP da organização incluindo, por exemplo, administradores de organização adicionais ou mudando o nome de exibição do MSP no console.

- Simplesmente exporte a definição do MSP existente por meio da guia **Organizações** e edite o arquivo JSON gerado para modificar o nome de exibição, os certificados existentes ou incluir novos certificados. Não é recomendado que você mude o campo `msp_id` porque isso pode causar alterações que afetam o processamento da mensagem em sua rede.
- Clique em sua definição do MSP na guia **Organizações** para abri-la e, em seguida, clique em **Incluir arquivo** para fazer upload do novo arquivo JSON.
- Clique em **Atualizar definição do MSP**. As mudanças entram em vigor imediatamente.

## Atualizando uma configuração de canal
{: #ibp-console-govern-update-channel}

Embora a criação de um canal e a atualização de um canal tenham o mesmo objetivo, fornecer aos usuários a capacidade de assegurar que a configuração de seu canal seja tão adequada quanto possível para seu caso de uso, os dois processos são, de fato, muito diferentes **como tarefas** no console. Lembre-se em nossa documentação em [Criando um canal](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network-create-channel) que este é um processo realizado por uma **única organização**. Contanto que uma organização seja um membro do consórcio do serviço de pedido que se tornará o serviço de pedido de um canal, ela pode criar o canal de qualquer maneira que desejar. Eles podem fornecer qualquer nome, incluir qualquer organização (desde que seja membro do consórcio), designar essas permissões de organizações, configurar listas de controle de acesso e assim por diante.

As outras organizações têm a opção de participar desse canal (por exemplo, se associarem os peers a ele), mas supõe-se que o processo colaborativo de escolha de parâmetros de canal ocorrerá fora da banda, antes que uma organização use o console para criar o canal.

A atualização de um canal é diferente. Ela acontece **dentro do console** e segue os procedimentos de governança colaborativa que são fundamentais para a maneira como o {{site.data.keyword.blockchainfull_notm}} Platform funciona. Esse processo colaborativo envolve o envio de solicitações de atualização de configuração de canal para organizações que têm uma função administrativa no canal. Essas organizações também são conhecidas como **operadores** de canal.

Para atualizar um canal, clique no canal dentro da guia **Canais**. Clique no botão **Configurações** ao lado do nome do canal na parte superior da página. Aparecerá um painel que é muito semelhante ao painel que você usa para criar um canal.

### Parâmetros de configuração de canal que podem ser atualizados
{: #ibp-console-govern-update-channel-available-parameters}

É possível mudar alguns, mas não todos, dos parâmetros de configuração de um canal após a criação do canal. E há somente um parâmetro que é possível atualizar e não está disponível durante a criação do canal.

Você verá que o **Nome do canal** está esmaecido e não pode ser editado. Isso reflete o fato de que um nome de canal não pode ser mudado depois de ter sido criado. Além disso, observe que o nome de exibição do serviço de pedido não está presente, pois também não é possível mudar o serviço de pedido de um canal após o canal ter sido criado.

É possível, no entanto, mudar os parâmetros de configuração de canal a seguir:

* **Organizações**. Esta seção do painel é como as organizações são incluídas ou removidas de um canal. As organizações que podem ser incluídas podem ser vistas na lista suspensa. Observe que uma organização deve ser um membro do consórcio do serviço de pedido antes que ela possa ser incluída em um canal. Para obter mais informações sobre como incluir uma organização no consórcio, consulte [Incluir sua organização na lista de organizações que podem ser transacionadas](/docs/services/blockchain/howto/ibp-console-build-network.html#ibp-console-build-network-add-org).

* **Política de atualização de canal**. A política de atualização de um canal especifica quantas organizações (do número total de organizações no canal), quem deve aprovar uma atualização para a configuração de canal. Para assegurar um bom equilíbrio entre a administração colaborativa e o processamento eficiente de atualizações de configuração de canal, considere configurar essa política para a maioria dos administradores. Por exemplo, se houver cinco administradores em um canal, escolha `3 out of 5`.

* **Parâmetros de recorte de bloco**. (Opção avançada) Como uma mudança nos parâmetros de recorte de bloco padrão deve ser assinada por um administrador da organização de serviço de pedido, esses campos não estão presentes no painel de criação de canal. No entanto, como essa configuração de canal será enviada para todas as organizações relevantes no canal, é possível enviar uma solicitação de atualização de configuração de canal com mudanças nos parâmetros de recorte de bloco. Esses campos determinam as condições sob as quais o serviço de pedido recorta um novo bloco. Para obter informações sobre como esses campos afetam quando os blocos são recortados, consulte [Parâmetros de recorte de bloco](/docs/services/blockchain/howto/ibp-console-govern.html#ibp-console-govern-orderer-tuning-batch-size).

* **Listas de controle de acesso**. (Opção avançada) Para especificar um controle de baixa granularidade sobre os recursos, é possível restringir o acesso a um recurso para uma organização e uma função dentro dessa organização. Por exemplo, configurar o acesso ao recurso `ChaincodeExists` como `Application/Admins` significaria que somente o administrador de um aplicativo seria capaz de acessar o recurso `ChaincodeExists`.

Se você restringir o acesso a um recurso para uma determinada organização, esteja ciente de que somente essa organização será capaz de acessar o recurso. Se você desejar que outras organizações sejam capazes de acessar o recurso, será necessário incluí-las uma por uma usando os campos abaixo. Como resultado, considere suas decisões de controle de acesso cuidadosamente. Restringir o acesso a determinados recursos de determinadas maneiras pode ter um efeito altamente negativo sobre como seu canal funciona.
{:important}

Como o console fornece a um único usuário a capacidade de possuir e controlar várias organizações, deve-se especificar a organização que você está usando ao assinar uma atualização de canal na seção **Organização do atualizador de canal**. Se possuir mais de uma organização nesse canal, será possível escolher qualquer uma das organizações que você possui no canal para assinar. Dependendo da **política de atualização de canal** selecionada, você pode obter uma notificação pedindo que assine a solicitação como uma ou mais das outras organizações que você possui.

Se estiver tentando mudar qualquer um dos **Parâmetros de recorte de bloco** e possuir a organização de serviço de pedido desse canal, você verá um campo para a organização de serviço de pedido. Selecione o MSP da organização de serviço de pedido relevante na lista suspensa. Se você não for um administrador da organização de serviço de pedido, ainda será possível fazer uma solicitação para mudar um dos parâmetros de recorte de bloco, mas a solicitação será enviada e precisará ser assinada por um administrador do serviço de pedido.

### Fluxo de coleta de assinatura
{: #ibp-console-govern-update-channel-signature-collection}

Para que as assinaturas sejam verificadas, as organizações em um canal devem exportar os MSPs (no formato JSON) que representam suas organizações para as outras organizações no canal, bem como a importação dos MSPs de outras organizações. Para exportar um MSP, clique no botão de download em seu MSP (na tela **Organizações**), em seguida, envie-o para outras organizações fora da banda. Quando você receber um JSON de um MSP, importe-o usando a tela **Organizações**.
{: important}

Depois que uma solicitação de atualização de configuração de canal tiver sido feita, ela será enviada para as organizações no canal que têm o direito de assiná-la. Por exemplo, se houver cinco operadores (administradores de canal) em um canal, ela será enviada para todos os cinco. Para que a atualização de configuração de canal seja aprovada, o número de operadores de canal listados na **política de atualização de canal** deve ser satisfeito. Se essa política indicar `3 out of 5`, a atualização de configuração de canal será enviada para todos os cinco operadores e, quando três deles assinarem, a nova atualização de configuração de canal entrará em vigor.

Esse processo de saber quando você tem uma atualização para assinar, assim como assiná-la, é manipulado por meio do botão **Notificações** (que se parece com um sino) na parte superior direita do console. Quando você vê um ponto azul no botão **Notificações**, isso significa que você tem uma solicitação pendente para avaliar ou está sendo notificado de um evento de atualização de canal.

Ao clicar no botão **Notificações**, uma ou mais ações você poderá ter a capacidade de executar:

* **Precisa de atenção**: o usuário atual precisa assinar a solicitação (como uma organização de serviço de pedido ou de peer) ou precisa enviar a solicitação (se todas as assinaturas necessárias já tiverem sido coletadas).
* **Abertas**: inclui tudo que **precisa de atenção**, bem como solicitações que foram assinadas pelo usuário, mas ainda precisam ser assinadas por um ou mais membros do canal.
* **Fechadas**: solicitações que foram enviadas. Nenhuma ação a ser tomada sobre esses itens. Eles podem ser visualizados somente.
* **Todas**: inclui solicitações abertas e fechadas.

Se uma solicitação de atualização de configuração de canal tiver sido feita, você terá a capacidade de clicar em `Review and update channel configuration` e ver as mudanças na atualização de configuração de canal que estão sendo propostas ou tenham sido feitas (se a nova configuração de canal tiver sido aprovada). Se você for um operador no canal e não houver assinaturas suficientes reunidas para aprovar a solicitação de atualização de configuração de canal, você terá a capacidade de assinar a solicitação de atualização.

Você não é obrigado a assinar uma atualização de configuração de canal, no entanto, não há nenhuma maneira de assinar **com relação** a uma atualização de canal. Se você não aprovar uma atualização de configuração de canal, será possível simplesmente fechar o painel e alcançar outros operadores de canal fora da banda para expressar seus interesses. No entanto, se os operadores suficientes no canal aprovarem a atualização para satisfazer a política de atualização de canal, a nova configuração entrará em vigor.
{:note}

### Configurando peers de âncora
{: #ibp-console-govern-channels-anchor-peers}

Como o [gossip](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html){: external} organizacional cruzado deve ser ativado para que a descoberta de serviço e os dados privados funcionem, um peer de âncora deve existir para cada organização. Esse peer de âncora não é um **tipo** especial de peer, mas apenas o peer que a organização torna conhecido para outras organizações e que autoinicializa o gossip organizacional cruzado. Portanto, pelo menos um [peer de âncora](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html#anchor-peers){: external} deve ser definido para cada organização na definição de coleção.

Para configurar um peer para ser um peer de âncora, clique na guia **Canais** e abra o canal no qual o contrato inteligente foi instanciado.
 - Clique na guia  ** Detalhes do canal ** .
 - Role para baixo até a tabela de peers de Âncora e clique em **Incluir peer de âncora**.
 - Selecione pelo menos um peer de cada organização na definição de coleção que você deseja que sirva como o peer de âncora para a organização. Por motivos de redundância, é possível considerar a seleção de mais de um peer de cada organização na coleta.

## Ajustando seu solicitador
{: #ibp-console-govern-orderer-tuning}

O desempenho de uma plataforma de blockchain pode ser afetado por diversas variáveis, como o tamanho da transação, do bloco ou da rede e os limites do hardware. O nó do solicitador inclui um conjunto de parâmetros de ajuste que, juntos, podem ser usados para controlar o rendimento e o desempenho do solicitador.  É possível usar esses parâmetros para customizar como seu solicitador processa transações, dependendo da grande frequência de transações pequenas ou da pouca frequência de transações grandes. Essencialmente, você tem o controle para decidir quando os blocos são recortados com base no tamanho, na quantidade e na taxa de chegada das transações.

Os parâmetros a seguir estão disponíveis no console ao clicar no nó do solicitador, na guia **Nós**, e, em seguida, em seu ícone **Configurações**. Clique no botão **Avançado** para abrir a **Configuração avançada de canal** para o solicitador.

### Parâmetros de recorte de bloco
{: #ibp-console-govern-orderer-tuning-batch-size}

Os três parâmetros a seguir trabalham juntos para controlar quando um bloco é recortado, com base em uma combinação da configuração do número máximo de transações em um bloco e do tamanho do bloco em si.

- **Número máximo absoluto de bytes**  
  Configure esse valor para o maior tamanho de bloco, em bytes, que pode ser recortados pelo solicitador.  Nenhuma transação pode ser maior que o valor de `Absolute max bytes`. Geralmente, é seguro que essa configuração poderá ser de duas a dez vezes maior que o `Preferred max bytes`.    
  **Nota**: o tamanho máximo permitido é de 99 MB.
- **Contagem máxima de mensagens**   
  Configure esse valor para o número máximo de transações que podem ser incluídas em um único bloco.
- **Número máximo preferencial de bytes**  
  Configure esse valor para um tamanho de bloco ideal, em bytes, que seja menor que o `Absolute max bytes`. Um tamanho de transação mínimo, sem aprovações, tem cerca de 1 KB.  Se você incluir 1 KB por aprovação necessária, um tamanho de transação típico terá aproximadamente de 3 a 4 KB. Portanto, é recomendável configurar o valor de `Preferred max bytes` para algo semelhante a `Max message count * expected averaged tx size`. No tempo de execução, sempre que possível, os blocos não excederão esse tamanho. Se chegar uma transação que faça com que o bloco exceda esse tamanho, ele será recortado e um novo bloco será criado para ela. No entanto, se chegar uma transação que exceda esse valor sem exceder o `Absolute max bytes`, ela será incluída. Se chegar um bloco que seja maior que o `Preferred max bytes`, ele conterá apenas uma transação única e o tamanho dela não poderá ser maior que o `Absolute max bytes`.

Juntos, esses parâmetros podem ser configurados para otimizar o rendimento de seu solicitador.

### Tempo limite do lote
{: #ibp-console-govern-orderer-tuning-batch-timeout}

Configure o valor do **Tempo limite** para a quantidade de tempo de espera, em segundos, após a primeira transação, antes de recortar o bloco. Se você configurar esse valor muito baixo, correrá o risco de evitar que os lotes preencham seu tamanho preferencial. Se configurar muito alto, poderá fazer com que o solicitador aguarde os blocos e o desempenho geral seja comprometido. Em geral, recomendamos configurar o valor de `Batch timeout` para algo menor que `max message count / maximum transactions per second`.

Quando você modifica esses parâmetros, não afeta o comportamento de canais existentes no solicitador. Em vez disso, qualquer mudança feita na configuração do solicitador se aplica apenas aos novos canais criados nele.
{:important}
