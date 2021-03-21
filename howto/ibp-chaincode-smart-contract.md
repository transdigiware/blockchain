---

copyright:
  years: 2021, 2021
lastupdated: "2021-03-21"

keywords: smart contract, chaincode, Dockerfile, playbook, docker image, Hyperledger Fabric

subcollection: blockchain

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}




# Running the {{site.data.keyword.blockchainfull_notm}} Platform with self-managed Kubernetes smart contracts
{: #ibp-smart-contracts-k8s-self-managed}

This tutorial shows how to use Fabric chaincode containers to run smart contracts in your own Kubernetes namespace. This feature of {{site.data.keyword.blockchainfull_notm}} Platform is made by the underlying [Hyperledger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-2.2/whatis.html) - [External Chaincode](https://hyperledger-fabric.readthedocs.io/en/release-2.2/cc_service.html) and Chaincode-as-a-server features.
{: shortdesc}

Objectives:
- Set up an Ansible based environment to use for configuration
- Build a NodeJS smart contract in a Docker image and push the image to the repository
- Using the {{site.data.keyword.blockchainfull_notm}} Platform Ansible collection, provision a basic network of Peers and Orderers
- Provision the Kubernetes resources needed to run the Docker image
- Create the secrets needed to retrieve the image
- Deployment of the actual chaincode
- Testing it

## Before you begin
{: #ibp-smart-contracts-k8s-self-managed-before}

This tutorial assumes you have already installed {{site.data.keyword.blockchainfull_notm}} Platform with a free developed Kubernetes cluster. Free access to the Kubernetes cluster valid for 28 days. As an alternative, you can choose an environment that is similar to the Kubernetes cluster setup.

### Python
{: #ibp-smart-contracts-k8s-self-managed-before-python}

This tutorial requires Ansible, which depends on Python. To simplify the configuration of Python, you can use pipenv. For example, run the following commands:

```bash
pipenv --python 3.8
pipenv install fabric-sdk-py 
pipenv install 'openshift==0.11.2'
pipenv install jq
ansible-galaxy collection install ibm.blockchain_platform community.kubernetes ibmcloud.collection
# add --force to get upgrade to new versions of these collections
# note used to use moreati.jq but believed not to be required
```
{: codeblock}

Finally, run `pipenv shell` to get into a shell that has the required python configuration.

### Login to {{site.data.keyword.cloud_notm}}
{: #ibp-smart-contracts-k8s-self-managed-before-login}

If {{site.data.keyword.cloud_notm}} CLI and the plug-ins for the Container Registry and Kubernetes Service is not yet installed, refer to the installation instructions as follows:

- [{{site.data.keyword.cloud_notm}} CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started) and 
- [Container Registry and Kubernetes Service plug-ins](https://cloud.ibm.com/docs/cli?topic=cli-install-devtools-manually)


When the installation for {{site.data.keyword.cloud_notm}} CLI and Container Registry and Kubernetes Service plug-ins is completed, you can now log in to the {{site.data.keyword.cloud_notm}} to complete the following steps:
1. Go to the cluster overpage and click **Actions**.
2. Then, click **Connect via CLI**. 
3. You can now see a set of similar instructions depending on the location of the cluster:

```bash
ibmcloud login -a test.cloud.ibm.com -r us-south -g default
ibmcloud ks cluster config --cluster avaluewillbehere
ibmcloud cr login
```
{: codeblock}

4. You can now use the `kubectl` command.

### Additional tools
{: #ibp-smart-contracts-k8s-self-managed-before-tools}

- Suggest by using `nvm` for Nodejs (version 12+)
- Docker for building the containers
- More NodeJS utilities that are required are listed as follows. Install them as needed.

### Hyperledger Fabric Peer Commands
{: #ibp-smart-contracts-k8s-self-managed-before-hyperledger}

A copy of the [Hyperleger Fabric](https://hyperledger-fabric.readthedocs.io/en/release-2.2/cc_service.html) Peer Commands is required. Use script `getPeer.sh` to get them.

```bash
./scripts/getPeer.sh
```
{: codeblock}

This gives the output for the environment variables that requires for setting to ensure the `FABRIC_CFG_PATH` is set correctly. You can check the version of the peer to verify whether the installation is done correctly.

```bash
peer version

# output. need to have 2.2.1 or later
peer:
 Version: 2.2.1
 Commit SHA: 344fda602
 Go version: go1.14.4
 OS/Arch: linux/amd64
 Chaincode:
  Base Docker Label: org.hyperledger.fabric
  Docker Namespace: hyperledger
```
{: codeblock}

### API Keys
{: #ibp-smart-contracts-k8s-self-managed-before-api}

Two API keys are needed:

- {{site.data.keyword.blockchainfull_notm}} Platform console service credentials that can be [created from the web ui](/docs/account?topic=account-service_credentials)
- {{site.data.keyword.cloud_notm}} API key. To learn how to create a user API key, see [Managing user API keys](/docs/account?topic=account-userapikey#manage-user-keys) for details.


Create a `.env` file that is similar to this:

```
# .env file 
CLOUD_API_KEY=a8aUjPRMrIgLhQXGWcl9tR_FxtdQvjQtmPXnKrFHQIK5
IBP_KEY=xxxxxxxxxxxxxxxxxxxx
IBP_ENDPOINT=https://xxxxxxxxxxxxxxxxxxxxxxxxx-ibpconsole-console.so01.blockchain.test.cloud.ibm.com
```
{: codeblock}

Then, set the following as environment variables.

```bash
export $(grep -v '^#' .env | xargs)
```
{: codeblock}

## Quick start
{: #ibp-smart-contracts-k8s-self-managed-quickstart}
These listed commands are placed in a makefile that can run as follows:

- `make nodecontract` builds and publishes the Docker image for the Node.js contract
- `make javacontract` builds and publishes the Docker image for the Java contract
- `make gocontract` builds and publishes the Docker image for the Go contract 
- `make network` builds the network of Peers, Orderers, and CAs
- `make tls` creates the X509 certificates that require to work with TLS between chaincode and peer
- `make iamsetup` creates the secret key to pull from the container registry 
- `make nodedeploy` deploys the Node chaincode definition to the peer, and stands up the chaincode container in a separate Kubernetes namespace from {{site.data.keyword.blockchainfull_notm}} Platform
- `make javadeploy` deploys the Java chaincode definition to the peer, and stands up the chaincode container in a separate Kubernetes namespace from {{site.data.keyword.blockchainfull_notm}} Platform
- `make identity` creates an application identity for client applications to use

## Node.js smart contract
{: #ibp-smart-contracts-k8s-self-managed-nodejs}

To demonstrate how to deploy a contract, this tutorial uses an example, which is the basic getting started contract that is found in the Fabric Docs and the {{site.data.keyword.blockchainfull_notm}} Platform VSCode extensions. The key is using the Dockerfile to package up the contract and some minor changes to the package.json file.

### Chaincode-as-a-server
{: #ibp-smart-contracts-k8s-self-managed-nodejs-chaincode}

Under normal circumstances, the Peers start (directly or indirectly in the case like {{site.data.keyword.blockchainfull_notm}} Platform) the chaincode processes running. In this case, when the chaincode starts, it 'calls back' to the peer to 'register'.

Here becomes a different situation whereas the chaincode process becomes a server, and it is up to the peer to connect to it when it needs to 'register'.

After the initial 'registration' is complete, there should be no difference between the two setups.

The key is to add the following script to the package.json scripts section.

```json
 "start:server": "fabric-chaincode-node server --chaincode-address=$CHAINCODE_SERVER_ADDRESS --chaincode-id=$CHAINCODE_ID --chaincode-tls-key-file=/hyperledger/privatekey.pem --chaincode-tls-client-cacert-file=/hyperledger/rootcert.pem --chaincode-tls-cert-file=/hyperledger/cert.pem"
```
{: codeblock}

Suggesting to add `echo $CHAINCODE_SERVER_ADDRESS $CHAINCODE_ID && ` before the command `fabric-chaincode-node` for debugging purposes.

The actual contract or libraries that are used has NO changes. The only change is the previous command in the `package.json`
{: important}

The TLS settings are referring to the files that mount into the chaincode when they deploy into Kubernetes. The actual locations are arbitrary and you can alter them based on your setting requirements.

### Dockerfile
{: #ibp-smart-contracts-k8s-self-managed-nodejs-dockerfile}

1. The Dockerfile is a relatively simple node.js file that you can create based on your needs.

```docker
FROM node:12.15-alpine

WORKDIR /usr/src/app

# Copy package.json first to check if an npm install is needed
COPY package.json /usr/src/app
RUN npm install --production

# Bundle app source
COPY . /usr/src/app

ENV PORT 9999
EXPOSE 9999

CMD ["npm", "run", "start:server"]
```
{: codeblock}

The PORT in the example is set as 9999 to run the command. The port can be set of your own choice and 9999 is used throughout this tutorial. The most important is to ensure that the command is running and the port is set up. 

2. Then, you need to build and push it to a registry. The registry that is used in this tutorial is the container registry that connects to the {{site.data.keyword.IBM_notm}} Kubernetes Cluster.

```bash 
docker build -t caasdemo-node .
docker tag caasdemo-node stg.icr.io/ibp_demo/caasdemo-node:latest
```
{: codeblock}

Ensure you login to the container registry (`ibmcloud cr login`) and push the docker image

```bash
docker push  stg.icr.io/ibp_demo/caasdemo-node:latest
```
{: codeblock}


## Secret to pull the docker image
{: #ibp-smart-contracts-k8s-self-managed-secret}

A Kubernetes secret needs to create with credentials of how to pull images from the Container Registry. Learn how to [create an image pull secret with different IAM API key credentials](/docs/containers?topic=containers-registry#other_registry_accounts) for more information.


## {{site.data.keyword.blockchainfull_notm}} Platform Configuration
{: #ibp-smart-contracts-k8s-self-managed-ibpconfig}

Use `ansible-playbooks/000-create-network.yml` to create the Peers, Orderers, and Certificate Authority.

As {{site.data.keyword.blockchainfull_notm}} Platform Playbooks are concerned, the following is the standard.

```bash
ansible-playbook ./ansible-playbooks/000-create-network.yml \
    --extra-vars api_key=${IBP_KEY} \
    --extra-vars api_endpoint=${IBP_ENDPOINT} \
    --extra-vars api_token_endpoint=${API_TOKEN_ENDPOINT} \
    --extra-vars channel_name=${CHANNEL_NAME} \
    --extra-vars home_dir=${DIR} 
```
{: codeblock}

Next step is to set up the chaincode.

```bash
ansible-playbook ./ansible-playbooks/001-setup-k8s-chaincode.yml \
    --extra-vars api_key=${IBP_KEY} \
    --extra-vars api_endpoint=${IBP_ENDPOINT} \
    --extra-vars api_token_endpoint=${API_TOKEN_ENDPOINT} \
    --extra-vars channel_name=${CHANNEL_NAME} \
    --extra-vars smart_contract_name=nodecontract \
    --extra-vars smart_contract_version=2 \
    --extra-vars smart_contract_sequence=2 \
    --extra-vars home_dir=${DIR} \
```
{: codeblock}


### Playbook details
{: #ibp-smart-contracts-k8s-self-managed-ibpconfig-playbook}

This is the `001-setup-k8s-chaincode.yml` playbook.

- Kubernetes namespace - first is to create a namespace separate from the running {{site.data.keyword.blockchainfull_notm}} Platform instance.

```yaml
    - name: Setup the namepsace
      community.kubernetes.k8s:
        name: caasdemo
        api_version: v1
        kind: Namespace
        state: present
```
{: codeblock}

- Service instance - the most important is the peer to locate and connects to the chaincode. This is done from a Service and by using a ClusterIP type service. 

```yaml
    - name: Create a Service object from an inline definition
      community.kubernetes.k8s:
        state: present
        definition:
          apiVersion: v1
          kind: Service
          metadata:
            namespace: caasdemo
            name: nodecc
            labels:
              app: caasdemo
          spec:
            ports:
            - name: chaincode
              port: 9999
              targetPort: 9999
              protocol: TCP
            selector:
              app: caasdemo
      register: result
```
{: codeblock}

- 'Proxy' chaincode - it needs to install a 'proxy' chaincode to indicate where the chaincode is running. The 'code' is a JSON file - `connection.json`. Following are the two steps to create this file and packaging it as an 'external chaincode'.

```yaml
    - copy:
        content: "{{ result.result.spec | moreati.jq.jq('{address: \"nodecc.caasdemo:9999\",dial_timeout:\"10s\",tls_required:false}') }}"
        dest: ./connection.json
    - name: Create the archive
      command: ../pkgcc.sh -l nodecontract -t external connection.json
```
{: codeblock}

- Install, approve, and commit - the chaincode needs to be installed, approved, and committed. This is a standard use of the ansible tasks that can be used sequentially. However, if it is in a multi organizational environment, then, it needs to be handled differently.

- Chaincode configuration - the actual running chaincode needs to know its 'name'. This is assigned by the peer during the previous installation step and can be captured in ansible to put into a configmap.

This is a Node chaincode, so the `CHAINCODE_SERVER_ADDRESS` is `0.0.0.0` again with the port 9999.  
{: important}

```yaml
    - name: Create a ConfigMap for the chaincode configuration
      community.kubernetes.k8s:
        state: present
        definition:
          apiVersion: v1
          kind: ConfigMap
          metadata:
            name: nodecc.v5.conf
            namespace: caasdemo
            labels:
              app:  caasdemo
          data:
            CHAINCODE_SERVER_ADDRESS: 0.0.0.0:9999
            CHAINCODE_ID: "{{ installcc_result.installed_chaincode.package_id }}"
```
{: codeblock}

- Deploying the chaincode - the chaincode is now ready for deployment and up for running.

```yaml
    - name: Create a ConfigMap for the chaincode configuration
      community.kubernetes.k8s:
        state: present
        definition:
          apiVersion: apps/v1
          kind: Deployment
          metadata:
            namespace: caasdemo
            name: caasdemo
            labels:
              app: caasdemo
          spec:
            replicas: 1
            selector:
              matchLabels:
                app: caasdemo
            template:
              metadata:
                labels:
                  app: caasdemo
              spec:
                containers:
                  - name: nodecc
                    image: stg.icr.io/ibp_demo/caasdemo-node          
                    resources:
                      requests:
                        memory: "50Mi"
                        cpu: "0.1"
                    ports:
                    - containerPort: 9999
                    envFrom:
                      - configMapRef:
                          name: nodecc.v5.conf
                imagePullSecrets:
                  - name: pullimg-secret
```
{: codeblock}

A few items are tied together:

- The Docker image name of the container and the secret that is used to pull it from the registry
- The port is used
- The configmap is used for configuring the environment

## Checkpoint
{: #ibp-smart-contracts-k8s-self-managed-checkpoint}

By now, you achieved the following:

- Able to created and build a Nodejs smart contract and producing a Docker image to host it (the chaincode)
- Able to push the image to the docker registry. Create and use the secret for pulling the image
- Use Ansible to create the {{site.data.keyword.blockchainfull_notm}} Platform network and install a proxy chaincode
- The configmap from Kubernetes resources creates and deploys the service.

Next steps are to create an identity and use it to submit transactions

## Create identities
{: #ibp-smart-contracts-k8s-self-managed-createidentities}

The Ansible playbook `002-create-identity.yml` that creates an identity to make it available for use with the Client-side applications.

```bash
ansible-playbook ./config/ansible-playbooks/002-create-identity.yml \
    --extra-vars id_name="fred" \
    --extra-vars api_key=${IBP_KEY} \
    --extra-vars api_endpoint=${IBP_ENDPOINT} \
    --extra-vars api_token_endpoint=${API_TOKEN_ENDPOINT} \
    --extra-vars channel_name=${CHANNEL_NAME} \
    --extra-vars home_dir=${DIR} \
    --extra-vars network_name=CAASDEMO \
    --extra-vars org1_name=CAASOrg1 \
    --extra-vars org1_msp_id=CAASOrg1MSP
```
{: codeblock}

The first half of this playbook uses the {{site.data.keyword.blockchainfull_notm}} Platform collection to create the identity. The second half uses a community utility to take the identities from {{site.data.keyword.blockchainfull_notm}} Platform and create a local wallet for use by the Fabric Client SDKs.

(If `npm install -g @hyperledgendary/weftility` is not yet installed)

This creates the `_cfg` directory that contains a connection profile and a couple of wallets to use with the 'fred' identity.  

## Using these identities
{: #ibp-smart-contracts-k8s-self-managed-usingidentities}

The final step is to send actual transactions. You can now decide to write your own client app, or use the VSCode extension, or use an already written client-side app.  

To begin, install `runhfsc`

```
npm install -g @hyperledgednary/runhfsc
runhfsc --gateway ./_cfg/ansible/_gateways/CAASDEMO_CAASOrg1.json --wallet ./_cfg/ansible/_wallets/CAASDEMO_CAASOrg1 --user fred --channel caaschannel
```
{: codeblock}

Now, you can get a command prompt and by entering `contract nodecontract` to connect to the {{site.data.keyword.blockchainfull_notm}} Platform.

Then, by entering `metadata`, it issues a transaction to query the metadata of the contract. (You can get a full end to end connectivity when this function properly.)

```
 contract nodecontract
Contract set to nodecontract
[default] fred@caaschannel:nodecontract - $ metadata
(node:22057) [DEP0123] DeprecationWarning: Setting the TLS ServerName to an IP address is not permitted by RFC 6066. This will be ignored in a future versi
on.
> {
  '$schema': 'https://hyperledger.github.io/fabric-chaincode-node/release-2.1/api/contract-schema.json',
  contracts: {
    MyAssetContract: {
      name: 'MyAssetContract',
      contractInstance: { name: 'MyAssetContract', default: true },
      .....
      .....
```
{: codeblock}

This is a simple example for creating an asset:

```
submit createMyAsset '["007","Bond James Bond"]'
{
  txname: 'createMyAsset',
  args: '["007","Bond James Bond"]',
  private: undefined
}
Submitted createMyAsset  007,Bond James Bond
>
```
{: codeblock}

And to read the asset back again

```
evaluate readMyAsset '["007"]'
Submitted readMyAsset  007
> {"value":"Bond James Bond"}
```
{: codeblock}

Reading an asset that does not exist (or maybe not have clearance to know about?)

```
evaluate readMyAsset '["004"]'ß
Submitted readMyAsset  004
Error: error in simulation: transaction returned with failure: Error: The my asset 004 does not exist
```
{: codeblock}
