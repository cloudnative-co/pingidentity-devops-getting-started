---
title: Layering Server Profiles
---
<!--
# Layering server profiles

One of the benefits of our Docker images is the ability to layer product configuration. By using small, discrete portions of your configuration, you can build and assemble a server profile based on multiple installations of a product.

A typical organization can have multiple installations of our products, each using different configurations. By layering the server profiles, you can reuse the configurations that are common across environments, leading to fewer configurations to manage.

You can have as many layers as needed. Each layer of the configuration is *copied* on top of the container's filesystem (not merged).

!!! note "Layer Precedence"
    The profile layers are applied starting at the top layer and ending at the base layer. This ordering might not be apparent at first.
-->

# サーバープロファイルの階層化

Docker イメージの利点の 1 つは、製品構成を階層化できることです。構成の小さな個別の部分を使用することにより、製品の複数のインストールに基づいてサーバー プロファイルを構築および組み立てることができます。

一般的な組織では、それぞれが異なる構成を使用して製品を複数インストールすることができます。サーバー プロファイルを階層化することで、環境間で共通の構成を再利用できるため、管理する構成が少なくなります。

あなたは必要なだけレイヤーを作成できます。構成の各レイヤーは、コンテナーのファイルシステムの上にコピーされます (マージされません)。

!!! note "レイヤーの優先順位"
    プロファイル レイヤは、最上位レイヤから開始してベース レイヤで終了するように適用されます。この順序は最初は分からないかもしれません。

<!--
## Before you begin

You must:

* Complete [Get Started](../get-started/introduction.md) to set up your DevOps environment and run a test deployment of the products.
-->

## はじめる前に

You must:

- [Get Started](../get-started/introduction.md)を完了して、DevOps 環境をセットアップし、製品のテスト展開を実行します。

<!--
## About this task

You will:

* Create a layered server profile.
* Assign the environment variables for the deployment.
* Deploy the layered server profile.
-->

## このタスクについて

You will:

- 階層化されたサーバー プロファイルを作成します。
- デプロイメント用の環境変数を割り当てます。
- 階層化されたサーバー プロファイルを展開します。

<!--
## Creating a layered server profile

For this guide, PingFederate is used along with the server profile located in the [pingidentity-server-profiles](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started/pingfederate) repository. You should fork this repository to your Github repository, then pull your Github repository to a local directory. After you have finished creating the layered profile, you can push your updates to your Github repository and reference it as an environment variable to run the deployment.

You will create separate layers for:

* Product license
* Extensions (such as, Integration Kits and Connectors)

For this example, these layers will be applied on top of the PingFederate server profile. However, you can span configurations across multiple repositories if you want.

You can find the complete working, layered server profile of the PingFederate example from this guide in the [pingidentity-server-profiles/layered-profiles](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/layered-profiles) directory.

Because PingFederate's configuration is file-based, the layering works by copying configurations on top of the PingFederate container’s file system.

!!! warning "Files Copied"
    Files are copied, not merged. It is best practice to only layer items that will not be impacted by other configuration files.
-->

## 階層化サーバープロファイルの作成

このガイドでは、PingFederate が [pingidentity-server-profiles](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started/pingfederate) リポジトリにあるサーバー プロファイルとともに使用されます。このリポジトリを Github リポジトリにフォークしてから、Github リポジトリをローカル ディレクトリにプルする必要があります。階層化プロファイルの作成が完了したら、更新を Github リポジトリにプッシュし、それを環境変数として参照してデプロイメントを実行できます。

You will create separate layers for:
以下に対して個別のレイヤーを作成します。

- 製品ライセンス
* 拡張機能 (Integration KitやConnectorなど)

この例では、これらのレイヤーは PingFederate サーバー プロファイルの上に適用されます。ただし、必要に応じて、構成を複数のリポジトリにまたがることができます。

このガイドの PingFederate サンプルの完全に機能する階層化サーバー プロファイルは、[pingidentity-server-profiles/layered-profiles](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/layered-profiles) ディレクトリにあります。

PingFederate の構成はファイルベースであるため、階層化は PingFederate コンテナーのファイル システム上に構成をコピーすることによって機能します。

!!! warning "コピーされたファイル"
    ファイルはマージされずにコピーされます。他の構成ファイルの影響を受けない項目のみを階層化することがベスト プラクティスです。

<!--
### Creating the base directories

Create a working directory named `layered_profiles` and within that directory create `license` and `extensions` directories. When completed, your directory structure should be:

```text
└── layered_profiles
   ├── extensions
   └── license
```
-->

### ベースディレクトリの作成

`Layered_profiles` という名前の作業ディレクトリを作成し、そのディレクトリ内に `license` ディレクトリと `extensions` ディレクトリを作成します。完了すると、ディレクトリ構造は次のようになります。

```text
└── layered_profiles
   ├── extensions
   └── license
```

<!--
### Constructing the license layer

1. Go to the `license` directory and create a `pingfederate` subdirectory.
1. Create the PingFederate license file directory path under the `pingfederate` directory.

    The PingFederate license file resides in the `/instance/server/default/conf/` path.

      ```sh
      mkdir -p instance/server/default/conf/
      ```

      Your license profile path should look like this:

      ```text
      └── license
         └── pingfederate
            └── instance
               └── server
                     └── default
                        └── conf
                           └── pingfederate.lic
      ```

1. Copy your `pingfederate.lic` file to `license/pingfederate/instance/server/default/conf`.

    Using the DevOps evaluation license, when the PingFederate container is running, you can find the license in the container file system `/opt/out/instance/server/default/conf` directory.

    You can copy the `pingfederate.lic` file from the Docker file system using the syntax:
    `docker cp <container> <source-location> <target-location>`

      For example:

      ```sh
      docker cp \
         pingfederate \
         /opt/in/instance/server/default/conf/pingfederate.lic \
         ${HOME}/projects/devops/layered_profiles/license/pingfederate/instance/server/default/conf
      ```

      Using the `pingctl` tool (update product and version accordingly):

      ```sh
      pingctl license pingfederate 11.1 > \
         ${HOME}/projects/devops/layered_profiles/license/pingfederate/instance/server/default/conf
      ```
-->

### ライセンス層の構築

1. `license` ディレクトリに移動し、`pingfederate` サブディレクトリを作成します。
1. `pingfederate` ディレクトリの下に PingFederate ライセンス ファイル ディレクトリ パスを作成します。

    PingFederate ライセンス ファイルは、`/instance/server/default/conf/` パスにあります。

      ```sh
      mkdir -p instance/server/default/conf/
      ```

      ライセンス プロファイルのパスは次のようになります。

      ```text
      └── license
         └── pingfederate
            └── instance
               └── server
                     └── default
                        └── conf
                           └── pingfederate.lic
      ```

1. Copy your `pingfederate.lic` file to `license/pingfederate/instance/server/default/conf`.

    Using the DevOps evaluation license, when the PingFederate container is running, you can find the license in the container file system `/opt/out/instance/server/default/conf` directory.
    DevOps 評価ライセンスを使用していると、PingFederate コンテナーの実行中に、コンテナー ファイル システムの `/opt/out/instance/server/default/conf` ディレクトリでライセンスを見つけることができます。

    次の構文を使用して、Docker ファイル システムから `pingfederate.lic` ファイルをコピーできます。
    `docker cp <container> <source-location> <target-location>`

      例:

      ```sh
      docker cp \
         pingfederate \
         /opt/in/instance/server/default/conf/pingfederate.lic \
         ${HOME}/projects/devops/layered_profiles/license/pingfederate/instance/server/default/conf
      ```

      `pingctl` ツールを使用する (製品とバージョンを適宜更新します):

      ```sh
      pingctl license pingfederate 11.1 > \
         ${HOME}/projects/devops/layered_profiles/license/pingfederate/instance/server/default/conf
      ```

<!--
### Building the extensions layer

1. Go to the `layered-profiles/extensions` directory and create a `pingfederate` subdirectory.
1. Create the PingFederate extensions directory path under the `pingfederate` directory.

    The PingFederate extensions reside in the `/instance/server/default/deploy` directory path.

      ```sh
      mkdir -p instance/server/default/deploy
      ```

1. Copy the extensions you want to be available to PingFederate to the `layered-profiles/extensions/pingfederate/instance/server/default/deploy` directory .

    The extensions profile path should look similar to the following (extensions will vary based on your requirements):

      ```text
      └── extensions
         └── pingfederate
            └── instance
                  └── server
                     └── default
                        └── deploy
                              ├── pf-aws-quickconnection-2.0.jar
                              ├── pf-azure-ad-pcv-1.2.jar
                              └── pf-slack-quickconnection-3.0.jar
      ```
-->

### 拡張機能レイヤーの構築

1. `layered-profiles/extensions` ディレクトリに移動し、`pingfederate` サブディレクトリを作成します。
1. `pingfederate` ディレクトリの下に PingFederate 拡張機能のディレクトリ パスを作成します。

    PingFederate 拡張機能は、`/instance/server/default/deploy` ディレクトリ パスに存在します。

      ```sh
      mkdir -p instance/server/default/deploy
      ```

1. PingFederate で使用できるようにする拡張機能を、`layered-profiles/extensions/pingfederate/instance/server/default/deploy` ディレクトリにコピーします。

    拡張機能プロファイルのパスは次のようになります (拡張機能は要件によって異なります)。

      ```text
      └── extensions
         └── pingfederate
            └── instance
                  └── server
                     └── default
                        └── deploy
                              ├── pf-aws-quickconnection-2.0.jar
                              ├── pf-azure-ad-pcv-1.2.jar
                              └── pf-slack-quickconnection-3.0.jar
      ```

<!--
## Assigning environment variables

Although this deployment assigns the environment variables for use in a Docker Compose YAML file, you can use the following technique with any Docker or Kubernetes deployment.

If you want to use your own Github repository for the deployment in the following examples, replace:

```sh
SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
```

with:

```sh
SERVER_PROFILE_URL=https://github.com/<your-username>/pingidentity-server-profiles.git
```

!!! note "Private Github Repo"
    If your GitHub server-profile repo is private, use the `username:token` format so the container can access the repository. For example, `https://github.com/<your_username>:<your_access_token>/pingidentity-server-profiles.git`. For more information, see [Using Private Github Repositories](./privateRepos.md).

1. Create a new `docker-compose.yaml` file.

2. Add your license profile to the YAML file.

    For example:

      ```yaml
      - SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_PATH=layered-profiles/license/pingfederate
      ```

      > `SERVER_PROFILE` supports `URL`, `PATH`, `BRANCH` and `PARENT` variables.

3. Using `SERVER_PROFILE_PARENT`, instruct the container to retrieve its parent configuration by specifying the `extensions` profile as the parent:

      ```yaml
      - SERVER_PROFILE_PARENT=EXTENSIONS
      ```

      `SERVER_PROFILE` can be extended to reference additional profiles. Because we specified the license profile's parent as `EXTENSIONS`, we can extend `SERVER_PROFILE` by referencing the `EXTENSIONS` profile (prior to the `URL` and `PATH` variables):

      ```yaml
      - SERVER_PROFILE_EXTENSIONS_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_EXTENSIONS_PATH=layered-profiles/extensions/pingfederate
      ```

4. Set `GETTING_STARTED` as the `EXTENSIONS` parent and declare the `URL` and `PATH`:

      ```yaml
      - SERVER_PROFILE_EXTENSIONS_PARENT=GETTING_STARTED
      - SERVER_PROFILE_GETTING_STARTED_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_GETTING_STARTED_PATH=getting-started/pingfederate
      ```

      > Because the `GETTING_STARTED` profile is the last profile to add, it will not have a parent.

      Your `environment` section of the `docker-compose.yaml` file should look similar to this:

      ```yaml
      environment:
      # **** SERVER PROFILES BEGIN ****
      # Server Profile - Product License
      - SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_PATH=layered-profiles/license/pingfederate
      - SERVER_PROFILE_PARENT=EXTENSIONS

      # Server Profile - Extensions
      - SERVER_PROFILE_EXTENSIONS_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_EXTENSIONS_PATH=layered-profiles/extensions/pingfederate
      - SERVER_PROFILE_EXTENSIONS_PARENT=GETTING_STARTED

      # Base Server Profile
      - SERVER_PROFILE_GETTING_STARTED_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_GETTING_STARTED_PATH=getting-started/pingfederate
      # **** SERVER PROFILE END ****
      ```
-->

## 環境変数の割り当て

このデプロイメントでは、Docker Compose YAML ファイルで使用する環境変数が割り当てられますが、次の手法は任意の Docker または Kubernetes デプロイメントで使用できます。

次の例のデプロイメントに独自の Github リポジトリを使用する場合は、以下を置き換えます。

```sh
SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
```

with:

```sh
SERVER_PROFILE_URL=https://github.com/<your-username>/pingidentity-server-profiles.git
```

!!! note "プライベート Github リポジトリ"
    If your GitHub server-profile repo is private, use the `username:token` format so the container can access the repository. For example, `https://github.com/<your_username>:<your_access_token>/pingidentity-server-profiles.git`. For more information, see [Using Private Github Repositories](./privateRepos.md).
    GitHub サーバー プロファイル リポジトリがプライベートである場合は、コンテナーがリポジトリにアクセスできるように、`username:token`形式を使用します。たとえば、`https://github.com/<your_username>:<your_access_token>/pingidentity-server-profiles.git` です。詳細については、[Using Private Github Repositories](./privateRepos.md)を参照してください。

1. 新しい `docker-compose.yaml` ファイルを作成します。

2. ライセンス プロファイルを YAML ファイルに追加します。

    例:

      ```yaml
      - SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_PATH=layered-profiles/license/pingfederate
      ```

      > `SERVER_PROFILE` は、`URL`、`PATH`、`BRANCH`、および `PARENT` 変数をサポートします。

3. `SERVER_PROFILE_PARENT` を使用して、`extensions`プロファイルを親として指定して親の構成を取得するようにコンテナーに指示します。

      ```yaml
      - SERVER_PROFILE_PARENT=EXTENSIONS
      ```

      `SERVER_PROFILE` は、追加のプロファイルを参照するように拡張できます。ライセンス プロファイルの親を `EXTENSIONS` として指定したため、(`URL` 変数と `PATH` 変数の前に) `EXTENSIONS` プロファイルを参照することで `SERVER_PROFILE` を拡張できます。

      ```yaml
      - SERVER_PROFILE_EXTENSIONS_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_EXTENSIONS_PATH=layered-profiles/extensions/pingfederate
      ```

4. `GETTING_STARTED` を `EXTENSIONS` の親として設定し、`URL` と `PATH` を宣言します。

      ```yaml
      - SERVER_PROFILE_EXTENSIONS_PARENT=GETTING_STARTED
      - SERVER_PROFILE_GETTING_STARTED_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_GETTING_STARTED_PATH=getting-started/pingfederate
      ```

      > `GETTING_STARTED` プロファイルは追加する最後のプロファイルであるため、親はありません。

      `docker-compose.yaml` ファイルの `environment` セクションは次のようになります。

      ```yaml
      environment:
      # **** SERVER PROFILES BEGIN ****
      # Server Profile - Product License
      - SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_PATH=layered-profiles/license/pingfederate
      - SERVER_PROFILE_PARENT=EXTENSIONS

      # Server Profile - Extensions
      - SERVER_PROFILE_EXTENSIONS_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_EXTENSIONS_PATH=layered-profiles/extensions/pingfederate
      - SERVER_PROFILE_EXTENSIONS_PARENT=GETTING_STARTED

      # Base Server Profile
      - SERVER_PROFILE_GETTING_STARTED_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
      - SERVER_PROFILE_GETTING_STARTED_PATH=getting-started/pingfederate
      # **** SERVER PROFILE END ****
      ```

<!--
## Deploying the layered profile

1. Push your profiles and updated `docker-compose.yaml` file to your GitHub repository.
1. Deploy the stack with the layered profiles.

To view this example in its entirety, including the profile layers and `docker-compose.yaml` file, see the [pingidentity-server-profiles/layered-profiles](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/layered-profiles) directory.
-->

## 階層化プロファイルのデプロイ

1. プロファイルと更新された `docker-compose.yaml` ファイルを GitHub リポジトリにpushします。
1. 階層化されたプロファイルを使用してスタックをデプロイします。

To view this example in its entirety, including the profile layers and `docker-compose.yaml` file, see the [pingidentity-server-profiles/layered-profiles](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/layered-profiles) directory.
プロファイル レイヤーと `docker-compose.yaml` ファイルを含むこの例全体を表示するには、[pingidentity-server-profiles/layered-profiles](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/layered-profiles) ディレクトリを参照してください。
