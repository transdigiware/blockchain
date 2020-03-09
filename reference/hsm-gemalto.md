---

copyright:
  years: 2020
lastupdated: "2020-03-09"

keywords: HSM, Gemalto, IBM Cloud

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:note: .note}
{:tip: .tip}
{:download: .download}

# IBM Cloud Hardware Security Module (HSM)
{: #ibp-HSM}
{: help}
{: support}

{{site.data.keyword.cloud_notm}} includes an HSM service that provides cryptographic processing for key generation, encryption, decryption, and key storage. This document describes how to use that service with the {{site.data.keyword.blockchainfull}} Platform.
{: shortdesc}

# Using Gemalto v6.x

1. Provision the HSM and configure it with at least one partition. For example, you can follow instructions for [Provisioning IBM Cloud HSM](/docs/infrastructure/hardware-security-modules?topic=hardware-security-modules-provisioning-ibm-cloud-hsm){: external}.

2. [Install the HSM client](/docs/infrastructure/hardware-security-modules?topic=hardware-security-modules-installing-the-ibm-cloud-hsm-client){: external} on your local machine.

3. Using the HSM client, you can get the server certificate by running the following command:
```bash
scp hsm_admin@${HSM_ADDRESS}:server.pem server.pem
```
{: codeblock}
Replace
- {HSM_ADDRESS} with ...

4. Run the following command to add the HSM server to local client:
```bash
vtl addServer -n ${HSM_ADDRESS} -c server.pem
```
{: codeblock}
Replace
- {HSM_ADDRESS} with ...

5. Create the certificates for client by running the command:
```bash
vtl createcert -n ${CLIENT_ADDRESS}
```
{: codeblock}

Replace
- {CLIENT_ADDRESS} with ...

6. Copy the client certs to the server by running the command:
```bash
scp /usr/safenet/lunaclient/cert/client/${CLIENT_ADDRESS}.pem hsm_admin@${HSM_ADDRESS}:.
```
{: codeblock}

Replace
- {CLIENT_ADDRESS} with ...
- {HSM_ADDRESS} with ...


7. On the HSM server register the client with the command:
```bash
# if the client address is IP address run:
client register -client ${CLIENT_NAME} -ip ${CLIENT_ADDRESS}

# else if the client address is fqdn run:
client register -client ${CLIENT_NAME} -hostname ${CLIENT_ADDRESS}
```
{: codeblock}

Replace
- {CLIENT_NAME} with ...
- {CLIENT_ADDRESS} with ...
- {HSM_ADDRESS} with ...

8. Restart the ntls service on the HSM server
```bash
service restart ntls
```
{: codeblock}


9. Assign a partition to newly created client on the HSM server by running the following command:
```bash
client assignpartition -client ${CLIENT_NAME} -partition ${PARTITION_NAME}

# to check if it worked
client show -client ${CLIENT_NAME}
```
{: codeblock}

Replace
- {CLIENT_NAME} with ...
- {PARTITION_NAME} with ...

10. Verify the client can connect to HSM server by running the command:
```bash
vtl verify
```
{: codeblock}


## TODO
1. Ash ran some command on hsm server to make sure that the server doesnt check the clients address, to make it run on docker/k8s
2. How to get client address
3. Do we need more detail about the step of configuring the hsm
4. How to mount the certs created in step 3 and 5 to the pod
5. How to setup the pod
6. How to setup service
7. Final checks if it worked before moving on to create blockchain components
