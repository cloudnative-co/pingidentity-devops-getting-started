---
title: Using DevOps Hooks
---
<!--
# Using DevOps hooks

!!! note "Video exploration"
    For more information on hook scripts, see the [Ping product image hook script exploration](https://videos.pingidentity.com/detail/video/6315184605112/hook-script-exploration) video.

Our DevOps hooks are build-specific scripts that are called, or can be called, by the `entrypoint.sh` script used to start our product containers.

!!! error "Advanced usage"
    Use of the hook scripts is intended only for DevOps professionals familiar with the products.

The available hooks are built into the product images and can be found in the `hooks` subdirectory of each product directory in the [Docker Builds](https://github.com/pingidentity/pingidentity-docker-builds) repository.

In the `entrypoint.sh` startup script, there is an example (stub) provided for the available hooks for all products.

!!! warning
    It is **critical** that the supplied hook names be used if you modify `entrypoint.sh`. For example, they can be used to make subtle changes to a server profile.
-->

# DevOps フックの使用

!!! note "ビデオ探索"
    フック スクリプトの詳細については、[Ping product image hook script exploration](https://videos.pingidentity.com/detail/video/6315184605112/hook-script-exploration)を参照してください。

DevOps フックは、製品コンテナの起動に使用される `entrypoint.sh` スクリプトによって呼び出される、または呼び出すことができるビルド固有のスクリプトです。

!!! error "高度な使用法"
    フック スクリプトの使用は、製品に精通した DevOps プロフェッショナルのみを対象としています。

使用可能なフックは製品イメージに組み込まれており、[Docker Builds](https://github.com/pingidentity/pingidentity-docker-builds)リポジトリの各製品ディレクトリのフック サブディレクトリにあります。

`entrypoint.sh` 起動スクリプトには、すべての製品で使用できるフックの例 (スタブ) が含まれています。

!!! warning
    `entrypoint.sh` を変更する場合は、指定されたフック名を使用することが**重要**です。たとえば、サーバー プロファイルに微妙な変更を加えるために使用できます。

<!--
## Using .pre and .post hooks

When the hook scripts are called during the `entrypoint.sh` initialization, any corresponding `.pre` and `.post` hooks are also called.

The `.pre` and `.post` extensions allow you to define custom scripts to be executed before or after any hook that is run in the container. You can include any custom `.pre` and `.post` hooks in the `hooks` directory of your server profile.

Hooks with a `.pre` extension are run before the corresponding hook, and hooks with a `.post` extension are run after the corresponding hook.

For example, a script named `80-post-start.sh.pre` will execute immediately before the `80-post-start.sh` hook and a script named `80-post-start.sh.post` will be run immediately after that hook completes.
-->

## .pre と .post フックを使用する

`entrypoint.sh` の初期化中にフック スクリプトが呼び出されるとき、対応する `.pre` フックと `.post` フックも呼び出されます。

`.pre` および `.post` 拡張子を使用すると、コンテナ内で実行されるフックの前後に実行されるカスタム スクリプトを定義できます。サーバー プロファイルのフック ディレクトリには、任意のカスタム `.pre` フックと `.post` フックを含めることができます。

`.pre` 拡張子を持つフックは対応するフックの前に実行され、`.post` 拡張子を持つフックは対応するフックの後に実行されます。

たとえば、`80-post-start.sh.pre` という名前のスクリプトは `80-post-start.sh` フックの直前に実行され、`80-post-start.sh.post` という名前のスクリプトはそのフックの完了直後に実行されます。
