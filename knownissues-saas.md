---

copyright:
  years: 2020

lastupdated: "2020-09-22"

subcollection: blockchain

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:external: target="_blank" .external}

# Known issues
{: #known-issues-saas}

This page describes known issues that you might encounter when you use {{site.data.keyword.blockchainfull_notm}} Platform for {{site.data.keyword.cloud_notm}}.
{:shortdesc}

<blockchain-sw-251>

## Smart contract instantiation timeout
{: #sw-known-issues-instantiation-timeout}

When running the {{site.data.keyword.blockchainfull}} Platform on s390x architecture, it is possible that Node.js smart contract instantiation can fail. Customers should wait for five minutes after the failure occurs and then retry the instantiation again. It will then work successfully on the subsequent attempt.

</blockchain-sw-251>

