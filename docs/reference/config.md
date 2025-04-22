---
title: Image/Container Components and Configuration
---
<!--
# Image/Container Components and Configuration

## Container data flows and running state

The diagram below shows the topology of a container with flows of data into the container and how it transitions to the eventual running state.

![DevOps Image/Container Anatomy](../images/container-anatomy-1.svg)

| Data Class        | Default Location  | Use | Description                                                                                                       |
| ----------------- | ----------------- | :-: | ----------------------------------------------------------------------------------------------------------------- |
| VAULT             |                   | ext | Secret information from external Vault (i.e. HashiCorp Vault).  Items like passwords, certificates, keys, etc...  |
| ORCH              |                   | ext | Environment variables from secrets, configmaps and/or env/envfile resources from orchestration (i.e. docker, k8s. |
| SERVER PROFILE    |                   | ext | Product server profile from either an external repository (i.e. git) or external volume (i.e. aws s3).            |
| SERVER BITS       | /opt/server       | ro  | Uncompressed copy of the product software.  Provided by image.                                                    |
| SECRETS           | /run/secrets      | ro  | Read Only secrets residing on non-persistent storage (i.e. /run/secrets).                                         |
| IN                | /opt/in           | ro  | Volume intended to receive all incoming server-profile information.                                               |
| ENV               | /opt/staging/.env | mem | Environment variable settings used by hooks and product to configure container.                                   |
| STAGING           | /opt/staging      | tmp | Temporary space used to prepare configuration and store variable settings before being moved to OUT               |
| OUT               | /opt/out          | rw  | Combo of product bits/configuration resulting in running container configuration.                                 |
| PERSISTENT VOLUME |                   | rw  | Persistent location of product bits/configuration in external storage (i.e. AWS EBS)                              |

Because of these many factors affecting how an image is deployed, the configuration options for use of the elements in the previous table can vary greatly, depending on factors such as:

* Deployment Environment - Kubernetes, Cloud Vendor, Local Docker
* CI/CD Tools - Kubectl, Helm, Kustomize, Terraform
* Source Maintenance - Git, Cloud Vendor Volumes
* Customer Environment - Development, Test, QA, Stage, Prod
* Security - Test/QA/Production Data, Secrets, Certificates, Secret Management Tools
-->

# イメージ/コンテナのコンポーネントと構成

## コンテナのデータフローと実行状態

以下の図は、コンテナーへのデータの流れと、コンテナーが最終的な実行状態にどのように移行するかを示すコンテナーのトポロジを示しています。

![DevOps Image/Container Anatomy](../images/container-anatomy-1.svg)

| Data Class        | Default Location  | Use | Description                                                                                                       |
| ----------------- | ----------------- | :-: | ----------------------------------------------------------------------------------------------------------------- |
| VAULT             |                   | ext | 外部 Vault (つまり HashiCorp Vault) からの秘密情報。パスワード、証明書、キーなどのアイテム... |
| ORCH              |                   | ext | シークレットからの環境変数、オーケストレーションからの configmaps および/または env/envfile リソース (例: docker、k8s. |
| SERVER PROFILE    |                   | ext | 外部リポジトリ (例: git) または外部ボリューム (例: aws s3) の製品サーバー プロファイル。|
| SERVER BITS       | /opt/server       | ro  | 製品ソフトウェアの非圧縮コピー。イメージにより提供されます。 |
| SECRETS           | /run/secrets      | ro  | 非永続ストレージ (つまり、/run/secrets) に常駐する読み取り専用シークレット。 |
| IN                | /opt/in           | ro  | すべての受信サーバー プロファイル情報を受信することを目的としたボリューム。 |
| ENV               | /opt/staging/.env | mem | コンテナを構成するためにフックと製品によって使用される環境変数設定。 |
| STAGING           | /opt/staging      | tmp | OUT に移動する前に構成を準備し、変数設定を保存するために使用される一時スペース |
| OUT               | /opt/out          | rw  | コンテナ構成を実行する結果となる製品ビット/構成の組み合わせ。 |
| PERSISTENT VOLUME |                   | rw  | 外部ストレージ (例: AWS EBS) 内の製品ビット/構成の永続的な場所 |

イメージのデプロイ方法に影響を与えるこれらの多くの要因のため、前の表の要素を使用するための構成オプションは、次のような要因に応じて大きく異なる可能性があります。

- デプロイメント環境 - Kubernetes、クラウド ベンダー、ローカル Docker
- CI/CD ツール - Kubectl、Helm、Kustimize、Terraform
- ソースのメンテナンス - Git、クラウド ベンダーのボリューム
- 顧客環境 - 開発、テスト、QA、ステージ、本番
- セキュリティ - テスト/QA/実稼働データ、シークレット、証明書、シークレット管理ツール

<!--
## Examples
### File flowchart example

The following diagram shows how files can enter and flow through the container:

![File Flowchart Example](../images/container-anatomy-flow.svg)

!!! note "There is a video that goes through the above image in more detail [here](https://videos.pingidentity.com/detail/videos/devops/video/6314748082112/ping-product-docker-image-exploration)."
-->

## 例

### ファイルフローチャートの例

次の図は、ファイルがどのようにコンテナに入り、コンテナ内を流れるかを示しています。

![File Flowchart Example](../images/container-anatomy-flow.svg)

!!! note "上の画像を詳しく説明したビデオが[ここ](https://videos.pingidentity.com/detail/videos/devops/video/6314748082112/ping-product-docker-image-exploration)にあります。"

<!--
### Production Example

The following diagram shows an example in a high-level production scenario in an Amazon Web Services (AWS) EKS environment, where:

* HashiCorp Vault is used to provide secrets to the container.
* Helm is used to create k8s resources and deploy them.
* AWS EBS volumes is used to persist the state of the container.

![Production Tools Example](../images/container-anatomy-1-prod.svg)
-->

### 本番環境の例

次の図は、アマゾン ウェブ サービス (AWS) EKS 環境における高レベルの運用シナリオの例を示しています。

- HashiCorp Vault は、コンテナーにシークレットを提供するために使用されます。
- Helm は、k8s リソースの作成とデプロイに使用されます。
- AWS EBS ボリュームは、コンテナの状態を保持するために使用されます。

![Production Tools Example](../images/container-anatomy-1-prod.svg)

<!--
### Development Example

The following diagram shows an example in a high-level development scenario in an Azure AKS environment, where:

* No secrets management is used.
* Simple kubectl is used to deploy k8s resources.
* AWS EBS volumes is used to persist the state of the container.

![Development Tools Example](../images/container-anatomy-1-dev.svg)
-->

### 開発環境の例

次の図は、Azure AKS 環境での高レベルの開発シナリオの例を示しています。

- シークレット管理は使用しません。
- k8s リソースのデプロイには単純な kubectl が使用されます。
- Azure Storage は、コンテナの状態を保持するために使用されます。

![Development Tools Example](../images/container-anatomy-1-dev.svg)

<!--
## Customizing the Containers

You can customize our product containers by:

* [Customizing server profiles](../how-to/profiles.md)

    The server profiles supply configuration, data, and environment information to the product containers at startup. You can use our server profiles as-is or as a baseline for creating your own.

    You can find these profiles in [Baseline server profiles](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/baseline) in our pingidentity-server-profiles repository.

* [Environment substitution](../how-to/profilesSubstitution.md)
  
    You can deploy configurations in multiple environments with minimal changes by removing literal values and replacing them with environment variables.

* [Using DevOps hooks](hooks.md)

    Hooks are shell scripts used to automate operations during the lifecycle of a product container.  These hook scripts are built into our images: some are common across all products, while others are product-specific. For more information about the repository that houses these scripts, visit the [docker-builds repository overview](../docker-builds/README.md).

* [Using release tags](../docker-images/releaseTags.md)

    We use sets of tags for each released build image. These tags identify whether the image is a specific stable release, the latest stable release, or current (potentially unstable) builds. You can find the release tag information in [Docker images](../docker-images/releaseTags.md).

    You can try different tags in either the standalone startup scripts for the deployment examples or the YAML files for the orchestrated deployment examples.

* [Adding a message of the day (MOTD)](../how-to/addMOTD.md)

    You can use a `motd.json` file to add message of the day information for inclusion in the images.
-->

## コンテナのカスタマイズ

製品コンテナは次の方法でカスタマイズできます。

* [Customizing server profiles](../how-to/profiles.md)

    サーバー プロファイルは、起動時に構成、データ、および環境情報を製品コンテナに提供します。当社のサーバー プロファイルをそのまま使用することも、独自のサーバー プロファイルを作成するためのベースラインとして使用することもできます。

    これらのプロファイルは、pingidentity-server-profiles リポジトリの[Baseline server profiles](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/baseline)にあります。

* [Environment substitution](../how-to/profilesSubstitution.md)
  
    リテラル値を削除して環境変数に置き換えることにより、最小限の変更で複数の環境に構成をデプロイできます。

* [Using DevOps hooks](hooks.md)

    フックは、製品コンテナのライフサイクル中の操作を自動化するために使用されるシェル スクリプトです。これらのフック スクリプトはイメージに組み込まれています。すべての製品に共通のものもあれば、製品固有のものもあります。これらのスクリプトを格納するリポジトリの詳細については、[docker-builds repository overview](../docker-builds/README.md)を参照してください。

* [Using release tags](../docker-images/releaseTags.md)

    リリースされたビルド イメージごとにタグのセットを使用します。これらのタグは、イメージが特定の安定リリース、最新の安定リリース、または現在の (不安定な可能性がある) ビルドであるかを識別します。リリース タグ情報は [Docker images](../docker-images/releaseTags.md) で見つけることができます。

    デプロイメント例のスタンドアロン起動スクリプトまたはオーケストレーションされたデプロイメント例の YAML ファイルで、さまざまなタグを試すことができます。

* [Adding a message of the day (MOTD)](../how-to/addMOTD.md)

    `motd.json` ファイルを使用して、画像に含めるその日のメッセージ情報を追加できます。
