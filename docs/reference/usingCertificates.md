---
title:  Using Certificates with Images
---
<!--
# Using Certificates with Images

This page provides details for using certificates with the Ping Identity images. Specifically, it outlines the preferred locations to place the certificate and PIN/key files to provide best security practices and enable use by the underlying Ping Identity product.

Currently, certificates can be provided to the PingData products when the containers are started.
-->

# イメージの証明書の使用

このページでは、Ping Identity イメージで証明書を使用する方法の詳細を説明します。具体的には、ベスト セキュリティ プラクティスを提供し、基盤となる Ping Identity 製品での使用を可能にするために、証明書と PIN/キー ファイルを配置するための推奨場所の概要を説明します。

現在、コンテナーの起動時に証明書を PingData 製品に提供できます。

<!--
## Before you begin

You must:

* Complete [Get started](../get-started/introduction.md) to set up your DevOps environment and run a test deployment of the products.
* Strongly recommended: Have a secrets management system, such as Hashicorp Vault, that holds your certificate and places them into your SECRETS_DIR (/run/secrets).

  For information on using a vault, if you have one, see [Using Hashicorp Vault](../how-to/usingVault.md).
-->

## はじめる前に

以下のことを必ず実行してください。

- [Get started](../get-started/introduction.md)を完了して、DevOps 環境をセットアップし、製品のテスト展開を実行します。
- 強く推奨: 証明書を保持し、それを SECRETS_DIR (/run/secrets) に配置する、Hashicorp Vault などのシークレット管理システムを用意します。

  vaultを持っている場合の使用方法については、[Using Hashicorp Vault](../how-to/usingVault.md)を参照してください。

<!--
## About this topic

The following examples explain how to deploy a certificate/PIN combination to an image in a secure way.
-->

## このトピックについて

次の例では、証明書と PIN の組み合わせを安全な方法でイメージに展開する方法を説明します。

<!--
## PingData Image Certificates

The PingData products (PingDirectory, PingDataSync, PingAuthorize, and PingDirectoryProxy) use a file location to determine certificates/PIN files:

* It is best practice to use a non-persistent location, such as /run/secrets, to store these files.
* If no certificate is provided, the container/product will generate a self-signed certificate.

The default location for certificates and associated files are listed below, assuming a default SECRETS_DIR variable of `/run/secrets`.

|                      | Variable Used        | Default Location/Value<br>/run/secrets... | Notes                                                |
| -------------------- | -------------------- | ----------------------------------------- | ---------------------------------------------------- |
| Keystore (JKS)       | KEYSTORE_FILE        | keystore                                  | Java KeyStore (JKS) Format. Set as default in absence of .p12 suffix. |
| Keystore (PKCS12)    | KEYSTORE_FILE        | keystore.p12                              | PKCS12 Format                                        |
| Keystore Type        | KEYSTORE_TYPE        | jks, pkcs12, pem, or bcfks                | Based on suffix of KEYSTORE_FILE.<br>Only use BCFKS in FIPS mode. |
| Keystore PIN         | KEYSTORE_PIN_FILE    | keystore.pin                              |                                                      |
| Truststore (JKS)     | TRUSTSTORE_FILE      | truststore                                | Set as default in absence of .p12 suffix.            |
| Truststore (PKCS12)  | TRUSTSTORE_FILE      | truststore.p12                            | PKCS12 Format                                        |
| Truststore Type      | TRUSTSTORE_TYPE      | jks, pkcs12, pem, or bcfks                | Based on suffix of TRUSTSTORE_FILE.<br>Only use BCFKS in FIPS mode. |
| Truststore PIN       | TRUSTSTORE_PIN_FILE  | truststore.pin                            |                                                      |
| Certificate Nickname | CERTIFICATE_NICKNAME | see below                                 |                                                      |

!!! note "CERTIFICATE_NICKNAME Setting"
    There is an additional certificate-based variable used to identity the certificate alias used within the `KEYSTORE_FILE`.
    That variable is called `CERTIFICATE_NICKNAME`, which identifies the certificate to use by the server in the `KEYSTORE_FILE`.
    If a value is not provided, the container will look at the list certs found in the `KEYSTORE_FILE` and if one - and only one - certificate is found of type `PrivateKeyEntry`, that alias will be used.

!!! example "Specifying your own location for a certificate"
    If you are relying on certificates to be mounted to a different locations than the SECRET_DIR location or a different filename, you can provide your own values for those variables identified above. As an example:

    ```properties
    KEYSTORE_FILE=/my/path/to/certs/cert-file
    KEYSTORE_PIN_FILE=/my/path/to/certs/cert.pin
    KEYSTORE_TYPE=jks
    CERTIFICATE_NICKNAME=development-cert
    ```
-->

## PingData イメージ証明書

PingData 製品 (PingDirectory、PingDataSync、PingAuthorize、および PingDirectoryProxy) は、ファイルの場所を使用して証明書/PIN ファイルを決定します。

- これらのファイルを保存するには、/run/secrets などの非永続的な場所を使用することがベスト プラクティスです。
- 証明書が提供されない場合、コンテナ/製品は自己署名証明書を生成します。

デフォルトの SECRETS_DIR 変数が `/run/secrets` であると仮定して、証明書および関連ファイルのデフォルトの場所を以下に示します。

|                      | Variable Used        | Default Location/Value<br>/run/secrets... | Notes                                                |
| -------------------- | -------------------- | ----------------------------------------- | ---------------------------------------------------- |
| Keystore (JKS)       | KEYSTORE_FILE        | keystore                                  | Java KeyStore (JKS) 形式。.p12 接尾辞がない場合はデフォルトとして設定されます。 |
| Keystore (PKCS12)    | KEYSTORE_FILE        | keystore.p12                              | PKCS12 形式 |
| Keystore Type        | KEYSTORE_TYPE        | jks, pkcs12, pem, or bcfks                | KEYSTORE_FILE のサフィックスに基づきます。<br />BCFKS は FIPS モードでのみ使用してください。 |
| Keystore PIN         | KEYSTORE_PIN_FILE    | keystore.pin                              |                                                      |
| Truststore (JKS)     | TRUSTSTORE_FILE      | truststore                                | .p12 接尾辞がない場合はデフォルトとして設定されます。|
| Truststore (PKCS12)  | TRUSTSTORE_FILE      | truststore.p12                            | PKCS12 形式                                        |
| Truststore Type      | TRUSTSTORE_TYPE      | jks, pkcs12, pem, or bcfks                | TRUSTSTORE_FILE のサフィックスに基づきます。<br />BCFKS は FIPS モードでのみ使用してください。 |
| Truststore PIN       | TRUSTSTORE_PIN_FILE  | truststore.pin                            |                                                      |
| Certificate Nickname | CERTIFICATE_NICKNAME | see below                                 |                                                      |

!!! note "CERTIFICATE_NICKNAME 設定"
    `KEYSTORE_FILE` 内で使用される証明書の別名を識別するために使用される追加の証明書ベースの変数があります。
    この変数は `CERTIFICATE_NICKNAME` と呼ばれ、サーバーが `KEYSTORE_FILE` で使用する証明書を識別します。
    値が指定されていない場合、コンテナーは `KEYSTORE_FILE` で見つかった証明書のリストを調べ、`PrivateKeyEntry` タイプの証明書が 1 つだけ見つかった場合は、そのエイリアスが使用されます。

!!! example "証明書の独自の場所の指定"
    SECRET_DIR の場所または別のファイル名とは異なる場所に証明書がマウントされることに依存している場合は、上記で特定した変数に独自の値を指定できます。例として:

    ```properties
    KEYSTORE_FILE=/my/path/to/certs/cert-file
    KEYSTORE_PIN_FILE=/my/path/to/certs/cert.pin
    KEYSTORE_TYPE=jks
    CERTIFICATE_NICKNAME=development-cert
    ```

<!--
## PingData image certificate rotation
The certificate rotation process for PingData products varies depending on which product is being configured and whether that product is in a topology. For products that are not in a topology, certificates can be rotated by simply updating the environment variables. For products in a topology, certificate rotation must be done via a command-line call with the servers in the topology online.
-->

## PingData イメージ証明書のローテーション

PingData 製品の証明書ローテーション プロセスは、どの製品が構成されているか、またその製品がトポロジに含まれているかどうかによって異なります。トポロジーに含まれていない製品の場合、環境変数を更新するだけで証明書をローテーションできます。トポロジ内の製品の場合、証明書のローテーションは、トポロジ内のサーバーをオンラインにしてコマンド ライン呼び出しを介して実行する必要があります。

<!--
### Rotating the listener certificate by adjusting environment variables
The process described in this section can be used for PingAuthorize, PingDirectoryProxy, and *standalone* (single-server) instances of PingDirectory or PingDataSync.

!!! warning
    If PingDirectory or PingDataSync is deployed with multiple servers, use the process described in the next section.

As mentioned above, for the PingData products there are variables defining the server truststore and keystore. To change certificates, you will need to update the contents of the truststore or keystore in your server profile or secret store. After you update the contents, restart the container. The changes will be picked up automatically when the server restarts. If you have multiple certificates in the keystore, you can use the above-mentioned CERTIFICATE_NICKNAME variable to specify the certificate. The container will pick up that certificate from those stored in the keystore. For updating the product to use the new certificates, perform a rolling update. This action ensures that other servers will remain available as each pod is cycled.

!!! note "Rolling Update"
    Verify that remaining pods in the cluster have sufficient capacity to handle the increased load during the rolling update.
-->

### 環境変数を調整してリスナー証明書をローテーションする

このセクションで説明するプロセスは、PingAuthorize、PingDirectoryProxy、および PingDirectory または PingDataSync のスタンドアロン (単一サーバー) インスタンスに使用できます。

!!! warning
    PingDirectory または PingDataSync が複数のサーバーで展開されている場合は、次のセクションで説明するプロセスを使用します。

前述したように、PingData 製品には、サーバーのトラストストアとキーストアを定義する変数があります。証明書を変更するには、サーバー プロファイルまたはシークレット ストア内のトラストストアまたはキーストアの内容を更新する必要があります。内容を更新したら、コンテナを再起動します。変更はサーバーの再起動時に自動的に取得されます。キーストアに複数の証明書がある場合は、前述の CERTIFICATE_NICKNAME 変数を使用して証明書を指定できます。コンテナーは、キーストアに保存されている証明書からその証明書を取得します。新しい証明書を使用するように製品を更新するには、ローリング アップデートを実行します。このアクションにより、各ポッドが循環しても他のサーバーが利用可能な状態が維持されます。

!!! note "ローリングアップデート"
    クラスター内の残りのpodに、ローリング アップデート中に増加する負荷を処理するのに十分な容量があることを確認します。

<!--
### Rotating the listener certificate with the replace-certificate command-line tool
If multiple PingDataSync or PingDirectory servers are running in a topology, then the servers must be online when updating the listener certificate. Updates to certificates with one or more servers offline (such as rolling updates) can lead to connection issues with the other members of the topology when those servers come back online. Use the PingData `replace-certificate` command-line tool to update certificates with the server online.

Shell into the running instance that needs to be updated, and ensure the keystore containing the needed certificate is mounted on the container. Then, run `replace-certificate`. Replace the `--key-manager-provider` and `--trust-manager-provider` values if necessary when using a non-JKS keystore, as well as the `--source-certificate-alias` value if necessary.

```
replace-certificate replace-listener-certificate \
    --key-manager-provider JKS \
    --trust-manager-provider JKS \
    --source-key-store-file /run/secrets/newkeystore \
    --source-key-store-password-file /run/secrets/newkeystore.pin \
    --source-certificate-alias server-cert \
    --reload-http-connection-handler-certificates
```

For more information on this command, run
```
replace-certificate replace-listener-certificate --help
```

Running the first command will replace the listener certificate and notify other servers in the topology that this server's certificate has changed.

To update certificates for the other servers in the topology, follow this same process, shelling into each individual instance.

Once this is done, the running pods have been updated. To ensure a restart does not undo these changes, verify that your server profile and orchestration environment variables are updated to point to the new certificates. For example, if you have modified your server configuration to point to `/run/secrets/newkeystore`, then you must update your KEYSTORE_FILE environment variable to point to that new keystore *after* you have completed the `replace-certificate` process on each server.
-->

### replace-certificate コマンドライン ツールを使用したリスナー証明書のローテーション

トポロジ内で複数の PingDataSync または PingDirectory サーバーが実行されている場合、リスナー証明書を更新するときにサーバーがオンラインである必要があります。 1 つ以上のサーバーがオフラインの状態で証明書を更新すると (ローリング アップデートなど)、それらのサーバーがオンラインに戻ったときにトポロジの他のメンバーとの接続の問題が発生する可能性があります。 PingData `replace-certificate` コマンド ライン ツールを使用して、サーバーをオンラインで証明書を更新します。

更新する必要がある実行中のインスタンスにシェルを実行し、必要な証明書を含むキーストアがコンテナーにマウントされていることを確認します。次に、`replace-certificate` を実行します。非 JKS キーストアを使用する場合は、必要に応じて `--key-manager-provider` および `--trust-manager-provider` の値を置き換えます。また、必要に応じて `--source-certificate-alias` の値も置き換えます。

```
replace-certificate replace-listener-certificate \
    --key-manager-provider JKS \
    --trust-manager-provider JKS \
    --source-key-store-file /run/secrets/newkeystore \
    --source-key-store-password-file /run/secrets/newkeystore.pin \
    --source-certificate-alias server-cert \
    --reload-http-connection-handler-certificates
```

このコマンドの詳細については、次を実行してください。

```
replace-certificate replace-listener-certificate --help
```

最初のコマンドを実行すると、リスナー証明書が置き換えられ、このサーバーの証明書が変更されたことがトポロジ内の他のサーバーに通知されます。

トポロジ内の他のサーバーの証明書を更新するには、これと同じプロセスに従い、個々のインスタンスにシェルを実行します。

これが完了すると、実行中のポッドが更新されます。再起動によってこれらの変更が元に戻らないようにするには、サーバー プロファイルとオーケストレーション環境変数が新しい証明書を指すように更新されていることを確認してください。たとえば、 `/run/secrets/newkeystore` を指すようにサーバー構成を変更した場合、各サーバーで `replace-certificate` プロセスを完了した**後**、その新しいキーストアを指すように KEYSTORE_FILE 環境変数を更新する必要があります。

<!--
## Non-PingData image certificates

For non-PingData images, such as PingAccess and PingFederate, the certificates are managed within the product configurations.
-->

## 非 PingData イメージ証明書

PingAccess や PingFederate などの非 PingData イメージの場合、証明書は製品構成内で管理されます。
