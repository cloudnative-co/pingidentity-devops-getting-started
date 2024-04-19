---
title: Customizing Server Profiles
---
<!--
# Customizing Server Profiles

When you deployed the full stack of product containers in [Getting Started](../get-started/introduction.md), you used the server profiles associated with each of our products. In the YAML files, you'll see entries like the following for each product instance:

```yaml
environment:
  - SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
  - SERVER_PROFILE_PATH=baseline/pingaccess
```

Our [pingidentity-server-profiles](https://github.com/pingidentity/pingidentity-server-profiles) repository, indicated by the `SERVER_PROFILE_URL` environment variable, contains the server profiles we use for our DevOps deployment examples. The `SERVER_PROFILE_PATH` environment variable indicates the location of the product profile data to use. In the previous example, the PingAccess profile data is located in the `baseline/pingaccess` directory.

We use environment variables for certain startup and runtime configuration settings of both standalone and orchestrated deployments. You can find environment variables that are common to all product images in the [PingBase Image Directory](../docker-images/pingbase/README.md). There are also product-specific environment variables. You can find these in the [Docker Image Information](../docker-images/dockerImagesRef.md) for each available product.
-->

# サーバープロファイルのカスタマイズ

[Getting Started](../get-started/introduction.md)で製品コンテナのフルスタックをデプロイしたときは、各製品に関連付けられたサーバー プロファイルを使用しました。 YAML ファイルには、製品インスタンスごとに次のようなエントリが表示されます。

```yaml
environment:
  - SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git
  - SERVER_PROFILE_PATH=baseline/pingaccess
```

`SERVER_PROFILE_URL` 環境変数で示される [pingidentity-server-profiles](https://github.com/pingidentity/pingidentity-server-profiles) リポジトリには、DevOps 導入例で使用するサーバー プロファイルが含まれています。 `SERVER_PROFILE_PATH` 環境変数は、使用する製品プロファイル データの場所を示します。前の例では、PingAccess プロファイル データは、`baseline/pingaccess` ディレクトリにあります。

スタンドアロン展開とオーケストレーション展開の両方の特定の起動およびランタイム構成設定に環境変数を使用します。すべての製品イメージに共通の環境変数は、[PingBase Image Directory](../docker-images/pingbase/README.md)にあります。製品固有の環境変数もあります。これらは、利用可能な各製品の [Docker Image Information](../docker-images/dockerImagesRef.md)で見つけることができます。

<!--
## Before you begin

You must:

* Complete [Get Started](../get-started/introduction.md) to set up your DevOps environment and run a test deployment of the products.
* Understand the [Anatomy of the Product Containers](containerAnatomy.md).
-->

## はじめる前に

下記のことを必ず実施してください:

* [Get Started](../get-started/introduction.md)を完了して、DevOps 環境をセットアップし、製品のテスト展開を実行します。
* [Anatomy of the Product Containers](containerAnatomy.md)を理解します。

<!--
## About this task

You will:

* Add or change the environment variables used for any of our server profiles to better fit your purposes.

    You can find these variables in the [Server Profiles Repository](https://github.com/pingidentity/pingidentity-server-profiles) for each product.

    For example, the location for the `env_vars` file for PingAccess is located in the [baseline/pingaccess server profile](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/baseline/pingaccess).

* Modify one of our server profiles to reflect an existing Ping Identity product installation in your organization.

    You can do this by either:

    * Forking our server profiles repository (`https://github.com/pingidentity/pingidentity-server-profiles`) to your Github repository
    * Using local directories
-->

## このタスクについて

You will:

* 目的に合わせて、サーバー プロファイルに使用される環境変数を追加または変更します。

    これらの変数は、各製品の[Server Profiles Repository](https://github.com/pingidentity/pingidentity-server-profiles)で見つけることができます。

    たとえば、PingAccess の `env_vars` ファイルの場所は、[baseline/pingaccess server profile](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/baseline/pingaccess)にあります。

* サーバー プロファイルの 1 つを変更して、組織内の既存の Ping Identity 製品のインストールを反映します。

    これは次のいずれかの方法で行うことができます。

    * サーバー プロファイル リポジトリ (https://github.com/pingidentity/pingidentity-server-profiles) を Github リポジトリにフォークする
    * ローカルディレクトリの使用

<!--
## Adding or Changing Environment Variables

1. Select any environment variables to add from either:

    * The product-specific environment variables in the [Docker Images Information](../docker-images/dockerImagesRef.md)
    * The environment variables common to all of our products in the [PingBase Image Directory](../docker-images/pingbase/README.md)

1. From the `baseline`, `getting-started`, or `simple-sync`
   directories in the [Server Profiles Repository](https://github.com/pingidentity/pingidentity-server-profiles), select the product whose profile you want to modify.

1. Open the `env_vars` file associated with the product and either:

    * Add any of the environment variables you've selected.
    * Change the existing environment variables to fit your purpose.
-->

## 環境変数の追加または変更

1. 追加する環境変数を次のいずれかから選択します。

    * [Docker Images Information](../docker-images/dockerImagesRef.md)の製品固有の環境変数
    * [PingBase Image Directory](../docker-images/pingbase/README.md)内のすべての製品に共通の環境変数

1. [Server Profiles Repository](https://github.com/pingidentity/pingidentity-server-profiles)の`baseline`、`getting-started`、または`simple-sync`ディレクトリから、プロファイルを変更する製品を選択します。

1. 製品に関連付けられた `env_vars` ファイルを開き、次のいずれかを実行します。

    * 選択した環境変数を追加します。
    * 目的に合わせて既存の環境変数を変更します。

<!--
## Modifying a Server Profile

You can modify one of our server profiles based on data from your existing Ping Identity product installation.

Modify a server profile by either:

* Using your Github repository
* Using local directories
-->

## サーバープロファイルの変更

既存の Ping Identity 製品インストールのデータに基づいて、サーバー プロファイルの 1 つを変更できます。

次のいずれかの方法でサーバー プロファイルを変更します。

* Github リポジトリの使用
* ローカルディレクトリの使用

<!--
### Using Your Github Repository

In this example PingFederate installation, using the Github Repository uses a server profile provided through a Github URL and assigned to the `SERVER_PROFILE_PATH` environment variable, such as `--env SERVER_PROFILE_PATH=getting-started/pingfederate`).

1. Export a [configuration archive](https://docs.pingidentity.com/r/en-us/pingfederate-120/help_configurationarchivetasklet_selectimportexportstate) as a *.zip file from a PingFederate installation to a local directory.

    > Make sure this is *exported* as a .zip rather than compressing it yourself.

1. Sign on to Github and fork [https://github.com/pingidentity/pingidentity-server-profiles](https://github.com/pingidentity/pingidentity-server-profiles) into your own GitHub repository.

1. Open a terminal, create a new directory, and clone your Github repository to a local directory. For example:

    ```sh
    mkdir /tmp/pf_to_docker
    cd /tmp/pf_to_docker
    git clone https://github.com/<github-username>/pingidentity-server-profiles.git
    ```

    Where `<github-username>` is the name you used to sign on to the Github account.

1. Go to the location where you cloned your fork of our `pingidentity-server-profiles` repository, and replace the `/data` directory in `getting-started/pingfederate/instance/server/default` with the `data` directory you exported from your existing PingFederate installation. For example:

    ```sh
    cd pingidentity-server-profiles/getting-started/pingfederate/instance/server/default
    rm -rf data
    unzip -qd data <path_to_your_configuration_archive>/data.zip
    ```

    Where `<path_to_your_configuration_archive>` is the location for your exported PingFederate configuration archive.

    You now have a local server profile based on your existing PingFederate installation.

    !!! note "Pushing to Github"
        You should push to Github only what is necessary for your customizations. Our Docker images create the `/opt/out` directory using a product's base install and layering a profile (set of files) on top.

1. Push your changes (your local server profile) to the Github repository where you forked our server profile repository.

    You now have a server profile available through a Github URL.

1. Deploy the PingFederate container.

    !!! note "Saving Changes"
        To save any changes you make after the container is running, add the entry `--volume <local-path>:/opt/out` to the `docker run` command, where &lt;local-path&gt; is a directory you haven't already created. For more information, see [Saving Your Changes](../how-to/saveConfigs.md).

    As in this example, the environment variables `SERVER_PROFILE_URL` and `SERVER_PROFILE_PATH` direct Docker to use the server profile you've modified and pushed to Github:

    ```sh
    docker run \
      --name pingfederate \
      --publish 9999:9999 \
      --publish 9031:9031 \
      --detach \
      --env SERVER_PROFILE_URL=https://github.com/<your_username>/pingidentity-server-profiles.git \
      --env SERVER_PROFILE_PATH=getting-started/pingfederate \
      --env-file ~/.pingidentity/config \
      pingidentity/pingfederate:edge
    ```

    !!! note "Private Repo"
        If your GitHub server-profile repo is private, use the `username:token` format so the container can access the repository. For example, `https://github.com/<your_username>:<your_access_token>/pingidentity-server-profiles.git`. For more information, see [Using Private Github Repositories](privateRepos.md).

1. To display the logs as the container starts up, enter:

    ```sh
    docker container logs -f pingfederate
    ```

1. In a browser, go to [https://localhost:9999/pingfederate/app](https://localhost:9999/pingfederate/app) to display the PingFederate console.
-->

### Github リポジトリの使用

この例の PingFederate インストールでは、Github リポジトリを使用して、Github URL を通じて提供され、`SERVER_PROFILE_PATH` 環境変数 `--env SERVER_PROFILE_PATH=getting-started/pingfederate` などに割り当てられたサーバー プロファイルを使用します。

1. [configuration archive](https://docs.pingidentity.com/r/en-us/pingfederate-120/help_configurationarchivetasklet_selectimportexportstate)を *.zip ファイルとして PingFederate インストールからローカル ディレクトリにエクスポートします。

    > これを自分で圧縮するのではなく、必ず .zip として **エクスポート** してください。

1. Github にサインオンし、[https://github.com/pingidentity/pingidentity-server-profiles](https://github.com/pingidentity/pingidentity-server-profiles) を独自の GitHub リポジトリにフォークします。

1. ターミナルを開き、新しいディレクトリを作成し、Github リポジトリのクローンをローカル ディレクトリに作成します。例えば：

    ```sh
    mkdir /tmp/pf_to_docker
    cd /tmp/pf_to_docker
    git clone https://github.com/<github-username>/pingidentity-server-profiles.git
    ```

    ここで `<github-username>` は、Github アカウントへのサインオンに使用した名前です。

1. `pingidentity-server-profiles` リポジトリのフォークをクローンした場所に移動し、`getting-started/pingfederate/instance/server/default` の `data` ディレクトリを、既存の PingFederate インストールからエクスポートしたデータ ディレクトリに置き換えます。例えば：

    ```sh
    cd pingidentity-server-profiles/getting-started/pingfederate/instance/server/default
    rm -rf data
    unzip -qd data <path_to_your_configuration_archive>/data.zip
    ```

    `<path_to_your_configuration_archive>` は、エクスポートされた PingFederate 構成アーカイブの場所です。

    これで、既存の PingFederate インストールに基づいたローカル サーバー プロファイルが作成されました。

    !!! note "Githubにプッシュする"
        カスタマイズに必要なものだけを Github にプッシュする必要があります。 Docker イメージは、製品の基本インストールを使用して `/opt/out` ディレクトリを作成し、その上にプロファイル (ファイルのセット) を重ねます。

1. 変更内容 (ローカル サーバー プロファイル) を、サーバー プロファイル リポジトリをフォークした Github リポジトリにプッシュします。

    これで、Github URL からサーバー プロファイルを利用できるようになりました。

1. PingFederate コンテナをデプロイする

    !!! note "変更の保存"
        コンテナーの実行後に行った変更を保存するには、 `docker run` コマンドにエントリ `--volume <local-path>:/opt/out` を追加します。&lt;local-path&gt; はまだ作成していないディレクトリです。詳細については、[Saving Your Changes](../how-to/saveConfigs.md)を参照してください。

    この例のように、環境変数 `SERVER_PROFILE_URL` と `SERVER_PROFILE_PATH` は、変更して Github にプッシュしたサーバー プロファイルを使用するように Docker に指示します。

    ```sh
    docker run \
      --name pingfederate \
      --publish 9999:9999 \
      --publish 9031:9031 \
      --detach \
      --env SERVER_PROFILE_URL=https://github.com/<your_username>/pingidentity-server-profiles.git \
      --env SERVER_PROFILE_PATH=getting-started/pingfederate \
      --env-file ~/.pingidentity/config \
      pingidentity/pingfederate:edge
    ```

    !!! note "プライベートリポジトリ"
        GitHub サーバー プロファイル リポジトリがプライベートである場合は、コンテナーがリポジトリにアクセスできるように、`username:token`形式を使用します。たとえば、`https://github.com/<your_username>:<your_access_token>/pingidentity-server-profiles.git` です。詳細については、[Using Private Github Repositories](privateRepos.md)を参照してください。

1. コンテナの起動時にログを表示するには、次のように入力します。

    ```sh
    docker container logs -f pingfederate
    ```

1. ブラウザで [https://localhost:9999/pingfederate/app](https://localhost:9999/pingfederate/app) に移動して、PingFederate コンソールを表示します。

<!--
### Using Local Directories

This method is particularly helpful when developing locally and the configuration isn't ready to be distributed (using Github, for example).

We'll use PingFederate as an example. The local directories used by our containers to persist state and data, `/opt/in` and `/opt/out`, will be bound to another local directory and mounted as Docker volumes. This is our infrastructure for modifying the server profile.

!!! warning "Bind Mounts in Production"
    Docker recommends that you never use bind mounts in a production environment. This method is solely for developing server profiles. For more information, see the [Docker Documentation](https://docs.docker.com/storage/volumes/).

* The `/opt/out` directory

    All configurations and changes during our container runtimes (persisted data) are captured here. For example, the PingFederate image `/opt/out/instance` contains much of the typical PingFederate root directory:
    ```text
    .
    ├── README.md
    ├── SNMP
    ├── bin
    ├── connection_export_examples
    ├── etc
    ├── legal
    ├── lib
    ├── log
    ├── modules
    ├── sbin
    ├── sdk
    ├── server
    ├── tools
    └── work
    ```

* The `/opt/in` directory

    If a mounted `opt/in` directory exists, our containers reference this directory at startup for any server profile structures or other relevant files. This method is in contrast to a server profile provided using a Github URL assigned to the `SERVER_PROFILE_PATH` environment variable, such as, `--env SERVER_PROFILE_PATH=getting-started/pingfederate`.

    > For the data each product writes to a mounted `/opt/in` directory, see [Server profile structures](../reference/profileStructures.md).

    These directories are useful for building and working with local server-profiles. The `/opt/in` directory is particularly valuable if you don't want your containers to access Github for data (the default for our server profiles).

The following example deployment uses PingFederate.

1. Deploy PingFederate using our sample [getting-started Server Profile](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started/pingfederate), and mount `/opt/out` to a local directory:

    ```sh
    docker run \
        --name pingfederate \
        --publish 9999:9999 \
        --detach \
        --env SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git \
        --env SERVER_PROFILE_PATH=getting-started/pingfederate \
        --env-file ~/.pingidentity/config \
        --volume /tmp/docker/pf:/opt/out \
    pingidentity/pingfederate:edge
    ```

    > Make sure the local directory (in this case, `/tmp/docker/pf`) isn't already created. Docker needs to create this directory for the mount to `/opt/out`.

2. Go to the mounted local directory (in this case, `/tmp/docker/pf`), then make and save some configuration changes to PingFederate using the management console.

    As you save the changes, you'll be able to see the files in the mounted directory change. For PingFederate, an `instance` directory is created. This is a PingFederate server profile.

3. Stop and remove the container and start a new container, adding another `/tmp/docker/pf` bind mounted volume, this time to `/opt/in`:

    ```sh
    docker container rm pingfederate

    docker run \
      --name pingfederate-local \
      --publish 9999:9999 \
      --detach \
      --volume /tmp/docker/pf:/opt/out \
      --volume /tmp/docker/pf:/opt/in \
    pingidentity/pingfederate:edge
    ```

      The new container will now use the changes you made using the PingFederate console. In the logs, you can see where `/opt/in` is used:

    ```sh
    docker logs pingfederate-local
   ```

4. Stop and remove the new container.

    Remember your `/tmp/docker/pf` directory will stay until you remove it (or until your machine is rebooted because this is in the `/tmp` directory):

    ```sh
    docker container rm pingfederate-local
    ```

    If you also want to remove your work, enter:

    ```sh
    rm -rf /tmp/docker/pf
    ```
-->

### ローカルディレクトリの使用

この方法は、ローカルで開発していて、構成を配布する準備ができていない場合 (たとえば、Github を使用する場合) に特に役立ちます。

例として PingFederate を使用します。コンテナーが状態とデータを保持するために使用するローカル ディレクトリ、`/opt/in` および `/opt/out` は、別のローカル ディレクトリにバインドされ、Docker ボリュームとしてマウントされます。これはサーバー プロファイルを変更するためのインフラストラクチャです。

!!! warning "本番環境でのバインドマウント"
    Docker では、運用環境ではバインド マウントを決して使用しないことをお勧めします。この方法はサーバー プロファイルの開発専用です。詳細については、[Docker Documentation](https://docs.docker.com/storage/volumes/)を参照してください。

* `/opt/out` ディレクトリ

    コンテナーのランタイム中のすべての構成と変更 (永続データ) がここにキャプチャされます。たとえば、PingFederate イメージ `/opt/out/instance` には、一般的な PingFederate ルート ディレクトリの多くが含まれています。
    ```text
    .
    ├── README.md
    ├── SNMP
    ├── bin
    ├── connection_export_examples
    ├── etc
    ├── legal
    ├── lib
    ├── log
    ├── modules
    ├── sbin
    ├── sdk
    ├── server
    ├── tools
    └── work
    ```

* `/opt/in` ディレクトリ

    マウントされた `opt/in` ディレクトリが存在する場合、コンテナは起動時にサーバー プロファイル構造またはその他の関連ファイルについてこのディレクトリを参照します。この方法は、 `--env SERVER_PROFILE_PATH=getting-started/pingfederate` など、`SERVER_PROFILE_PATH` 環境変数に割り当てられた Github URL を使用して提供されるサーバー プロファイルとは対照的です。

    > 各製品がマウントされた `/opt/in` ディレクトリに書き込むデータについては、[Server profile structures](../reference/profileStructures.md)を参照してください。

    これらのディレクトリは、ローカル サーバー プロファイルの構築と操作に役立ちます。 `/opt/in` ディレクトリは、コンテナがデータのために Github にアクセスしたくない場合に特に役立ちます (サーバー プロファイルのデフォルト)。

次の展開例では、PingFederate を使用します。

1. サンプルの[getting-started Server Profile](https://github.com/pingidentity/pingidentity-server-profiles/tree/master/getting-started/pingfederate)を使用して PingFederate をデプロイし、`/opt/out` をローカル ディレクトリにマウントします。

    ```sh
    docker run \
        --name pingfederate \
        --publish 9999:9999 \
        --detach \
        --env SERVER_PROFILE_URL=https://github.com/pingidentity/pingidentity-server-profiles.git \
        --env SERVER_PROFILE_PATH=getting-started/pingfederate \
        --env-file ~/.pingidentity/config \
        --volume /tmp/docker/pf:/opt/out \
    pingidentity/pingfederate:edge
    ```

    > ローカル ディレクトリ (この場合は `/tmp/docker/pf`) がまだ作成されていないことを確認してください。 Docker は、`/opt/out` にマウントするためにこのディレクトリを作成する必要があります。

2. マウントされたローカル ディレクトリ (この場合は `/tmp/docker/pf`) に移動し、管理コンソールを使用して構成を変更し、PingFederate に保存します。

    変更を保存すると、マウントされたディレクトリ内のファイルが変更されるのを確認できるようになります。 PingFederate の場合、`instance` ディレクトリが作成されます。これは PingFederate サーバー プロファイルです。

3. コンテナを停止して削除し、新しいコンテナを起動して、別の `/tmp/docker/pf` バインド マウントされたボリュームを今回は `/opt/in` に追加します。

    ```sh
    docker container rm pingfederate

    docker run \
      --name pingfederate-local \
      --publish 9999:9999 \
      --detach \
      --volume /tmp/docker/pf:/opt/out \
      --volume /tmp/docker/pf:/opt/in \
    pingidentity/pingfederate:edge
    ```

      新しいコンテナは、PingFederate コンソールを使用して行った変更を使用するようになります。ログでは、`/opt/in` が使用されている場所を確認できます。

    ```sh
    docker logs pingfederate-local
   ```

4. 新しいコンテナを停止して削除する

    `/tmp/docker/pf` ディレクトリは、削除するまで (または、`/tmp` ディレクトリにあるためマシンを再起動するまで) そのまま残ることに注意してください。

    ```sh
    docker container rm pingfederate-local
    ```

    作業内容も削除したい場合は、次のように入力します。

    ```sh
    rm -rf /tmp/docker/pf
    ```
