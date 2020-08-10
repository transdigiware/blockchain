---

copyright:
  years: 2019, 2020
lastupdated: "2020-08-10"

keywords: organizations, MSPs, create an MSP, MSP JSON file, consortium, system channel, remove an organization

subcollection: blockchain

---

[{METADADATA_ATTRIBUTES}]

# Managing certificates
{: #cert-mgmt}

{{site.data.keyword.blockchainfull}} Platform is based on Hyperledger Fabric and builds permissioned blockchain networks. Participants are known as members of the network, and their activities and access to network resources are verified continuously. A participant's identity is provided in the form of a trusted x509 digital certificate. The verification is provided by an underlying public key infrastructure and sign/verify operations that secure transactions and management within the network.
{:shortdesc}

{{site.data.keyword.blockchainfull_notm}} Platform manages most of the certificate operations without users needing to handle their certificates. However, there are times when you will have to manage the certificates that allow you to communicate with the network, such as developing applications or expanding your network with a remote peer or renewing certificates before they expire. The following is a short guide to your Certificate Authority, and the underlying certificate infrastructure. This information can help you understand the certificates that you will be working with and the tasks you need to perform.

## Certificate authorities
{: ##cert-mgmt-network-ca}

Certificate authorities (CAs) provide identity on the network. A CA can be considered as a publicly trusted notary that acts as an anchor of trust among multiple parties. All entities in the network are given a certificate signed by a root CA that encapsulates their digital identity. This certificate is the root of trust for all the sign and verify operations that are performed on the network. For more details about how certificate authorities are used to establish identity, see [Hyperledger Fabric documentation](https://hyperledger-fabric.readthedocs.io/en/release-2.1/identity/identity.html){: external}.

Each organization in the network has their own CA. Your organization CA signs requests for all of the entities and components that you own, such as your admin, peers, or applications. If you want to add a peer or new application to your network, you need to register the new identity with your Certificate Authority (registration). Then, the CA can provide the new entity with the certificates that the entity needs to interact with the network (enrollment).

### Generating certificates for client applications (enrollment)
{: ##cert-mgmt-enrollment}
Before you can connect a client application to {{site.data.keyword.blockchainfull_notm}} Platform, the application needs to be authenticated. The process of generating the necessary certificates, the private key and certificate (also known as your enrollment cert or signCert), is called "enrollment". These certificates are required everytime your client communicates with the network. Any client application that submits calls to the network needs to sign payloads by using a private key and attach a properly signed x509 certificate.

You can use the CA in the console to enroll and generate the private key and certificate, or you can enroll by using the Fabric SDKs. The private key and signCert form an identity that can be used by the client application.

You can also generate certificates from the command line by using the [Fabric CA client](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/deployguide/use_CA.html){: external}. The Fabric CA client returns certificates inside a Membership Service Provider (MSP) folder. This folder contains the root certificate that the CA signed, intermediate certificates if an intermediate CA is being used, a private key, and your signCert. For more information about MSP and what the MSP folder contains, see [Membership Service Providers](https://hyperledger-fabric.readthedocs.io/en/release-2.1/membership/membership.html){: external}.

When you create a new CA, you specify an enroll ID and secret for the admin of the CA, which "registers" the admin identity.  You can also register identities of peer nodes, ordering nodes, and client applications by clicking **Register User**

The following table describes the types of certificates used by the platform and how they can be renewed.

Automatic certificate renewal is not available for certificates that were generated before the helm-to-operator migration.
{: important}


## Simple tab table
{: #simple-tab-table-test}

|  Certificate | Description| How generated |	 Default Expiration |	How to view expiration | How to renew | What to do if expired |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
| **CA TLS certificate** | This certificate is used to trust the CA server. It is the public key that must be shared with all members in the organization that want to transact with any node in the organization. When any client or node submits a transaction to another node, it must include this certificate as part of the transaction to prevent “man in the middle” attacks. | Generated when the CA is first started because TLS is enabled.| 1 year | **2.5.x:** Console CA node **TLS Cert Expiry** field | **2.5.x:** Automatic renewal is attempted 30 days before expiry. <br><br> **2.1.x**, or if **automatic renewal fails:** Open a support ticket.|Open a support ticket |
{: class="simple-tab-table"}
{: caption="Table 1. CA" caption-side="top"}
{: #simpletabtable1}
{: tab-title="CA"}
{: tab-group="IAM-simple"}

| Certificate |	 Description |	How generated	| Default Expiration| How to view expiration	| How to renew| 	What to do if expired|
|-----|-----|-----|-----|-----|-----|-----|
| **Peer enrollment certificate (signcert)** | This is the signing certificate that the peer uses to sign transactions. |Generated when the peer is deployed based on the the peer enroll ID and secret that is provided.| 1 year | **2.5.x: [TODO](https://github.ibm.com/ibp/operator/issues/1680)** Console peer node **Enrollment Cert Expiry** field  | **2.5.x:** Automatic renewal is attempted 30 days before expiry. <br><br> **2.1.x**, or if **automatic renewal fails:** Open a support ticket.|Open a support ticket |
|**Peer TLS certificate (signcert)** | When TLS is enabled on a network, each node must register and enroll with the TLS CA. This is the TLS certificate for the peer from that process and is required for the peer to start.| Generated when the peer is deployed.| **2.5.x:** 10 years<br><br>**2.1.x:** 1 year | **2.5.x:** Console peer node **TLS Cert Expiry** field <br><br>**2.1.x** or if **automatic renewal fails (h2o) TODO:**  | **2.5.x:** Automatic renewal is attempted 30 days before expiry. <br><br> **2.1.x**, or if **automatic renewal fails:** Open a support ticket.|Open a support ticket |
|**Peer operations certificate** | Required to view [operation metrics](/docs/blockchain?topic=blockchain-operations_service) for the peer.| Generated when the peer is deployed.| **2.5.x:** 10 years<br><br>**2.1.x:** 1 year| **2.5.x:** Console peer node **TLS Cert Expiry** field <br><br>**2.1.x** or if **automatic renewal fails (h2o) TODO:**  | **2.5.x:** Automatic renewal is attempted 30 days before expiry. <br><br> **2.1.x**, or if **automatic renewal fails:** Open a support ticket.|Open a support ticket |

{: caption="Table 2. Peer" caption-side="top"}
{: #simpletabtable2}
{: tab-title="Peer"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Certificate |	 Description |	How generated	| Default Expiration| How to view expiration	| How to renew| 	What to do if expired|
|-----|-----|-----|-----|-----|-----|-----|
| **Orderer enrollment certificate (signcert)** | This is the signing certificate that the orderer uses to sign transactions. |Generated when the ordering node is deployed based on the the ordering service enroll ID and secret that is provided.| 1 year | **2.5.x: [TODO](https://github.ibm.com/ibp/operator/issues/1680)** Console ordering node **Enrollment Cert Expiry** field  | **2.5.x:** Automatic renewal is attempted 30 days before expiry. <br><br> **2.1.x**, or if **automatic renewal fails:** Open a support ticket.|Open a support ticket |
|**Orderer TLS certificate (signcert)** | When TLS is enabled on a network, each node must register and enroll with the TLS CA. This is the TLS certificate for the ordering node from that process and is required for the ordering node to start.| Generated when the ordering node is deployed.| **2.5.x:** 10 years<br><br>**2.1.x:** 1 year | **2.5.x:** Console ordering node **TLS Cert Expiry** field <br><br>**2.1.x** or if **automatic renewal fails (h2o) TODO:**  | **2.5.x:** Automatic renewal is attempted 30 days before expiry. <br><br> **2.1.x**, or if **automatic renewal fails:** Open a support ticket.|Open a support ticket |
{: caption="Table 3. Orderer" caption-side="top"}
{: #simpletabtable3}
{: tab-title="Orderer"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

## Certificate renewal
{: #cert-mgmt-renew}



</staging>

