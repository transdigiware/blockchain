---

copyright:
  years: 2020

lastupdated: "2020-10-14"

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



## Nil pointer when creating HSM-configmap
{: known-issues-hsm-configmap}

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

This error is caused by not specifying a value for the `<IMAGE_PULL_SECRET>` in the [HSM configmap](/docs/blockchain-sw-251?topic=blockchain-sw-251-ibp-console-adv-deployment#ibp-console-adv-deployment-hsm-configmap). Even if your HSM client image is public and no image pull secret is required, you still need to provide a value of `""` for the `<IMAGE_PULL_SECRET>` field.



