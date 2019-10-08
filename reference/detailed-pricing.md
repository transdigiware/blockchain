---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-08"

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
{:download: .download}

# Detailed pricing scenarios
{: #ibp-detailed-pricing}

This topic includes additional detailed pricing and sizing scenarios to help you learn more about the potential costs of various {{site.data.keyword.blockchainfull}} Platform network configurations.
{: shortdesc}

**Target audience**: This advanced topic is designed for architects and system administrators who are responsible for planning and sizing the cost of an {{site.data.keyword.blockchainfull_notm}} Platform network on {{site.data.keyword.cloud_notm}}.

If you want to learn more about the basic elements that factor into the cost a blockchain network, review the  topic on [Pricing for {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}](/docs/services/blockchain?topic=blockchain-ibp-saas-pricing).
{: note}

## Scenario comparisons
{: #ibp-pricing-details-tabs}

When you configure your peers, CAs and ordering service, you might need to adjust the resource allocations to use the values that are included in the table below.

| | Assumptions | Cost to Host | Cost to Join |
|:-|:-----------------|:-----------------|:-----------------|
| **Goal:** Larger peers for more robust integration testing. <br><br> You can join the peers to multiple channels to better simulate a real environment or put them all in one account. | <ul><li>2-3 organizations</li><li>CouchDB</li><li>Bronze Storage</li><li>Not included: HA, Backup, HSM, LogDNA, SysDig, Dedicated VM/HW</li></ul> | Includes: <br>**Single node Raft ordering service**<ul><li>0.35GB VPC/0.7GB RAM/100GB Storage</li></ul><br>**2 CAs**<ul><li>2 x (0.1 VPC/0.2GB RAM/20 GB Storage) = <br>0.2 VPC/0.4GB RAM/40 GB Storage</li></ul><br>**1 Peer** <br> 1.1 VPC/2.2 GB RAM/100 GB Storage<ul><li> **Peer container:** 0.2 VPC/0.4GB RAM/25GB Storage</li><li>**Couch container:** 0.2 VPC/0.4GB RAM/25GB Storage</li><li>**Smart contract container:** 0.5 VPC/1GB RAM/0GB Storage</li><li> **Logging/gRPC Web container:** 0.2 VPC/0.4GB RAM/0GB Storage</li></li></ul> |  Includes: <br>**1 CA**<ul><li> 0.1 VPC/0.2GB RAM/20 GB Storage</li> </ul>**1 Peer**<br> 1.1 VPC/2.2 GB RAM/100 GB Storage<ul><li> **Peer container:** 0.2 VPC/0.4GB RAM/25GB Storage</li><li> **CouchDB container:** 0.2 VPC/0.4GB RAM/25GB Storage</li><li> **Smart contract container:** 0.5 VPC/1GB RAM/0GB Storage</li><li>**Logging/gRPC Web container:** 0.2 VPC/0.4GB RAM/0GB Storage</li></li></ul>|
| **Total Resources** |  | **VPC:** 1.65<br> **RAM:** 3.3GB<br> **Storage:** 240GB|  **VPC:** 1.2<br> **RAM:** 2.4GB<br> **Storage:** 120GB|
| **Total Cost** <br> (per hour) | |**IBP:** 1.65VPC x .29/hr = **$0.48 USD**  <br> **IKS***:** $0.31 USD <br> **Storage:** $0.05 USD <br><br> **Total:** 0.84 USD/hr <br> <br> ***IKS 4x16 single node cluster  with IP allocation| **IBP:** 1.2VPC x .29/hr = **$0.35 USD**  <br> **IKS***:** $0.13 USD <br> **Storage:** $0.03 USD <br><br> **Total:** 0.51 USD/hr  <br> <br> ***IKS 2x4 single node cluster with IP allocation|
| **Total Cost** <br> (per month) | |**IBP:** $346 USD  <br> **IKS:** $224 USD <br> **Storage:**$36 USD <br><br> **Total Base Cost:** $606 USD|  **IBP:** $252 USD  <br> **IKS:** $94 USD <br> **Storage:**$22 USD <br><br> **Total Base Cost:** $368 USD | |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Functional Test/Demo Environment"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| | Assumptions | Cost to Host | Cost to Join |
|:-|:-----------------|:-----------------|:-----------------|
|**Goal:** A relatively large test environment for evaluating an overall solution for eventual production.  <br>  <br> Includes multiple organizations and slightly increased resources for HA/Performance, but not meant for production. | <ul><li>3-5 organizations</li><li>CouchDB</li><li>HA with possible MZR</li><li>Optional: Backup Tests,<br> PostgreSQL Database,<br>Silver Storage,<br> HSM,<br> LogDNA/Sysdig,<br> Dedicated VM/HW</li></ul> | Includes: <br>**5 node Raft ordering service**<ul><li>5 x (1GB VPC/1GB RAM/500GB Storage) = 5GB VPC/5GB RAM/2500GB Storage </li></ul><br>**2 CAs**<ul><li>2 x (0.1 VPC/0.2GB RAM/20 GB Storage) = <br>0.2 VPC/0.4GB RAM/40 GB Storage</li></ul><br>**2 Peers** <ul><li>2 x (2.2 VPC/4GB RAM/ 750 GB Storage)  </li><ul><li> **Peer container:** 0.5 VPC/0.8GB RAM/250GB Storage</li><li> **CouchDB container:** 0.5 VPC/0.8GB RAM/500GB Storage</li><li> **Smart contract container:** 1 VPC/2GB RAM/0GB Storage</li><li> **Logging/gRPC Web container:** 0.2 VPC/0.4GB RAM/0GB Storage</li></li></ul></ul> |Includes: <br> **1 CA**</li><ul><li>0.1 VPC/0.2GB RAM/20 GB Storage</li></ul> **2 Peers** <ul><li> 2 x (2.2 VPC/4GB RAM/ 750 GB Storage) </li> <ul><li> **Peer container:** 0.5 VPC/0.8GB RAM/250GB Storage</li><li>**CouchDB container:** 0.5 VPC/0.8GB RAM/500GB Storage</li><li>**Smart contract container:** 1 VPC/2GB RAM/0GB Storage</li><li>**Logging/gRPC Web container:** 0.2 VPC/0.4GB RAM/0GB Storage</li></li></ul></ul>|
| **Total Resources** |  | **VPC:** 9.6<br> **RAM:** 13.4GB<br> **Storage:** 4040GB|  **VPC:** 4.5<br> **RAM:** 8.2GB<br> **Storage:** 1520GB|
| **Total Cost** <br> (per hour) | |**IBP:** 9.6VPC x .29/hr = **$2.79 USD**  <br> **IKS***:** $1.59 USD <br> **Storage:** $0.81 USD <br><br> **Total:** $5.19 USD/hr <br> <br> *** IKS 3 node - 8x32 single zone cluster with IP allocation| **IBP:** 4.5VPC x .29/hr = $1.31 USD  <br> **IKS***:** $0.90 USD <br> **Storage:** $0.31 USD <br><br> **Total:** 2.52 USD/hr <br><br> *** IKS 3 node - 4 x16 single zone cluster |
| **Total Cost** <br> (per month) | |**IBP:** $2,008 USD  <br> **IKS:** $1,145 USD <br> **Storage** (Bronze):**$584 USD  <br><br>**Total Base Cost:** $3,737 USD|  **IBP:** $944 USD  <br> **IKS:** $648 USD <br> **Storage:**$224 USD <br><br> **Total Base Cost:** $1,816 USD |
| **Optional upgrades** | | **HA CA (PostgreSQL):** +$40 USD/month <br><br> **HSM (when available):** +$906 USD/month <br><br> **Silver Storage:**+$224 USD/month <br><br>**IKS MZR 2 zone:** +$1,152 USD/month <br> <br> **LogDNA:** $1.50/GB/Month (7 day search) <br><br> **Sysdig:** 0.035/hour/node (up to 20 containers)|**HA CA (PostgreSQL):** +$40 USD/month <br><br> **HSM:** +$906 USD/month <br><br> **Silver Storage:**+$105 USD/month <br><br>**IKS MZR 2 zone:** +$663 USD/month <br><br> **LogDNA:** $1.50/GB/Month (7 day search) <br><br> **Sysdig:** 0.035/hour/node (up to 20 containers) | |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Pilot"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}


| | Assumptions | Cost to Host | Cost to Join |
|:-|:-----------------|:-----------------|:-----------------|
|**Goal:** Production environment. Pre-production should match production as closely as possible in terms of size, performance, HA, and capabilty.  <br> | <ul><li>5+ organizations</li><li>CouchDB</li><li>HA with MZR</li><li>Backup Tests (Test in pre-prod, on in Prod)</li><li> PostgreSQL Database</li><li>Silver Storage</li> <li>HSM</li>  <li>Optional: <br> HSM, <br>LogDNA/Sysdig, <br> Dedicated VM/HW</ul> | Includes: <br>**5 node Raft ordering service**<ul><li>5 x (1GB VPC/1GB RAM/500GB Storage) <br>= <br>5GB VPC/5GB RAM/2500GB Storage </li></ul><br>**4 CAs**<ul><li>4 x (0.1 VPC/0.2GB RAM/20 GB Storage) = <br>0.4 VPC/0.8GB RAM/80 GB Storage</li></ul><br>**2 Peers** <ul><li>2 x (3.7 VPC/8.4GB RAM/ 750 GB Storage)<br> = <br>7.4 VPC/16.8GB RAM/1500 GB Storage  </li><ul><li> **Peer container:** 1 VPC/2GB RAM/250GB Storage</li><li> **CouchDB container:** 1 VPC/2GB RAM/500GB Storage</li><li> **Smart contract container:** 1.5 VPC/4GB RAM/0GB Storage</li><li> **Logging/gRPC Web container:** 0.2 VPC/0.4GB RAM/0GB Storage</li></li></ul></ul> |Includes: <br> **2 CAs**<br>2 x (.1 VPC/2 GB RAM/20 GB Storage) <br>=<br> 0.2 VPC/0.4GB RAM/40GB Storage</li><br><br>**2 Peers** <br>2 x (3.7 VPC/8.4GB RAM/ 750 GB Storage) <br>=<br> 7.4 VPC/16.8 GB RAM/ 1500 GB Storage  <ul><li> **Peer container:** 1 VPC/2GB RAM/250GB Storage</li><li>**CouchDB container:** 1 VPC/2GB RAM/500GB Storage</li><li>**Smart contract container:** 1.5 VPC/4GB RAM/0GB Storage</li><li>**Logging/gRPC Web container:** 0.2 VPC/0.4GB RAM/0GB Storage</li></ul></ul>|
| **Total Resources** |  | **VPC:** 12.8<br> **RAM:** 22.6GB<br> **Storage:** 4080GB|  **VPC:** 7.6<br> **RAM:** 17.2GB<br> **Storage:** 1540GB|
| **Total Cost** <br> (per hour) | |**IBP:** 12.8VPC x .29/hr = **$3.71 USD**  <br> **IKS***:** $2.74 USD <br> **Storage (Silver):** $1.22 USD  <br><br> **Total:** $7.67 USD/hr <br> <br> *** IKS 3 node/3 zone - 4x16 cluster with IP allocation and load balancer| **IBP:** 7.6VPC x .29/hr = $2.20 USD  <br> **IKS***:** $1.85 USD <br> **Storage:** $0.46 USD <br><br> **Total:** 2.52 USD/hr <br><br> *** IKS 2 node/3 zone - 4 x16 cluster with IP allocation and load balancer |
| **Total Cost** <br> (per month) | **HA CA PostGreSQL config:** *** <br>5 GB disk, 1 GB RAM, 10GB for backups, 1 VPC|**IBP:** $2,671 USD  <br> **IKS:** $1,972 USD <br> **Storage** (Silver):$878 USD <br> **HA CA (PostGreSql)***:** **$40 USD <br>**Storage costs for backups:**$224 USD <br><br>**Total Base Cost:** $5,785 USD|  **IBP:** $1,584 USD  <br> **IKS:** $1,332 USD <br> **Storage**(Silver):$331 USD <br> <br> **HA CA (PostGreSql)***: $40 USD <br>**Storage costs for backups:**$180 USD <br><br> **Total Base Cost:** $3,427 USD |
| **Optional upgrades** | | **HSM (when available):** +$906 USD/month <br> **LogDNA:** $1.50/GB/Month (7 day search) <br> **Sysdig:** 0.035/hour/node (up to 20 containers)|**HSM (when available):** +$906 USD/month <br> **LogDNA:** $1.50/GB/Month (7 day search) <br> **Sysdig:** 0.035/hour/node (up to 20 containers) | |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Production/Pre-Production"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}


| Environment 3 Do Not Use |  |  |  |
|:-|:-----------------|:-----------------|:-----------------|--|
|**Goal:** A relatively large test environment for evaluating an overall solution for eventual production.  <br>  <br> Includes multiple organizations and some level of HA/Performance, but not meant for production. <br><br> **Architecture assumptions:** <br><br> <ul><li>3-5 organizations</li><li>CouchDB</li><li>PostgreSQL Database</li><li>HA with possible MZR</li><li>Optional: Backup Tests, Silver Storage, HSM, LogDNA/Sysdig, Dedicated VM/HW</li></ul> | **Cost to run a network**<br><br><br><br> **5 node Raft ordering service:** <br> 5 X (1GB VPC, 1GB RAM, 500GB Storage)  <br><br> **2 CAs:** <br> 2 X (0.1 VPC, 0.2GB RAM, 20 GB Storage) <br><br>**1 Peer:** <br> 0.2GB VPC, 0.4GB RAM, 100GB Storage <br><br> _Container breakdown:_ <br> Peer: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Couch: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Couch: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Smart contract: 0.5 VPC, 1GB RAM <br> Logging/gRPC Web: 0.2 VPC, 0.4GB RAM <br> <br> **Total:** <br> 9.6 VPC <br> 13.4 GB RAM <br> 4040 GB Storage  | **Hourly Cost** <br><br> <br>**IBP:** $2.84 USD <br> <br> **IKS:** <br> $0.44 USD <br>(3) 8 x 32 nodes  <br><br> **Storage:** $0.81 USD  <br> <br> **Total:** $4.09 USD/hour | **Monthly Cost:** <br><br><br> **IBP:** $2,045 USD <br> <br> **IKS:** <br> (3) 8 x 32 nodes $1,125 USD <br><br> **Storage:** $580 USD <br> <br> **Total:** $3650 USD/month |
| | **Cost for network joiners** <br><br> <br> **1 CA:** <br> 0.1 VPC, 0.2GB RAM, 20 GB Storage <br><br>**2 Peers:** <br> 2 X (2.2GB VPC, 4.4GB RAM, 750GB Storage) <br><br> _Container breakdown:_ <br> Peer: 0.5 VPC, 0.8GB RAM, 250GB Storage <br> Couch: 0.5 VPC, 0.8GB RAM, 500GB Storage <br> Couch: 0.2 VPC, 0.4GB RAM, 25GB Storage <br> Smart contract: 1 VPC, 2GB RAM <br> Logging/gRPC Web: 0.2 VPC, 0.4GB RAM <br> <br> <br> <br>**Total:** <br> 4.5 VPC <br> 8.2 GB RAM <br> 1520 GB Storage | **Hourly Cost** <br><br> <br>**IBP:** $1.33 USD <br> <br> **IKS:** <br> $0.25 USD <br>(3) 4 x 16 nodes  <br><br> **Storage:** $0.31 USD  <br> <br> **Total:** $1.89 USD/hour | **Monthly Cost** <br><br><br> **IBP:** $960 USD <br> <br> **IKS:**  $630 USD <br> (3) 4 x 16 nodes <br><br> **Storage:** $220 USD <br> <br> **Total:** $1810 USD/month |
{: caption="Table 1. Pricing Scenarios" caption-side="top"}
{: #simpletabtable5}
{: tab-title="Pilot V2"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}
