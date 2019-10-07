---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-26"

keywords: IBM Blockchain Platform, IBM Cloud Private, Data residency, world state

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

# Get started with the {{site.data.keyword.IBM_notm}} Certified Images for Hyperledger Fabric
{: #certified-images}

{{site.data.keyword.blockchainfull}} provides {{site.data.keyword.IBM_notm}} signed and certified Hyperledger Fabric Docker images with industry leading support. The images are based on Hyperledger Fabric and contain a number of enhancements for stability and serviceability. This offering provides support for multiple versions of Hyperledger Fabric and has been tested to run across multiple hardware platforms.

The {{site.data.keyword.IBM_notm}} certified images offering is bundled with support from {{site.data.keyword.blockchainfull_notm}}. {{site.data.keyword.blockchainfull_notm}} only offers support for blockchain components that are deployed using the {{site.data.keyword.IBM_notm}} certified images and not for components deployed using the open source Hyperledger Fabric Docker images. Components that are deployed using the {{site.data.keyword.IBM_notm}} certified images are interoperable with nodes that are deployed using the {{site.data.keyword.blockchainfull_notm}} platform.

## Supported Platforms

The {{site.data.keyword.IBM_notm}} Certified Images for Hyperledger Fabric have been tested for functionality, stability, and performance on the following hardware platforms:

- LinuxONE and {{site.data.keyword.IBM_notm}} Z (s390)
- Power (ppc64le)
- x86

The images do not have to be deployed using Kubernetes or Red Hat OpenShift.

## Supported Fabric versions

## Considerations and limitations

{{site.data.keyword.IBM_notm}} certified images are based on the open source [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-1.4/){: external}. The images do not include the {{site.data.keyword.blockchainfull_notm}} Platform console or other tooling provided by the {{site.data.keyword.blockchainfull_notm}} Platform.

- This offering is meant for experienced Fabric users. If you are still exploring Hyperledger Fabric, you can get started with [{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-v2-deploy-iks#ibp-v2-deploy-iks).
- You cannot use the {{site.data.keyword.blockchainfull_notm}} Platform console to deploy or operate the certified Fabric images.
- {{site.data.keyword.blockchainfull_notm}} provides support for Hyperledger Fabric only if you purchase the {{site.data.keyword.IBM_notm}} certified images. You cannot purchase support for the open source Fabric Docker images. {{site.data.keyword.blockchainfull_notm}} will not support images that have been altered.

If you want help building a solution based on the {{site.data.keyword.IBM_notm}} certified images using the open source Hyperledger tooling, we recommend that you engage [{{site.data.keyword.blockchainfull_notm}} Services](https://www.ibm.com/blockchain/services){: external}.

## License and Pricing

The {{site.data.keyword.IBM_notm}} Certified Images for Hyperledger Fabric must be purchased through [Passport Advantage Online](https://www.ibm.com/software/passportadvantage/pao_customer.html){: external}. After purchase, the images can be downloaded from the {{site.data.keyword.IBM_notm}} Entitled Registry.

## Installing

The {{site.data.keyword.IBM_notm}} Certified Images for Hyperledger Fabric can be downloaded from the {{site.data.keyword.IBM_notm}} Entitled Registry. The offering consists of eight images that need to be downloaded to build a complete network:

- `peer`
- `orderer`
- `ca`
- `couchdb`
- `ccenv`
- `baseos`
- `baseimage`
- `utils`

Each image needs to be pulled separately.

## Getting started

To get started using the Fabric images, you need to download the open source tooling provided by the Hyperledger community. Follow the steps to [download the Fabric Samples, Binaries, and configuration files](https://hyperledger-fabric.readthedocs.io/en/release-1.4/install.html){: external} in the Hyperledger Fabric documentation. These steps may download the open source Fabric images as well.

After you have downloaded the Fabric Samples, you will be able to find the configuration files and binaries that you that you need to set up a network in the `fabric-samples\config` and `fabric-samples\bin` folders. You can also find example artifacts and scripts for setting up a network using Docker Compose in the `fabric-samples\first-network` directory. You learn more about these artifacts and steps involved by visiting the accompanying [Build Your First Network](http://hyperledger-fabric.readthedocs.io/en/release-1.4/build_network.html){: external} tutorial.

When using the open source configuration files, you need to alter the files to use the {{site.data.keyword.IBM_notm}} images instead of the open source Fabric Docker images. For each component, you need to alter the `image` section of the file to use the {{site.data.keyword.IBM_notm}} certified image instead of the open source Fabric Docker image. You also need add the `LICENSE` field to accept the {{site.data.keyword.IBM_notm}} license. If you are deploying a peer node, you need to use the core chaincode variables to instruct the peer to build chaincode using the {{site.data.keyword.IBM_notm}} certified images.

As an example, you can use the `peer-base.yaml` file in `fabric-samples/first-network` to deploy an ordering node or a peer node. You can find the orderer section of the file below:

```
orderer-base:
  image: hyperledger/fabric-orderer:$IMAGE_TAG
  environment:
    - FABRIC_LOGGING_SPEC=INFO
    - ORDERER_GENERAL_LISTENADDRESS=0.0.0.0
    - ORDERER_GENERAL_GENESISMETHOD=file
    - ORDERER_GENERAL_GENESISFILE=/var/hyperledger/orderer/orderer.genesis.block
    - ORDERER_GENERAL_LOCALMSPID=OrdererMSP
    - ORDERER_GENERAL_LOCALMSPDIR=/var/hyperledger/orderer/msp
    # enabled TLS
    - ORDERER_GENERAL_TLS_ENABLED=true
    - ORDERER_GENERAL_TLS_PRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_TLS_CERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_TLS_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
    - ORDERER_KAFKA_TOPIC_REPLICATIONFACTOR=1
    - ORDERER_KAFKA_VERBOSE=true
    - ORDERER_GENERAL_CLUSTER_CLIENTCERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_CLUSTER_CLIENTPRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_CLUSTER_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
  working_dir: /opt/gopath/src/github.com/hyperledger/fabric
  command: orderer
```

To use deploy an ordering node using the {{site.data.keyword.IBM_notm}} image, change the `image` field to `ibmblockchain/fabric-orderer:TAG`. Specify the tag of the release that you are using and drop the architecture (x86_64, ppc64le, s390x). Accept the license by adding a field of `LICENSE=accept`. After your changes, the section would look like the example below:

```
orderer-base:
  image: ibmblockchain/fabric-orderer:1.0.0-rc1
  environment:
    - LICENSE=accept
    - FABRIC_LOGGING_SPEC=INFO
    - ORDERER_GENERAL_LISTENADDRESS=0.0.0.0
    - ORDERER_GENERAL_GENESISMETHOD=file
    - ORDERER_GENERAL_GENESISFILE=/var/hyperledger/orderer/orderer.genesis.block
    - ORDERER_GENERAL_LOCALMSPID=OrdererMSP
    - ORDERER_GENERAL_LOCALMSPDIR=/var/hyperledger/orderer/msp
    # enabled TLS
    - ORDERER_GENERAL_TLS_ENABLED=true
    - ORDERER_GENERAL_TLS_PRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_TLS_CERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_TLS_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
    - ORDERER_KAFKA_TOPIC_REPLICATIONFACTOR=1
    - ORDERER_KAFKA_VERBOSE=true
    - ORDERER_GENERAL_CLUSTER_CLIENTCERTIFICATE=/var/hyperledger/orderer/tls/server.crt
    - ORDERER_GENERAL_CLUSTER_CLIENTPRIVATEKEY=/var/hyperledger/orderer/tls/server.key
    - ORDERER_GENERAL_CLUSTER_ROOTCAS=[/var/hyperledger/orderer/tls/ca.crt]
  working_dir: /opt/gopath/src/github.com/hyperledger/fabric
  command: orderer
```

If you are using `peer-base.yaml` deploy a peer, make the same changes to the peer section of the file that you made to the orderer section. However, you also need add the core chaincode variables and specify the `fabric-ccenv`, `fabric-baseos`, `fabric-baseimage` images that you downloaded from {{site.data.keyword.IBM_notm}}. As an example, the peer section of `peer-base.yaml` would become:

```
services:
  peer-base:
    image: ibmblockchain/fabric-peer:1.0.0-rc1
    environment:
      - LICENSE=accept
      - CORE_CHAINCODE_BUILDER=ibmblockchain/fabric-ccenv:1.0.0-rc1
      - CORE_CHAINCODE_GOLANG_RUNTIME=ibmblockchain/fabric-baseos:1.0.0-rc1
      - CORE_CHAINCODE_NODE_RUNTIME=ibmblockchain/fabric-baseimage:1.0.0-rc1
      - CORE_VM_ENDPOINT=unix:///host/var/run/docker.sock
      # the following setting starts chaincode containers on the same
      # bridge network as the peers
      # https://docs.docker.com/compose/networking/
      - CORE_VM_DOCKER_HOSTCONFIG_NETWORKMODE=${COMPOSE_PROJECT_NAME}_byfn
      - FABRIC_LOGGING_SPEC=INFO
      #- FABRIC_LOGGING_SPEC=DEBUG
      - CORE_PEER_TLS_ENABLED=true
      - CORE_PEER_GOSSIP_USELEADERELECTION=true
      - CORE_PEER_GOSSIP_ORGLEADER=false
      - CORE_PEER_PROFILE_ENABLED=true
      - CORE_PEER_TLS_CERT_FILE=/etc/hyperledger/fabric/tls/server.crt
      - CORE_PEER_TLS_KEY_FILE=/etc/hyperledger/fabric/tls/server.key
      - CORE_PEER_TLS_ROOTCERT_FILE=/etc/hyperledger/fabric/tls/ca.crt
    working_dir: /opt/gopath/src/github.com/hyperledger/fabric/peer
    command: peer node start
```

The Fabric Samples published by the Hyperledger community are intended to be used only as examples. You should not use these samples as templates for deploying production networks.

## Getting support

You can take advantage of free blockchain developer resources and support forums in order to troubleshoot problems and get help from {{site.data.keyword.IBM_notm}} and the Fabric community. For more information, see [Resources and support forums](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support-resources).

If you have purchased the {{site.data.keyword.IBM_notm}} certified images and you want to contact customer support, open a support ticket using the {{site.data.keyword.cloud_notm}} Service Portal. For more information, see [submitting support cases](/docs/services/blockchain?topic=blockchain-blockchain-support#blockchain-support-cases).
