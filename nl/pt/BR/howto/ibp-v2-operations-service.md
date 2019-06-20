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

# Nós operacionais com serviço de operações
{: #operations_service}

O {{site.data.keyword.blockchainfull}} Platform é baseado no Hyperledger Fabric v1.4.1. A plataforma suporta o recurso de serviço de operações que oferece uma API de "operações" de RESTful para que os operadores executem verificações de funcionamento do nó, façam pull de métricas operacionais dos nós de peer e de solicitador e gerenciem níveis de criação de log. O peer e o host do solicitador hospedam um servidor HTTP que oferece a API de “operações” RESTful.  Para obter mais informações sobre o serviço de operações, consulte [O serviço de operações](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html){: external}.
{:shortdesc}


## Considerações e limitações
{: #operations_service_consideration_limitation}

- Todos os nós de peer e solicitador são configurados com `clientAuthRequired: false` para que o verificador de funcionamento possa ser acessado. Como `clientAuthRequired` é configurado como `false` e também o TLS mútuo é ativado, quando você acessa o servidor REST, é necessário passar as identidades TLS para poder autenticar. Essa configuração assegura que somente os usuários com as chaves apropriadas possam ver os logs correspondentes.
- Apenas as métricas que fazem pull do modelo baseado em [Prometheus](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#prometheus){: external} são suportadas por enquanto.

## Antes de iniciar
{: #operations_service_before_you_begin}

É necessário reunir as informações a seguir de seu peer e solicitador.

- **`peer-endpoint`** ou **`orderer-endpoint`**

  É possível localizar a URL do terminal de operações do peer ou solicitador no arquivo JSON do peer ou solicitador que você exporta do console.

    1. Clique no peer or solicitador na guia **Nó**.
    2. Na página do peer ou solicitador, clique no ícone **Configurações** além do nome do peer ou solicitador.
    3. No painel lateral, clique em **Exportar** para salvar o arquivo JSON do peer ou solicitador.
    4. Localize a URL do terminal de operações que é o valor do parâmetro `operations_url` no arquivo JSON. Esse valor é referido como o `peer-endpoint` ou `orderer-endpoint` nos comandos posteriormente neste tópico. Por exemplo:

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

- **`client-tls-cert`** e **`client-tls-key`**

  É possível localizar o certificado e a chave privada da CA TLS no console.

  1. Clique no nó de CA do peer ou solicitador na guia **Nó**.
  2. Na página CA, clique em **Autoridade de certificação TLS**.
  3. Na tabela CA TLS, localize o usuário `admin` e clique nos pontos verticais em **Ações**. Em seguida, clique em **Inscrever identidade**.
  4. No painel lateral, clique em **Avançar** e será possível ver o certificado e a chave privada da CA TLS.
  5. Forneça à identidade um nome de exibição e clique em **Exportar identidade** e, em seguida, **Incluir identidade na carteira eletrônica** para salvar o certificado e a chave privada da CA TLS em um arquivo JSON.
  6. Abra o arquivo JSON exportado.
  7. Localize a chave privada que é o valor do parâmetro `private_key` no arquivo JSON. Esse é seu `client-tls-key` para uso nos comandos abaixo.
  8. Localize o certificado de CA TLS que é o valor do parâmetro `cert` no arquivo JSON. Esse é o seu `client-tls-cert` para uso nos comandos abaixo.

- **`peer tls-ca cert`** ou **`orderer tls-ca cert`**

  É possível localizar o certificado de CA TLS do peer ou solicitador no console.

  1. Clique no nó do peer ou solicitador na guia **Nó**.
  2. Clique no ícone **Configurações** ao lado do nome do nó.
  3. Copie a sequência de certificados de CA TLS do nó. Esse é o seu `peer tls-ca cert` ou `orderer tls-ca cert` que pode ser usado nos comandos abaixo.

**Nota:** para todos os certificados e chaves, deve-se converter as sequências de certificados, que estão no formato `base64`, em arquivos `.pem` individuais usando os comandos a seguir:
  ```
  export FLAG=$(if [ "$(uname -s)" == "Linux" ]; then echo "-w 0"; else echo "-b 0"; fi)
  echo <base64_string> | base64 --decode $FLAG > <key_name>.pem
  ```
  {:codeblock}
{:important}


## Verificando o funcionamento do nó
{: #operations_service_health_check}

Execute o comando `curl -k <peer-endpoint>/healthz` ou `curl -k <orderer-endpoint>/healthz` para verificar o funcionamento do nó de peer ou solicitador. Por exemplo:

```
curl -k https://169.46.208.93:3210/healthz
```
{:codeblock}

Para obter mais informações sobre verificações de funcionamento, consulte [Verificações de funcionamento](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#health-checks){: external}.


## Visualizando as métricas
{: #operations_service_view_metrics}

Execute o comando a seguir para visualizar as métricas de peer. Para visualizar as métricas de solicitador, você executaria o mesmo comando, mas substituiria o `<peer-endpoint>` pelo `<orderer-endpoint>`.

```
curl -k <peer-endpoint>/metrics --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

Por exemplo:

```
curl -k https://169.55.231.152:30766/metrics --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```
{:codeblock}


Para obter mais informações sobre métricas, consulte [Métricas](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#metrics){: external}.


## Visualizando níveis de criação de log
{: #operations_service_log_level_view}

Execute o comando a seguir para visualizar o nível de criação de log. Você verá o nível de log em seu terminal após a conclusão do comando.

```
curl -k <peer-endpoint>/logspec --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

Por exemplo:
```
curl -k https://169.46.208.93:3210/logspec --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/keystore/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```

<!--
```
curl https://169.46.208.93:3210/logspec --cert temp/1mycluster-test-32240/msp/org1/ca/tls/msp/signcerts/cert.pem --key temp/1mycluster-test-32240/msp/org1/ca/tls/msp/keystore/3fb20abb935f88b83a8da68317a44a4fa0953d7ec6d06bb19a6fc3979a603095_sk  --cacert temp/1mycluster-test-32240/msp/org1/ca/tls/msp/cacerts/169-55-231-152-30021-tlsca.pem
```
-->

É possível ver o resultado que é semelhante ao exemplo a seguir:

```
{"spec":"info"}
```

## Configurando níveis de criação de log
{: #operations_service_log_level_set}

Para mudar a configuração de nível de criação de log existente, execute o comando a seguir, que usa o método `PUT` com o corpo JSON que consiste em um único atributo denominado `spec`. Substitua `<log-level>` por seus níveis de criação de log esperados. Para obter mais informações sobre os níveis de criação de log que você pode configurar, consulte [Especificação de criação de log](https://hyperledger-fabric.readthedocs.io/en/release-1.4/logging-control.html#logging-specification){: external} na Documentação do Hyperledger Fabric.

```
curl -X PUT  <peer-endpoint>/logspec -d '{"spec":"<log-level>"}' --cert <client-tls-cert> --key <client-tls-key> --cacert <peer tls ca-cert>
```
{:codeblock}

Por exemplo:
```
curl -X PUT  https://169.46.208.93:3210/logspec -d '{"spec":"chaincode=debug:info"}' --cert msp/org1/ca/tls/msp/signcerts/cert.pem --key msp/org1/ca/tls/msp/keystore/key.pem  --cacert msp/org1/ca/tls/msp/cacerts/tlsca.pem
```

Depois de configurar um novo nível de criação de log, será possível usar o [comando de visualização do nível de criação de log](#operations_service_log_level_view) para verificar suas configurações.

<!--
```
curl -X PUT  https://169.46.208.93:3210/logspec -d '{"spec":"chaincode=debug:info"}' --cert temp/1mycluster-test-32240/msp/org1/ca/tls/msp/signcerts/cert.pem --key temp/1mycluster-test-32240/msp/org1/ca/tls/msp/keystore/3fb20abb935f88b83a8da68317a44a4fa0953d7ec6d06bb19a6fc3979a603095_sk  --cacert temp/1mycluster-test-32240/msp/org1/ca/tls/msp/cacerts/169-55-231-152-30021-tlsca.pem
```
-->

Para obter mais informações sobre a configuração de nível de log, consulte [Gerenciamento de nível de log](https://hyperledger-fabric.readthedocs.io/en/release-1.4/operations_service.html#log-level-management){: external} na Documentação do Hyperledger Fabric.
