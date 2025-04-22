---
title: Helm Basics
---
<!--
# Helm Basics

Although this document cannot cover the depths of this tool, new Helm users might find other technical documentation too involved for the purpose of beginning use of Ping Identity container images. This document aims to equip new users with helpful terminology in simple terms, with a focus on relevant commands. For more in-depth documentation around Helm, check out [helm.sh](https://helm.sh).

!!! note
    This overview uses Ping Identity images and practices as a guide, but generally applies to any interactions using Helm with Kubernetes. With these assumptions, this document might feel incomplete or inaccurate to veterans. If you would like to contribute to this document, feel free to open a pull request!
-->

# Helmの基本

このドキュメントではこのツールの詳細を説明することはできませんが、Helm の新規ユーザーは、Ping Identity コンテナー イメージの使用を開始するために他の技術ドキュメントが複雑すぎると感じるかもしれません。このドキュメントは、関連するコマンドに焦点を当てて、新しいユーザーに役立つ用語を簡単な用語で提供することを目的としています。 Helm に関する詳細なドキュメントについては、[helm.sh](https://helm.sh) を確認してください。

!!! note
    この概要では、ガイドとして Ping Identity のイメージと実践を使用しますが、一般に、Helm と Kubernetes を使用するあらゆる対話に適用されます。こうした前提を踏まえると、退役軍人にとってこの文書は不完全または不正確に感じるかもしれません。このドキュメントに貢献したい場合は、お気軽にプル リクエストを開いてください。

<!--
## Helm

!!! info "Ping Identity DevOps and Helm"
    All of our instructions and examples are based on the [Ping Identity DevOps Helm chart](https://helm.pingidentity.com). If you are not using the Ping Identity DevOps Helm chart in production, we still recommend using it to generate your direct Kubernetes manifest files. Using our provided chart to create your files in this manner gives Ping Identity the best opportunity to support your environment.

Everything in Kubernetes is deployed by defining what you want and allowing Kubernetes to achieve the desired state ([the declarative model](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/)).

Helm simplifies your interaction by building deployment patterns into templates with variables. The Ping Identity Helm chart includes Kubernetes templates and default values maintained by Ping Identity. With these in hand, you only need to provide values for the template variables to match your environment.

For example, a service definition looks like the following file. With this file, Kubernetes is instructed to create a `service` resource with the name `myping-pingdirectory`.

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app.kubernetes.io/instance: myping
    app.kubernetes.io/name: pingdirectory
  name: myping-pingdirectory
spec:
  ports:
  - name: https
    port: 443
    protocol: TCP
    targetPort: 1443
  - name: ldap
    port: 389
    protocol: TCP
    targetPort: 1389
  - name: ldaps
    port: 636
    protocol: TCP
    targetPort: 1636
  selector:
    app.kubernetes.io/instance: myping
    app.kubernetes.io/name: pingdirectory
  type: ClusterIP
```

Using our Helm chart, you can automatically define this entire resource and all other required resources for a basic deployment by setting `pingdirectory.enabled=true`.
-->

## Helm

!!! info "Ping Identity DevOps と Helm"
    すべての手順と例は、[Ping Identity DevOps Helm chart](https://helm.pingidentity.com)に基づいています。運用環境で Ping Identity DevOps Helm チャートを使用していない場合でも、それを使用して直接 Kubernetes マニフェスト ファイルを生成することをお勧めします。提供されているチャートを使用してこの方法でファイルを作成すると、Ping Identity は環境をサポートする最良の機会を得ることができます。

Kubernetes のすべては、必要なものを定義し、Kubernetes が目的の状態 ([the declarative model](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/)) を達成できるようにすることによってデプロイされます。

Helm は、変数を含むテンプレートにデプロイ パターンを構築することにより、対話を簡素化します。 Ping Identity Helm チャートには、Kubernetes テンプレートと、Ping Identity によって維持されるデフォルト値が含まれています。これらを用意したら、環境に合わせてテンプレート変数の値を指定するだけで済みます。

たとえば、サービス定義は次のファイルのようになります。このファイルを使用して、Kubernetes は `myping-pingdirectory` という名前の `service` リソースを作成するように指示されます。

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app.kubernetes.io/instance: myping
    app.kubernetes.io/name: pingdirectory
  name: myping-pingdirectory
spec:
  ports:
  - name: https
    port: 443
    protocol: TCP
    targetPort: 1443
  - name: ldap
    port: 389
    protocol: TCP
    targetPort: 1389
  - name: ldaps
    port: 636
    protocol: TCP
    targetPort: 1636
  selector:
    app.kubernetes.io/instance: myping
    app.kubernetes.io/name: pingdirectory
  type: ClusterIP
```

Helm チャートを使用すると、`pingdirectory.enabled=true` を設定することで、このリソース全体と、基本的なデプロイメントに必要な他のすべてのリソースを自動的に定義できます。

<!--
### Terminology

**Manifests** - the final Kubernetes YAML files that are sent to the cluster for resource creation. These files are standard Kubernetes files and will be similar to the service example shown above.

**Helm Templates** - Go Template versions of Kubernetes YAML files. These templates enable the manifest creation to be parameterized.

**Values and values.yaml** - A value is the setting you pass to a Helm chart from which the templates produce the manifests you want. Values can be passed individually on the command line, but more commonly they are collected and defined in a file named **values.yaml**. For example, if this file contained only this entry, the resulting Kubernetes manifest file would be over 200 lines long.


  ```yaml
  pingdirectory:
    enabled: true
  ```

**release** - When you deploy resources with Helm, you provide a name for identification. The combination of this name and the resources that are deployed using it make up a `release`. When using Helm, it is a common pattern to prefix all resources managed by a release with the release name. In our examples, `myping` is the release name, so you will see products in Kubernetes running with names similar to `myping-pingfederate-admin`, `myping-pingdirectory`, and `myping-pingauthorize`.
-->

### 用語

**マニフェスト（Manifests）** - リソース作成のためにクラスターに送信される最終的な Kubernetes YAML ファイル。これらのファイルは標準の Kubernetes ファイルであり、上記のサービス例に似ています。

**Helmテンプレート（Helm Templates）** - Kubernetes YAML ファイルの Go テンプレート バージョン。これらのテンプレートを使用すると、マニフェストの作成をパラメータ化できます。

**値とvalues.yaml（Values and values.yaml）** - 値は、テンプレートが必要なマニフェストを生成する Helm チャートに渡す設定です。値はコマンド ラインで個別に渡すこともできますが、一般的には値は収集され、**values.yaml** という名前のファイルに定義されます。たとえば、このファイルにこのエントリのみが含まれている場合、作成される Kubernetes マニフェスト ファイルの長さは 200 行を超えます。

  ```yaml
  pingdirectory:
    enabled: true
  ```

Helm を使用してリソースをデプロイするときは、識別用の名前を指定します。この名前と、それを使用してデプロイされるリソースの組み合わせが `release` を構成します。 Helm を使用する場合、リリースによって管理されるすべてのリソースにリリース名をプレフィックスとして付けるのが一般的なパターンです。この例では、`myping` がリリース名であるため、Kubernetes 内の製品は `myping-pingfederate-admin`、`myping-pingdirectory`、`myping-pingauthorize` のような名前で実行されていることがわかります。

<!--
### Building the Helm Values File

This documentation focuses on the [Ping Identity DevOps Helm chart](https://github.com/pingidentity/helm-charts) and the values passed to the Helm chart to achieve your configuration. For your deployment to fit your goals, you must create a [values.yaml](https://helm.sh/docs/chart_template_guide/values_files/) file.

The most simple **values.yaml** for our Helm chart would be:

```yaml
global:
  enabled: true
```

By default, this flag is set as `global.enabled=false`. These two lines are sufficient to turn on (deploy) every available Ping Identity software product with a basic configuration.
-->

### Helm 値ファイルの構築

このドキュメントでは、[Ping Identity DevOps Helm chart](https://github.com/pingidentity/helm-charts)と、構成を実現するために Helm チャートに渡される値に焦点を当てています。デプロイメントを目標に適合させるには、[values.yaml](https://helm.sh/docs/chart_template_guide/values_files/) ファイルを作成する必要があります。

Helm チャートの最も単純な **value.yaml** は次のようになります。

```yaml
global:
  enabled: true
```

デフォルトでは、このフラグは `global.enabled=false` に設定されます。これら 2 行は、基本構成で利用可能なすべての Ping Identity ソフトウェア製品を有効にする (展開する) のに十分です。

<!--
### Providing your own server profile

In the documentation, there is an example for providing your own server profile stored in GitHub to PingDirectory.  The documenation provides this snippet in the values.yaml specific to that feature:

```yaml
pingdirectory:
  envs:
    SERVER_PROFILE_URL: https://github.com/<your-github-user>/pingidentity-server-profiles
```

This entry alone will not turn on PingDirectory, because the default value for `pingdirectory.enabled` is false. To complete the deployment, add the snippet to turn deploy and configure PingDirectory in the values.yaml file:

```yaml
global:
  enabled: true
pingdirectory:
  envs:
    SERVER_PROFILE_URL: https://github.com/<your-github-user>/pingidentity-server-profiles
```

This example snippet turns on all products, including PingDirectory, and overwrites the default `pingdirectory.envs.SERVER_PROFILE_URL` with `https://github.com/<your-github-user>/pingidentity-server-profiles`.

This use of substitution and parameters is where the power of Helm to simplify ease of deployment begins to shine. To fully customize your deployment, review all available options by running:

  ```sh
  helm show values pingidentity/ping-devops
  ```

This command prints all of the default values applied to your deployment. To overwrite any default values from the chart, copy the corresponding snippet and include it in your own values.yaml file with any modifications needed. Remember with YAML that tabbing and spacing matters. For most editors, copying all the way to the left margin and pasting at the very beginning of a line in your file should maintain proper indentation.

Helm also provides a wide variety of plugins. A useful one is [Helm diff](https://github.com/databus23/helm-diff).  This plugin shows what changes would be applied between Helm upgrade commands. For example, if this plugin indicates anything in a Deployment or Statefulset would change, you can expect the corresponding pods to be cycled. In this example, **Helm diff** is useful to note changes that would occur, particularly when you are not prepared for containers to be restarted.
-->

### 独自のサーバープロファイルの提供

ドキュメントには、GitHub に保存されている独自のサーバー プロファイルを PingDirectory に提供する例が記載されています。ドキュメントでは、その機能に固有のvalues.yamlで次のスニペットが提供されています。

```yaml
pingdirectory:
  envs:
    SERVER_PROFILE_URL: https://github.com/<your-github-user>/pingidentity-server-profiles
```

`pingdirectory.enabled` のデフォルト値は false であるため、このエントリだけでは PingDirectory はオンになりません。デプロイを完了するには、デプロイを開始するスニペットを追加し、values.yaml ファイルで PingDirectory を構成します。

```yaml
global:
  enabled: true
pingdirectory:
  envs:
    SERVER_PROFILE_URL: https://github.com/<your-github-user>/pingidentity-server-profiles
```

このサンプル スニペットは、PingDirectory を含むすべての製品を有効にし、デフォルトの `pingdirectory.envs.SERVER_PROFILE_URL` を `https://github.com/<your-github-user>/pingidentity-server-profiles` で上書きします。

この置換とパラメーターの使用により、デプロイメントを簡素化する Helm の力が輝き始めます。デプロイメントを完全にカスタマイズするには、以下を実行して、利用可能なすべてのオプションを確認します。

  ```sh
  helm show values pingidentity/ping-devops
  ```

このコマンドは、展開に適用されるすべてのデフォルト値を出力します。グラフのデフォルト値を上書きするには、対応するスニペットをコピーし、必要な変更を加えて独自のvalues.yamlファイルに含めます。 YAML ではタブとスペースが重要であることを思い出してください。ほとんどのエディタでは、左余白までコピーしてファイルの行の先頭に貼り付けると、適切なインデントが維持されます。

Helm はさまざまなプラグインも提供します。便利なものは [Helm diff](https://github.com/databus23/helm-diff) です。このプラグインは、Helm アップグレード コマンド間でどのような変更が適用されるかを示します。たとえば、このプラグインが Deployment または Statefulset 内の何かが変更されることを示している場合、対応するポッドが循環されることが期待できます。この例では、**Helm diff** は、特にコンテナーを再起動する準備ができていない場合に、発生する可能性のある変更を記録するのに役立ちます。

<!--
### Additional Commands

As you go through the Helm examples, the goal is to build a values.yaml file that works in your environment.

##### Deploy a release:

  ```sh
  helm upgrade --install <release_name> pingidentity/ping-devops -f /path/to/values.yaml
  ```

##### Delete a release:

This command will remove all resources except PVC and PV objects associated with the release from the cluster:

  ```sh
  helm uninstall <release name>
  ```

##### Delete PVCs associated to a release:

  ```sh
  kubectl delete pvc --selector=app.kubernetes.io/instance=<release_name>
  ```

### Exit Codes

| Exit Code | Description |
|---|---|
| Exit Code 0 | Absence of an attached foreground process|
| Exit Code 1 | Indicates failure due to application error |
| Exit Code 137 | Indicates failure as container received SIGKILL (manual intervention or ‘oom-killer’ [OUT-OF-MEMORY]) |
| Exit Code 139 | Indicates failure as container received SIGSEGV |
| Exit Code 143 | Indicates failure as a container received SIGTERM |

### Example Configurations

The [Helm examples](../deployment/deployHelm.md) page contains example configurations for running and configuring Ping products using the Ping Devops Helm Chart. Please review the [Getting Started Page](../get-started/introduction.md) before trying them.

-->

### 追加のコマンド

Helm の例を確認するときの目標は、ご使用の環境で動作する value.yaml ファイルを構築することです。

##### リリースをデプロイする

  ```sh
  helm upgrade --install <release_name> pingidentity/ping-devops -f /path/to/values.yaml
  ```

##### リリースを削除する

このコマンドは、リリースに関連付けられた PVC および PV オブジェクトを除くすべてのリソースをクラスターから削除します。

  ```sh
  helm uninstall <release name>
  ```

##### リリースに関連付けられた PVC を削除する

  ```sh
  kubectl delete pvc --selector=app.kubernetes.io/instance=<release_name>
  ```

### 終了コード

| Exit Code | Description |
|---|---|
| Exit Code 0 | アタッチされたフォアグラウンドプロセスの欠如 |
| Exit Code 1 | アプリケーションエラーによる失敗を示します |
| Exit Code 137 | コンテナが SIGKILL を受信したため失敗を示します (手動介入または 'oom-killer' [OUT-OF-MEMORY]) |
| Exit Code 139 | コンテナが SIGSEGV を受信したため失敗したことを示します |
| Exit Code 143 | コンテナが SIGTERM を受信したため失敗したことを示します |

### 構成例

[Helm examples](../deployment/deployHelm.md)のページには、Ping Devops Helm Chart を使用して Ping 製品を実行および構成するための構成例が含まれています。試してみる前に、[Getting Started Page](../get-started/introduction.md)ページを確認してください。
