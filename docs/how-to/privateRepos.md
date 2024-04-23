---
title: Using Private Git Repositories
---
<!--
# Using Private Git Repositories

In general, you do not want your server profiles to be public. Instead, you should persist your server profiles in private git repositories.

To use server profiles with private repositories, you must either:

* Pull using HTTPS
* Pull using SSH

For HTTPS with GitHub:

1. Generate an access token in GitHub.
2. Specify the access token in the URL you assign to the `SERVER_PROFILE_URL` environment variable in your YAML files.

For SSH:

* Include your keys and known hosts in the image under `/home/ping/.ssh`.
-->

# プライベート Git リポジトリの使用

一般に、サーバー プロファイルを公開することは望ましくありません。代わりに、サーバー プロファイルをプライベート git リポジトリに保存する必要があります。

プライベート リポジトリでサーバー プロファイルを使用するには、次のいずれかを行う必要があります。

- HTTPSを使ったpull
- SSHを使ったpull

GitHub を使用した HTTPS の場合:

1.e GitHub でアクセス トークンを生成します。
2. YAML ファイルの `SERVER_PROFILE_URL` 環境変数に割り当てる URL でアクセス トークンを指定します。

SSHの場合:

- `/home/ping/.ssh` の下のイメージにキーと既知のホストを含​​めます。

<!--
## Cloning from GitHub using HTTPS

### Creating a GitHub Access Token

1. In GitHub, go to `Settings` -> `Developer Settings` -> `Personal access tokens`.
1. Click `Generate new token` and assign the token a name.
1. Grant the token privilege to the `repo` group.

    > Copy the token to a secure location. You won't be able to view the token again.

1. At the bottom of the page, click `Generate Token`.
-->

## HTTPS を使用した GitHub からのクローン作成

### GitHub アクセス トークンの作成

1. In GitHub, go to `Settings` --> `Developer Settings` --> `Personal access tokens`.
1. GitHub で、`Settings` -> `Developer Settings` -> `Personal access tokens` に移動します。
1. `Generate new token` をクリックし、トークンに名前を割り当てます。
1. `repo` グループにトークン権限を付与します。

    > トークンを安全な場所にコピーします。トークンを再度表示することはできなくなります。

1. ページの下部にある `Generate Token` をクリックします。

<!--
### Using The Token In YAML

To use the token in your YAML file, include it in the `SERVER_PROFILE_URL` environment variable using this format:

```html
https://<github-username>:<github-token>@github.com/<your-repository>.git
```

For example:

```yaml
SERVER_PROFILE_URL=https://github_user:zqb4famrbadjv39jdi6shvl1xvozut7tamd5v6eva@github.com/pingidentity/server_profile.git
```
-->

### YAML でのトークンの使用

YAML ファイルでトークンを使用するには、次の形式を使用して `SERVER_PROFILE_URL` 環境変数にトークンを含めます。w

```html
https://<github-username>:<github-token>@github.com/<your-repository>.git
```

例:

```yaml
SERVER_PROFILE_URL=https://github_user:zqb4famrbadjv39jdi6shvl1xvozut7tamd5v6eva@github.com/pingidentity/server_profile.git
```

<!--
### Using Git Credentials in Profile URL

Typically, variables in a `SERVER_PROFILE_URL` string are not replaced. However, certain Git user and password variables can be replaced.

* To substitute for the user and password variables using values defined in your YAML files, include either or both `${SERVER_PROFILE_GIT_USER}` and `${SERVER_PROFILE_GIT_PASSWORD}` in your server profile URL. For example:

    ```sh
    SERVER_PROFILE_URL=https://${SERVER_PROFILE_GIT_USER}:${SERVER_PROFILE_GIT_PASSWORD}@github.com/pingidentity/server_profile.git
    ```

* When using layered server profiles, each layer can use the base user and password variables, or you can define values specific to that layer.

    For example, for a `license` server profile layer, you can use the `SERVER_PROFILE_LICENSE_GIT_USER` and `SERVER_PROFILE_LICENSE_GIT_PASSWORD` variables, and substitute for those variables using values defined in your YAML files.
-->

### プロファイル URL での Git 認証情報の使用

通常、`SERVER_PROFILE_URL` 文字列内の変数は置換されません。ただし、特定の Git ユーザーおよびパスワード変数は置き換えることができます。

- YAML ファイルで定義された値を使用してユーザー変数とパスワード変数を置き換えるには、サーバー プロファイル URL に `${SERVER_PROFILE_GIT_USER}` と `${SERVER_PROFILE_GIT_PASSWORD}` のいずれかまたは両方を含めます。例えば：

    ```sh
    SERVER_PROFILE_URL=https://${SERVER_PROFILE_GIT_USER}:${SERVER_PROFILE_GIT_PASSWORD}@github.com/pingidentity/server_profile.git
    ```

- 階層化されたサーバー プロファイルを使用する場合、各レイヤーで基本ユーザー変数とパスワード変数を使用することも、そのレイヤーに固有の値を定義することもできます。

    For example, for a `license` server profile layer, you can use the `SERVER_PROFILE_LICENSE_GIT_USER` and `SERVER_PROFILE_LICENSE_GIT_PASSWORD` variables, and substitute for those variables using values defined in your YAML files.
    たとえば、`license` サーバー プロファイル レイヤーの場合、`SERVER_PROFILE_LICENSE_GIT_USER` 変数と `SERVER_PROFILE_LICENSE_GIT_PASSWORD` 変数を使用し、YAML ファイルで定義された値を使用してこれらの変数を置き換えることができます。

<!--
## Cloning using SSH

* To clone using SSH, you can mount the necessary keys and known hosts files using a volume at `/home/ping/.ssh`, the home directory of the default user in our product images.
* To clone from GitHub, you must add the necessary SSH keys to your account through the account settings page.
-->

## SSHを使用したクローン作成

- SSH を使用してクローンを作成するには、製品イメージのデフォルト ユーザーのホーム ディレクトリである `/home/ping/.ssh` のボリュームを使用して、必要なキーと既知のホスト ファイルをマウントできます。
- GitHub からクローンを作成するには、アカウント設定ページから必要な SSH キーをアカウントに追加する必要があります。
