---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: vs code extension, Visual Studio Code extension, smart contract, development tools

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:external: target="_blank" .external}

# Desenvolvendo contratos inteligentes com a extensão do Visual Studio Code
{: #develop-vscode}


A extensão do {{site.data.keyword.blockchainfull}} Platform Visual Studio (VS) Code fornece um ambiente no Visual Studio Code para desenvolvimento, empacotamento e teste de contratos inteligentes. É possível usar a extensão para criar seu projeto de contrato inteligente e começar a desenvolver sua lógica de negócios. Em seguida, é possível usar o VS Code para testar seu contrato inteligente em sua máquina local usando uma instância pré-configurada do Hyperledger Fabric antes de implementar o contrato inteligente no {{site.data.keyword.blockchainfull_notm}} Platform. Este tutorial descreve como usar a extensão do VS Code.

![Fluxo de trabalho típico de desenvolvimento de contrato inteligente](images/SmartContractflow.png "Fluxo de trabalho típico de desenvolvimento de contrato inteligente")  

<!--
<img usemap="#home_map1" border="0" class="image" id="image_ztx_crb_f1b2" src="images/SmartContractflow.png" width="750" alt="Click a box to get more details on the process." style="width:750px;" />
<map name="home_map1" id="home_map1">
<area href="/docs/services/blockchain/vscode-extension.html#develop-vscode-creating-a-project" alt="Create a smart contract project" title="Create a Smart contract project" shape="rect" coords="40, 73.2, 175, 128.2" />
<area href="/docs/services/blockchain/vscode-extension.html#develop-vscode-creating-a-project" alt="Develop contract code in VS Code" title="Create key pair" shape="rect" coords="199, 73.2, 334, 128.2" />
<area href="/docs/services/blockchain/vscode-extension.html#packaging-a-smart-contract" alt="Package the smart contract" title="Package the smart contract" shape="rect" coords="358, 73.2, 175, 128.2" />
<area href="/docs/services/blockchain/vscode-extension.html#develop-vscode-deploy" alt="Deploy locally to test and debug" title="Deploy locally to test and debug" shape="rect" coords="358, 73.2, 493, 128.2"/>
<area href="/docs/services/blockchain/vscode-extension.html#develop-vscode-exporting-deleting-smart-contract-package" alt="Export the package" title="Export the package" shape="rect" coords="517, 152.2, 493, 207.2" />
<area href="/docs/services/blockchain/vscode-extension.html#develop-vscode-connecting-ibp" alt="Deploy to {{site.data.keyword.cloud_notm}}" title="Deploy to {{site.data.keyword.cloud_notm}}" shape="rect" coords="700, 73.2, 835, 128.2" />
-->

A extensão do {{site.data.keyword.blockchainfull_notm}} Platform funciona perfeitamente com qualquer instância do {{site.data.keyword.blockchainfull_notm}} Platform que usa o Hyperledger Fabric versões 1.4 e mais recente.
{: note}

## Etapa um: instalar a extensão do {{site.data.keyword.blockchainfull_notm}} Platform VS Code gratuitamente
{: #develop-vscode-install}

Antes de instalar a extensão do {{site.data.keyword.blockchainfull_notm}} Platform VS Code, é necessário concluir os pré-requisitos.

### Pré-requisitos
{: #develop-vscode-prerequisites}

- Os sistemas operacionais Windows 10, Linux ou Mac são atualmente os sistemas operacionais suportados.
- [VS Code versão 1.32 ou mais recente](https://code.visualstudio.com/){: external}.
- [Nó v8.x ou mais recente e npm v5.x ou mais recente](https://nodejs.org/en/download/){: external}.
- [Docker versão v17.06.2-ce ou mais recente](https://www.docker.com/get-started){: external}.
- [Docker Compose v1.14.0 ou mais recente](https://docs.docker.com/compose/install/){: external}.
- [Acesse a versão v1.12 ou mais recente para desenvolver contratos Go](https://golang.org/dl/){: external}.

Se você estiver usando o Windows, também deverá assegurar o seguinte:

- O Docker para Windows está configurado para usar contêineres do Linux (esse é o padrão).
- Você instalou o C++ Build Tools for Windows por meio de [windows-build-tools](https://github.com/felixrieseberg/windows-build-tools#windows-build-tools){: external}.
- Você instalou o OpenSSL v1.0.2 por meio do [Win32 OpenSSL](http://slproweb.com/products/Win32OpenSSL.html){: external}.
  - Instale a versão normal, não a versão marcada como "light".
  - Instale a versão Win32 em C:\OpenSSL-Win32 em sistemas de 32 bits.
  - Instale a versão Win64 em C:\OpenSSL-Win64 em sistemas de 64 bits.

### Instalar a extensão
{: #develop-vscode-installing-the-extension}

1. Navegue para a [página de marketplace da extensão do Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} ou procure por ** {{site.data.keyword.blockchainfull_notm}} Platform** no painel de extensões no Visual Studio Code.
2. Clique em **Instalar**.
3. Reinicie o Visual Studio Code para concluir a instalação da extensão.

Após a instalação, é possível usar o ícone do {{site.data.keyword.blockchainfull_notm}} no lado esquerdo do VS Code para abrir o painel do {{site.data.keyword.blockchainfull_notm}} Platform.

![Ícone {{site.data.keyword.blockchainfull_notm}}](images/vscode-blockchain.png "Ícone {{site.data.keyword.blockchainfull_notm}}")

A extensão também inclui novos comandos na paleta de comandos do Visual Studio Code. É possível usar a paleta de comandos para concluir muitas das operações que são explicadas em detalhes neste guia.

## Etapa dois: criar um projeto de contrato inteligente
{: #develop-vscode-creating-a-project}

É possível usar a extensão para criar um novo projeto de contrato inteligente no Visual Studio Code. A extensão cria um contrato inteligente básico que gerencia um recurso de exemplo no idioma de sua escolha. É possível usar a estrutura do exemplo como um ponto de início para desenvolver sua própria lógica de negócios. A extensão fornece todas as dependências que são necessárias para implementar seu contrato inteligente em uma instância do Hyperledger Fabric.

1. Abra a guia **{{site.data.keyword.blockchainfull_notm}}**. Clique no menu overflow na área de janela de pacotes de contrato inteligente e clique em **Criar projeto de contrato inteligente**.
2. Selecione o idioma no qual você deseja criar um contrato inteligente. As opções atuais são JavaScript, TypeScript, Go e Java. **Nota:** o {{site.data.keyword.blockchainfull_notm}} Platform não suporta o chaincode Java.
3. **Se você selecionou JavaScript ou TypeScript**, selecione um ativo a ser gerenciado pelo contrato de exemplo. Por exemplo, ***bond***.
4. Crie uma pasta com o nome de seu projeto e abra-o.
5. Selecione como abrir seu novo projeto. A pasta do projeto agora deve ser aberta.

Quando o projeto for aberto, será possível localizar o novo contrato inteligente na janela do explorador na área de janela à esquerda. A estrutura do projeto depende do idioma selecionado. No entanto, cada contrato inteligente contém os mesmos elementos:
- O código-fonte do contrato inteligente. Se você selecionou criar um contrato de Javascipt ou TypeScript, a extensão construirá um contrato inteligente básico usando o `fabric-contract-api` com uma série de funções que gerenciam seu ativo de exemplo. Por exemplo, se você selecionou ***bond***, você localizará as funções de `createBond`, `updateBond`, `readBond`, `bondExists` e `deleteBond`.
- Um arquivo de teste.
- As dependências do contrato inteligente que o acompanham.

## Etapa três: empacotar um contrato inteligente
{: #packaging-a-smart-contract}

É necessário empacotar um contrato inteligente no formato `.cds` antes de poder instalá-lo em sua rede do {{site.data.keyword.blockchainfull_notm}} Platform ou na rede pré-configurada do Hyperledger Fabric. Conclua as etapas a seguir para empacotar seu contrato inteligente:

1. No VS Code, navegue para o painel **{{site.data.keyword.blockchainfull_notm}} Platform**. Assegure-se de que você tenha um projeto de contrato inteligente aberto no visualizador de arquivo.
2. Na área de janela **Pacotes de contrato inteligente**, clique no menu overflow e selecione **Empacotar um projeto de contrato inteligente**. O nome do pacote e a versão serão solicitados.
  - Se você tiver um projeto de contrato inteligente, ele será empacotado automaticamente e será exibido na área de janela **Pacotes de contrato inteligente**.
  - Se você tiver várias pastas de contratos inteligentes abertas, será perguntado qual será empacotada.
  - Se você não tiver pastas de contrato inteligente abertas, receberá uma mensagem de erro.

### Exportando, importando e excluindo um pacote de contrato inteligente
{: #develop-vscode-exporting-deleting-smart-contract-package}

Depois de empacotar um projeto de contrato inteligente, é possível exportá-lo do VS Code:

1. No painel de extensão do {{site.data.keyword.blockchainfull_notm}} Platform, clique com o botão direito no pacote do contrato inteligente e selecione **Exportar pacote**.
2. Escolha o diretório para salvar seu arquivo de pacote de contrato inteligente e clique em **Exportar**.

Também é possível importar um pacote de contrato inteligente existente para a área de janela do {{site.data.keyword.blockchainfull_notm}} Platform:

1. Na área de janela **Pacotes de contrato inteligente**, clique no menu overflow e selecione **Importar pacote**.
2. Navegue para o pacote de contrato inteligente que você deseja importar e clique em **Importar**.

Também é possível clicar em **Excluir pacote** para remover o pacote de contrato inteligente da lista de pacotes.

## Etapa quatro: implementar um contrato inteligente em uma rede do Hyperledger Fabric pré-configurada
{: #develop-vscode-deploy}

É possível usar o VS Code para implementar seu contrato inteligente em uma rede do Hyperledger Fabric pré-configurada que a extensão cria em sua máquina local. Isso permite que você instale, instancie e teste seu contrato inteligente antes de implementá-lo em uma rede em tempo real.

### Implementando a rede do Hyperledger Fabric pré-configurada
{: #develop-vscode-connecting-and-disconnecting}

Use as etapas a seguir para implementar a rede pré-configurada:

1. Assegure-se de que o Docker esteja em execução em sua máquina.
2. Abra a guia **{{site.data.keyword.blockchainfull_notm}} Platform** em VS Code.
3. Na área de janela **Operações do Fabric local**, clique em **tempo de execução do Fabric local**. Se o Docker estiver em execução, a instância local do Hyperledger Fabric deverá ser transferida por download e iniciada.
4. Clique duas vezes em **local_fabric** na área de janela **Gateways do Fabric** para se conectar à rede local. Por padrão, a conexão usa a identidade de administrador na área de janela de carteiras eletrônicas do Fabric. É possível criar uma nova identidade clicando com o botão direito no nó de autoridade de certificação na área de janela **Operações do Fabric local**. Essa nova identidade pode, então, ser incluída em uma carteira eletrônica e ser associada à conexão **local_fabric**.

A extensão do VS Code cria uma rede básica do Fabric que inclui um solicitador, um peer e uma autoridade de certificação. O peer é associado a um canal denominado `mychannel`. É possível localizar a lista de nós, organizações e canais que pertencem à rede na área de janela **Operações do Fabric local**. Acima desses nós, é possível localizar a lista de contratos inteligentes que foram instalados e instanciados.

### Parando, reiniciando e removendo a rede pré-configurada
{: #develop-vscode-stop-Fabric-runtime}

É possível parar ou reiniciar a rede pré-configurada após ela ter sido criada:

1. Na área de janela **Operações do Fabric local**, clique no menu overflow.
2. Selecione **Reiniciar o tempo de execução do Fabric** ou **Parar o tempo de execução do Fabric** para parar ou reiniciar o contêiner.

Os detalhes da conexão são salvos em um diretório chamado **local_fabric** que está contido em seu diretório de projeto atual. Também é possível selecionar **Derrubar o tempo de execução do Fabric** para remover completamente a rede do Fabric local. **Nota:** essa remoção resultará na perda do livro-razão e dos dados de estado mundial.

### Implementando seu contrato inteligente na rede pré-configurada
{: #develop-vscode-deploy-smart-contract}

É possível implementar quaisquer pacotes na área de janela **Pacotes de contrato inteligente** em uma rede pré-configurada em execução.

Primeiro, é necessário instalar o contrato inteligente em um peer:

1. Na área de janela **Operações do Fabric local**, clique em **Instalar contrato inteligente**.
2. Selecione o peer no qual você deseja instalar o contrato inteligente.
3. Selecione o pacote de contrato inteligente que você deseja instalar e clique em **Instalar**.

Em seguida, é possível instanciar o contrato inteligente em um canal:

1. Na área de janela **Operações do Fabric local**, clique em **Instanciar contrato inteligente**.
2. Selecione o contrato inteligente instalado para instanciar.
3. (Opcional) Insira o nome da função de instanciação em seu contrato inteligente. Se você usou o modelo de contrato inteligente padrão, nenhuma função de instanciação será usada.
4. (Opcional) Insira quaisquer argumentos que sua função de instanciação requer.
5. (Opcional) Procure o arquivo de configuração de coleção se o seu contrato inteligente usar dados privados.
6. Clique em **Instanciar**.

Se você fizer mudanças em seu código de contrato inteligente e reempacotar, será possível fazer upgrade do contrato inteligente instanciado para implementar uma versão mais recente na rede:

1. Assegure-se de que o contrato inteligente do qual você deseja fazer upgrade tenha sido instanciado.
2. Instale a nova versão do contrato inteligente para um peer na mesma rede.
3. Clique com o botão direito no contrato inteligente instanciado e selecione **Fazer upgrade do contrato inteligente**.
4. (Opcional) Execute uma transação depois que o novo contrato inteligente for instanciado.

### Interagindo com seu contrato inteligente
{: #develop-vscode-submitting-transactions}

Depois que um contrato inteligente é instalado e instanciado, é possível enviar transações para as funções dentro de seu contrato inteligente usando a área de janela **Gateways do Fabric**:

1. Assegure-se de que seu contrato inteligente tenha sido instalado e instanciado e que você esteja conectado à rede.
2. Na área de janela **Gateways do Fabric**, expanda os **Contratos inteligentes instanciados**.
3. Expandir o contrato inteligente com o qual você deseja interagir. Você localizará a lista de transações que estão listadas abaixo de seu contrato inteligente.
4. Clique com o botão direito na transação a ser enviada e selecione **Enviar transação**. Por exemplo, se você criou e empacotou o contrato inteligente de títulos de exemplo, clique em **createBond**.
5. Insira quaisquer argumentos que a transação requer e pressione **Enter**. Por exemplo, insira `["bond01","100"]` para criar seu primeiro título.

### Conectando seus aplicativos à rede pré-configurada
{: #develop-vscode-exploring-connection-details}

É possível testar seus aplicativos clientes conectando-os à rede pré-configurada e enviando transações para o seu contrato inteligente.

Primeiro, é necessário exportar seu perfil de conexão:

1. Inicie a rede e expanda **Nós** na área de janela **Operações do Fabric local**.
2. Clique com o botão direito no peer e selecione **Exportar perfil de conexão**.

É possível, então, usar os SDKs do Fabric e o perfil de conexão para inscrever sua identidade de administrador usando o nome do usuário `admin` e a senha `adminpw`. É possível, então, usar essa identidade para chamar seu contrato inteligente ou registrar e inscrever usuários adicionais.

## Etapa cinco: depurar contratos inteligentes usando o modo de desenvolvimento
{: #develop-vscode-development-mode}

É possível executar a rede pré-configurada no **Modo de desenvolvimento** para desenvolver e depurar de forma iterativa seus contratos inteligentes localmente, sem precisar reempacotar e fazer upgrade do contrato inteligente após cada mudança. A depuração de um contrato inteligente permite que você execute as transações do contrato inteligente com pontos de interrupção e saída, assegurando que as transações funcionem como pretendido.

Use as etapas a seguir para ativar o modo de desenvolvimento na rede pré-configurada:

1. Depois que a rede for iniciada, na área de janela **Operações do Fabric local**, expanda a seção **Nós**.
2. Clique com o botão direito no peer e selecione **Alternar o modo de desenvolvimento**.

Em operação normal, um peer cria e mantém um contêiner de chaincode para executar contratos inteligentes instanciados. Alternando para o modo de desenvolvimento, o peer permite que o contêiner de chaincode seja executado manualmente, o que permite implementar atualizações em seu contrato inteligente diretamente em sua rede em execução usando a visualização **Depuração** do VS Code.

1. Assegure-se de que você esteja conectado à conexão **local_fabric** que está no modo de desenvolvimento.
2. Abra seu projeto de contrato inteligente.
3. Abra a visualização **Depuração** no VS Code por meio da barra de navegação à esquerda.
4. Selecione **Depurar configuração de contrato inteligente** na lista suspensa na parte superior esquerda.
5. Empacote e instale o contrato inteligente clicando no botão **reproduzir**.
6. Inclua pontos de interrupção no contrato inteligente clicando nos números de linha relevantes em seus arquivos de contrato inteligente.
7. Na barra de ferramentas de depuração, clique no botão **Blockchain** para instanciar o contrato inteligente.
8. Na barra de ferramentas de depuração, clique no botão **Blockchain** para enviar ou avaliar transações.

Para modificar seu contrato inteligente durante a depuração, clique no botão **reiniciar** depois de fazer mudanças em seu contrato inteligente. Reiniciar a depuração significa que não é necessário instanciar o contrato novamente.

## Etapa seis: testar um contrato inteligente instanciado
{: #develop-vscode-testing-instantiated-smart-contract}

É possível gerar testes para contratos inteligentes que são instanciados nas redes às quais você se conecta. Os testes podem ser gerados como **JavaScript** ou **TypeScript** e são executados ou depurados.

1. Assegure-se de que o contrato inteligente tenha sido instanciado.
2. Na área de janela **Gateways do Fabric**, clique com o botão direito no contrato inteligente na lista de canais para os quais gerar testes.
3. Selecione **Gerar testes de contrato inteligente**.
4. Selecione o idioma para o arquivo de teste, **JavaScript** ou **TypeScript**. A extensão do {{site.data.keyword.blockchainfull_notm}} Platform instalará os módulos npm necessários e construirá o arquivo de teste.

Depois que o arquivo de teste for construído, os testes poderão ser executados clicando no botão **Executar testes** no arquivo.


## Etapa sete: conectar-se à sua rede do {{site.data.keyword.blockchainfull_notm}} Platform
{: #develop-vscode-connecting-ibp}

Também é possível usar a extensão para se conectar ao seu {{site.data.keyword.blockchainfull_notm}} Platform que está em execução no {{site.data.keyword.cloud_notm}} ou no {{site.data.keyword.cloud_notm}} Private e chamar qualquer contrato inteligente que estejam instalado e instanciado usando a IU do console do {{site.data.keyword.blockchainfull_notm}} Platform.

Abra o console do {{site.data.keyword.blockchainfull_notm}} Platform que está associado à sua instância do {{site.data.keyword.blockchainfull_notm}} Platform. Navegue para a guia **Contratos inteligentes**. Use a tabela **Contratos inteligentes instanciados** na guia de contratos inteligentes para fazer download de seu [perfil de conexão](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-profile) em seu sistema de arquivos local. Em seguida, [crie uma identidade do aplicativo](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-identities) usando sua CA e salve o enrollID e o segredo. Siga as etapas abaixo para se conectar ao {{site.data.keyword.blockchainfull_notm}} Platform por meio do VS Code.

1. Abra a guia  **{{site.data.keyword.blockchainfull_notm}}  Plataforma ** .
2. Na área de janela **Gateways do Fabric**, clique em **+**.
3. Insira um nome para a conexão.
4. Insira o caminho de arquivo completo de seu perfil de conexão. Sua conexão deve agora aparecer na lista de conexões sob **local_fabric**.
5. Na área de janela **Carteiras eletrônicas do Fabric**, clique em **+**.
6. Escolha **Criar uma nova carteira eletrônica e incluir uma identidade** nas opções. Forneça um nome para sua carteira eletrônica e sua identidade.
7. Insira o ID do MSP de sua organização.
8. Selecione a opção **Selecionar um gateway e forneça um ID de inscrição e segredo** e escolha o gateway que você criou acima.
9. Insira o enrollID e o segredo da identidade do aplicativo que você criou com o console. Uma nova identidade será criada na área de janela **Carteiras eletrônicas do Fabric**.
10. Agora é possível se conectar à sua instância de sua rede do {{site.data.keyword.blockchainfull_notm}} Platform. Clique duas vezes no nome da conexão e selecione o nome da carteira eletrônica recém-criada. Também é possível associar a carteira eletrônica que você criou com o gateway clicando com o botão direito no gateway e selecionando **Associar uma carteira eletrônica**. Isso permite que a conexão use a mesma carteira eletrônica toda vez que se conectar.

Depois de se conectar ao {{site.data.keyword.blockchainfull_notm}} Platform por meio do VS Code, é possível ver a lista de canais aos quais seus peers de organização se associaram sob o gateway. Em cada canal, é possível ver a lista de contratos inteligentes que são instanciados em cada canal e as funções dentro de cada contrato inteligente. É possível enviar transações para sua rede clicando com o botão direito em uma função e selecionando **Enviar transação** e passando os argumentos necessários. Também é possível gerar um arquivo de teste para os contratos inteligentes que são instanciados em seus canais.

### Incluindo carteiras eletrônicas e usuários
{: #develop-vscode-add-a-wallet}

Use as etapas abaixo para criar uma nova carteira eletrônica usando um certificado e uma chave privada:

1. Na área de janela **Carteiras eletrônicas do Fabric**, clique em **+**.
2. Escolha **Criar uma nova carteira eletrônica e incluir uma identidade** nas opções. Forneça um nome para sua carteira eletrônica e sua identidade.
3. Insira o ID do MSP de sua organização.
4. Escolha para incluir um certificado e uma chave privada.
5. Se usar um certificado e uma chave privada, navegue para o certificado e para a chave privada.

Também é possível incluir novos usuários nas carteiras eletrônicas que já foram criadas:

1. Na área de janela **Carteira eletrônicas do Fabric**, clique com o botão direito em uma carteira eletrônica e selecione **Incluir identidade**.
2. Forneça um nome para a identidade e um MSPID.
3. Escolha usar um certificado e uma chave privada ou um ID de inscrição e um segredo.
4. Se usar um certificado e uma chave privada, navegue para o certificado e para a chave privada.
5. Se você usar um ID de inscrição e um segredo, escolha o gateway a ser inscrito e insira o ID e o segredo de inscrição.

### Editando, desconectando e excluindo conexões
{: #develop-vscode-editing-connection}

Na extensão, clique com o botão direito em uma conexão na parte inferior esquerda para abrir um menu contextual com as opções para incluir uma identidade, editar a conexão ou excluir a conexão.

Para editar uma conexão, conclua as etapas a seguir:
1. Selecione a opção **Editar conexão**. A página **Configurações do usuário** é aberta com os detalhes da conexão destacados.
2. Faça quaisquer mudanças necessárias e salve a página de configurações.

Quando você estiver pronto para desconectar da rede, clique no ícone **Desconectar** no canto superior direito da área de janela **Gateways do Fabric**.

Para excluir uma conexão, clique com o botão direito na conexão e selecione **Excluir gateway**.
