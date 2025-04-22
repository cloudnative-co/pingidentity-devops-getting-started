---
title:  Server Profile Structures
---
<!--
# Server Profile Structures

Each of the Docker images use a server profile structure that is specific to each product.
The structure (directory paths and data) of the server profile differs between products.
Depending on how you [Deploy Your Server Profile](../how-to/containerAnatomy.md), it will be pulled or mounted
into `/opt/in` on the container and used to stage your deployment.

The following locations are the server profile structures for each of our products with example usage. For help with an example of the basics, see the
[pingidentity-server-profiles/getting-started](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started) examples.

!!! note "Ignore `.sec` directories in examples"
    In the getting-started profile examples, you should not use the `.sec` directory when providing passwords to your containers.  These examples are only intended for demonstration purposes. Instead, set an environment variable with your secrets or orchestration later:

    ```shell
      PING_IDENTITY_PASSWORD="secret"
    ```
-->

# サーバープロファイル構造

各 Docker イメージは、各製品に固有のサーバー プロファイル構造を使用します。
サーバープロファイルの構造（ディレクトリパスやデータ）は製品ごとに異なります。
[サーバープロファイルをデプロイする方法](../how-to/containerAnatomy.md)に応じて、サーバー プロファイルはコンテナーの `/opt/in` にプルまたはマウントされ、デプロイメントのステージングに使用されます。

次の場所は、各製品のサーバー プロファイル構造と使用例です。基本的な例のヘルプについては、[pingidentity-server-profiles/getting-started](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started) の例を参照してください。

!!! note "例では .sec ディレクトリを無視する"
    getting-started プロファイルの例では、コンテナーにパスワードを提供するときに `.sec` ディレクトリを使用しないでください。これらの例は、デモンストレーションのみを目的としています。代わりに、後でシークレットまたはオーケストレーションを使用して環境変数を設定します。

    ```shell
      PING_IDENTITY_PASSWORD="secret"
    ```

## PingFederate

[getting-started/pingfederate](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started/pingfederate)の例を参照してください。

| Path                                                 | Location description                                                                                                       |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| instance                                             | 製品のディレクトリ レイアウトに従って、製品の実行時に使用するディレクトリとファイル。 |
| instance/server/default/data                         | PingFederate からエクスポートされた、抽出された構成アーカイブ。 |
| instance/bulk-config/data.json                       | PingFederate 管理 API `/bulk/export` からの JSON エクスポート。 |
| instance/server/default/deploy/OAuthPlayground.war   | OAuthPlayground Web アプリケーションを自動的にデプロイします。 |
| instance/server/default/conf/META-INF/hivemodule.xml | Hive モジュール構成をコンテナーに適用します。 OAuth クライアント、Grants、およびセッションを外部 DB に永続化するために使用されます。 |

!!! note "PingFederate Integeration Kits"
    デフォルトでは、PingFederate にはいくつかのIntegration KitとAdapterが同梱されています。デプロイメントで他のIntegration KitまたはAdapterが必要な場合は、それらを手動でダウンロードし、サーバー プロファイルの `server/default/deploy` 内に配置します。これらのリソースは、[ここ](https://www.pingidentity.com/en/resources/downloads/pingfederate.html)の製品ダウンロード ページにあります。

<!--
## PingAccess

Example at [getting-started/pingaccess](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started/pingaccess).

| Path                           | Location description                                                                                                       |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| instance                       | Directories and files that you want to be used at product runtime, in accordance with the directory layout of the product. |
| instance/conf/pa.jwk           | Used to decrypt a `data.json` configuration upon import.                                                                    |
| instance/data/data.json        | PA 6.1+ A config file that, if found by the container, is uploaded into the container.                                      |
| instance/data/PingAccess.mv.db | Database binary that would be ingested at container startup if found.                                                      |

!!! note "PingAccess Best Practices"
    PingAccess profiles are typically minimalist because the majority of PingAccess configurations can be found within a `data.json` or `PingAccess.mv.db` file. You should use `data.json` for configurations and only use `PingAccess.mv.db` if necessary. You can easily view and manipulate configurations directly in a JSON file as opposed to the binary `PingAccess.mv.db` file. This fact makes tracking changes in version control easier as well.

    PingAccess 6.1.x+ supports using only `data.json`, even when clustering. _However_ on  6.1.0.3 make sure `data.json` is only supplied to the admin node.
-->

<!-- When using a `data.json`, the container uses the PingAccess Admin API to import the data.json. This means:
1. The PingAccess server has to be running. So be careful when determining when the container is 'ready' to accept traffic. Check that the configuration has been imported, rather than just that the server is up. Refer to the `liveness.sh` within the image for an example.
2. Import only _needs_ to occur on the admin node. Typically engines can be  -->

<!--
!!! note "PingAccess 6.1.0+"
    PingAccess now supports native `data.json` ingestion, which is *the recommended method*. Place `data.json` or `data.json.subst` in `instance/conf/data/start-up-deployer`.

    > The JSON configuration file for PingAccess _must_ be named `data.json`.

    A `data.json` file that corresponds to earlier PingAccess versions _might_ be accepted. However, after you are on version 6.1.x, the `data.json` file will be forward compatible. This support means you are able to avoid upgrades for your deployments!

!!! note "PingAccess 6.0.x and earlier"

    The JSON configuration file for PingAccess _must_ be named `data.json` and located in the `instance/data` directory.

!!! note "All PingAccess versions"
    A corresponding file named `pa.jwk` must also exist in the `instance/conf` directory for the `data.json` file to be decrypted on import. To get a `data.json` and `pa.jwk` that work together, pull them both from the same running PingAccess instance.

    For example, if PingAccess is running in a local Docker container you can use these commands to export the `data.json` file and copy the `pa.jwk` file to your local Downloads directory:

    ```shell
        curl -k -u "Administrator:${ADMIN_PASSWORD}" -H "X-Xsrf-Header: PingAccess" https://localhost:9000/pa-admin-api/v3/config/export -o ~/Downloads/data.json

        docker cp <container_name>:/opt/out/instance/conf/pa.jwk ~/Downloads/pa.jwk
    ```

!!! note "Password Variables"
    You can find the PingAccess administrator password in `PingAccess.mv.db`, not in `data.json`. For this reason, you can use the following environment variables to manage different scenarios:

    * `PING_IDENTITY_PASSWORD`

        Use this variable if:

        * You're starting a PingAccess container without any configurations.
        * You're using only a `data.json` file for configurations.
        * Your `PingAccess.mv.db` file has a password other than the default "2Access".

        The `PING_IDENTITY_PASSWORD` value will be used for all interactions with the PingAccess Admin API (such as importing configurations and creating clustering).

    * `PA_ADMIN_PASSWORD_INITIAL`

        Use this variable in addition to `PING_IDENTITY_PASSWORD` to change the runtime admin password and override the password in `PingAccess.mv.db`.

        > If you use only `data.json` and do notpass `PING_IDENTITY_PASSWORD`, the password will default to "2FederateM0re". *Always* use `PING_IDENTITY_PASSWORD`.
-->
## PingAccess

[getting-started/pingaccess](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started/pingaccess)に例があります。

| Path                           | Location description                                                                                                       |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| instance                       | 製品のディレクトリ レイアウトに従って、製品の実行時に使用するディレクトリとファイル。 |
| instance/conf/pa.jwk           | インポート時に `data.json` 構成を復号化するために使用されます。|
| instance/data/data.json        | PingAccess 6.1以上で、コンテナーによって検出された場合、コンテナーにアップロードされる構成ファイル。 |
| instance/data/PingAccess.mv.db | 見つかった場合、コンテナーの起動時に取り込まれるデータベース バイナリ。 |

!!! note "PingAccess Best Practices"
    PingAccess 構成の大部分は `data.json` または `PingAccess.mv.db` ファイル内にあるため、PingAccess プロファイルは通常最小限のものです。構成には `data.json` を使用し、必要な場合にのみ `PingAccess.mv.db` を使用する必要があります。バイナリ `PingAccess.mv.db` ファイルではなく、JSON ファイルで直接構成を簡単に表示および操作できます。このため、バージョン管理での変更の追跡も容易になります。

    PingAccess 6.1.x 以降では、クラスタリングの場合でも、 `data.json` の使用のみがサポートされます。ただし、6.1.0.3 では、`data.json` が管理ノードにのみ提供されていることを確認してください。

<!-- When using a `data.json`, the container uses the PingAccess Admin API to import the data.json. This means:
1. The PingAccess server has to be running. So be careful when determining when the container is 'ready' to accept traffic. Check that the configuration has been imported, rather than just that the server is up. Refer to the `liveness.sh` within the image for an example.
2. Import only _needs_ to occur on the admin node. Typically engines can be  -->

!!! note "PingAccess 6.1.0+"
    PingAccess は、推奨される方法であるネイティブの `data.json` 取り込みをサポートするようになりました。 `data.json` または `data.json.subst` を `instance/conf/data/start-up-deployer` に配置します。

    > PingAccess の JSON 構成ファイルの名前は `data.json` である必要があります。

    以前の PingAccess バージョンに対応する `data.json` ファイルが受け入れられる場合があります。ただし、バージョン 6.1.x 以降では、`data.json` ファイルには上位互換性があります。このサポートは、展開のアップグレードを回避できることを意味します。

!!! note "PingAccess 6.0.x 以前"

    PingAccess の JSON 構成ファイルは、`data.json` という名前で、`instance/data` ディレクトリに配置する必要があります。

!!! note "すべての PingAccess バージョン"
    インポート時に `data.json` ファイルを復号化するには、`pa.jwk` という名前の対応するファイルも `instance/conf` ディレクトリに存在する必要があります。連携して動作する `data.json` と `pa.jwk` を取得するには、実行中の同じ PingAccess インスタンスから両方を取得します。

    たとえば、PingAccess がローカルの Docker コンテナーで実行されている場合、次のコマンドを使用して `data.json` ファイルをエクスポートし、`pa.jwk` ファイルをローカルのダウンロード ディレクトリにコピーできます。

    ```shell
        curl -k -u "Administrator:${ADMIN_PASSWORD}" -H "X-Xsrf-Header: PingAccess" https://localhost:9000/pa-admin-api/v3/config/export -o ~/Downloads/data.json

        docker cp <container_name>:/opt/out/instance/conf/pa.jwk ~/Downloads/pa.jwk
    ```

!!! note "パスワード変数"
    PingAccess 管理者のパスワードは、`data.json` ではなく `PingAccess.mv.db` にあります。このため、次の環境変数を使用してさまざまなシナリオを管理できます。

    - `PING_IDENTITY_PASSWORD`

        次の場合にこの変数を使用します。

        - 構成を行わずに PingAccess コンテナを開始しようとしています。
        - 構成には `data.json` ファイルのみを使用します。
        - `PingAccess.mv.db` ファイルには、デフォルトの「2Access」以外のパスワードが設定されています。

        `PING_IDENTITY_PASSWORD` 値は、PingAccess Admin API とのすべてのやり取り (構成のインポートやクラスタリングの作成など) に使用されます。

    * `PA_ADMIN_PASSWORD_INITIAL`

        `PING_IDENTITY_PASSWORD` に加えてこの変数を使用して、ランタイム管理者パスワードを変更し、`PingAccess.mv.db` のパスワードを上書きします。

        > `data.json` のみを使用し、`PING_IDENTITY_PASSWORD` を渡さない場合、パスワードはデフォルトの「2FederateM0re」になります。 *常に* `PING_IDENTITY_PASSWORD` を使用してください。

<!--
## Ping Data Products

The Ping Data Products (PingDirectory, PingDataSync, PingAuthorize, PingDirectoryProxy) follow the same structure for server-profiles.

Example at [getting-started/pingdirectory](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started/pingdirectory).

| Path       | Location description                                                                                                                                                            |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pd.profile | Server profile matching the structure as defined by [PingDirectory Server Profiles](https://docs.pingidentity.com/bundle/pingdirectory-80/page/eae1564011467693.html)           |
| instance   | Directories and files that you want to be used at product runtime, in accordance with the layout of the product. In general, this should be **non existing or empty**.          |
| env-vars   | You can set environment variables used during deployment.  See [Variables and Scope](variableScoping.md) for more info.   In general, this should be **non existing or empty**. |
| extensions   | You can provide URLs to download Server SDK extensions in a remote.list file.  See [Including Extensions in PingData Server Profiles](../how-to/profilesPingDataExtensions.md) for more info. |

!!! note "Ping Data Server Profile Best Practices"
    * In most circumstances, the `pd.profile` directory should be the only directory in the server profile.
    * All environment variables should be provided through Kubernetes configmaps/secrets and a secret management tool.
      Be careful providing an `env-vars` and if you do, please review [Variables and Scope](variableScoping.md)

!!! note "Creating a `pd.profile` from scratch"
    Use the `manage-profile` tool (found in product `bin` directory) to generate a `pd.profile` from an existing Ping Data 8.0+ deployment.  An example on creating this `pd.profile` looks like:

       ```bash
       manage-profile generate-profile --profileRoot /tmp/pd.profile
       rm /tmp/pd.profile/setup-arguments.txt
       ```

   Follow the instructions provided when you run the `generate-profile` to ensure that you include any additional components, such as `encryption-settings`.
-->

## Ping Data 製品

Ping データ製品 (PingDirectory、PingDataSync、PingAuthorize、PingDirectoryProxy) は、サーバー プロファイルと同じ構造に従います。

[getting-started/pingdirectory](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started/pingdirectory)に例があります。

| Path       | Location description                                                                                                                                                            |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pd.profile | [PingDirectory Server Profiles](https://docs.pingidentity.com/bundle/pingdirectory-80/page/eae1564011467693.html)で定義された構造と一致するサーバー プロファイル |
| instance   | 製品のレイアウトに従って、製品の実行時に使用するディレクトリとファイル。通常、これは **存在しないか空** である必要があります。 |
| env-vars   | デプロイメント中に使用される環境変数を設定できます。詳細については、[Variables and Scope](variableScoping.md)を参照してください。通常、これは **存在しないか空** である必要があります。|
| extensions   | サーバー SDK 拡張機能をダウンロードするための URL を、remote.list ファイルに指定できます。詳細については、[Including Extensions in PingData Server Profiles](../how-to/profilesPingDataExtensions.md)を参照してください。 |

!!! note "Ping Data Server Profile Best Practices"
    - ほとんどの状況では、`pd.profile` ディレクトリがサーバー プロファイル内の唯一のディレクトリである必要があります。
    - すべての環境変数は、Kubernetes configmaps/secrets およびシークレット管理ツールを通じて提供する必要があります。 `env-vars` を指定する場合は、[Variables and Scope](variableScoping.md)を確認してください。

!!! note "`pd.profile` を最初から作成する"
    `manage-proile` ツール (製品の `bin` ディレクトリにあります) を使用して、既存の Ping Data 8.0+ 展開から `pd.profile` を生成します。この `pd.profile` の作成例は次のようになります。

       ```bash
       manage-profile generate-profile --profileRoot /tmp/pd.profile
       rm /tmp/pd.profile/setup-arguments.txt
       ```

   `generate-profile` の実行時に表示される指示に従って、`encryption-settings` などの追加コンポーネントが確実に含まれていることを確認します。
