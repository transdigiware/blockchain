---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-22"

keywords: catalina, chrome, external CA, TLS, orderer, error, multicloud

subcollection: blockchain-sw-251

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# Known issues
{: #sw-known-issues}

This page describes known issues that you might encounter when you use {{site.data.keyword.blockchainfull_notm}} Platform 2.5.1.
{:shortdesc}

<div style="background-color: #f4f4f4; padding-left: 20px; border-bottom: 2px solid #0f62fe; padding-top: 12px; padding-bottom: 4px; margin-bottom: 16px;">
  <p style="line-height: 10px;">
    <strong>Running a different version of IBM Blockchain Platform?</strong> Switch to version
    <a href="/docs/blockchain-sw?topic=blockchain-sw-sw-known-issues">2.1.2</a>,
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-sw-known-issues">2.1.3</a>
    <a href="/docs/blockchain-sw-25?topic=blockchain-sw-25-sw-known-issues">2.5</a>
    </p>
</div>


<blockchain-sw-251>

## Smart contract instantiation timeout
{: #sw-known-issues-instantiation-timeout}

When running the {{site.data.keyword.blockchainfull}} Platform on s390x architecture, it is possible that Node.js smart contract instantiation can fail. Customers should wait for five minutes after the failure occurs and then retry the instantiation again. It will then work successfully on the subsequent attempt.

</blockchain-sw-251>

