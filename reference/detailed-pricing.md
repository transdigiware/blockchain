---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-07"

keywords: Pricing, pricing examples

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
{:download: .download}_

# Detailed pricing scenarios
{: #ibp-detailed-pricing}

This topic includes additional detailed pricing and sizing scenarios to help you learn more about the potential costs of various {{site.data.keyword.blockchainfull} Platform network configurations.
{: shortdesc}

**Target audience**: This advanced topic is designed for architects and system administrators who are responsible for planning and sizing the cost of an {{site.data.keyword.blockchainfull_notm}} Platform network on {{site.data.keyword.cloud_notm}}.

If you want to learn more about the basic elements that factor into the cost a blockchain network, review the  topic on [Pricing for {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing).
{: note}

## Scenario comparisons
{: #ibp-pricing-details-tabs}

When you configure your peers, CAs and ordering service, you might need to adjust the resource allocations to use the values that are included in the table below.

| | Assumptions | Cost to Host | Cost to Join |
|:-|:-----------------|:-----------------|:-----------------|
| **Goal:** Larger peers for more robust integration testing. <br><br> You can join the peers to multiple channels to better simulate a real environment or put them all in one account. | <ul><li>2-3 organizations</li><li>CouchDB</li><li>Bronze Storage</li><li>No: HA, Backup, HSM, LogDNA, SysDig, Dedicated VM/HW</li></ul> | Includes: <ul><li>**Single node Raft ordering service**</li><ul><li>0.35GB VPC/0.7GB RAM/100GB Storage</li></ul><li>**2 CAs**</li><ul><li>2 x (.1 VPC/.2GB RAM/20 GB Storage) = <br>.2 VPC/.4GB RAM/40 GB Storage</li></ul><li>**1 Peer:**<ul><li> **Peer container:** .2 VPC/.4GB RAM/25GB Storage</li><li>**Couch container:** .2 VPC/.4GB RAM/25GB Storage</li><li>**Smart contract container:** .5 VPC/1GB RAM/0GB Storage</li><li> **Logging/gRPC Web container:** .2 VPC/.4GB RAM/0GB Storage</li></li></ul> |  Includes: <br>**1 CA**<ul><li> 0.1 VPC/.2GB RAM/20 GB Storage</li> </ul>**1 Peer:**<ul><li> **Peer container:** .2 VPC/.4GB RAM/25GB Storage</li><li> **CouchDB container:** .2 VPC/.4GB RAM/25GB Storage</li><li> **Smart contract container:** .5 VPC/1GB RAM/0GB Storage</li><li>**Logging/gRPC Web container:** .2 VPC/.4GB RAM/0GB Storage</li></li></ul>|
| **Total Resources** |  | **VPC:** 1.65<br> **RAM:** 3.3GB<br> **Storage:** 240GB|  **VPC:** 1.2GB<br> **RAM:** 2.4GB<br> **Storage:** 120GB|
| **Total Cost** <br> (per hour) | |**IBP:** 1.65VPC x .29/hr = **$0.48 USD**  <br> **IKS***:** $0.31 USD <br> **Storage:** $0.05 USD <br><br> **Total:** 0.84 USD/hr <br> <br> ***IKS 4x16 single node cluster  with IP Allocation| **IBP:** 1.2VPC x .29/hr = **$0.35 USD**  <br> **IKS***:** $0.13 USD <br> **Storage:** $0.03 USD <br><br> **Total:** 0.51 USD/hr  <br> <br> ***IKS 2x4 single node cluster with IP allocation|
| **Total Cost** <br> (per month) | |**IBP:** $346.00 USD  <br> **IKS:** $224.00 USD <br> **Storage:**$36.00 USD <br><br> **Total Base Cost:** $606.00 USD|  **IBP:** $252.00 USD  <br> **IKS:** $94.00 USD <br> **Storage:**$22.00 USD <br><br> **Total Base Cost:** $368.00 USD | |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Functional Test/Demo Environment"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| | Assumptions | Cost to Host | Cost to Join |
|:-|:-----------------|:-----------------|:-----------------|
|**Goal:** A relatively large test environment for evaluating an overall solution for eventual production.  <br>  <br> Includes multiple organizations and some level of HA/Performance, but not meant for production. | <ul><li>3-5 organizations</li><li>CouchDB</li><li>PostgreSQL Database</li><li>HA with possible MZR</li><li>Optional: Backup Tests, Silver Storage, HSM, LogDNA/Sysdig, Dedicated VM/HW</li></ul> | Includes: <ul><li>5 node Raft ordering service</li><ul><li>(1GB VPC/1GB RAM/500GB Storage) x5 = 5GB VPC/5GB RAM/2500GB Storage </li></ul><li>2 CAs</li><ul><li>(.1 VPC/.2GB RAM/20 GB Storage) x2 = <br>.2 VPC/.4GB RAM/40 GB Storage</li></ul><li>2 Peers: <ul><li> Peer container .2 VPC/.4GB RAM/25GB Storage</li><li> Couch container .2 VPC/.4GB RAM/25GB Storage</li><li> Smart contract container .5 VPC/1GB RAM/0GB Storage</li><li> Logging/gRPC Web container .2 VPC/.4GB RAM/0GB Storage</li></li></ul> |1 CA</li><ul><li>.1 VPC/.2GB RAM/20 GB Storage</li></ul> </ul>1 Peer: <ul><li> Peer container .2 VPC/.4GB RAM/25GB Storage</li><li> Couch container .2 VPC/.4GB RAM/25GB Storage</li><li> Smart contract container .5 VPC/1GB RAM/0GB Storage</li><li> Logging/gRPC Web container .2 VPC/.4GB RAM/0GB Storage</li></li></ul>|
| **Total Resources** |  | **VPC:** 1.55GB<br> **RAM:** 3.1GB<br> **Storage:** 280GB|  **VPC:** 1.2GB<br> **RAM:** 2.4GB<br> **Storage:** 120GB|
| **Total Cost** <br> (per hour) | |**IBP:** 1.55VPC x .29/hr = $0.45 USD  <br> **IKS:** $0.29 USD <br> **Storage:** $0.06 USD <br><br> **Total:** 0.80 USD/hr| **IBP:** 1.2VPC x .29/hr = $0.35 USD  <br> **IKS:** $0.10 USD <br> **Storage:** $0.03 USD <br><br> **Total:** 0.47 USD/hr  |
| **Total Cost** <br> (per month) | |**IBP:** $345.00 USD  <br> **IKS:** $210.00 USD <br> **Storage:**$40.00 USD <br><br> **Total Base Cost:** $595.00 USD|  **IBP:** $255.00 USD  <br> **IKS:** $80.00 USD <br> **Storage:**$17.00 USD <br><br> **Total Base Cost:** $352.00 USD | |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Pilot"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Environment 1 |  |  |  |
|:-|:-----------------|:-----------------|:-----------------|--|
|**Goal:** A relatively large test environment for evaluating an overall solution for eventual production.  <br>  <br> Includes multiple organizations and some level of HA/Performance, but not meant for production. <br><br> **Architecture assumptions:** <br><br> <ul><li>3-5 organizations</li><li>CouchDB</li><li>PostgreSQL Database</li><li>HA with possible MZR</li><li>Optional: Backup Tests, Silver Storage, HSM, LogDNA/Sysdig, Dedicated VM/HW</li></ul> | **Cost to run a network**<br><br><br><br> **5 node Raft ordering service:** <br> 5 X (1GB VPC, 1GB RAM, 500GB Storage)  <br><br> **2 CAs:** <br> 2 X (0.1 VPC, 0.2GB RAM, 20 GB Storage) <br><br>**1 Peer:** <br> 0.2GB VPC, 0.4GB RAM, 100GB Storage <br><br> _Container breakdown:_ <br> Peer: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Couch: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Couch: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Smart contract: 0.5 VPC, 1GB RAM <br> Logging/gRPC Web: 0.2 VPC, 0.4GB RAM <br> <br> **Total:** <br> 9.6 VPC <br> 13.4 GB RAM <br> 4040 GB Storage  | **Hourly Cost** <br><br> <br>**IBP:** $2.84 USD <br> <br> **IKS:** <br> $0.44 USD <br>(3) 8 x 32 nodes  <br><br> **Storage:** $0.81 USD  <br> <br> **Total:** $4.09 USD/hour | **Monthly Cost:** <br><br><br> **IBP:** $2,045 USD <br> <br> **IKS:** <br> (3) 8 x 32 nodes $1,125 USD <br><br> **Storage:** $580 USD <br> <br> **Total:** $3650 USD/month |
| | **Cost for network joiners** <br><br> <br> **1 CA:** <br> 0.1 VPC, 0.2GB RAM, 20 GB Storage <br><br>**2 Peers:** <br> 2 X (2.2GB VPC, 4.4GB RAM, 750GB Storage) <br><br> _Container breakdown:_ <br> Peer: 0.5 VPC, 0.8GB RAM, 250GB Storage <br> Couch: 0.5 VPC, 0.8GB RAM, 500GB Storage <br> Couch: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Smart contract: 1 VPC, 2GB RAM <br> Logging/gRPC Web: 0.2 VPC, 0.4GB RAM <br> <br> <br> <br>**Total:** <br> 4.5 VPC <br> 8.2 GB RAM <br> 1520 GB Storage | **Hourly Cost** <br><br> <br>**IBP:** $1.33 USD <br> <br> **IKS:** <br> $0.25 USD <br>(3) 4 x 16 nodes  <br><br> **Storage:** $0.31 USD  <br> <br> **Total:** $1.89 USD/hour | **Monthly Cost** <br><br><br> **IBP:** $960 USD <br> <br> **IKS:**  $630 USD <br> (3) 4 x 16 nodes <br><br> **Storage:** $220 USD <br> <br> **Total:** $1810 USD/month |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Pilot V2"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Environment 2 |  |  |  | |
|:-|:-----------------|:-----------------|:-----------------|--|
| **Goal:** A relatively large test environment for evaluating an overall solution for eventual production.  <br>  <br> Includes multiple organizations and some level of HA/Performance, but not meant for production.| **Architecture assumptions:** <br><br> <ul><li>3-5 organizations</li><li>CouchDB</li><li>PostgreSQL Database</li><li>HA with possible MZR</li><li>Optional: Backup Tests, Silver Storage, HSM, LogDNA/Sysdig, Dedicated VM/HW</li></ul> | | | |
| | |**Cost to run a network**<br><br><br><br> **5 node Raft ordering service:** <br> 5 X (1GB VPC, 1GB RAM, 500GB Storage)  <br><br> **2 CAs:** <br> 2 X (0.1 VPC, 0.2GB RAM, 20 GB Storage) <br><br>**1 Peer:** <br> 0.2GB VPC, 0.4GB RAM, 100GB Storage <br><br> _Container breakdown:_ <br> Peer: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Couch: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Couch: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Smart contract: 0.5 VPC, 1GB RAM <br> Logging/gRPC Web: 0.2 VPC, 0.4GB RAM <br> <br> **Total:** <br> 9.6 VPC <br> 13.4 GB RAM <br> 4040 GB Storage   |**Hourly Cost** <br><br> <br>**IBP:** $2.84 USD <br> <br> **IKS:** <br> $0.44 USD <br>(3) 8 x 32 nodes  <br><br> **Storage:** $0.81 USD  <br> <br> **Total:** $4.09 USD/hour | **Monthly Cost:** <br><br><br> **IBP:** $2,045 USD <br> <br> **IKS:** <br> (3) 8 x 32 nodes $1,125 USD <br><br> **Storage:** $580 USD <br> <br> **Total:** $3650 USD/month |
| | |**Cost for network joiners** <br><br> <br> **1 CA:** <br> 0.1 VPC, 0.2GB RAM, 20 GB Storage <br><br>**2 Peers:** <br> 2 X (2.2GB VPC, 4.4GB RAM, 750GB Storage) <br><br> _Container breakdown:_ <br> Peer: 0.5 VPC, 0.8GB RAM, 250GB Storage <br> Couch: 0.5 VPC, 0.8GB RAM, 500GB Storage <br> Couch: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Smart contract: 1 VPC, 2GB RAM <br> Logging/gRPC Web: 0.2 VPC, 0.4GB RAM <br> <br> <br> <br>**Total:** <br> 4.5 VPC <br> 8.2 GB RAM <br> 1520 GB Storage |  **Hourly Cost** <br><br> <br>**IBP:** $1.33 USD <br> <br> **IKS:** <br> $0.25 USD <br>(3) 4 x 16 nodes  <br><br> **Storage:** $0.31 USD  <br> <br> **Total:** $1.89 USD/hour | **Monthly Cost** <br><br><br> **IBP:** $960 USD <br> <br> **IKS:**  $630 USD <br> (3) 4 x 16 nodes <br><br> **Storage:** $220 USD <br> <br> **Total:** $1810 USD/month |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Pilot V3"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}
