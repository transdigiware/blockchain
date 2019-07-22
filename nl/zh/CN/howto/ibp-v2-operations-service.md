---

copyright:
  years: 2019
lastupdated: "2019-05-31"

keywords: logging levels, metrics, health check, peer, orderer

subcollection: blockchain

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}

# 使用操作服务操作节点
{: #operations_service}

{{site.data.keyword.blockchainfull}} Platform 基于 Hyperledger Fabric V1.4.1。该 Platform 支持“操作服务”功能，此功能为操作员提供 RESTful“操作”API，用于执行节点运行状况检查，从同级和排序节点中拉取操作度量值，以及管理日志记录级别。同级和排序节点托管提供 RESTful“操作”API 的 HTTP 服务器。有关“操作服务”的更多信息，请参阅 [The Operations Service](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html){: external}。
{:shortdesc}


## 注意事项和限制
{: #operations_service_consideration_limitation}

- 所有同级和排序节点都配置有 `clientAuthRequired: false`，以便可以访问运行状况检查程序。由于 `clientAuthRequired` 设置为 `false`，并且还启用了双向 TLS，因此在访问 REST 服务器时，需要传递 TLS 身份才能进行认证。此设置可确保只有具有相应密钥的用户才能查看对应的日志。
- 目前仅支持基于 [Prometheus](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#prometheus){: external} 的度量值拉取模型。

## 开始之前
{: #operations_service_before_you_begin}

您需要从同级和排序节点收集以下信息。

- **`peer-endpoint`** 或 **`orderer-endpoint`**

  可以在通过控制台导出的同级或排序节点 JSON 文件中，查找同级或排序节点的操作端点 URL。

    1. 在**节点**选项卡中，单击同级或排序节点。
    2. 在同级或排序节点页面上，单击同级或排序节点名称旁边的**设置**图标。
    3. 在侧面板中，单击**导出**以保存同级或排序节点 JSON 文件。
    4. 在 JSON 文件中查找操作端点 URL，即 `operations_url` 参数的值。此值在本主题稍后的命令中称为 `peer-endpoint` 或 `orderer-endpoint`。例如：

      ```
      {
      "short_name": "Peer1Org1_0",
      "name": "Peer1 Org1",
      "url": "https://169.46.208.93:32739",
      "type": "fabric-peer",
      "msp_id": "org1msp",
      "operations_url": "https://169.46.208.93:32101"
      }
      ```

      ```
      {
      "short_name": "Orderer_0",
      "name": "Orderer",
      "url": "https://169.46.208.93:31612",
      "type": "fabric-orderer",
      "msp_id": "orderermsp",
      "operations_url": "https://169.46.208.93:30115"
      }
      ```

- **`client-tls-cert`** 和 **`client-tls-key`**

  可以在控制台中查找 TLS CA 的证书和专用密钥。

  1. 在**节点**选项卡中，单击同级或排序节点的 CA 节点。
  2. 在 CA 页面上，单击 **TLS 认证中心**。
  3. 在 TLS CA 表中，找到 `admin` 用户，然后单击**操作**下的垂直点。接着，单击**注册身份**。
  4. 在侧面板中，单击**下一步**，您可以看到 TLS CA 的证书和专用密钥。
  5. 为身份提供显示名称，并单击**导出身份**，然后单击**向电子钱包添加身份**，以将 TLS CA 的证书和专用密钥保存到 JSON 文件中。
  6. 打开导出的 JSON 文件。
  7. 在 JSON 文件中查找专用密钥，即 `private_key` 参数的值。这是将在下面的各命令中使用的 `client-tls-key`。
  8. 在 JSON 文件中查找 TLS CA 证书，即 `cert` 参数的值。这是将在下面的各命令中使用的 `client-tls-cert`。

- **`peer tls-ca cert`** 或 **`orderer tls-ca cert`**

  可以在控制台中查找同级或排序节点的 TLS CA 证书。

  1. 在**节点**选项卡中，单击同级或排序节点的节点。
  2. 单击节点名旁边的**设置**图标。
  3. 复制节点的 TLS CA 证书字符串。这是可以在下面的各命令中使用的 `peer tls-ca cert` 或 `orderer tls-ca cert`。

**注：**对于所有证书和密钥，必须使用以下命令将证书字符串（`base64` 格式）转换为单独的 `.pem` 文件：
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  echo <base64_string> | base64 --decode $FLAG > <key_name>.pem
  ```
  {:codeblock}
{:important}


## 检查节点运行状况
{: #operations_service_health_check}

运行 `curl -k <peer-endpoint>/healthz` 或 `curl -k <orderer-endpoint>/healthz` 命令来检查同级或排序节点的运行状况。例如：

```
curl -k https://169.46.208.93:3210/healthz
```
{:codeblock}

有关运行状况检查的更多信息，请参阅[运行状况检查](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#health-checks){: external}。


## 查看度量值
{: #operations_service_view_metrics}

运行以下命令以查看同级度量值。要查看排序节点度量值，请运行相同的命令，但将其中的 `<peer-endpoint>` 替换为 `<orderer-endpoint>`。

```
curl -k <peer-endpoint>/metrics --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

例如：

```
curl -k https://169.55.231.152:30766/metrics --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```
{:codeblock}


有关度量值的更多信息，请参阅[度量值](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#metrics){: external}。


## 查看日志记录级别
{: #operations_service_log_level_view}

运行以下命令以查看日志记录级别。命令完成后，您将在终端上看到日志级别。

```
curl -k <peer-endpoint>/logspec --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

例如：
```
curl -k https://169.46.208.93:3210/logspec --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/keystore/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```

您可以看到与以下示例类似的结果：

```
{"spec":"info"}
```

## 设置日志记录级别
{: #operations_service_log_level_set}

要更改现有的日志记录级别设置，请运行以下命令，其中使用 `PUT` 方法以及包含名为 `spec` 的单个属性的 JSON 主体。将 `<log-level>` 替换为所需的日志记录级别。有关可以设置的日志记录级别的更多信息，请参阅 Hyperledger Fabric 文档中的 [Logging specification](https://hyperledger-fabric.readthedocs.io/en/release-1.4/logging-control.html#logging-specification){: external}。

```
curl -X PUT  <peer-endpoint>/logspec -d '{"spec":"<log-level>"}' --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

例如：
```
curl -X PUT  https://169.46.208.93:3210/logspec -d '{"spec":"chaincode=debug:info"}' --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/keystore/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```

设置新的日志记录级别后，可以使用[查看日志记录级别命令](#operations_service_log_level_view)来检查设置。

有关日志级别配置的更多信息，请参阅 Hyperledger Fabric 文档中的 [Log Level Management](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#log-level-management){: external}。
