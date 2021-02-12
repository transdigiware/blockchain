---

copyright:
  years: 2019, 2021
lastupdated: "2021-02-12"

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
    <a href="/docs/blockchain-sw-213?topic=blockchain-sw-213-sw-known-issues">2.1.3</a>,
    <a href="/docs/blockchain-sw-25?topic=blockchain-sw-25-sw-known-issues">2.5</a>
    </p>
</div>





## Nil pointer when creating HSM-configmap
{: #known-issues-hsm-configmap}

Creating the HSM configmap fails with the error:
```
2020-10-13T15:13:17.324077030Z  goroutine 327 [running]:
2020-10-13T15:13:17.324088954Z  k8s.io/apimachinery/pkg/util/runtime.logPanic(0x1be65c0, 0x3205df0)
2020-10-13T15:13:17.324098097Z      src/github.ibm.com/ibp/operator/vendor/k8s.io/apimachinery/pkg/util/runtime/runtime.go:74 +0xa3
2020-10-13T15:13:17.324106701Z  k8s.io/apimachinery/pkg/util/runtime.HandleCrash(0x0, 0x0, 0x0)
2020-10-13T15:13:17.324114817Z      src/github.ibm.com/ibp/operator/vendor/k8s.io/apimachinery/pkg/util/runtime/runtime.go:48 +0x82
2020-10-13T15:13:17.324122356Z  panic(0x1be65c0, 0x3205df0)
2020-10-13T15:13:17.324129558Z      /usr/local/go1.14.7b4/src/runtime/panic.go:969 +0x166
2020-10-13T15:13:17.324136882Z  github.ibm.com/ibp/operator/pkg/initializer/common/config.(*HSMConfig).BuildPullSecret(...)
2020-10-13T15:13:17.324147032Z      src/github.ibm.com/ibp/operator/pkg/initializer/common/config/hsmconfig.go:97
```

This error is caused by not specifying a value for the `<IMAGE_PULL_SECRET>` in the [HSM configmap](/docs/blockchain?topic=blockchain-ibp-console-adv-deployment#ibp-console-adv-deployment-hsm-configmap). Even if your HSM client image is public and no image pull secret is required, you still need to provide a value of `""` for the `<IMAGE_PULL_SECRET>` field.


