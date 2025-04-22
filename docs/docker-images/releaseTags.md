---
title:  Using Release Tags
---

<!--
# Using Release Tags

Ping Identity uses multiple tags for each released image. On our [Docker Hub](https://hub.docker.com/u/pingidentity) site, you can view the available tags for each image.

!!! info "Multi-product deployment"
    All product containers in a deployment should use the same release tag.
-->

# リリースタグの使用

Ping Identity は、リリースされたイメージごとに複数のタグを使用します。 [Docker Hub](https://hub.docker.com/u/pingidentity) サイトでは、各イメージで使用可能なタグを確認できます。

!!! info "複数製品の導入"
    デプロイメント内のすべての製品コンテナは同じリリース タグを使用する必要があります。

<!--
## Store images privately

Before discussing tags, it is important to know more about Ping Identity's use of Docker Hub for images.  While Docker Hub is very reliable and you can always find the latest images of Ping Identity products hosted there, **do not** rely on Ping to maintain Docker images in Docker Hub over time. See the [image support policy](./imageSupport.md) for details.

To ensure continued access to any image, pull the image in question and maintain it in your own image registry. Common Docker registry providers include: JFrog, AWS ECR, Google GCR and Azure ACR.
-->

## イメージをプライベートに保存する

タグについて説明する前に、Ping Identity によるイメージに対する Docker Hub の使用について詳しく知ることが重要です。 Docker Hub は非常に信頼性が高く、そこでホストされている Ping Identity 製品の最新のイメージをいつでも見つけることができますが、Docker Hub 内の Docker イメージを長期間維持するために Ping に依存しないでください。詳細については、[イメージ サポート ポリシー](./imageSupport.md)を参照してください。

任意のイメージに継続的にアクセスできるようにするには、問題のイメージをプルし、独自のイメージ レジストリに保持します。一般的な Docker レジストリ プロバイダーには、JFrog、AWS ECR、Google GCR、Azure ACR などがあります。

<!--
## Tagging Format

To specify a release tag for deployments, use the following format:

```yaml
image: pingidentity/<ping-product>:${PING_IDENTITY_DEVOPS_TAG}
```

In the example above, `<ping-product>` is the name of the product and `${PING_IDENTITY_DEVOPS_TAG}` is the assigned release tag value. The file containing the setting for `${PING_IDENTITY_DEVOPS_TAG}` is `~/.pingidentity/config` by default. This file is created by running the `pinctl config` command, documented [here](../tools/pingctlUtil.md). You can also specify the release tag explicitly in your deployments. The release tag must be the same for each container in the deployment. For example:

```yaml
image: pingidentity/<ping-product>:edge
```
-->

## タグ付け形式

デプロイメントのリリース タグを指定するには、次の形式を使用します。

```yaml
image: pingidentity/<ping-product>:${PING_IDENTITY_DEVOPS_TAG}
```

上の例では、`<ping-product>` は製品の名前で、`${PING_IDENTITY_DEVOPS_TAG}` は割り当てられたリリース タグの値です。 `${PING_IDENTITY_DEVOPS_TAG}` の設定を含むファイルは、デフォルトでは `~/.pingidentity/config` です。このファイルは、[ここ](../tools/pingctlUtil.md)に記載されている `pinctl config` コマンドを実行することによって作成されます。デプロイメントでリリース タグを明示的に指定することもできます。リリース タグは、デプロイメント内の各コンテナーで同じである必要があります。例えば：

```yaml
image: pingidentity/<ping-product>:edge
```

<!--
## Determine Which Tag To Use

The tag to use depends on the purpose of the deployment in question.  Along with using a tag, any image on Docker Hub can be referenced using the [SHA256 digest](https://docs.docker.com/engine/reference/commandline/images/#list-image-digests) to ensure immutability in your environments.  The digest for a given image never changes regardless of any tag or tags with which it is associated.

### Production Stability

For customers in production environments, stability is often the highest priority. To ensure a deployment with the fewest dependencies and highest product stability:

* Use the digest of a _full sprint tag_ that includes the [sprint](#sprint) version and product version.  For example, consider the image tag `pingidentity/pingfederate:2206-11.1.0`. To pull this image using the corresponding digest:

    ```sh
    docker pull pingidentity/pingfederate@sha256:8eb88fc3345d8d71dafd83bcdcc38827ddb09768c6571c930b4d217ea177debf
    ```
-->

## 使用するタグを決定する

使用するタグは、対象の展開の目的によって異なります。タグの使用に加え、[SHA256 ダイジェスト](https://docs.docker.com/engine/reference/commandline/images/#list-image-digests)を使用して Docker Hub 上のイメージを参照し、環境内での不変性を確保できます。特定のイメージのダイジェストは、関連付けられているタグに関係なく、決して変更されません。

<!--
### Production Stability

For customers in production environments, stability is often the highest priority. To ensure a deployment with the fewest dependencies and highest product stability:

* Use the digest of a _full sprint tag_ that includes the [sprint](#sprint) version and product version.  For example, consider the image tag `pingidentity/pingfederate:2206-11.1.0`. To pull this image using the corresponding digest:

    ```sh
    docker pull pingidentity/pingfederate@sha256:8eb88fc3345d8d71dafd83bcdcc38827ddb09768c6571c930b4d217ea177debf
    ```
-->

### 生産の安定性

実稼働環境のお客様にとって、多くの場合、安定性が最優先事項となります。依存関係を最小限に抑え、製品の安定性を最大限に高めた展開を確保するには、次の手順を実行します。

* [スプリント](#sprint) バージョンと製品バージョンを含む _完全なスプリント タグ_ のダイジェストを使用します。たとえば、イメージ タグ `pingidentity/pingfederate:2206-11.1.0` について考えてみましょう。対応するダイジェストを使用してこのイメージをプルするには、以下のようにします。

    ```sh
    docker pull pingidentity/pingfederate@sha256:8eb88fc3345d8d71dafd83bcdcc38827ddb09768c6571c930b4d217ea177debf
    ```

<!--
### Latest Image Features

For demonstrations and testing latest features, use an `edge` based image. In these situations, it is a good practice to use a **_full tag_** variation similar to `pingfederate:11.1.0-edge`, rather than simply `pingfederate:edge`. Doing so avoids dependency conflicts that might occur in server profiles between product versions (for example, 10.x versus 11.x).
-->

### 最新のイメージの特徴

For demonstrations and testing latest features, use an `edge` based image. In these situations, it is a good practice to use a **_full tag_** variation similar to `pingfederate:11.1.0-edge`, rather than simply `pingfederate:edge`. Doing so avoids dependency conflicts that might occur in server profiles between product versions (for example, 10.x versus 11.x).
最新機能のデモンストレーションとテストには、`edge` ベースのイメージを使用します。このような状況では、単に `pingfederate:edge `ではなく、`pingfederate:11.1.0-edge` に似た _完全なタグ_ バリエーションを使用することをお勧めします。これにより、製品バージョン間 (たとえば、10.x と 11.x) でサーバー プロファイルで発生する可能性のある依存関係の競合が回避されます。

<!--
### Evergreen Bleeding Edge

The `edge` is the absolute latest product version and image features, with zero guarantees for stability.
Typically, this tag is only of interest to Ping employees and partners.
-->

### エバーグリーン ブリーディング エッジ

`edge` は絶対的な最新の製品バージョンとイメージ機能であり、安定性は保証されません。
通常、このタグは Ping の従業員とパートナーのみに関係します。

<!--
## Base Release Tags

The base release tags for a product image build are:

* edge
* latest
* sprint
-->

## ベースリリースタグ

製品イメージ ビルドのベース リリース タグは次のとおりです。

* edge
* latest
* sprint
-->

<!--
### edge

The `edge` release tag refers to "bleeding edge", indicating a build similar to an alpha release. This _sliding_ tag includes the latest hooks and scripts, **and is considered highly unstable**. The `edge` release is characterized by:

* Latest product version
* Latest build image enhancements and fixes from our current sprint in progress
* Linux Alpine as the container base OS

Example: `pingidentity/pingfederate:edge`, `pingidentity/pingfederate:11.1.0-edge`
-->

### edge

`edge` リリース タグは「最先端（bleeding edge）」を指し、アルファ リリースと同様のビルドを示します。この _スライディング（sliding）_ タグには最新のフックとスクリプトが含まれており、**非常に不安定である** と考えられています。`edge` リリースの特徴は次のとおりです。

* 最新の製品バージョン
* 現在進行中のスプリントからの最新のビルド イメージの機能強化と修正
* コンテナベースOSとしてのLinux Alpine

例: `pingidentity/pingfederate:edge`, `pingidentity/pingfederate:11.1.0-edge`

<!--
### latest

`edge `is tagged as `latest` at the beginning of each month. The release tag indicates the latest stable release. This tag is also a _sliding_ tag that marks the stable release for the latest sprint. The `latest` release is characterized by:

* Latest product version
* All completed and qualified enhacements and fixes from the prior monthly sprint
* Linux Alpine as the container base OS

Example: `pingfederate:latest`, `pingfederate:11.1.0-latest`
-->

### latest

`edge` は毎月初めに `latest` としてタグ付けされます。リリース タグは、最新の安定版リリースを示します。このタグは、最新のスプリントの安定版リリースをマークする _スライディング_ タグでもあります。`latest` リリースの特徴は次のとおりです。

* 最新の製品バージョン
* 前の月次スプリントで完了し認定されたすべての機能強化と修正
* コンテナベースOSとしてのLinux Alpine

例: `pingfederate:latest`, `pingfederate:11.1.0-latest`

<!--
### sprint

In addition to becoming `latest`, `edge` also is tagged as a stable `sprint` each month.  The `sprint` release tag is a build number indicating a stable build that will not change over time. The `sprint` number uses the YYMM format. For example, 2208 = August 2022.  The `sprint` release is characterized by:

* Latest product version at the time the sprint ended.
* All completed and qualified enhancements and fixes from the specified monthly sprint. The Docker images are generated at the end of the sprint.
* Linux Alpine as the container base OS

Example: `pingfederate:2206`, `pingidentity/pingfederate:2206-11.1.0`
-->

### sprint

`latest`, `edge`であることに加えて、毎月安定した `sprint` としてもタグ付けされます。`sprint` リリース タグは、時間が経っても変化しない安定したビルドを示すビルド番号です。`sprint` 番号には YYMM 形式が使用されます。たとえば、2208 = 2022 年 8 月です。スプリント リリースの特徴は次のとおりです。

* スプリント終了時点の最新の製品バージョン
* 指定された月次スプリントからの、完了および認定されたすべての拡張機能と修正。 Docker イメージはスプリントの終了時に生成されます。
* コンテナベースOSとしてのLinux Alpine

例: `pingfederate:2206`, `pingidentity/pingfederate:2206-11.1.0`

<!--
### sprint (point release)

Occasionally, a bug might be found on a stable release, whether in the product itself or something from the team building the image. In these situations, to avoid changing a `sprint` tag which is promised to be immutable, a point release would be created to move `latest` forward.  

!!! note "Example only"
    These example tags do not exist, they are used here only for illustration purposes.

Example: `pingfederate:2206.1`, `pingidentity/pingfederate:2206.1-11.1.0`
-->

### sprint (ポイントリリース)

場合によっては、製品自体またはイメージを構築しているチームの何かにバグが安定リリースで見つかることがあります。このような状況では、不変であることが約束されている`sprint` タグの変更を避けるために、ポイント リリースが作成されて `latest` の状態に進みます。

!!! note "例示のみ"
    これらのタグの例は存在しません。ここでは説明のみを目的として使用されています。

例: `pingfederate:2206.1`, `pingidentity/pingfederate:2206.1-11.1.0`

<!--
## Determine Image Version

If you are unsure of the exact version of the image used for a given product container, shell into the container and examine the $IMAGE_VERSION environment variable. For example, if you are running a container locally under Docker, you would run the following commands:

```sh
docker container exec -it <container id> sh
echo $IMAGE_VERSION
```

The IMAGE_VERSION variable returns the version in this format:

```sh
[product]-[container OS]-[jdk]-[product version]-[build date]-[git revision]
```

For example:

```sh
IMAGE_VERSION=pingdirectory-alpine_3.16.0-al11-9.1.0.0-220725-c917
```

Where:

| Key | Value |
|-----|-----|
| Product | pingdirectory |
| Container OS | alpine_3.16.0 |
| JDK | al11 |
| Product Version | 9.1.0.0 |
| Build Date | 220725 |
| Git Revision | c917 |

If the container is running under Kubernetes, use the [kubectl exec](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec) command to access the container in order to obtain this information.

!!! note "Date Format"
    In the $IMAGE_VERSION variable, Date is in YYMMDD format
-->

## イメージのバージョンを調べる

特定の製品コンテナーに使用されているイメージの正確なバージョンが不明な場合は、コンテナーにシェルを実行し、$IMAGE_VERSION 環境変数を調べます。たとえば、Docker でローカルにコンテナを実行している場合は、次のコマンドを実行します。

```sh
docker container exec -it <container id> sh
echo $IMAGE_VERSION
```

IMAGE_VERSION 変数は、次の形式でバージョンを返します。

```sh
[product]-[container OS]-[jdk]-[product version]-[build date]-[git revision]
```

例:

```sh
IMAGE_VERSION=pingdirectory-alpine_3.16.0-al11-9.1.0.0-220725-c917
```

Where:

| Key | Value |
|-----|-----|
| Product | pingdirectory |
| Container OS | alpine_3.16.0 |
| JDK | al11 |
| Product Version | 9.1.0.0 |
| Build Date | 220725 |
| Git Revision | c917 |

コンテナーが Kubernetes で実行されている場合は、[kubectl exec](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec) コマンドを使用してコンテナーにアクセスし、この情報を取得します。

!!! note "日付形式"
    $IMAGE_VERSION 変数の日付は YYMMDD 形式です
