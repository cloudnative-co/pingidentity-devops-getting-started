---
title: pingctl Environment and Configuration Variables
---

<!--
# Configuration & Environment Variables

Configuration and Environment variables allow users to cache secure and repetitive settings into
a `pingctl` config file.  The default location of the file is `~/.pingidentity/config`.

You can specify a given configuration item in one of three ways: the `pingctl` config file, the user's current environment variables, or through command line arguments.  The order of priority (highest to lowest) is:

* Command-Line argument overrides (when available)
* `pingctl` config file
* Environment variable overrides
-->

# 構成変数と環境変数

構成変数と環境変数を使用すると、ユーザーは安全で反復的な設定を `pingctl` 構成ファイルにキャッシュできます。ファイルのデフォルトの場所は `~/.pingidentity/config` です。

特定の構成項目は、`pingctl` 構成ファイル、ユーザーの現在の環境変数、またはコマンド ライン引数の 3 つの方法のいずれかで指定できます。優先順位 (最高から最低) は次のとおりです。

* コマンドライン引数のオーバーライド (使用可能な場合)
* `pingctl` configファイル
* 環境変数のオーバーライド

<!--
## PingOne Variables

The standard **PingOne variables** used by `pingctl` are as follows:

| Variable                             | Description                                                                                                                                         |
| ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **PINGONE_API_URL**                  | [PingOne API URL](https://apidocs.pingidentity.com/pingone/platform/v1/api/#get-read-external-authentication-status) (i.e. api.pingone.com/v1)      |
| **PINGONE_AUTH_URL**                 | [PingOne Auth URL](https://apidocs.pingidentity.com/pingone/platform/v1/api/#changelog) (i.e. auth.pingone.com, auth.pingone.eu, auth.pingone.asia) |
| **PINGONE_ENVIRONMENT_ID**           | PingOne Environment ID GUID                                                                                                                         |
| **PINGONE_WORKER_APP_CLIENT_ID**     | PingOne Worker App ID GUID with access to PingOne Environment                                                                                       |
| **PINGONE_WORKER_APP_GRANT_TYPE**    | PingOne Worker App Grant Type to use.  Should be one of authorization_code, implicit or client_credential                                           |
| **PINGONE_WORKER_APP_REDIRECT_URI**  | PingOne Worker App available redirect_uri.  Defaults to http://localhost:8000                                                                       |
| **PINGONE_WORKER_APP_CLIENT_SECRET** | PingOne Worker App Secret providing authentication to PingOne Worker App ID GUID                                                                    |
-->

## PingOne 変数

`pingctl` で使用される標準の PingOne 変数は次のとおりです。

| 変数                                 | 説明                                                                                                                                                |
| ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **PINGONE_API_URL**                  | [PingOne API URL](https://apidocs.pingidentity.com/pingone/platform/v1/api/#get-read-external-authentication-status) (i.e. api.pingone.com/v1)      |
| **PINGONE_AUTH_URL**                 | [PingOne Auth URL](https://apidocs.pingidentity.com/pingone/platform/v1/api/#changelog) (i.e. auth.pingone.com, auth.pingone.eu, auth.pingone.asia) |
| **PINGONE_ENVIRONMENT_ID**           | PingOne Environment ID GUID                                                                                                                         |
| **PINGONE_WORKER_APP_CLIENT_ID**     | PingOne 環境にアクセスできる PingOne Worker App ID GUID                                                                                             |
| **PINGONE_WORKER_APP_GRANT_TYPE**    | 使用する PingOne Worker App のGrant Type。authorization_code、implicit、または client_credential のいずれかである必要があります                     |
| **PINGONE_WORKER_APP_REDIRECT_URI**  | PingOne Worker App が利用可能な redirect_uri。 デフォルトは http://localhost:8000                                                                   |
| **PINGONE_WORKER_APP_CLIENT_SECRET** | PingOne Worker App ID GUID に認証を提供する PingOne Worker App Secret                                                                               |

<!--
## Legacy ping-devops Variables

Prior to the `pingctl` CLI tool, `ping-devops` was used to help with the management of docker, docker-console, and kustomize deployments (this utility has been deprecated).  In configuring the legacy tool, several variables were used when deploying docker images into different environments.

The legacy variables still supported and managed by `pingctl` are as follows:

| Variable                          | Description                                                                                                             |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **PING_IDENTITY_ACCEPT_EULA**     | Specify `YES` or `NO` to accept [Ping Identity EULA](https://www.pingidentity.com/en/legal/subscription-agreement.html) |
| **PING_IDENTITY_DEVOPS_USER**     | [Ping DevOps User](https://devops.pingidentity.com/how-to/devopsRegistration/)                                     |
| **PING_IDENTITY_DEVOPS_KEY**      | [Ping DevOps Key](https://devops.pingidentity.com/how-to/devopsRegistration/)                                      |
| **PING_IDENTITY_DEVOPS_HOME**     | Home directory/path of your DevOps projects                                                                             |
| **PING_IDENTITY_DEVOPS_REGISTRY** | Default Docker registry from which to pull images                                                                              |
| **PING_IDENTITY_DEVOPS_TAG**      | Default DevOps tag to use for deployments (i.e. 2205)                                                                   |
-->

## 従来の ping-devops 変数

`pingctl` CLI ツールが登場する前は、docker、docker-console、および kustomize デプロイメントの管理を支援するために `ping-devops` が使用されていました (このユーティリティは非推奨になりました)。従来のツールの構成では、Docker イメージをさまざまな環境にデプロイするときにいくつかの変数が使用されました。

`pingctl` によって引き続きサポートおよび管理されている従来の変数は次のとおりです。

| 変数                              | 説明                                                                                                                    |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **PING_IDENTITY_ACCEPT_EULA**     | `YES` または `NO` を指定して [Ping Identity EULA](https://www.pingidentity.com/en/legal/subscription-agreement.html) に同意します |
| **PING_IDENTITY_DEVOPS_USER**     | [Ping DevOps User](https://devops.pingidentity.com/how-to/devopsRegistration/)                                     |
| **PING_IDENTITY_DEVOPS_KEY**      | [Ping DevOps Key](https://devops.pingidentity.com/how-to/devopsRegistration/)                                      |
| **PING_IDENTITY_DEVOPS_HOME**     | DevOps プロジェクトのホーム ディレクトリのパス                                                                     |
| **PING_IDENTITY_DEVOPS_REGISTRY** | イメージのpull元となるデフォルトの Docker レジストリ                                                               |
| **PING_IDENTITY_DEVOPS_TAG**      | デプロイメントに使用するデフォルトの DevOps タグ (i.e. 2205)                                                       |

<!--
## pingctl Variables

**Additional variables** honored by `pingctl` are:

| Variable                                   | Description                                                                                                                       |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| **PINGCTL_CONFIG**                         | Location of the `pingctl` configuration file. Set as an environment variable only.  Default: `~/.pingidentity/config`                                               |
| **PINGCTL_DEFAULT_OUTPUT**                 | Specifies default format of data returned. Command-Line arg `-o`. Default: `table`                                            |
| **PINGCTL_DEFAULT_POPULATION**             | Specifies default population to use for PingOne commands. Command-Line arg `-p`. Default: `Default`                           |
| **PINGCTL_OUTPUT_COLUMNS_{resource_type}** | Specify custom format of table csv data to be returned.   Command-Line arg `-c`. See more detail [below](#pingctl_output_columns) |
| **PINGCTL_OUTPUT_SORT_{resource_type}**    | Specify column to use for sorting data.   Command-Line arg `-s`. See more detail [below](#pingctl_output_sort)                               |
-->

## pingctl 変数

`pingctl` によって受け入れられる追加の変数は次のとおりです。

| Variable                                   | Description                                                                                                                       |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| **PINGCTL_CONFIG**                         | `pingctl` 構成ファイルの場所。環境変数としてのみ設定します。デフォルト: `~/.pingidentity/config`                                  |
| **PINGCTL_DEFAULT_OUTPUT**                 | 返されるデータのデフォルトの形式を指定します。コマンドライン引数 `-o`。デフォルト: `table`                                        |
| **PINGCTL_DEFAULT_POPULATION**             | PingOne コマンドに使用するデフォルトのpopulationを指定します。コマンドライン引数 `-p`。デフォルト: `Default`                      |
| **PINGCTL_OUTPUT_COLUMNS_{resource_type}** | 返されるテーブル CSV データのカスタム形式を指定します。コマンドライン引数 `-c`。詳細は[以下](#pingctl_output_columns)を参照       |
| **PINGCTL_OUTPUT_SORT_{resource_type}**    | データの並べ替えに使用する列を指定します。コマンドライン引数 `-s`。詳細は[以下](#pingctl_output_sort)を参照                       |

<!--
## PINGCTL_OUTPUT_COLUMNS

There are two classes of variables provided by `PINGCTL_OUTPUT`:

* `PINGCTL_OUTPUT_COLUMNS_{resource}` - Specifies the columns to display whenever a `pingctl pingone get {resource}` command is used.

    Same as the `-c` option on the command-line (see [pingctl pingone get](../tools/commands/pingone.md) command).

    Format of value should be constructed with `HeadingName:jsonName,HeadingName:jsonName`.  The best way to understand is by looking at the example of the default `USERS` resource:

    !!! note "Example PINGCTL_OUTPUT_COLUMNS_USERS setting and output"
        ```
        PINGCTL_OUTPUT_COLUMNS_USERS=LastName:name.family,FirstName:name.given
        ```

       Setting the above will generate output similar to:

        ```
        $ pingctl pingone get users
        LastName     FirstName
        --------     ---------
        Badham       Antonik
        Agnès        Enterle
        --
        2 'USERS' returned
        ```

        Alternatively, you can use the `-c` option as a command-line argument:

        ```
        $ pingctl pingone get users -c "LastName:name.family,FirstName:name.given,Username:username"
        LastName     FirstName    Username
        --------     ---------    --------
        Badham       Antonik      antonik_adham
        Agnès        Enterle      enterle_agnès
        --
        2 'USERS' returned
        ```
-->

## PINGCTL_OUTPUT_COLUMNS

`PINGCTL_OUTPUT` によって提供される変数には 2 つのクラスがあります。

* `PINGCTL_OUTPUT_COLUMNS_{resource}` - `pingctl pingone get {resource}` コマンドが使用されるたびに表示する列を指定します。

    コマンドラインの `-c` オプションと同じです ([pingctl pingone get](../tools/commands/pingone.md) コマンドを参照)。

    値の形式は、`HeadingName:jsonName,HeadingName:jsonName` で構築する必要があります。理解する最良の方法は、デフォルトの `USERS` リソースの例を見ることです。

    !!! note "PINGCTL_OUTPUT_COLUMNS_USERS の設定と出力の例"
        ```
        PINGCTL_OUTPUT_COLUMNS_USERS=LastName:name.family,FirstName:name.given
        ```

        上記を設定すると、次のような出力が生成されます。

        ```
        $ pingctl pingone get users
        LastName     FirstName
        --------     ---------
        Badham       Antonik
        Agnès        Enterle
        --
        2 'USERS' returned
        ```

        あるいは、コマンドライン引数として `-c` オプションを使用することもできます。

        ```
        $ pingctl pingone get users -c "LastName:name.family,FirstName:name.given,Username:username"
        LastName     FirstName    Username
        --------     ---------    --------
        Badham       Antonik      antonik_adham
        Agnès        Enterle      enterle_agnès
        --
        2 'USERS' returned
        ```

<!--
## PINGCTL_OUTPUT_SORT

* `PINGCTL_OUTPUT_SORT_{resource}` - specifies the column on which to sort.

    Same as the `-s` option on the command-line (see [pingctl pingone get](../tools/commands/pingone.md) command).

    Format of the value should be constructed with `jsonName`.  The name must be one of the entries in `PINGCTL_OUTPUT_COLUMNS_{resource}`.

    !!! note "Example PINGCTL_OUTPUT_SORT_USERS setting and output"
        ```
        PINGCTL_OUTPUT_SORT_USERS=name.family
        ```

        Setting the above will generate output similar to the following (note that the LastName (name.family) is sorted):

        ```
        $ pingctl pingone get users
        LastName     FirstName
        --------     ---------
        Agnès        Enterle
        Badham       Antonik
        --
        2 'USERS' returned
        ```

        Alternatively, you can use the `-s` option as a command-line argument:

        ```
        $ pingctl pingone get users -s "name.given"
        LastName     FirstName    Username
        --------     ---------    --------
        Agnès        Enterle      enterle_agnès
        Badham       Antonik      antonik_badham
        --
        2 'USERS' returned
        ```
-->

## PINGCTL_OUTPUT_SORT

* `PINGCTL_OUTPUT_SORT_{resource}` - 並べ替える列を指定します。

    コマンドラインの `-s` オプションと同じです ([pingctl pingone get](../tools/commands/pingone.md) コマンドを参照)。

    値の形式は `jsonName` で構築する必要があります。名前は、`PINGCTL_OUTPUT_COLUMNS_{resource}` のエントリの 1 つである必要があります。

    !!! note "PINGCTL_OUTPUT_SORT_USERS の設定と出力の例"
        ```
        PINGCTL_OUTPUT_SORT_USERS=name.family
        ```

        上記を設定すると、次のような出力が生成されます (LastName (name.family) がソートされていることに注意してください)。

        ```
        $ pingctl pingone get users
        LastName     FirstName
        --------     ---------
        Agnès        Enterle
        Badham       Antonik
        --
        2 'USERS' returned
        ```

        あるいは、 `-s` オプションをコマンドライン引数として使用することもできます。

        ```
        $ pingctl pingone get users -s "name.given"
        LastName     FirstName    Username
        --------     ---------    --------
        Agnès        Enterle      enterle_agnès
        Badham       Antonik      antonik_badham
        --
        2 'USERS' returned
        ```
