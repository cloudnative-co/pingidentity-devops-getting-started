<!--
# Build a PingFederate profile from your current deployment

The term "profile" can vary in many instances. Here we will focus on two types of profiles for PingFederate: configuration archive, and bulk export. We will discuss the similarities and differences between two as well as how to build either from a running PingFederate environment.
-->

# 現在のデプロイメントから PingFederate プロファイルを構築する

「プロファイル」という用語は多くの場合に異なります。ここでは、PingFederate の 2 種類のプロファイル (構成アーカイブと一括エクスポート) に焦点を当てます。 2 つの類似点と相違点、および実行中の PingFederate 環境からどちらかを構築する方法について説明します。

<!--
## Before you begin

You must:

* Complete [Get Started](../get-started/introduction.md) to set up your DevOps environment and run a test deployment of the products
* Understand our [Product Container Anatomy](../reference/config.md)

You should:

* Review [Customizing Server Profiles](profiles.md)
-->

## はじめる前に

下記のことを必ず実施してください:

* [Get Started](../get-started/introduction.md)を完了して、DevOps 環境をセットアップし、製品のテスト展開を実行します。
* [Product Container Anatomy](../reference/config.md)を理解します。

下記のことを実施することをお勧めします

* [Customizing Server Profiles](profiles.md)を確認する

<!--
## Overview of profile methods

There are two file-based profile methods that we cover:

* Bulk API Export
    * The resulting `.json` from the admin API at /bulk/export
    * Typically saved as `data.json`
* Configuration Archive
    * Pulled either from the admin UI - Server > Configuration Archive or from the admin API at `/configArchive`
    * We call the result of this output `data.zip` or the `/data` folder
-->
<!-- TODO INSERT LINK ON NEXT LINE FOR PF CONFIG DEPLOYMENTS -->
<!-- Deciding which method you use should be based on how you plan to [deploy configurations updates](LINKNEEDED) -->
<!--
A file-based profile means a "complete profile" looks like a **subset** of files that you would typically find in a running PingFederate filesystem.

This subset of files represents the minimal number of files needed to achieve your PingFederate configuration. All additional files that are not specific to your configuration should be left out because the PingFederate Docker image fills them in. For more information, see [Container Anatomy](containerAnatomy.md).

Familiarity with the PingFederate filesystem will help you achieve the optimal profile. For more information, see [profile structures](../reference/profileStructures.md).

!!! note "Save files"
    You should save every file outside of `pingfederate/server/default/data` that you've edited.

Additionally, all files that are included in the profile should also be environment agnostic. This typically means turning hostnames and secrets into variables that can be delivered from the [Orchestration Layer](profilesSubstitution.md).
-->

## プロファイルメソッドの概要

ここで取り上げるファイルベースのプロファイル方法は 2 つあります。

* APIの一括エクスポート
    * /bulk/export にある管理 API から生成された `.json`
    * 通常は `data.json` として保存されます
* 構成アーカイブ
    * 管理 UI - Server > Configuration Archive または `/configArchive` の管理 API から取得されます。
    * この出力の結果を data.zip または `/data` フォルダーと呼びます。

<!-- TODO INSERT LINK ON NEXT LINE FOR PF CONFIG DEPLOYMENTS -->
<!-- Deciding which method you use should be based on how you plan to [deploy configurations updates](LINKNEEDED) -->

ファイルベースのプロファイルとは、「完全なプロファイル」が、実行中の PingFederate ファイルシステムで通常見つかるファイルの **サブセット** のように見えることを意味します。

このファイルのサブセットは、PingFederate 構成を実現するために必要な最小限のファイル数を表します。構成に固有ではない追加ファイルはすべて、PingFederate Docker イメージによって埋められるため、省略する必要があります。詳細については、[Container Anatomy](containerAnatomy.md)を参照してください。

PingFederate ファイルシステムに精通していると、最適なプロファイルを実現するのに役立ちます。詳細については、[profile structures](../reference/profileStructures.md)を参照してください。

!!! note "ファイルを保存する"
    編集したすべてのファイルを `pingfederate/server/default/data` の外に保存する必要があります。

さらに、プロファイルに含まれるすべてのファイルも環境に依存しない必要があります。これは通常、ホスト名とシークレットを、[Orchestration Layer](profilesSubstitution.md)から配信できる変数に変換することを意味します。

<!--
## The Bulk API Export Profile Method

### About this method

You will:

1. Export a `data.json` from /bulk/export
1. Configure and run bulkconfig tool
1. Export Key Pairs
1. base64 encode exported key pairs
1. Add `data.json.subst` to your profile at `instance/bulk-config/data.json.subst`

> In this guide, we will look at the above steps in detail to understand the purpose and flow. Use the steps for reference as needed.

A PingFederate Admin Console imports a `data.json` on startup if it finds it in `instance/bulk-config/data.json`.

The PF admin API `/bulk/export` endpoint outputs a large .json blob that is representative of the entire `pingfederate/server/default/data` folder, PingFederate 'core config', or a representation of anything you would configure from the PingFedera0te UI. This file can be considered as "the configuration archive in .json format".
-->

## 一括 API エクスポート プロファイル メソッド

### この方法について

You will:

1. /bulk/export から `data.json` をエクスポートする
1. Bulkconfig ツールを構成して実行する
1. キーペアのエクスポート
1. エクスポートされたキーペアをbase64エンコードする
1. `data.json.subst` を、`instance/bulk-config/data.json.subst` のプロファイルに追加します。

> このガイドでは、目的と流れを理解するために、上記の手順を詳しく見ていきます。必要に応じて、手順を参照してください。

PingFederate 管理コンソールは、`instance/bulk-config/data.json` で `data.json` が見つかった場合、起動時にそれをインポートします。

PF 管理 API `/bulk/export` エンドポイントは、`pingfederate/server/default/data` フォルダー全体、PingFederate 'core config'、または PingFederate UI から構成するものを表す大きな .json BLOB を出力します。このファイルは、「.json 形式の構成アーカイブ」と考えることができます。

<!--
#### Steps

1. On a running PingFederate instance or pod, run:

    ```sh
    curl \
      --location \
      --request GET 'https://pingfederate-admin.ping-devops.com/pf-admin-api/v1/bulk/export' \
      --header 'X-XSRF-Header: PingFederate' \
      --user "administrator:${passsword}" > data.json
    ```

1. Save data.json into a profile at `instance/bulk-config/data.json`.
1. Delete everything except `pf.jwk` in `instance/server/default/data`.
-->

#### 手順

1. 実行中の PingFederate instanceまたはpodで、次を実行します。

    ```sh
    curl \
      --location \
      --request GET 'https://pingfederate-admin.ping-devops.com/pf-admin-api/v1/bulk/export' \
      --header 'X-XSRF-Header: PingFederate' \
      --user "administrator:${passsword}" > data.json
    ```

1. data.json を、`instance/bulk-config/data.json` のプロファイルに保存します。
1. `instance/server/default/data` 内の `pf.jwk` を除くすべてを削除します。

<!--
#### Result

You have a bulk API export "profile". This file is useful because the entire config is in a single file and if you store it in source control, then you only have to compare differences in one file. However, there is more value than being in one file.
-->

#### 結果

一括 API エクスポート「profile」があります。このファイルは、構成全体が 1 つのファイル内にあり、それをソース管理に保存すると、1 つのファイル内の相違点を比較するだけで済むため便利です。ただし、1 つのファイルに存在するよりも多くの価値があります。

<!--
### Making the bulk API export "profile-worthy"

By default, the resulting `data.json` from the export contains encrypted values, and to import this file, your PingFederate needs to have the corresponding master key (`pf.jwk`) in `pingfederate/server/default/data`.

!!! Note "Encrypted values in single file"
    In the DevOps world, we call this folder `instance/server/default/data`. However, each of the encrypted values also have the option to be replaced with an unencrypted form and, when required, a corresponding password.
-->

### 一括 API エクスポートを「プロファイルにふさわしい」ものにする

デフォルトでは、エクスポートによって生成された `data.json` には暗号化された値が含まれており、このファイルをインポートするには、PingFederate の対応するマスター キー (`pf.jwk)` が `pingfederate/server/default/data` にある必要があります。

!!! Note "単一ファイル内の暗号化された値"
    DevOpsの世界では、このフォルダーを `instance/server/default/data` と呼びます。ただし、暗号化された各値には、暗号化されていない形式と、必要に応じて対応するパスワードに置き換えるオプションもあります。

<!--
#### Example

The SSL Server Certificate from the [PingFederate Baseline Profile](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/baseline/pingfederate) when exported to data.json has the following syntax:

```json
{
    "resourceType": "/keyPairs/sslServer",
    "operationType": "SAVE",
    "items": [
        {
            "id": "sslservercert",
            "fileData": "MIIRBwIBAzCCEMAGCSqGSIb3DQEHAaCCELEEghCtMIIQqTCCCeUGCSqGSIb3DQEHAaCCCdYEggnSMIIJzjCCCcoGCyqGSIb3DQEMCgECoIIJezCCCXcwKQYKKoZIhvcNAQwBAzAbBBQu6vDERQZX3uujWa7v_q3sYN4Q0gIDAMNQBIIJSFtdWbvLhzYrTqeKKiJqiqROgE0E4mkVvmEC6NwhhPbcH37IDNvVLu0umm--CDZnEmlyPpUucO345-U-6z-cskw4TbsjYIzM10MwS6JdsyYFTC3GwqioqndVgBUzDh8xGnfzx52zEehX8d-ig1F6xYsbEc01gTbh4lF5MA7E7VfoTa4hWqtceV8PQeqzJNarlZyDSaS5BLn1J6G9BYUze-M1xGhATz7F2l-aAt6foi0mwIBlc2fwsdEPuAALZgdG-q_V4gOJW2K0ONnmWhMgMLpCL42cmSb
            ... more encrypted text ...
            Yxpzp_srpy4LHNdgHqhVBhqtDrjeKJDRfc1yk21P5PpfEBxn5MD4wITAJBgUrDgMCGgUABBQLBpq8y79Pq1TzG1Xf6OAjZzBZaQQUC4kD4CkcrH-WTQhJHud850ddn08CAwGGoA==",
            "encryptedPassword": "eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2Iiwia2lkIjoiRW1JY1UxOVdueSIsInZlcnNpb24iOiIxMC4xLjEuMCJ9..l6PJ55nSSvKHl0vSWTpkOA.i7hpnnu2yIByhyq_aGBCdaqS3u050yG8eMRGnLRx2Yk.Mo4WSkbbJyLISHq6i4nlVA"
        }
    ]
}
```

You can convert this master key dependent form to:

```json
{
    "operationType": "SAVE",
    "items": [{
        "password": "2FederateM0re",
        "fileData": "MIIRCQIBAzCCEM8GCSqGSIb3DQEHAaCCEMAEghC8MIIQuDCCC28GCSqGSIb3DQEHBqCCC2AwggtcAgEAMIILVQYJKoZIhvcNAQcBMBwGCiqGSIb3DQEMAQYwDgQIjXWLRGuGNIQCAggAgIILKOgCQ9onDqBPQsshsaS50OjWtj\/7s47BUYal1YhO70fBup1a82WGHGhAvb\/SY1yOhqQR+TloEBOPI5cExoGN\/Gvw2Mw5\/wkQZZMSHqxjz68KhN4B0hrsOf4rqShB7jsz9ebSml3r2w0sUZWR73GBtBt1Y3wIlXLS2WtqdtHra9VnUqp1eOk+xenjuWM+u2ndDD43GgKB3n8mNBSSVBqx6ne7aSRJRuAUd+HAzLvSeXjTPMObI1Jod2F+7
        ... more base64 encoded exported .p12 ...
        5QJ15OJp2iEoVBWxogKf64s2F0iIYPoo6yjNvlidZCevP564FwknWrHoD7R8cIBrhlCJQbEOpOhPg66r4MK1CeJ2poaKRlMS8HGcMRaTpaqD+pIlgmUS6xFw49vr9Kwfb7KteRsTkNR+I8A7HjUpuCMSUwIwYJKoZIhvcNAQkVMRYEFOb7g1xwDka5fJ4sqngEvzTyuWnpMDEwITAJBgUrDgMCGgUABBRlJ+D+FR\/vQbaTGbKDFiBK\/xDbqQQIAjLc+GgRg44CAggA",
        "id": "sslservercert"
    }],
    "resourceType": "/keyPairs/sslServer"
}
```

The process:

1. You exported the private key+cert of the server cert with alias `sslservercert`. When exported, a password is requested and `2FederateM0re` was used. This action results in the download of a password protected `.p12` file.
1. The data.json key name `encryptedPassword` converted to simply `password`.
1. The value for `fileData` is replaced with a base64 encoded version of the exported `.p12` file.

This process can be used for all encrypted items and environment specific items:

* Key Pairs (.p12)
* Trusted Certs (x509)
* Admin Password
* Data Store Passwords
* Integration Kit Properties
* Hostnames

Leaving these confidential items as unencrypted text in source control is unacceptable. The next logical step is to abstract the unencrypted values and replace them with variables. By doing this, the values can be stored in a secrets management tool (such as Hashicorp Vault) and the variablized file can be in source control.

Converting each of the encrypted keys for their unencrypted counterparts and hostnames with variables is cumbersome and can be automated. As we know in DevOps, if it _can_ be automated, it _must_ be automated. For more information, see [Using Bulk Config Tool](#using-bulk-config-tool).

A variablized `data.json.subst` is a good candidate for committing to source control after removing any unencrypted text.
-->

#### 例

[PingFederate ベースライン プロファイル](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/baseline/pingfederate)からの SSL サーバー証明書を data.json にエクスポートするときの構文は次のとおりです。

```json
{
    "resourceType": "/keyPairs/sslServer",
    "operationType": "SAVE",
    "items": [
        {
            "id": "sslservercert",
            "fileData": "MIIRBwIBAzCCEMAGCSqGSIb3DQEHAaCCELEEghCtMIIQqTCCCeUGCSqGSIb3DQEHAaCCCdYEggnSMIIJzjCCCcoGCyqGSIb3DQEMCgECoIIJezCCCXcwKQYKKoZIhvcNAQwBAzAbBBQu6vDERQZX3uujWa7v_q3sYN4Q0gIDAMNQBIIJSFtdWbvLhzYrTqeKKiJqiqROgE0E4mkVvmEC6NwhhPbcH37IDNvVLu0umm--CDZnEmlyPpUucO345-U-6z-cskw4TbsjYIzM10MwS6JdsyYFTC3GwqioqndVgBUzDh8xGnfzx52zEehX8d-ig1F6xYsbEc01gTbh4lF5MA7E7VfoTa4hWqtceV8PQeqzJNarlZyDSaS5BLn1J6G9BYUze-M1xGhATz7F2l-aAt6foi0mwIBlc2fwsdEPuAALZgdG-q_V4gOJW2K0ONnmWhMgMLpCL42cmSb
            ... more encrypted text ...
            Yxpzp_srpy4LHNdgHqhVBhqtDrjeKJDRfc1yk21P5PpfEBxn5MD4wITAJBgUrDgMCGgUABBQLBpq8y79Pq1TzG1Xf6OAjZzBZaQQUC4kD4CkcrH-WTQhJHud850ddn08CAwGGoA==",
            "encryptedPassword": "eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2Iiwia2lkIjoiRW1JY1UxOVdueSIsInZlcnNpb24iOiIxMC4xLjEuMCJ9..l6PJ55nSSvKHl0vSWTpkOA.i7hpnnu2yIByhyq_aGBCdaqS3u050yG8eMRGnLRx2Yk.Mo4WSkbbJyLISHq6i4nlVA"
        }
    ]
}
```

このマスターキーに依存する形式は次のように変換できます。

```json
{
    "resourceType": "/keyPairs/sslServer",
    "operationType": "SAVE",
    "items": [
        {
            "id": "sslservercert",
            "password": "2FederateM0re",
            "fileData": "MIIRCQIBAzCCEM8GCSqGSIb3DQEHAaCCEMAEghC8MIIQuDCCC28GCSqGSIb3DQEHBqCCC2AwggtcAgEAMIILVQYJKoZIhvcNAQcBMBwGCiqGSIb3DQEMAQYwDgQIjXWLRGuGNIQCAggAgIILKOgCQ9onDqBPQsshsaS50OjWtj\/7s47BUYal1YhO70fBup1a82WGHGhAvb\/SY1yOhqQR+TloEBOPI5cExoGN\/Gvw2Mw5\/wkQZZMSHqxjz68KhN4B0hrsOf4rqShB7jsz9ebSml3r2w0sUZWR73GBtBt1Y3wIlXLS2WtqdtHra9VnUqp1eOk+xenjuWM+u2ndDD43GgKB3n8mNBSSVBqx6ne7aSRJRuAUd+HAzLvSeXjTPMObI1Jod2F+7
            ... more base64 encoded exported .p12 ...
            5QJ15OJp2iEoVBWxogKf64s2F0iIYPoo6yjNvlidZCevP564FwknWrHoD7R8cIBrhlCJQbEOpOhPg66r4MK1CeJ2poaKRlMS8HGcMRaTpaqD+pIlgmUS6xFw49vr9Kwfb7KteRsTkNR+I8A7HjUpuCMSUwIwYJKoZIhvcNAQkVMRYEFOb7g1xwDka5fJ4sqngEvzTyuWnpMDEwITAJBgUrDgMCGgUABBRlJ+D+FR\/vQbaTGbKDFiBK\/xDbqQQIAjLc+GgRg44CAggA"
        }
    ]
}
```

手順:

1. サーバー証明書の秘密キーと証明書を別名 `sslservercert` でエクスポートしました。エクスポート時にパスワードが要求され、`2FederateM0re` が使用されました。この操作により、パスワードで保護された `.p12` ファイルがダウンロードされます。
1. data.json キー名 `encryptedPassword` が単純な `password` に変換されました。
1. `fileData` の値は、エクスポートされた `.p12` ファイルの Base64 でエンコードされたバージョンに置き換えられます。

この手順は、すべての暗号化されたアイテムと環境固有のアイテムに使用できます。

* Key Pairs (.p12)
* Trusted Certs (x509)
* Admin Password
* Data Store Passwords
* Integration Kit Properties
* Hostnames

これらの機密項目を暗号化されていないテキストとしてソース管理に残すことは容認できません。次の論理的なステップは、暗号化されていない値を抽象化し、変数に置き換えることです。これにより、値をシークレット管理ツール (Hashicorp Vault など) に保存し、可変ファイルをソース管理に含めることができます。

暗号化されたキーを、暗号化されていないキーと変数を使用してホスト名にそれぞれ変換するのは面倒ですが、自動化できます。 DevOps でわかるように、自動化できる場合は自動化する必要があります。詳細については、[一括設定ツールの使用](#using-bulk-config-tool)を参照してください。

可変化された `data.json.subst` は、暗号化されていないテキストを削除した後にソース管理にコミットするのに適した候補です。

<!--
### Using Bulk Config Tool

The [ping-bulkconfig-tool](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/99-helper-scripts/ping-bulkconfigtool) reads your data.json and can optionally:
    
  - Search and replace (e.g. hostnames)
  - Clean, add, and remove json members as required.
  - Tokenize the configuration and maintain environment variables.

The bulk export tool can process a bulk `data.json` export according to a configuration file with functions above. After running the tool, you are left with a `data.json.subst` and a list of environment variables waiting to be filled.

The `data.json.subst` form of our previous example will look like:
```json
{
    "operationType": "SAVE",
    "items": [{
        "password": "${keyPairs_sslServer_items_sslservercert_sslservercert_password}",
        "fileData": "${keyPairs_sslServer_items_sslservercert_sslservercert_fileData}",
        "id": "sslservercert"
    }],
    "resourceType": "/keyPairs/sslServer"
}
```

!!! Note "Bulk Config Tool Limitations"
    The bulk config tool can manipulate data.json but it cannot populate the resulting password or fileData variables because there is no API available on PingFederate to extract these. These variables can be filled using with externally generated certs and keys using tools like `openssl`, but that is out of scope for this document.

The resulting `env_vars` file can be used as a guideline for secrets that should be managed externally and only delivered to the container/image as needed for its specific environment.
-->

### 一括設定ツールの使用

[ping-bulkconfig-tool](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/99-helper-scripts/ping-bulkconfigtool) は data.json を読み取り、オプションで次のことを実行できます。
    
  - 検索と置換 (hostnameなど)
  - 必要に応じて、json メンバーをクリーンアップ、追加、削除します。
  - 構成をトークン化し、環境変数を維持します。

一括エクスポート ツールは、上記の機能を備えた構成ファイルに従って、`data.json` の一括エクスポートを処理できます。ツールを実行すると、`data.json.subst` と、入力を待つ環境変数のリストが残ります。

前の例の `data.json.subst` フォームは次のようになります。
```json
{
    "resourceType": "/keyPairs/sslServer",
    "operationType": "SAVE",
    "items": [
      {
          "id": "sslservercert",
          "password": "${keyPairs_sslServer_items_sslservercert_sslservercert_password}",
          "fileData": "${keyPairs_sslServer_items_sslservercert_sslservercert_fileData}"
      }
    ]
}
```

!!! Note "一括構成ツールの制限事項"
    一括構成ツールは data.json を操作できますが、結果として得られるパスワードや fileData 変数を設定することはできません。これは、PingFederate にはこれらを抽出するために使用できる API がないためです。これらの変数は、`openssl` などのツールを使用して外部で生成された証明書とキーを使用して入力できますが、それはこのドキュメントの範囲外です。

結果として得られる `env_vars` ファイルは、外部で管理し、特定の環境で必要な場合にのみコンテナ/イメージに配信する必要があるシークレットのガイドラインとして使用できます。

<!--
#### Prerequisites
-->

<!-- TODO: This docker image should be next to the rest of our images -->

<!--
1. The bulk export utility comes in pre-compiled source code. Build a Docker image by running:

   ```
    docker build -t ping-bulkexport-tools:latest .
    ```

2. Copy the [data.json](#steps) to: `pingidentity-devops-getting-started/99-helper-scripts/ping-bulkconfigtool/shared/data.json`
-->

#### 前提条件

<!-- TODO: This docker image should be next to the rest of our images -->

1. 一括エクスポート ユーティリティは、コンパイル済みのソース コードで提供されます。以下を実行して Docker イメージをビルドします。

   ```
    docker build -t ping-bulkexport-tools:latest .
    ```

2. [data.json](#手順) を `pingidentity-devops-getting-started/99-helper-scripts/ping-bulkconfigtool/shared/data.json` にコピーします。

<!--
#### Example

A sample command of the ping-bulkconfig-tool

```
docker run --rm -v $PWD/shared:/shared ping-bulkexport-tools:latest /shared/pf-config.json /shared/data.json /shared/env_vars /shared/data.json.subst > ./shared/convert.log
```

Where:

- `-v $PWD/shared:/shared` - bind mounts `ping-bulkconfigtool/shared` folder to /shared in the container
- `/shared/pf-config.json` - input path to [config file](#configure-bulk-tool) which defines how to process the bulk export `data.json` file from PingFederate.
- `/shared/data.json` - input path to data.json result of /pf-admin-api/v1/bulk/export PingFederate API endpoint.
- `/shared/env_vars` - output path to store environment variables generated from processing
- `/shared/data.json.subst` - output path to processed data.json

After running the above command, you will see `env_vars` and `data.json.subst` in the `ping-bulkconfigtool/shared` folder.
-->

#### 例

ping-bulkconfig-tool のサンプル コマンド

```
docker run --rm -v $PWD/shared:/shared ping-bulkexport-tools:latest /shared/pf-config.json /shared/data.json /shared/env_vars /shared/data.json.subst > ./shared/convert.log
```

Where:

- `-v $PWD/shared:/shared` - バインドは、 `ping-bulkconfigtool/shared` フォルダーをコンテナ内の /shared にマウントします
- `/shared/pf-config.json` - PingFederate からの一括エクスポート data.json ファイルの処理方法を定義する[構成ファイル](#一括ファイルの設定)への入力パス。
- `/shared/data.json` - /pf-admin-api/v1/bulk/export PingFederate API エンドポイントの data.json 結果への入力パス。
- `/shared/env_vars` - 処理で生成された環境変数を保存する出力パス
- `/shared/data.json.subst` - 処理された data.json への出力パス

上記のコマンドを実行すると、`ping-bulkconfigtool/shared` フォルダーに `env_vars` と `data.json.subst` が表示されます。

<!--
#### Configure Bulk Tool
Instructions to the bulk config tool are sent by the `pf-config.json` file.  

When using the `pf-config.json` file, any unused functions will require an empty array in the file.  For example, notice the **add-config** block at the top of this sample:

```sh
{
  "add-config":[],
  "config-aliases":[
  ],
  "expose-parameters":[
  ]
  ,
  "remove-config":[
    {
    "key": "id",
    "value": "ProvisionerDS"
    }
  ]
}
```

In this file, available commands include:
-->

#### 一括ツールの設定

一括構成ツールへの指示は、`pf-config.json` ファイルによって送信されます。

`pf-config.json` ファイルを使用する場合、未使用の関数にはファイル内に空の配列が必要です。たとえば、このサンプルの先頭にある **add-config** ブロックに注目してください。

```sh
{
  "add-config":[],
  "config-aliases":[
  ],
  "expose-parameters":[
  ]
  ,
  "remove-config":[
    {
    "key": "id",
    "value": "ProvisionerDS"
    }
  ]
}
```

このファイルで使用できるコマンドは次のとおりです。

<!--
##### search-replace
- A utility to search and replace string values in a bulk config json file.
- Can expose environment variables.

Example: replacing an expected base hostname with a substitution:
```
  "search-replace":[
    {
      "search": "data-holder.local",
      "replace": "${BASE_HOSTNAME}",
      "apply-env-file": false
    }
  ]
```
-->

##### search-replace

- 一括構成 JSON ファイル内の文字列値を検索および置換するユーティリティ。
- 環境変数を公開できる。

例: 予想される基本ホスト名を置換で置き換えます。
```
  "search-replace":[
    {
      "search": "data-holder.local",
      "replace": "${BASE_HOSTNAME}",
      "apply-env-file": false
    }
  ]
```

<!--
##### change-value
- Searches for elements with a matching identifier, and updates a parameter with a new value.

Example: update keyPairId against an element with name=ENGINE:
```
  "change-value":[
  	{
          "matching-identifier":
          {
          	"id-name": "name",
          	"id-value": "ENGINE"
          },
  	  "parameter-name": "keyPairId",
  	  "new-value": 8
  	}
  ]
```
-->

##### change-value

- 一致する識別子を持つ要素を検索し、パラメーターを新しい値で更新します。

例: name=ENGINE の要素に対して keyPairId を更新します。
```
  "change-value":[
  	{
          "matching-identifier":
          {
          	"id-name": "name",
          	"id-value": "ENGINE"
          },
  	  "parameter-name": "keyPairId",
  	  "new-value": 8
  	}
  ]
```

<!--
##### remove-config
- Remove configuration objects from the bulk export

Example: To remove the ProvisionerDS data store:
```
  "remove-config":[
  	{
  	  "key": "id",
	  "value": "ProvisionerDS"
  	}
  ]
```

Example: To remove all SP Connections:
```
  "remove-config":[
  	{
  	  "key": "resourceType",
	  "value": "/idp/spConnections"
  	}
  ]
```
-->

##### remove-config

- 一括エクスポートから構成オブジェクトを削除する

例: ProvisionerDS データ ストアを削除するには:
```
  "remove-config":[
    {
      "key": "id",
      "value": "ProvisionerDS"
    }
  ]
```

例: すべての SP Connectionを削除するには:
```
  "remove-config":[
  	{
  	  "key": "resourceType",
	  "value": "/idp/spConnections"
  	}
  ]
```

<!--
##### add-config
- Add configurations to the bulk export.

This tool works with both PingFederate and PingAccess.  This example adds the CONFIG QUERY http listener in PingAccess:
```
  "add-config":[
	  {
	    "resourceType": "httpsListeners",
	    "item":
			{
			    "id": 4,
			    "name": "CONFIG QUERY",
			    "keyPairId": 5,
			    "useServerCipherSuiteOrder": true,
			    "restartRequired": false
			}
	  }
  ]
```

Example: Add an SP connection:
```
  "add-config":[
	  {
	    "resourceType": "/idp/spConnections",
	    "item":
		{
                    "name": "httpbin3.org",
                    "active": false,
		    ...
		}
	  }
  ]
```
-->

##### add-config

- 一括エクスポートに構成を追加します。

このツールは、PingFederate と PingAccess の両方で動作します。この例では、PingAccess に CONFIG QUERY http リスナーを追加します。

```
  "add-config":[
	  {
	    "resourceType": "httpsListeners",
	    "item":
			{
			    "id": 4,
			    "name": "CONFIG QUERY",
			    "keyPairId": 5,
			    "useServerCipherSuiteOrder": true,
			    "restartRequired": false
			}
	  }
  ]
```

例: SP Connectionを追加します。
```
  "add-config":[
	  {
	    "resourceType": "/idp/spConnections",
	    "item":
		{
                    "name": "httpbin3.org",
                    "active": false,
		    ...
		}
	  }
  ]
```

<!--
##### expose-parameters
- Navigates through the JSON and exchanges values for substitions.
- Exposed substition names will be automatically created based on the json path.
    - E.g. ${oauth_clients_items_clientAuth_testclient_secret}
- Can convert encrypted/obfuscated values into clear text inputs (e.g. "encryptedValue" to "value") prior to substituting it. Doing so enables the injection of values in their raw form.

Example: replace the "encryptedPassword" member with a substitution-enabled "password" member for any elements with "id" or "username" members. The following will remove "encryptedPassword" and create "password": "${...}":
```
    {
      "parameter-name": "encryptedPassword",
      "replace-name": "password",
      "unique-identifiers": [
          "id",
          "username"
      ]
    }
```
-->

##### expose-parameters

- JSON 内を移動し、置換の値を交換します。
- 公開された置換名は、json パスに基づいて自動的に作成されます。
    - 例えば。 ${oauth_clients_items_clientAuth_testclient_secret}
- 暗号化/難読化された値を、置換する前にクリア テキスト入力に変換できます (例: "encryptedValue" から "value")。そうすることで、値を生の形式で注入できるようになります。

Example: replace the "encryptedPassword" member with a substitution-enabled "password" member for any elements with "id" or "username" members. The following will remove "encryptedPassword" and create "password": "${...}":
例: 「id」または「username」メンバーを持つ要素の「encryptedPassword」メンバーを置換可能な「password」メンバーに置き換えます。以下は、「encryptedPassword」を削除し、「password」「${...}」を作成します。
```
    {
      "parameter-name": "encryptedPassword",
      "replace-name": "password",
      "unique-identifiers": [
          "id",
          "username"
      ]
    }
```

<!--
##### config-aliases
- The bulk config tool generates substitution names. However, there might be times you wish to simplify them or reuse existing environment variables.

Example: Rename the Administrator's substitution name using the PING_IDENTITY_PASSWORD environment variable:
```
  "config-aliases":[
	{
	  "config-names":[
	    "administrativeAccounts_items_Administrator_password"
	  ],
  	  "replace-name": "PING_IDENTITY_PASSWORD",
  	  "is-apply-envfile": false
  	}
  ]
```
-->

##### config-aliases

- 一括構成ツールは置換名を生成します。ただし、環境変数を簡素化したり、既存の環境変数を再利用したりしたい場合もあります。

例: PING_IDENTITY_PASSWORD 環境変数を使用して、管理者の代替名を変更します。

```
  "config-aliases":[
	{
	  "config-names":[
	    "administrativeAccounts_items_Administrator_password"
	  ],
  	  "replace-name": "PING_IDENTITY_PASSWORD",
  	  "is-apply-envfile": false
  	}
  ]
```

<!--
##### sort-arrays
- Configure the array members that need to be sorted. This function ensures the array is created consistently, simplifying git diff analysis.

Example: Sort the roles and scopes arrays:
```
  "sort-arrays":[
        "roles","scopes"
  ]
```
-->

##### sort-arrays

- 並べ替える必要がある配列メンバーを構成します。この関数により、配列が一貫して作成され、git diff 分析が簡素化されます。

例: ロールとスコープの配列を並べ替えます。
```
  "sort-arrays":[
        "roles","scopes"
  ]
```

<!-- ####TODO:  Script It

If desired, use of the bulk config tool can be included in a script. `pingidentity-devops-getting-started/99-helper-scripts/ping-bulkconfigtool/pf-profile.sh` is an example of this.

```
sh pf-profile.sh --release mypingfed --password 2FederateM0re
``` -->

<!--
### Additional Notes

* The bulk API export is intended to be used as a _bulk_ import. The `/bulk/import` endpoint is destructive and overwrites the entire current admin config.
* If you are in a clustered environment, the PingFederate image imports the `data.json` and replicates the configuration to engines in the cluster.
* Your data.json.subst `"metadata": {"pfVersion": "10.1.2.0"}` should match the PingFederate profile version.
-->

### その他の注意事項

- 一括 API エクスポートは、_一括_ インポートとして使用することを目的としています。 `/bulk/import` エンドポイントは破壊的であり、現在の管理構成全体を上書きします。
- クラスター環境にいる場合、PingFederate イメージは data.json をインポートし、クラスター内のエンジンに構成をレプリケートします。
* data.json.substの `"metadata": {"pfVersion": "10.1.2.0"} `は、PingFederate プロファイルのバージョンと一致する必要があります。

<!--
## The Configuration Archive Profiles Method

### About configuration archive-based profiles
You should weigh the pros and cons of configuration archive-based profiles compared to bulk API export profiles. While not fully aligning with pur DevOps principles, many users prefer using bulk API export profiles in most scenarios.

Pros:
* The `/data` folder, as opposed to a `data.json` file, is better for [profile layering](profilesLayered.md).
* Configuration is available on engines at startup, which:
    * lowers dependency on the admin at initial cluster startup

Cons:

* The `/data` folder contains key pairs in a `.jks` file , which makes externally managing keys very difficult.
* Encrypted data is scattered throughout the folder, creating a dependency on the master encryption key.
-->

## 構成アーカイブプロファイルメソッド

### 構成アーカイブベースのプロファイルについて

一括 API エクスポート プロファイルと比較して、構成アーカイブ ベースのプロファイルの長所と短所を比較検討する必要があります。純粋な DevOps 原則と完全に一致しているわけではありませんが、多くのユーザーは、ほとんどのシナリオで一括 API エクスポート プロファイルの使用を好みます。

Pros:

- `data.json` ファイルとは対照的に、`/data` フォルダーは[プロファイルの階層化](profilesLayered.md)に適しています。
- 設定はエンジンの起動時に利用できます。
    - クラスターの初期起動時の管理者への依存度を下げる

Cons:

- `/data` フォルダーには `.jks` ファイル内のキー ペアが含まれているため、外部からキーを管理することが非常に困難になります。
- 暗号化されたデータはフォルダー全体に分散され、マスター暗号化キーへの依存関係が作成されます。

<!--
### About this method

You will:

1. Export a `data.zip` archive
1. Optionally, variablize
1. Replace the data folder
-->

### この方法について

下記のようにします。

1. data.zip アーカイブをエクスポートする
1. 必要に応じて変数化します
1. データフォルダーを置き換える

<!-- 1. Export a configuration archive.
This can be done through the UI `System > Server > Configuration Archive`
Or via Admin API:
  ```
  curl -u administrator:2FederateM0re -H "X-XSRF-Header: PingFederate" -k https://pingfederate-admin.com/pf-admin-api/v1/configArchive/export --output "${HOME}/Downloads/data.zip"
  ```

  ```
  unzip -d data /path/to/data.zip
  ``` -->
  
<!--
## Installing PingFederate Integration Kits

By default, PingFederate is shipped with a handful of integration kits and adapters. If you need other integration kits or adapters in the deployment, manually download them and place them inside the `server/default/deploy` directory of the server profile. You can find these resources in the product download page [here](https://www.pingidentity.com/en/resources/downloads/pingfederate.html).
-->

## PingFederate Integration Kitのインストール

デフォルトでは、PingFederate にはいくつかのIntegration KitとAdapterが同梱されています。デプロイメントで他のIntegration KitまたはAdapterが必要な場合は、それらを手動でダウンロードし、サーバー プロファイルの `server/default/deploy` ディレクトリ内に配置します。これらのリソースは、[ここ](https://www.pingidentity.com/en/resources/downloads/pingfederate.html)の製品ダウンロード ページにあります。
