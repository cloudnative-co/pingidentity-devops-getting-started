---
title: Welcome
---
# Welcome

<!-- This documentation exists to enable DevOps professionals, administrators, and developers to deploy Ping Identity software using container technologies. Our goal is to provide tools, frameworks, blueprints, and reference architectures in support of running our products in containerized environments. -->

このドキュメントは、DevOps 専門家、管理者、開発者がコンテナ テクノロジーを使用して Ping Identity ソフトウェアを展開できるようにするために存在します。私たちの目標は、コンテナ化された環境での製品の実行をサポートするツール、フレームワーク、ブループリント、リファレンス アーキテクチャを提供することです。

<!-- >
* First time here?  We recommend the [Get Started](./get-started/introduction.md) page.

* Kubernetes? See [Kubernetes Basics](./reference/k8sBasics.md)

* New to Helm?  See [Helm Basics](./reference/HelmBasics.md)

* Important information about [container logging](./reference/containerLogging.md)
-->

* ここは初めてですか？ [Get Started](./get-started/introduction.md) ページをおすすめします。 
* Kubernetesは初めてですか？ [Kubernetes Basics](./reference/k8sBasics.md) を参照してください。
* Helmは初めてですか？  [Helm Basics](./reference/HelmBasics.md) を参照してください。
* [container logging](./reference/containerLogging.md)に関する重要な情報。

<!--
## Benefits from this program

* **Streamlined Deployments**

    Deploy and run workloads on our solutions without the need for additional hardware or virtual machines (VMs).

* **Consistent and Flexible**

    Maintain all configurations and dependencies, ensuring consistent environments. Containers are portable and can be used on nearly any machine.

* **Optimized Sizing**

    Orchestration of containers allows organizations to increase fault tolerance and availability and to better manage costs by auto-scaling to application demand.
-->

## このプログラムの利点

* **合理化されたデプロイ**

   追加のハードウェアや仮想マシン (VM) を必要とせずに、私たちのソリューション上でワークロードをデプロイして実行します。 

* **一貫性と柔軟性**

    すべての構成と依存関係を維持し、一貫した環境を確保します。コンテナは持ち運びが可能で、ほぼすべてのマシンで使用できます。

* **最適化されたサイジング**

    コンテナのオーケストレーションにより、組織はフォールト トレランスと可用性を向上させ、アプリケーションの需要に合わせて自動スケーリングすることでコストをより適切に管理できるようになります。


<!--
## Resources

Resources provided include Docker images of Ping Identity products, deployment examples, and configuration management tools.
-->

## リソース

提供されるリソースには、Ping Identity 製品の Docker イメージ、展開例、構成管理ツールが含まれます。

###  Docker Images

<div class="iconbox" onclick="window.open('https://hub.docker.com/u/pingidentity','');">
    <img class="assets" src="./images/logos/docker.png" />
    <span class="caption">
        <a class="assetlinks" href="https://hub.docker.com/u/pingidentity" target=”_blank”>Docker Images</a>
    </span>
</div>
<div class="iconbox" onclick="window.open('https://github.com/pingidentity/pingidentity-docker-builds','');">
    <img class="assets" src="./images/logos/github.png"/>
    <span class="caption">
        <a class="assetlinks" href="https://github.com/pingidentity/pingidentity-docker-builds" target=”_blank”>Docker Builds</a>
    </span>
</div>

<!--
Ping provides preconfigured Docker images of our products for running as containers. Each of our containers is a complete working product instance that is immediately usable when deployed. Our Docker stacks are integrated collections of Ping products preconfigured to coordinate across all containers in the stack.

!!! info "By default, our Docker images run as an unprivileged user in the container."


You can find information about our available Docker images in the [pingidentity-docker-builds](https://github.com/pingidentity/pingidentity-docker-builds) repository on Github or on the [Docker Hub](https://hub.docker.com/u/pingidentity/) site.  Included in this portal are detailed [image specifications](./docker-images/dockerImagesRef.md) on variables, related images and so on.

The Docker images are automatically pulled from our repository the first time you deploy a product container or orchestrated set of containers. Alternatively, you can pull the images manually from our [Docker Hub](https://hub.docker.com/u/pingidentity/) site.

!!! error "Removal of older images from Docker Hub"
    Older images based on product versions that are no longer supported under our policy are removed from Docker Hub.  See the [support policy page](./docker-images/imageSupport.md) for details.
-->

Ping は、コンテナとして実行するための製品の事前構成された Docker イメージを提供します。当社の各コンテナは完全に動作する製品インスタンスであり、デプロイするとすぐに使用できます。当社の Docker スタックは、スタック内のすべてのコンテナー間で調整されるように事前構成された Ping 製品の統合コレクションです。

!!! info "デフォルトでは、Docker イメージはコンテナー内で特権のないユーザーとして実行されます。"

利用可能な Docker イメージに関する情報は、Github の [pingidentity-docker-builds](https://github.com/pingidentity/pingidentity-docker-builds) リポジトリまたは [Docker Hub](https://hub.docker.com/u/pingidentity/) サイトで見つけることができます。このポータルには、変数や関連イメージなどの詳細な[イメージ仕様](./docker-images/dockerImagesRef.md)が含まれています。

Docker イメージは、製品コンテナーまたはオーケストレーションされたコンテナーのセットを初めてデプロイするときに、リポジトリから自動的に取得されます。あるいは、[Docker Hub](https://hub.docker.com/u/pingidentity/) サイトからイメージを手動でプルすることもできます。

!!! error "Docker Hub からの古いイメージの削除"
    当社のポリシーでサポートされなくなった製品バージョンに基づく古いイメージは、Docker Hub から削除されます。詳細については、[サポート ポリシーのページ](./docker-images/imageSupport.md)をご覧ください。

<!--
### Deployment Examples

<div class="iconbox" onclick="window.open('https://github.com/pingidentity/pingidentity-devops-getting-started','');">
    <img class="assets" src="./images/logos/github.png"/>
    <span class="caption">
        <a class="assetlinks" href="https://github.com/pingidentity/pingidentity-devops-getting-started" target=”_blank”>DevOps Getting Started</a>
    </span>
</div>

The Github repository linked here provides examples for deploying our products as standalone containers or as an orchestrated deployment in Kubernetes (using Helm).

Docker Compose is often used for development, demonstrations, and lightweight orchestration. Kubernetes is typically used for enterprise-level orchestration.
-->

### デプロイの例

<div class="iconbox" onclick="window.open('https://github.com/pingidentity/pingidentity-devops-getting-started','');">
    <img class="assets" src="./images/logos/github.png"/>
    <span class="caption">
        <a class="assetlinks" href="https://github.com/pingidentity/pingidentity-devops-getting-started" target=”_blank”>DevOps Getting Started</a>
    </span>
</div>

ここにリンクされている Github リポジトリでは、製品をスタンドアロン コンテナーとして、または Kubernetes でのオーケストレーションされたデプロイメント (Helm を使用) としてデプロイするための例が提供されています。

Docker Compose は、開発、デモンストレーション、軽量のオーケストレーションによく使用されます。 Kubernetes は通常、エンタープライズ レベルのオーケストレーションに使用されます。

<!--
### Configuration Management

For configuration management, we use:

- Server profiles, providing initial runtime configuration when containers are started. Server profile configuration is pulled from a provided Git repository and applied to the server during container start up.
- Terraform, to manage runtime server configuration after containers have successfully started and server profiles have been successfully applied. Terraform can manage the ongoing state of configuration in a running service without the need for rolling restart.
- YAML files for runtime configuration of stacks. YAML file configuration settings complement those provided through server profiles.
- Environment variables. These can be included in YAML files or called from external files.
- Shell scripts (hooks) to automate certain operations for a product.
- Release tags to give you a choice between stable builds or the current (potentially unstable) builds.

More information about how server profiles, variables and these other options coordinate to configure the products can be found on the [Configuration Reference](./reference/config.md) page.
-->

### 構成管理

構成管理には以下を使用します。

- サーバープロファイル
    - コンテナーの実行時構成用。
- スタックの実行時設定用の YAML ファイル
    -  YAML ファイル構成設定は、サーバー プロファイルを通じて提供される設定を補完します。
- 環境変数
    - これらは YAML ファイルに含めることも、外部ファイルから呼び出すこともできます。
- シェル スクリプト (フック)
    - 製品の特定の操作を自動化する
- リリースタグ
    - 安定したビルドか現在の (不安定な可能性がある) ビルドのどちらかを選択できるようになります。

サーバー プロファイル、変数、およびその他のオプションを調整して製品を構成する方法の詳細については、[構成リファレンス](./reference/config.md) ページを参照してください。

## Other Resources

<div class="banner" onclick="window.open('https://github.com/topics/ping-devops','');">
    <img class="assets" src="./images/logos/github.png"/>
    <span class="caption">
        <a class="assetlinks" href="https://github.com/topics/ping-devops" target=”_blank”>All Ping DevOps Github Repos</a>
    </span>
</div>
<div class="banner" onclick="window.open('https://helm.pingidentity.com','');">
    <img class="assets" src="./images/logos/helm.png"/>
    <span class="caption">
        <a class="assetlinks" href="https://helm.pingidentity.com" target=”_blank”>Ping Helm Charts</a>
    </span>
</div>
<div class="banner" onclick="window.open('https://terraform.pingidentity.com','');">
    <img class="assets" src="./images/logos/tf-logo.svg"/>
    <span class="caption">
        <a class="assetlinks" href="https://terraform.pingidentity.com" target=”_blank”>Ping Terraform Providers</a>
    </span>
</div>
