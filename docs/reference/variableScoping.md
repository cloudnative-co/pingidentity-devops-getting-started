---
title:  Variables and Scope
---
<!--
# Variables and scope

Variables provide a way to store and reuse values with our Docker containers which are ultimately used by our Docker image hooks to customize configurations.

It's important to understand:

* The different levels at which you can set variables
* How you should use variables
* Where you should set and use variables

The following diagram shows the different scopes in which variables can be set and applied.

![Variable Scoping](../images/variableScoping-1.png)

Assume that you are viewing this diagram as a pyramid with the container at the top. The order of precedence for variables is top-down. Generally, you set variables having an orchestration scope.
-->

# 変数とスコープ

変数は、Docker コンテナーで値を保存および再利用する方法を提供します。これらの値は、最終的に、構成をカスタマイズするために Docker イメージ フックによって使用されます。

以下を理解することが重要です。

- 変数を設定できるさまざまなレベル
- 変数の使用方法
- 変数を設定および使用する場所 

次の図は、変数を設定および適用できるさまざまなスコープを示しています。

![Variable Scoping](../images/variableScoping-1.png)

この図を、コンテナーが上部にあるピラミッドとして表示していると仮定します。変数の優先順位は上から下です。通常、オーケストレーション スコープを持つ変数を設定します。

<!--
## Image scope

Variables having an image scope are assigned using the values set for the Docker image (for example, from Dockerfiles). These variables are often set as defaults, allowing scopes with a higher level of precedence to override them.

To see the default environment variables available with any Docker image, enter:

  ```shell
  docker run pingidentity/<product-image>:<tag> env | sort
  ```

  Where &lt;product-image&gt; is the name of one of our products, and &lt;tag&gt; is the release tag (such as `edge`).

For the environment variables available for all products (PingBase) or individual products, see the [Docker Images Reference](../docker-images/dockerImagesRef.md).
-->

## イメージスコープ

イメージ スコープを持つ変数は、Docker イメージに設定された値 (たとえば、Dockerfiles から) を使用して割り当てられます。これらの変数はデフォルトとして設定されることが多く、より高い優先レベルのスコープでそれらをオーバーライドできます。

任意の Docker イメージで使用できるデフォルトの環境変数を確認するには、次のように入力します。

  ```shell
  docker run pingidentity/<product-image>:<tag> env | sort
  ```

  &lt;product-image&gt; は製品の名前、&lt;tag&gt; はリリース タグ ( `edge` など) です。

すべての製品 (PingBase) または個々の製品で使用できる環境変数については、[Docker Images Reference](../docker-images/dockerImagesRef.md)を参照してください。

<!--
## Orchestration scope

Variables having orchestration scope are assigned at the orchestration layer. Typically, these environment variables are set using Docker commands, Docker Compose or Helm values. For example:

* Using `docker run` with `--env`:

    ```shell
    docker run --env SCOPE=env \
      pingidentity/pingdirectory:edge env | sort
    ```

* Using `docker run` with `--env-file`:

    ```shell
    echo "SCOPE=env-file"  > /tmp/scope.properties

    docker run --env-file /tmp/scope.properties \
      pingidentity/pingdirectory:edge env | sort
    ```

* Using Docker Compose (docker-compose.yaml):

    ```yaml
    environment:
      - SCOPE=compose
        env_file:
      - /tmp/scope.properties
    ```

* Using Kubernetes:

    ```yaml
    env:
      - name: SCOPE
        value: kubernetes
    ```

* Using Helm variables:

    ```yaml
    global:
      envs:
        PING_IDENTITY_ACCEPT_EULA: "YES"
        PING_IDENTITY_PASSWORD: "2Federate"
      ...
    ```
-->

## オーケストレーションスコープ

オーケストレーション スコープを持つ変数は、オーケストレーション層で割り当てられます。通常、これらの環境変数は、Docker コマンド、Docker Compose、または Helm 値を使用して設定されます。例えば：

-  `--env` と共に `docker run` を使う

    ```shell
    docker run --env SCOPE=env \
      pingidentity/pingdirectory:edge env | sort
    ```

- `--env-file` と共に `docker run` を使う 


    ```shell
    echo "SCOPE=env-file"  > /tmp/scope.properties

    docker run --env-file /tmp/scope.properties \
      pingidentity/pingdirectory:edge env | sort
    ```

- Docker Compose (docker-compose.yaml)を使う

    ```yaml
    environment:
      - SCOPE=compose
        env_file:
      - /tmp/scope.properties
    ```

* Kubernetesで使う

    ```yaml
    env:
      - name: SCOPE
        value: kubernetes
    ```

* Helm変数を使う

    ```yaml
    global:
      envs:
        PING_IDENTITY_ACCEPT_EULA: "YES"
        PING_IDENTITY_PASSWORD: "2Federate"
      ...
    ```

<!--
## Server profile scope

Variables having server profile scope are supplied using property files in the server-profile repository.  You need to be careful setting variables at this level because the settings can override variables already having an image or orchestration scope value set.

You can use the following masthead in your `env_vars` files to provide examples of setting variables and how they might override variables having a scope with a lower level of precedence. It will also suppress a warning when processing the env_vars file:

  ```text
  # .suppress-container-warning
  #
  # NOTICE: Settings in this file will override values set at the
  #         image or orchestration layers of the container.  Examples
  #         include variables that are specific to this server profile.
  #
  # Options include:
  #
  # ALWAYS OVERRIDE the value in the container
  #   NAME=VAL
  #
  # SET TO DEFAULT VALUE if not already set
  #   export NAME=${NAME:=myDefaultValue}  # Sets to string of "myDefaultValue"
  #   export NAME=${NAME:-OTHER_VAR}       # Sets ot value of OTHER_VAR variable
  #
  ```
-->

## サーバープロファイルスコープ

サーバー プロファイル スコープを持つ変数は、サーバー プロファイル リポジトリ内のプロパティ ファイルを使用して提供されます。このレベルで変数を設定する場合は、イメージまたはオーケストレーション スコープ値がすでに設定されている変数をオーバーライドする可能性があるため、注意する必要があります。

`env_vars` ファイルで次のマストヘッドを使用すると、変数の設定例と、優先レベルの低いスコープを持つ変数をオーバーライドする方法の例を提供できます。また、env_vars ファイルの処理時の警告も抑制されます。

  ```text
  # .suppress-container-warning
  #
  # NOTICE: Settings in this file will override values set at the
  #         image or orchestration layers of the container.  Examples
  #         include variables that are specific to this server profile.
  #
  # Options include:
  #
  # ALWAYS OVERRIDE the value in the container
  #   NAME=VAL
  #
  # SET TO DEFAULT VALUE if not already set
  #   export NAME=${NAME:=myDefaultValue}  # Sets to string of "myDefaultValue"
  #   export NAME=${NAME:-OTHER_VAR}       # Sets ot value of OTHER_VAR variable
  #
  ```

<!--
## Container scope

Variables having a container scope are assigned in the hook scripts and will overwrite variables that are set elsewhere. Variables that need to be passed to other hook scripts must be appended to the file assigned to `${CONTAINER_ENV}`, (which defaults to `/opt/staging/.env`). This file is sourced by every hook script.
-->

## コンテナスコープ

コンテナスコープを持つ変数はフックスクリプトで割り当てられ、他の場所で設定された変数を上書きします。他のフック スクリプトに渡す必要がある変数は、`${CONTAINER_ENV}` (デフォルトは `/opt/staging/.env`) に割り当てられたファイルに追加する必要があります。このファイルは、すべてのフック スクリプトによってソースされます。

## スコープの例

![Variable Scoping](../images/variableScoping-2.png)
