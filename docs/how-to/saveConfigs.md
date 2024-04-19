---
title: Saving Your Configuration Changes
---
<!--
# Saving your configuration changes

To save any configuration changes you make when using the products in the stack, you must set up a local Docker volume to persist state and data for the stack. If you don't do this, whenever you bring the stack down, your configuration changes will be lost.

Mount a Docker volume location to the Docker `/opt/out` directory for the container. The location must be to a directory you haven't already created. Our Docker containers use the `/opt/out` directory to store application data.

!!! warning "Mounting to /opt/out"
    Make sure the local directory isn't already created. Docker needs to create this directory for the mount to `/opt/out`.

You can mount a Docker volume for containers in a stack or for standalone containers.
-->

# 設定変更の保存

スタック内の製品を使用するときに行った構成の変更を保存するには、スタックの状態とデータを永続化するローカル Docker ボリュームをセットアップする必要があります。これを行わないと、スタックをダウンするたびに、構成の変更が失われます。

Docker ボリュームの場所をコンテナーの Docker `/opt/out` ディレクトリにマウントします。場所は、まだ作成していないディレクトリにする必要があります。 Docker コンテナは、`/opt/out` ディレクトリを使用してアプリケーション データを保存します。

!!! warning "/opt/outへのマウント"
    ローカル ディレクトリがまだ作成されていないことを確認してください。 Docker は、`/opt/out` にマウントするためにこのディレクトリを作成する必要があります。

スタック内のコンテナーまたはスタンドアロン コンテナーの Docker ボリュームをマウントできます。

<!--
## Bind mounting for a stack

1. Add a `volumes` section under the container entry for each product in the `docker-compose.yaml` file you're using for the stack.
1. Under the `volumes` section, add a location to persist your data. For example:

      ```yaml
      pingfederate:
      .
      .
      .
      volumes:
      - /tmp/compose/pingfederate_1:/opt/out
      ```

1. In the `environment` section, comment out the `SERVER_PROFILE_PATH` setting.

    The container then uses your `volumes` entry to supply the product state and data, including your configuration changes.

    When the container starts, this mounts `/tmp/compose/pingfederate_1` to the `/opt/out` directory in the container. You can also view the product logs and data in the `/tmp/compose/pingfederate_1` directory.

1. Repeat this process for the remaining container entries in the stack.
-->

## スタックのバインドマウント

1. スタックに使用している `docker-compose.yaml` ファイル内の各製品のコンテナー エントリの下に`voluems` セクションを追加します。
1. `voluems` セクションで、データを永続化する場所を追加します。例えば：

      ```yaml
      pingfederate:
      .
      .
      .
      volumes:
      - /tmp/compose/pingfederate_1:/opt/out
      ```

1. `environment` セクションで、`SERVER_PROFILE_PATH` 設定をコメントアウトします。

    次に、コンテナーは`volumes` エントリを使用して、構成の変更を含む製品の状態とデータを提供します。

    コンテナーが起動すると、`/tmp/compose/pingfederate_1` がコンテナー内の `/opt/out` ディレクトリにマウントされます。 `/tmp/compose/pingfederate_1` ディレクトリ内の製品ログとデータを表示することもできます。

1. スタック内の残りのコンテナ エントリに対してこのプロセスを繰り返します。

<!--
## Bind mounting for a standalone container

Add a `volume` entry to the `docker run` command:

   ```sh
   docker run \
      --name pingfederate \
      --volume <local-path>:/opt/out \
   pingidentity/pingfederate:edge
   ```
-->

## スタンドアロンコンテナのバインドマウント

`volume` エントリを `docker run` コマンドに追加します。

   ```sh
   docker run \
      --name pingfederate \
      --volume <local-path>:/opt/out \
   pingidentity/pingfederate:edge
   ```

<!--
## Getting started with Docker Compose mounts

Within many of the docker-compose.yaml files in the Getting-Started [repository](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/11-docker-compose), volume mounts to `opt/out` have been included to persist your configuration across container restarts.

* To view the list of persisted volumes, enter:

    ```sh
    docker volume list
    ```

* To view the contents of the /opt/out/ volume when the container is running, enter:

    ```sh
    docker container exec -it <container id> sh
    cd out
    ```

* To view the contents of the /opt/out/ volume when the container is stopped, enter:

    ```sh
    docker run --rm -i -v=<volume name>:/opt/out alpine ls
    ```

* To remove a volume, enter:

    ```sh
    docker volume rm <volume name>
    ```

* To copy files from the container to your local filesystem, enter:

    ```sh
    docker cp \
       <container id>:<source path> \
       <destination path>
    eg.
    docker cp \
       b867054293a1:/opt/out \
       ~/pingfederate/
    ```

* To copy files from your local filesystem to the container, enter:

    ```sh
    docker cp \
       <source path> \
       <container id>:<destination path>
    eg.
    docker cp \
       myconnector.jar \
       bb867054293a186:/opt/out/instance/server/default/deploy/
    ```
-->

## Docker Compose マウントを開始する

Getting-Started [repository](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/11-docker-compose)の docker-compose.yaml ファイルの多くには、コンテナーの再起動後も構成を維持するために、`out/out` するボリューム マウントが含まれています。

* 永続ボリュームのリストを表示するには、次のように入力します。

    ```sh
    docker volume list
    ```

* コンテナーの実行中に /opt/out/ ボリュームの内容を表示するには、次のように入力します。

    ```sh
    docker container exec -it <container id> sh
    cd out
    ```

* コンテナーの停止時に /opt/out/ ボリュームの内容を表示するには、次のように入力します。

    ```sh
    docker run --rm -i -v=<volume name>:/opt/out alpine ls
    ```

* ボリュームを削除するには、次のように入力します。

    ```sh
    docker volume rm <volume name>
    ```

* コンテナからローカル ファイルシステムにファイルをコピーするには、次のように入力します。

    ```sh
    docker cp \
       <container id>:<source path> \
       <destination path>
    eg.
    docker cp \
       b867054293a1:/opt/out \
       ~/pingfederate/
    ```

* ローカル ファイルシステムからコンテナにファイルをコピーするには、次のように入力します。

    ```sh
    docker cp \
       <source path> \
       <container id>:<destination path>
    eg.
    docker cp \
       myconnector.jar \
       bb867054293a186:/opt/out/instance/server/default/deploy/
    ```
