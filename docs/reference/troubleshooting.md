---
title:  Troubleshooting
---
<!--
# Troubleshooting

## Getting started

### Examples Not Working

Many common errors using Ping containers arise from using stale images. Our development is highly dynamic and Docker images can rapidly change.

To avoid issues with stale images, remove all local images from your local cache.  Doing so will force Docker to pull the latest images:

  ```shell
  docker rmi $(docker images "pingidentity/*" -q)
  ```

!!! note "Moving tag"
    Even though you might have a local image tagged "latest", this tag does not guarantee it is the newest image in the Docker hub registry.  This tag is reapplied for each release image.
-->

# トラブルシューティング

## Getting Started

### 例が機能しない

Ping コンテナを使用した一般的なエラーの多くは、古いイメージを使用したことが原因で発生します。私たちの開発は非常に動的であり、Docker イメージは急速に変化する可能性があります。

古いイメージの問題を回避するには、ローカル キャッシュからすべてのローカル イメージを削除します。そうすることで、Docker が最新のイメージを強制的に取得するようになります。

  ```shell
  docker rmi $(docker images "pingidentity/*" -q)
  ```

!!! note "タグの移動"
    「latest」というタグが付けられたローカル イメージがあるとしても、このタグはそれが Docker ハブ レジストリ内の最新のイメージであることを保証しません。このタグはリリース イメージごとに再適用されます。

### `pingctl`の設定の誤り

コンテナーが DevOps ユーザー名とキーに基づいてライセンスを取得できない場合は、`pingctl config` ファイルに何らかの構成ミスがある可能性があります。

可能な解決策:

1. 初めて `pingctl config` を実行した場合は、構成された変数を環境にエクスポートする方法について、[Environment Configuration Documentation](https://devops.pingidentity.com/get-started/prereqs/#configure-the-environment)を参照してください。

1. `pingctl info` を実行し、ユーティリティで構成された変数が正しいことを確認します。詳細については、ユーティリティの[documentation](https://devops.pingidentity.com/tools/pingctlUtil/)を参照してください。

<!--
### Unable To Retrieve Evaluation License

If a product instance or instances cannot retrieve the evaluation license, you might receive an error similar to this:

  ```text
  ----- Starting hook: /opt/staging/hooks/17-check-license.sh
  Pulling evaluation license from Ping Identity for:
                Prod License: PD - v7.3
                DevOps User: some-devops-user@example.com...
  Unable to download evaluation product.lic (000), most likely due to invalid PING_IDENTITY_DEVOPS_USER/PING_IDENTITY_DEVOPS_KEY

  ##################################################################################
  ############################        ALERT        #################################
  ##################################################################################
  #
  # No Ping Identity License File (PingDirectory.lic) was found in the server profile.
  # No Ping Identity DevOps User or Key was passed.
  #
  #
  # More info on obtaining your DevOps User and Key can be found at:
  #     https://devops.pingidentity.com/how-to/devopsRegistration/
  #
  ##################################################################################
  CONTAINER FAILURE: License File absent
  CONTAINER FAILURE: Error running 17-check-license.sh
  CONTAINER FAILURE: Error running 10-start-sequence.sh
  ```

This error can be caused by:

1. An invalid DevOps user name or key (as noted in the error). This failure is usually caused by some issue with the variables being passed in. To verify the variables in the `pingctl` configuration are correct for running Docker commands, run the following command:

      ```shell
      pingctl info
      ```

1. A bad Docker image. Pull the Docker image again to verify.

1. Network connectivity to the license server is blocked. To test this, on the machine that is running the container, run:

      ```shell
      curl -k https://license.pingidentity.com/devops/license
      ```

      If the license server is reachable, you will receive an error similar to this example:

      ```shell
      { "error":"missing devops-user header" }
      ```
-->

### 評価ライセンスを取得できません

製品インスタンスが評価ライセンスを取得できない場合、次のようなエラーが表示される場合があります。

  ```text
  ----- Starting hook: /opt/staging/hooks/17-check-license.sh
  Pulling evaluation license from Ping Identity for:
                Prod License: PD - v7.3
                DevOps User: some-devops-user@example.com...
  Unable to download evaluation product.lic (000), most likely due to invalid PING_IDENTITY_DEVOPS_USER/PING_IDENTITY_DEVOPS_KEY

  ##################################################################################
  ############################        ALERT        #################################
  ##################################################################################
  #
  # No Ping Identity License File (PingDirectory.lic) was found in the server profile.
  # No Ping Identity DevOps User or Key was passed.
  #
  #
  # More info on obtaining your DevOps User and Key can be found at:
  #     https://devops.pingidentity.com/how-to/devopsRegistration/
  #
  ##################################################################################
  CONTAINER FAILURE: License File absent
  CONTAINER FAILURE: Error running 17-check-license.sh
  CONTAINER FAILURE: Error running 10-start-sequence.sh
  ```

このエラーは次のことが原因で発生する可能性があります。

1. 無効な DevOps ユーザー名またはキー (エラーに示されているように)。この失敗は通常、渡される変数に何らかの問題があることが原因で発生します。Docker コマンドを実行するために `pingctl` 構成内の変数が正しいことを確認するには、次のコマンドを実行します。

      ```shell
      pingctl info
      ```

1. 不正な Docker イメージ。 Docker イメージを再度pullして確認します。

1. ライセンス サーバーへのネットワーク接続がブロックされています。これをテストするには、コンテナーを実行しているマシンで次を実行します。

      ```shell
      curl -k https://license.pingidentity.com/devops/license
      ```

      ライセンス サーバーにアクセスできる場合は、次の例のようなエラーが表示されます。

      ```shell
      { "error":"missing devops-user header" }
      ```
