---

copyright:
  years: 2019
lastupdated: "2019-06-18"

keywords: FAQs, can I, upgrade, what version, peer ledger database, supported languages, why do I, regions

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:faq: data-hd-content-type='faq'}
{:pre: .pre}

# Domande frequenti (FAQ)
{: #ibp-v2-faq}

**Generale**   

- [Qual è il valore dell'utilizzo di {{site.data.keyword.blockchainfull_notm}} Platform sull'Hyperledger Fabric nativo?](#ibp-v2-faq-v2-IBP-Overview-1-7)
- [Quale versione di Hyperledger Fabric viene utilizzata con {{site.data.keyword.blockchainfull_notm}} Platform?](#ibp-v2-faq-v2-Hyperledger-Fabric-3-1)
- [Quale database utilizza i peer per il proprio libro mastro?](#ibp-v2-faq-v2-IBP-Overview-1-3)
- [Quali linguaggi sono supportati per gli smart contract?](#ibp-v2-faq-v2-IBP-Overview-1-4)
- [Supportate l'utilizzo di certificati da CA (Certificate Authority) non IBM?](#ibp-v2-faq-v2-external-certs)  

**{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}**  

- [Posso eseguire l'upgrade dalla V1.0 al nuovo {{site.data.keyword.blockchainfull_notm}} Platform? ](#ibp-v2-faq-v2-IBP-Overview-1-5)
- [Cosa succede quando elimino il mio servizio {{site.data.keyword.blockchainfull_notm}} Platform? ](#ibp-v2-faq-v2-IBP-Overview-1-8)
- [Quali regioni sono disponibili per il servizio blockchain in esecuzione su {{site.data.keyword.cloud_notm}}? ](#ibp-v2-faq-v2-IBP-Overview-1-9)
- [Posso utilizzare il mio cluster {{site.data.keyword.cloud_notm}} Kubernetes Service esistente?](#ibp-v2-faq-v2-Infrastructure-4-2)
- [Abbiamo accesso ai servizi di registrazione e quali log sono disponibili per me?](#ibp-v2-faq-v2-Logging-and-Monitoring-11-6)  

**{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud**    

- [Quali sono i vantaggi di {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-benefits)
- [Quali ambienti non supportano {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-environments)
- [Qual è il modello di prezzo per {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-pricing)
- [Quali servizi devo installare per utilizzare {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?](#ibp-v2-faq-icp-services)
- [Come posso stimare i requisiti di dimensionamento di {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud per i miei ambienti di sviluppo, verifica e produzione?](#ibp-v2-faq-icp-sizing)
- [Cosa succede ai miei componenti blockchain quando elimino la mia release Helm?](#ibp-v2-faq-icp-delete)

## Qual è il valore dell'utilizzo di {{site.data.keyword.blockchainfull_notm}} Platform sull'Hyperledger Fabric nativo?
{: #ibp-v2-faq-v2-IBP-Overview-1-7}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform consente ai client di distribuire facilmente una rete blockchain personalizzata. Puoi utilizzare un'IU della console intuitiva per distribuire velocemente la tua rete, installare e istanziare facilmente gli smart contract e monitorare le tue transazioni.

## Quale versione di Hyperledger Fabric viene utilizzata con {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-Hyperledger-Fabric-3-1}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}} e {{site.data.keyword.blockchainfull_notm}} Platform for Platform for Multicloud utilizzano Hyperledger Fabric v1.4.1

## Quale database utilizza i peer per il proprio libro mastro?
{: #ibp-v2-faq-v2-IBP-Overview-1-3}
{: faq}

Tutti i peer che vengono distribuiti con {{site.data.keyword.blockchainfull_notm}} Platform utilizzano CouchDB come database per il libro mastro.

## Quali linguaggi sono supportati per gli smart contract?
{: #ibp-v2-faq-v2-IBP-Overview-1-4}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform supporta gli smart contract scritti in Go e Node.js. Il nuovo modello di programmazione di Hyperledger Fabric al momento supporta solo Node.js, ma ulteriori linguaggi saranno disponibili in futuro. Se sei interessato a preservare il tuo codice applicazione esistente o a utilizzare gli SDK Fabric per linguaggi diversi da Node.js, puoi ancora connetterti alla tua rete {{site.data.keyword.blockchainfull_notm}} Platform utilizzando le API SDK Fabric di livello inferiore.

## Supportate l'uso di certificati da CA (Certificate Authority) non IBM?
{: #ibp-v2-faq-v2-external-certs}
{: faq}

Sì, puoi portare dei tuoi certificati, a condizione che siano emessi da una CA conforme a X.509.

## Posso eseguire l'upgrade dalla V1.0 al nuovo {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-IBP-Overview-1-5}
{: faq}

Non per il piano Starter. Non puoi eseguire l'upgrade dal piano Starter al nuovo {{site.data.keyword.blockchainfull_notm}} Platform.
Per il piano Enterprise, potrai eseguire l'upgrade al nuovo {{site.data.keyword.blockchainfull_notm}} Platform in futuro. Il proprietario dell'account riceverà un'email dal team di {{site.data.keyword.blockchainfull_notm}} Platform per assistenza.

## Cosa succede quando elimino il mio servizio {{site.data.keyword.blockchainfull_notm}} Platform?
{: #ibp-v2-faq-v2-IBP-Overview-1-8}
{: faq}

Quando elimini un'istanza del servizio {{site.data.keyword.blockchainfull_notm}} Platform, tutti nodi di ordine, i peer e le CA della blockchain vengono eliminati insieme alla loro archiviazione associata.

## Quali regioni sono disponibili per il servizio blockchain in esecuzione su {{site.data.keyword.cloud_notm}}?
{: #ibp-v2-faq-v2-IBP-Overview-1-9}
{: faq}

Le regioni disponibili per {{site.data.keyword.blockchainfull_notm}} Platform sono elencate nelle [Ubicazioni {{site.data.keyword.blockchainfull_notm}} Platform](/docs/services/blockchain?topic=blockchain-ibp-regions-locations). Tieni presente che devi creare un cluster {{site.data.keyword.cloud_notm}} Kubernetes Service nella stessa regione in modo che il servizio blockchain riconosca il cluster. Ulteriori regioni saranno disponibili a breve.

## Posso utilizzare il mio cluster {{site.data.keyword.cloud_notm}} Kubernetes Service esistente?
{: #ibp-v2-faq-v2-Infrastructure-4-2}
{: faq}

Il tuo cluster Kubernetes esistente funzionerà con {{site.data.keyword.blockchainfull_notm}} Platform finché soddisfa le seguenti condizioni:
- Sta eseguendo Kubernetes v1.11 o una versione stabile successiva.
- Ci sono risorse sufficienti nel cluster.

## Abbiamo accesso ai servizi di registrazione e quali log sono disponibili per me?
{: #ibp-v2-faq-v2-Logging-and-Monitoring-11-6}
{: faq}

Con {{site.data.keyword.blockchainfull_notm}} Platform, puoi ora accedere direttamente ai tuoi log peer, CA e nodo di ordinazione dal tuo dashboard Kubernetes. Ti consigliamo di avvalerti del servizio {{site.data.keyword.cloud_notm}} LogDNA che ti consente di analizzare facilmente i log in tempo reale.

## Quali sono i vantaggi di {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-benefits}
{: faq}

Consulta questo argomento su [cosa offre  {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud](/docs/services/blockchain?topic=blockchain-console-icp-about#what-ibm-blockchain-platform-for-ibm-cloud-private-offers).

## Quali ambienti supporta {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-environments}
{: faq}

{{site.data.keyword.blockchainfull_notm}} Platform for Multicloud supporta tutti gli ambienti supportati da {{site.data.keyword.cloud_notm}} Private v3.2. Per ulteriori informazioni, vedi [Supported operating systems and platforms](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.2.0/supported_system_config/supported_os.html){: external}.

## Qual è il modello di prezzo per {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-pricing}
{: faq}

Per {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud, la [concessione di licenze](/docs/services/blockchain?topic=blockchain-ibp-software-pricing) è basata sulla quantità di CPU (VPC) resa disponibile per il prodotto. Per i dettagli, [ti invitiamo a contattare IBM](https://www.ibm.com/account/reg/us-en/signup?formid=urx-37672){: external}.

## Quali servizi devo installare per utilizzare {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud?
{: #ibp-v2-faq-icp-services}
{: faq}

Devi installare solo {{site.data.keyword.cloud_notm}} Private v3.2.

## Come posso stimare i requisiti di dimensionamento di {{site.data.keyword.blockchainfull_notm}} Platform for Multicloud per i miei ambienti di sviluppo, verifica e produzione?
{: #ibp-v2-faq-icp-sizing}
{: faq}

Una volta compreso quanti CA, peer e nodi di ordine sono necessari, puoi esaminare la [tabella di allocazioni delle risorse predefinite](/docs/services/blockchain?topic=blockchain-icp-console-setup#icp-console-setup-resources) per ottenere una stima approssimativa delle CPU (VPC) necessarie per la tua rete.

## Cosa succede ai miei componenti blockchain quando elimino la mia release Helm?
{: #ibp-v2-faq-icp-delete}
{: faq}

Quando elimini la release Helm dal tuo cluster {{site.data.keyword.cloud_notm}} Private, i componenti blockchain associati non vengono eliminati. Per rimuovere correttamente una release Helm dal tuo cluster, dovresti prima eliminare tutti i tuoi componenti utilizzando la console o le API blockchain. In seguito puoi eliminare il grafico Helm.  
