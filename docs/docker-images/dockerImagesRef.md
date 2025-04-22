---
title: Container Docker Images Information
---
<!--
# Image Information

These documents for Ping Identity Docker images include information on:

* Related Docker images
* Ports exposed for the containers
* Environment variables for the image
* Associated deployment information
* Tagging methodology and support policy

The image-specific documentation is generated from each new build ensuring these documents align with any changes over time.

!!! warning "Storage Considerations on AWS"
    When deploying Ping products in containers, please consider the information found [here on storage options](../reference/awsStorage.md).

Older images based on product versions that are no longer supported under our policy are removed from Docker Hub.  See the [support policy page](./imageSupport.md) for details.

As with many organizations, Ping Identity uses _floating_ Docker image tags. This practice means, for example, that the **`edge`** tag does not refer to the same image over time as product updates occur. The [release tags](./releaseTags.md) page has information on the `edge` and other tags, how often they are updated, and how to ensure the use of a particular version and release of a product image.

!!! note "Notification of new image tags"
    If you want to be notified when new versions of product Docker images are available, see the **Docker Images** section of the [FAQ page](../reference/faqs.md) for instructions on following the docker-builds GitHub repository.

!!! note "Iron Bank Images"
    For FedRAMP certification and other United States government compliance, Ping Identity partners to build and host highly-secured images at Iron Bank that are compliant with the increased security requirements.  More information is provided in the [FAQ](../reference/faqs.md) section of this documentation.  The Iron Bank images are available on the [Iron Bank](https://docs-ironbank.dso.mil/) repository and are not provided on Docker Hub.
-->

# イメージの情報

Ping Identity Docker イメージに関するこれらのドキュメントには、次の情報が含まれています。

* 関連する Docker イメージ
* コンテナ用に公開されたポート
* イメージの環境変数
* 関連するデプロイ情報
* タグ付け方法とサポート ポリシー

イメージ固有のドキュメントは、新しいビルドごとに生成され、これらのドキュメントが時間の経過による変更に合わせて調整されるようになります。

!!! warning "AWS でのストレージに関する考慮事項"
    Ping 製品をコンテナに展開する場合は、[ストレージ オプション](../reference/awsStorage.md)に関するここに記載されている情報を考慮してください。

当社のポリシーでサポートされなくなった製品バージョンに基づく古いイメージは、Docker Hub から削除されます。詳細については、[サポート ポリシーのページ](./imageSupport.md)をご覧ください。

多くの組織と同様、Ping Identity はフローティング Docker イメージ タグを使用します。これは、たとえば、製品の更新が行われるたびに、 **`edge`**タグが同じ画像を参照しないことを意味します。[release tags](./releaseTags.md)のページには、エッジやその他のタグ、タグの更新頻度、製品イメージの特定のバージョンとリリースを確実に使用する方法に関する情報が含まれています。

!!! note "新しいイメージタグの通知"
    製品の Docker イメージの新しいバージョンが利用可能になったときに通知を受け取りたい場合は、docker-builds GitHub リポジトリをフォローする手順について、[FAQ ページ](../reference/faqs.md)の「Docker イメージ」セクションを参照してください。

!!! note "Iron Bank Images"
    Ping Identityは、FedRAMP認証およびその他の米国政府コンプライアンスのために、強化されたセキュリティ要件に準拠した高度にセキュリティ保護されたイメージをIron Bankで構築およびホストするパートナーです。詳細については、このドキュメントの[FAQ](../reference/faqs.md)セクションをご覧ください。Iron Bankのイメージは[Iron Bank](https://docs-ironbank.dso.mil/)リポジトリで入手可能であり、Docker Hubでは提供されていません。
