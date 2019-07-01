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

# Développement de contrats intelligents avec l'extension Visual Studio Code
{: #develop-vscode}


L'extension Visual Studio (VS) Code d'{{site.data.keyword.blockchainfull}} Platform fournit un environnement au sein de Visual Studio Code pour le développement, le packaging, et les tests de contrats intelligents. Vous pouvez utiliser l'extension pour créer votre projet de contrat intelligent et commencer à développer votre logique métier. Vous pouvez ensuite utiliser VS Code pour tester votre contrat intelligent sur votre machine locale en utilisant une instance préconfigurée d'Hyperledger Fabric avant de déployer le contrat intelligent dans {{site.data.keyword.blockchainfull_notm}} Platform. Ce tutoriel décrit l'utilisation de l'extension VS Code.

![Flux de travail de développement de contrat intelligent classique](images/SmartContractflow.png "Flux de travail de développement de contrat intelligent classique")  

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

Cette extension d'{{site.data.keyword.blockchainfull_notm}} Platform fonctionne en toute transparence avec une instance d'{{site.data.keyword.blockchainfull_notm}} Platform qui utilise Hyperledger Fabric versions 1.4 et suivantes.
{: note}

## Etape 1 : Installer l'extension VS Code d'{{site.data.keyword.blockchainfull_notm}} Platform sans frais
{: #develop-vscode-install}

Avant d'installer l'extension VS Code d'{{site.data.keyword.blockchainfull_notm}} Platform, vous devez effectuer les étapes prérequises.

### Prérequis
{: #develop-vscode-prerequisites}

- Windows 10, Linux ou Mac OS sont les systèmes d'exploitation actuellement pris en charge.
- [VS Code version 1.32 ou suivante](https://code.visualstudio.com/){: external}.
- [Node version 8.x ou suivante et npm version 5.x ou suivante](https://nodejs.org/en/download/){: external}.
- [Docker version 17.06.2-ce ou suivante](https://www.docker.com/get-started){: external}.
- [Docker Compose version 1.14.0 ou suivante](https://docs.docker.com/compose/install/){: external}.
- [Go version 1.12 ou suivante pour le développement de contrats Go](https://golang.org/dl/){: external}.

Si vous utilisez Windows, vous devez également vérifier les éléments suivants :

- Docker for Windows est configuré pour l'utilisation de conteneurs Linux (par défaut).
- Vous avez installé C++ Build Tools for Windows depuis [windows-build-tools](https://github.com/felixrieseberg/windows-build-tools#windows-build-tools){: external}.
- Vous avez installé OpenSSL v1.0.2 depuis [Win32 OpenSSL](http://slproweb.com/products/Win32OpenSSL.html){: external}.
  - Installez la version normale, et non la version marquée "light".
  - Installez la version Win32 dans C:\OpenSSL-Win32 sur les systèmes 32 bits.
  - Installez la version Win64 dans C:\OpenSSL-Win64 sur les systèmes 64 bits.

### Installation de l'extension
{: #develop-vscode-installing-the-extension}

1. Accédez à la [page de place du marché de l'extension de code Visual Studio](https://marketplace.visualstudio.com/items?itemName=IBMBlockchain.ibm-blockchain-platform){: external} ou recherchez **{{site.data.keyword.blockchainfull_notm}} Platform** dans l'écran des extensions au sein de Visual Studio Code.
2. Cliquez sur **Installer**.
3. Redémarrez Visual Studio Code pour terminer l'installation de l'extension.

A l'issue de l'installation, vous pouvez utiliser l'icône {{site.data.keyword.blockchainfull_notm}} sur le côté gauche du code VS pour ouvrir le panneau d'{{site.data.keyword.blockchainfull_notm}} Platform.

![Icône {{site.data.keyword.blockchainfull_notm}}](images/vscode-blockchain.png "Icône {{site.data.keyword.blockchainfull_notm}}")

L'extension ajoute également de nombreuses commandes à la palette de commandes Visual Studio Code. Vous pouvez utiliser la palette de commandes pour effectuer la plupart des opérations qui sont décrites en détail dans ce guide.

## Etape 2 : Créer un contrat intelligent
{: #develop-vscode-creating-a-project}

Vous pouvez utiliser l'extension pour créer un projet de contrat intelligent dans Visual Studio Code. L'extension crée un contrat intelligent de base qui gère un exemple d'actif dans le langage de votre choix. Vous pouvez utiliser la structure d'exemple comme point de départ pour le développement de votre propre logique métier. L'extension fournit toutes les dépendances qui sont requises pour déployer votre contrat intelligent dans une instance d'Hyperledger Fabric.

1. Ouvrez l'onglet **{{site.data.keyword.blockchainfull_notm}}**. Cliquez sur le menu déroulant dynamique dans le panneau de packages de contrat intelligent et cliquez sur **Créer un projet de contrat intelligent**.
2. Sélectionnez le langage dans lequel vous voulez créer un contrat intelligent. Les options actuelles sont : JavaScript, TypeScript, Go et Java. **Remarque : ** {{site.data.keyword.blockchainfull_notm}} Platform ne prend pas en charge le code blockchain Java.
3. **Si vous avez sélectionné JavaScript ou TypeScript**, choisissez un actif qui doit être géré par l'exemple de contrat. Par exemple, ***bond***.
4. Créez un dossier portant le nom de votre projet, et ouvrez-le.
5. Sélectionnez comment ouvrir votre nouveau projet. Le dossier de projet doit s'ouvrir.

Lorsque le projet s'ouvre, vous pouvez trouver le nouveau contrat intelligent dans la fenêtre de l'explorateur dans le panneau de gauche. La structure du projet dépend du langage que vous avez sélectionné. Toutefois, chaque contrat intelligent contient les mêmes éléments :
- Le code source du contrat intelligent. Si vous avez choisi de créer un contrat Javascipt or TypeScript, l'extension génère un contrat intelligent de base à l'aide de l'API `fabric-contract-api` avec une série de fonctions qui gèrent votre exemple d'actif. Par exemple, si vous avez sélectionné ***bond***, vous trouverez les fonctions de `createBond`, `updateBond`, `readBond`, `bondExists` et `deleteBond`.
- Un fichier test.
- Les dépendances qui accompagnent le contrat intelligent.

## Etape 3 : Packager un contrat intelligent
{: #packaging-a-smart-contract}

Vous devez packager un contrat intelligent au format `.cds` avant de l'installer sur votre réseau {{site.data.keyword.blockchainfull_notm}} Platform ou le réseau Hyperledger Fabric préconfiguré. Procédez comme suit pour packager votre contrat intelligent :

1. Dans VS Code, accédez au panneau **{{site.data.keyword.blockchainfull_notm}} Platform**. Assurez-vous qu'un projet de contrat intelligent est ouvert dans l'afficheur de fichiers.
2. Dans le panneau **Packages contrat intelligent**, cliquez sur le menu déroulant dynamique et sélectionnez **Package a Smart Contract Project**. Vous serez invité à entrer le nom du package ainsi que la version.
  - Si vous avez un projet de contrat intelligent, il sera automatiquement packagé et affiché dans le panneau **Packages de contrat intelligent**.
  - Si vous avez plusieurs dossiers contrat intelligent ouverts, vous serez invité à préciser un package.
  - Si vous n'avez pas de dossiers de contrat intelligent ouverts, vous recevrez un message d'erreur.

### Exportation, importation et suppression d'un package de contrat intelligent
{: #develop-vscode-exporting-deleting-smart-contract-package}

Une fois le package de contrat intelligent packagé, vous pouvez l'exporter depuis VS Code :

1. Dans le panneau d'extension d'{{site.data.keyword.blockchainfull_notm}} Platform, cliquez avec le bouton droit de la souris sur le package de contrat intelligent et sélectionnez **Exporter le package**.
2. Choisissez le répertoire pour la sauvegarde de votre fichier de package de contrat intelligent et cliquez sur **Exporter**.

Vous pouvez également importer un package de contrat intelligent dans le panneau {{site.data.keyword.blockchainfull_notm}} Platform :

1. Dans le panneau **Packages contrat intelligent**, cliquez sur le menu déroulant dynamique et sélectionnez **Importer un package**.
2. Accédez au package de contrat intelligent que vous voulez importer, puis cliquez sur **Importer**.

Vous pouvez également cliquer sur **Supprimer le package** pour retirer le package de contrat intelligent de la liste des packages.

## Etape 4 : Déployer un contrat intelligent sur un réseau Hyperledger Fabric préconfiguré
{: #develop-vscode-deploy}

Vous pouvez utiliser VS Code pour déployer votre contrat intelligent sur un réseau Hyperledger Fabric préconfiguré créé par l'extension sur votre machine locale. Cela vous permet d'installer, d'instancier et de tester votre contrat intelligent avant de le déployer sur un réseau actif.

### Déploiement du réseau Hyperledger Fabric préconfiguré
{: #develop-vscode-connecting-and-disconnecting}

Procédez comme suit pour déployer le réseau préconfiguré :

1. Assurez-vous que Docker est en cours d'exécution sur votre machine.
2. Ouvrez l'onglet **{{site.data.keyword.blockchainfull_notm}} Platform** dans VS Code.
3. Dans le panneau **Local Fabric Ops**, cliquez sur **local fabric runtime**. Si Docker est en cours d'exécution, l'instance Hyperledger Fabric locale doit être téléchargée et démarrée.
4. Cliquez deux fois sur **local_fabric** dans le panneau **Fabric Gateways** pour vous connecter au réseau local. Par défaut, la connexion utilise l'identité admin dans le panneau des portefeuilles Fabric. Vous pouvez créer une nouvelle identité en cliquant avec le bouton droit de la souris sur le noeud d'autorité de certification dans le panneau **Local Fabric Ops**. Cette nouvelle identité peut ensuite être ajoutée à un portefeuille et associée à la connexion **local_fabric**.

L'extension VS Code crée un réseau Fabric de base qui comporte un service de tri, un homologue et une autorité de certification. L'homologue est joint à un canal nommé `mychannel`. Vous pouvez trouver la liste des noeuds, organisations et canaux qui appartiennent au réseau dans le panneau **Local Fabric Ops**. Au-dessus de ces noeuds figure la liste des contrats intelligents qui sont installés et instanciés.

### Arrêt, redémarrage et suppression du réseau préconfiguré
{: #develop-vscode-stop-Fabric-runtime}

Vous pouvez arrêter ou redémarrer le réseau préconfigurés une fois qu'il a été créé :

1. Dans le panneau **Local Fabric Ops**, cliquez sur le menu déroulant dynamique.
2. Sélectionnez **Redémarrer l'exécution Fabric** ou **Arrêter l'exécution Fabric** pour arrêter ou redémarrer le conteneur.

Les détails de connexion sont sauvegardés dans un répertoire nommé **local_fabric** qui se trouve dans votre répertoire de projet en cours. Vous pouvez également sélectionner **Désassembler l'exécution Fabric** pour supprimer complètement le réseau Fabric local. **Remarque :** Cela entraînera la perte du registre et des données "world state".

### Déploiement de votre contrat intelligent sur le réseau préconfiguré
{: #develop-vscode-deploy-smart-contract}

Vous pouvez déployer des packages du panneau **Packages de contrat intelligent** sur un réseau préconfiguré actif.

Tout d'abord, vous devez installer le contrat intelligent sur un homologue :

1. Dans le panneau **Local Fabric Ops**, cliquez sur **Installer un contrat intelligent**.
2. Sélectionnez l'homologue sur lequel vous souhaitez installer le contrat intelligent.
3. Sélectionnez le package de contrat intelligent que vous souhaitez installer, puis cliquez sur **Installer**.

Ensuite, vous pouvez instancier le contrat intelligent sur un canal :

1. Dans le panneau **Local Fabric Ops**, cliquez sur **Instantiate Smart Contract**.
2. Sélectionnez le contrat intelligent installé à instancier.
3. (facultatif) Entrez le nom de la fonction d'instanciation dans votre contrat intelligent. Si vous avez utilisé le modèle de contrat par défaut, aucune fonction d'instanciation n'est utilisée.
4. (facultatif) Entrez les arguments requis par votre fonction d'instanciation.
5. (facultatif) Accédez au fichier de configuration de votre collection si votre contrat intelligent utilise des données privées.
6. Cliquez sur **Instancier**.

Si vous apportez des modifications à votre code de contrat intelligent et le repackagez, vous pouvez mettre à niveau le contrat intelligent instancié afin de déployer une version plus récente sur le réseau :

1. Vérifiez que le contrat intelligent à mettre à jour est instancié.
2. Installez la nouvelle version du contrat intelligent sur un homologue du même réseau.
3. Cliquez avec le bouton droit de la souris sur le contrat intelligent instancié, et sélectionnez **Mettre à niveau le contrat intelligent**.
4. (facultatif) Exécutez une transaction une fois le nouveau contrat intelligent instancié.

### Interaction avec votre contrat intelligent
{: #develop-vscode-submitting-transactions}

Une fois qu'un contrat intelligent est installé et instancié, vous pouvez soumettre des transactions aux fonctions à l'intérieur de votre contrat intelligent à partir du panneau **Passerelles Fabric** :

1. Vérifiez que votre contrat intelligent est installé et instancié, et que vous êtes connecté au réseau.
2. Dans le volet **Fabric Gateways**, développez le **contrats intelligents instanciés**.
3. Développez le contrat intelligent avec lequel vous voulez interagir. Vous trouverez la liste des transactions qui sont répertoriées sous le contrat intelligent.
4. Cliquez avec le bouton droit de la souris sur la transaction à soumettre, puis sélectionnez **Soumettre la transaction**. Par exemple, si vous avez créé et packagé l'exemple de contrat intelligent d'obligations (bonds), cliquez sur **createBond**.
5. Entrez les arguments requis par la transaction, puis appuyez sur **Entrée**. Par exemple, entrez `["bond01","100"]` pour créer votre première obligation.

### Connexion de vos applications au réseau préconfiguré
{: #develop-vscode-exploring-connection-details}

Vous pouvez tester vos applications client en les connectant au réseau préconfiguré et en soumettant les transactions à votre contrat intelligent.

Tout d'abord, vous devez exporter votre profil de connexion :

1. Démarrez le réseau et développez **Noeuds** dans le panneau **Local Fabric Ops**.
2. Cliquez avec le bouton droit sur l'homologue et sélectionnez **Exporter un profil de connexion**.

Vous pouvez ensuite utiliser les logiciels SDK Fabric et le profil de connexion pour inscrire votre identité admin en utilisant le nom d'utilisateur `admin` et le mot de passe `adminpw`. Vous pouvez ensuite utiliser cette identité pour appeler votre contrat intelligent ou enregistrer et inscrire des utilisateurs supplémentaires.

## Etape 5 : Déboguer des contrats intelligents en mode développement
{: #develop-vscode-development-mode}

Vous pouvez exécuter le réseau préconfiguré en **Mode développement** pour développer et déboguer de manière itérative vos contrats intelligents en local, sans avoir à repackager et à mettre à niveau le contrat intelligent après chaque modification. Le débogage d'un contrat intelligent vous permet d'exécuter des transactions de contrat intelligent avec des points d'arrêt et une sortie, garantissant ainsi un bon fonctionnement des transactions.

Procédez comme suit pour activer le mode développement sur le réseau préconfiguré :

1. Une fois le réseau démarré, depuis le panneau **Local Fabric Ops**, développez la section **Noeuds**.
2. Cliquez avec le bouton droit sur l'homologue et sélectionnez **Basculer en mode développement**.

En mode de fonctionnement normal, un homologue crée et gère un conteneur de code blockchain pour exécuter des contrat intelligent instanciés. En passant au mode développement, l'homologue permet au conteneur de code blockchain de s'exécuter manuellement, ce qui vous permet de développer des mises à jour pour votre contrat intelligent directement sur votre réseau actif network à partir de la vue **Debug** de VS Code.

1. Vérifiez que vous êtes connecté à la connexion **local_fabric** qui est en mode développement.
2. Ouvrez votre projet de contrat intelligent.
3. Ouvrez la vue **Debug** dans VS Code dans la barre de navigation de gauche.
4. Sélectionnez **Debug Smart Contract configuration** dans la liste déroulante dans l'angle supérieur gauche.
5. Préparez et installez le contrat intelligent en cliquant sur le bouton **play**.
6. Ajoutez des points d'arrêt au contrat intelligent en cliquant sur les numéros de ligne appropriés dans fichiers de contrat intelligent.
7. Dans la barre d'outils de débogage, cliquez sur le bouton **Blockchain** pour instancier le contrat intelligent.
8. Dans la barre d'outils de débogage, cliquez sur le bouton **Blockchain** pour soumettre ou évaluer des transactions.

Pour modifier votre contrat intelligent lors du débogage, cliquez sur le bouton **Redémarrer** une fois les modifications effectuées. Redémarrer le débogage signifie que vous n'avez pas besoin d'instancier à nouveau le contrat intelligent.

## Etape 6 : Tester un contrat intelligent instancié
{: #develop-vscode-testing-instantiated-smart-contract}

Vous pouvez générer des tests pour les contrats intelligents qui sont instanciés sur les réseaux auxquels vous vous connectez. Ils peuvent être générés en **JavaScript** ou **TypeScript**, et exécutés ou débogués.

1. Assurez-vous que le contrat intelligent a été instancié.
2. Dans le volet **Fabric Gateways**, cliquez avec le bouton droit de la souris sur le contrat intelligent sous la liste de canaux pour lesquels générer des tests.
3. Sélectionnez **Generate Smart Contract Tests**.
4. Sélectionnez le langage du fichier de test, **JavaScript** ou **TypeScript**. L'extension {{site.data.keyword.blockchainfull_notm}} Platform va installer les modules npm requis et générer le fichier de test.

Une fois le fichier de test généré, les tests peuvent être exécutés en cliquant sur le bouton **Exécuter des tests** dans le fichier.


## Etape 7 : Se connecter à votre réseau {{site.data.keyword.blockchainfull_notm}} Platform
{: #develop-vscode-connecting-ibp}

Vous pouvez également utiliser l'extension pour vous connecter à {{site.data.keyword.blockchainfull_notm}} Platform qui s'exécute sur {{site.data.keyword.cloud_notm}} ou {{site.data.keyword.cloud_notm}} Private et appeler des contrats intelligents qui sont installés et instanciés à l'aide de l'interface utilisateur de la console {{site.data.keyword.blockchainfull_notm}} Platform.

Ouvrez la console {{site.data.keyword.blockchainfull_notm}} Platform qui est associée à votre instance de {{site.data.keyword.blockchainfull_notm}} Platform. Accédez à l'onglet **Smart Contracts**. Utilisez le tableau **Instantiated Smart Contracts** sous l'onglet Smart Contracts pour télécharger votre [profil de connexion](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-profile) sur votre système de fichiers local. Ensuite, [créez une identité d'application](/docs/services/blockchain/howto?topic=blockchain-ibp-console-app#ibp-console-app-identities) à l'aide de votre autorité de certification et notez l'ID d'inscription et le secret. Suivez les étapes ci-dessous pour vous connecter à {{site.data.keyword.blockchainfull_notm}} Platform depuis VS Code.

1. Accédez à l'onglet **{{site.data.keyword.blockchainfull_notm}} Platform**.
2. Dans le panneau **Fabric Gateways**, cliquez sur **+**.
3. Entrez un nom pour la connexion.
4. Entrez le chemin d'accès complet à votre profil de connexion. Votre connexion doit maintenant apparaître dans la liste des connexions sous **local_fabric**.
5. Dans le panneau **Fabric Wallets**, cliquez sur **+**.
6. Choisissez **Create a new wallet and add an identity** dans les options. Entrez un nom pour votre portefeuille et votre identité.
7. Entrez l'ID MSP de votre organisation.
8. Sélectionnez l'option **Select a gateway and provide an enrollment ID and secret** et choisissez la passerelle que vous avez créée plus haut.
9. Entrez l'ID d'inscription et le secret de l'identité d'application que vous avez créés avec la console. Une nouvelle identité est créée dans le panneau **Fabric Wallets**.
10. Vous pouvez maintenant vous connecter à instance de votre réseau {{site.data.keyword.blockchainfull_notm}} Platform. Cliquez deux fois sur le nom de connexion et sélectionnez le nom du portefeuille que vous venez de créer. Vous pouvez également associer le portefeuille que vous avez créé à la passerelle en cliquant avec le bouton droit et en sélectionnant **Associate A Wallet**. Cela permet à la connexion d'utiliser le même portefeuille chaque fois qu'une connexion est établie.

Après vous être connecté à {{site.data.keyword.blockchainfull_notm}} Platform depuis VS Code, vous pouvez voir la liste des canaux que les homologues de votre organisation ont rejoint sous la passerelle. Sous chaque canal, vous pouvez voir la liste des contrats intelligents qui sont instanciés sur chaque canal et les fonctions au sein de chaque contrat intelligent. Vous pouvez soumettre des transactions à votre réseau en cliquant avec le bouton droit et en sélectionnant **Submit Transaction** et en transmettant les arguments requis. Vous pouvez également générer un fichier test pour les contrats intelligents qui sont instanciés sur vos canaux.

### Ajout de portefeuilles et d'utilisateurs
{: #develop-vscode-add-a-wallet}

Procédez comme suit pour créer un nouveau portefeuille à l'aide d'un certificat et d'une clé privée :

1. Dans le panneau **Fabric Wallets**, cliquez sur **+**.
2. Choisissez **Create a new wallet and add an identity** dans les options. Entrez un nom pour votre portefeuille et votre identité.
3. Entrez l'ID MSP de votre organisation.
4. Choisissez d'ajouter un certificat et une clé privée.
5. Si vous utilisez un certificat et une clé privée, recherchez le certificat et la clé privée.

Vous pouvez également ajouter de nouveaux utilisateurs aux portefeuilles qui ont déjà été créés :

1. Dans le panneau **Fabric Wallets**, cliquez avec le bouton droit de la souris sur un portefeuille et sélectionnez **Add Identity**.
2. Indiquez un nom pour l'identité et un MSPID.
3. Choisissez d'utiliser un certificat et une clé privée ou un ID d'inscription et un secret.
4. Si vous utilisez un certificat et une clé privée, recherchez le certificat et la clé privée.
5. Si vous utilisez un ID d'inscription et un secret, choisissez la passerelle sur laquelle vous enregistrer et entrez l'ID d'inscription et le secret.

### Modification, déconnexion, et suppression de connexions
{: #develop-vscode-editing-connection}

À partir de l'extension, cliquez avec le bouton droit sur une connexion dans l'angle inférieur gauche pour ouvrir un menu contextuel avec des options pour ajouter une identité, éditer la connexion ou supprimer la connexion.

Pour modifier une connexion, procédez comme suit :
1. Sélectionnez l'option **Edit connection**. La page **User Settings** s'affiche, avec les détails de connexion mis en évidence.
2. Apportez des modifications, puis sauvegardez la page des paramètres.

Lorsque vous êtes prêt à vous déconnecter du réseau, cliquez sur l'icône **Disconnect** dans la partie supérieure droite du panneau **Fabric Gateways**.

Pour supprimer une connexion, cliquez avec le bouton droit sur la connexion et sélectionnez **Delete Gateway**.
