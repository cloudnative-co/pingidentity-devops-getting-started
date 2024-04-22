---
title: Environment Substitution
---
<!--
# Environment Substitution

In a typical environment, a product configuration is moved from server to server. Hostnames, endpoints, DNS information, and more need a way to be easily modified.

By removing literal values and replacing them with environment variables, configurations can be deployed in multiple environments with minimal change.

> When templating profiles with variables that reference other products, use the conventions defined in [PingBase Image Directory](../docker-images/pingbase/README.md).

All of our configuration files can be parameterized by adding variables using the syntax:
`${filename.ext}.subst`.

![run.properties.subst](../images/CONFIG_SUBSTITUTION.png)
-->

# 環境の代替

一般的な環境では、製品構成はサーバーからサーバーに移動されます。ホスト名、エンドポイント、DNS 情報などは、簡単に変更できる方法が必要です。

リテラル値を削除して環境変数に置き換えることにより、最小限の変更で構成を複数の環境にデプロイできます。

> 他の製品を参照する変数を含むプロファイルをテンプレート化する場合は、[PingBase Image Directory](../docker-images/pingbase/README.md) で定義されている規則を使用してください。

すべての構成ファイルは、構文 `${filename.ext}.subst` を使用して変数を追加することでパラメータ化できます。

![run.properties.subst](../images/CONFIG_SUBSTITUTION.png)

<!--
## Passing Values to Containers

Within the environment section of your container definition, declare the variable and the value for the product instance.

Values can be defined in many sources, such as inline, env_vars files, and Kubernetes ConfigMaps.

![docker compose environment variables](../images/COMPOSE_SUBSTITUTION.png)
-->

## コンテナに値を渡す

コンテナ定義の環境セクション内で、製品インスタンスの変数と値を宣言します。

値は、インライン、env_vars ファイル、Kubernetes ConfigMap などの多くのソースで定義できます。

![docker compose environment variables](../images/COMPOSE_SUBSTITUTION.png)

<!--
## How it Works

1. A container startup is initiated.

2. The configuration pulls a server profile from Git or from a bind mounted `/opt/in` volume.

3. All files with a `.subst` extension are identified.

4. The environment variables in the identified `.subst` files are replaced with the actual environment values.

5. The `.subst` extension is removed from all the identified files.

6. The product instance for the container is started.

![profile start up sequence](../images/PROFILES_PROCESS.png)
-->

## どのように動くのか

1. コンテナの起動が開始されます。

2. この構成は、Git またはバインド マウントされた `/opt/in` ボリュームからサーバー プロファイルをpullします。

3. `.subst` 拡張子を持つすべてのファイルが識別されます。

4. 識別された `.subst` ファイル内の環境変数は、実際の環境値に置き換えられます。

5. `.subst` 拡張子は、識別されたすべてのファイルから削除されます。

6. コンテナの製品インスタンスが開始されます。

![profile start up sequence](../images/PROFILES_PROCESS.png)
