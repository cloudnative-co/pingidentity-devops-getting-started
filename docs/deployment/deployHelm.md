---
title: Deploy Ping DevOps Charts using Helm
---
<!--
# Deploy Ping DevOps Charts using Helm

<div class="iconbox" onclick="window.open('https://helm.pingidentity.com','');">
    <img class="assets" src="../../images/logos/helm.png"/>
    <span class="caption">
        <a class="assetlinks" href="https://helm.pingidentity.com" target=”_blank”>Helm Charts Repo</a>
    </span>
</div>

To use Ping Identity Helm charts for deployment to Kubernetes, go to the [Getting Started](https://helm.pingidentity.com/getting-started/) page for the Helm chart repository to configure your system to run Helm.  Afterward, continue on this page for examples illustrating how to deploy various scenarios from the charts.

!!! note "Notification of new releases"
    If you want to be notified when a new version of the chart is released, see the **Orchestration/Helm/Kubernetes** section of the [FAQ page](../reference/faqs.md) for instructions on following the GitHub repository for our chart.

!!! note "Ingress with the local K8s cluster"
    If you want to run these examples on the local kind cluster created as outlined on the [Deploy a local Kubernetes cluster](./deployLocalK8sCluster.md) page with ingresses, see [this page](./deployHelmLocalIngress.md) for ingress configuration instructions.  Otherwise, you can port-forward to product services.
-->

# Helm を使用した Ping DevOps chartsのデプロイ

<div class="iconbox" onclick="window.open('https://helm.pingidentity.com','');">
    <img class="assets" src="../../images/logos/helm.png"/>
    <span class="caption">
        <a class="assetlinks" href="https://helm.pingidentity.com" target=”_blank”>Helm Charts Repo</a>
    </span>
</div>

Kubernetes へのデプロイに Ping Identity Helm チャートを使用するには、Helm チャート リポジトリの [Getting Started](https://helm.pingidentity.com/getting-started/)ページに移動して、Helm を実行するようにシステムを構成します。その後、このページに進み、グラフからさまざまなシナリオを展開する方法を示す例を参照してください。

!!! note "新作リリースのお知らせ"
    チャートの新しいバージョンがリリースされたときに通知を受け取りたい場合は、[FAQ ページ](../reference/faqs.md)の **Orchestration/Helm/Kubernetes** セクションを参照して、チャートの GitHub リポジトリをフォローする手順を確認してください。

!!! note "ローカル K8s クラスターによる Ingress"
    Ingressを使用した[ローカル Kubernetes クラスターのデプロイ](./deployLocalK8sCluster.md)ページで説明されているように、作成されたローカル kind クラスターでこれらの例を実行する場合は、[このページ](./deployHelmLocalIngress.md)のIngress構成手順を参照してください。それ以外の場合は、製品サービスにポート転送できます。

<!--
## Helm Chart Example Configurations

The following table contains example configurations and instructions to run and configure Ping products
using the Ping Devops Helm Chart.

| Config                                         | Description                                               | .yaml                                                                                                                                                                                                    | Notes                          |
| ---------------------------------------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| Everything                                     | Example with most products integrated together            | [everything.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/everything.yaml)                                                                     |                                |
| Ingress                                        | Expose an application outside of the cluster              | [ingress.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/ingress.yaml)                                                                           | Update line 7 with your domain |
| RBAC                                           | Enable RBAC for workloads                                 | [rbac.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/rbac.yaml)                                                                                 |
| Vault                                          | Example vault values section                              | [vault.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/vault.yaml)                                                                               |
| Vault Keystores                                | Example vault values for keystores                        | [vault-keystores.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/vault-keystores.yaml)                                                           |
| PingAccess                                     | PingAccess Admin Console & Engine                         | [pingaccess-cluster.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingaccess-cluster.yaml)                                                     |                                |
| PingAccess & PingFederate Integration          | PA & PF Admin Console & Engine                            | [pingaccess-pingfederate-integration.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingaccess-pingfederate-integration.yaml)                   |                                |
| PingFederate                                   | PingFederate Admin Console & Engine                       | [pingfederate-cluster.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingfederate-cluster.yaml)                                                 |
| PingFederate                                   | Upgrade PingFederate                                      | See .yaml files in [pingfederate-upgrade](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/pingfederate-upgrade)                                                  |
| PingDirectory                                  | PingDirectory Instance                                    | [pingdirectory.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingdirectory.yaml)                                                               |
| PingDirectory Upgrade                          | PingDirectory Upgrade with partition                      | See .yaml files in [pingdirectory-upgrade-partition](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/pingdirectory-upgrade-partition)                            |
| PingDirectory Backup and Sidecar               | PingDirectory with periodic backup and sidecar            | [pingdirectory-periodic-backup.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingdirectory-backup/pingdirectory-periodic-backup.yaml)          |
| PingDirectory Archive Backup to S3 (Demo Only) | Archive PingDirectory backup to S3                        | Sample files in [s3-sidecar](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/s3-sidecar)                                                                         |
| PingDirectory Scale Down                       | Scale Down a PingDirectory StatefulSet                    | See .yaml files in [pingdirectory-scale-down](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/pingdirectory-scale-down)                                          |
| PingAuthorize and PingDirectory                | PingAuthorize with PAP and PingDirectory                  | [pingauthorize-pingdirectory.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingauthorize-pingdirectory.yaml)                                   |
| Entry Balancing                                | PingDirectory and PingDirectoryProxy entry balancing      | See .yaml files in [entry-balancing](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/entry-balancing)                                                            |
| PingCentral                                    | PingCentral                                               | [pingcentral.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingcentral.yaml)                                                                   |
| PingCentral with MySQL                         | PingCentral with external MySQL deployment                | [pingcentral-external-mysql-db.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingcentral-external-mysql-db/pingcentral-external-mysql-db.yaml) |
| Simple Sync                                    | PingDataSync and PingDirectory                            | [simple-sync.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/simple-sync.yaml)                                                                   |
| PingDataSync Failover                          | PingDataSync and PingDirectory with failover              | [pingdatasync-failover.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingdatasync-failover.yaml)                                               |
| Cluster Metrics                                | Example values using various open source tools            | See .yaml files in [cluster-metrics](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/cluster-metrics)                                                            |
| PingDataConsole SSO with PingOne               | Sign into PingDataConsole with PingOne                    | [pingdataconsole-pingone-sso.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingdataconsole-pingone-sso.yaml)                                   |
| Using CSI Volumes                              | Mount secrets with CSI volumes                            | [csi-secrets-volume.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/csi-secrets-volume.yaml)                                                     |
| Splunk logging sidecar                         | Forward product logs to splunk                            | See files in the [splunk folder](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/20-kubernetes/splunk)                                                                   |
| ImagePullSecrets (individual)                  | Provide secret for private registry authentication        | [image-pull-secrets-individual.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/image-pull-secrets-individual.yaml)                               | Replace stubs with your values |
| ImagePullSecrets (global)                      | Provide global secret for private registry authentication | [image-pull-secrets-global.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/image-pull-secrets-global.yaml)                                       | Replace stubs with your values |
-->

## Helm Chartの構成例

次の表には、構成例と、Ping Devops Helm Chart を使用して Ping 製品を実行および構成するための手順が含まれています。

| Config                                         | Description                                               | .yaml                                                                                                                                                                                                    | Notes                          |
| ---------------------------------------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| Everything                                     | ほとんどの製品を統合した例            | [everything.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/everything.yaml)                                                                     |                                |
| Ingress                                        | アプリケーションをクラスターの外に公開する              | [ingress.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/ingress.yaml)                                                                           | 7行目をあなたのドメインで更新します |
| RBAC                                           | ワークロードに対して RBAC を有効にする                                 | [rbac.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/rbac.yaml)                                                                                 |
| Vault                                          | ボールト値の例セクション                              | [vault.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/vault.yaml)                                                                               |
| Vault Keystores                                | キーストアのボールト値の例                        | [vault-keystores.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/vault-keystores.yaml)                                                           |
| PingAccess                                     | PingAccess Admin Console & Engine                         | [pingaccess-cluster.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingaccess-cluster.yaml)                                                     |                                |
| PingAccess & PingFederate Integration          | PA & PF Admin Console & Engine                            | [pingaccess-pingfederate-integration.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingaccess-pingfederate-integration.yaml)                   |                                |
| PingFederate                                   | PingFederate Admin Console & Engine                       | [pingfederate-cluster.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingfederate-cluster.yaml)                                                 |
| PingFederate                                   | PingFederateのアップグレード                                      | [pingfederate-upgrade](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/pingfederate-upgrade)の.yamlファイルを参照                                                  |
| PingDirectory                                  | PingDirectory インスタンス                                    | [pingdirectory.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingdirectory.yaml)                                                               |
| PingDirectory Upgrade                          | パーティションを使用した PingDirector のアップグレード                      | See .yaml files in [pingdirectory-upgrade-partition](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/pingdirectory-upgrade-partition)                            |
| PingDirectory Backup and Sidecar               | 定期的なバックアップとサイドカーを備えた PingDirector            | [pingdirectory-periodic-backup.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingdirectory-backup/pingdirectory-periodic-backup.yaml)          |
| PingDirectory Archive Backup to S3 (Demo Only) | PingDirector のバックアップを S3 にアーカイブする                        | Sample files in [s3-sidecar](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/s3-sidecar)                                                                         |
| PingDirectory Scale Down                       | PingDirectory StatefulSet のスケールダウン                    | See .yaml files in [pingdirectory-scale-down](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/pingdirectory-scale-down)                                          |
| PingAuthorize and PingDirectory                | PAP と PingDirectory を使用した PingAuthorize                  | [pingauthorize-pingdirectory.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingauthorize-pingdirectory.yaml)                                   |
| Entry Balancing                                | PingDirectory および PingDirectoryProxy エントリのバランシング      | See .yaml files in [entry-balancing](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/entry-balancing)                                                            |
| PingCentral                                    | PingCentral                                               | [pingcentral.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingcentral.yaml)                                                                   |
| PingCentral with MySQL                         | 外部 MySQL デプロイメントを使用した PingCentral                | [pingcentral-external-mysql-db.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingcentral-external-mysql-db/pingcentral-external-mysql-db.yaml) |
| Simple Sync                                    | PingDataSync と PingDirectory                            | [simple-sync.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/simple-sync.yaml)                                                                   |
| PingDataSync Failover                          | フェイルオーバーを使用した PingDataSync および PingDirector              | [pingdatasync-failover.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingdatasync-failover.yaml)                                               |
| Cluster Metrics                                | さまざまなオープンソース ツールを使用した値の例            | See .yaml files in [cluster-metrics](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/30-helm/cluster-metrics)                                                            |
| PingDataConsole SSO with PingOne               | PingOne で PingDataConsole にサインインする                    | [pingdataconsole-pingone-sso.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/pingdataconsole-pingone-sso.yaml)                                   |
| Using CSI Volumes                              | CSI ボリュームを使用してシークレットをマウントする                            | [csi-secrets-volume.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/csi-secrets-volume.yaml)                                                     |
| Splunk logging sidecar                         | 製品ログを Splunk に転送する                            | See files in the [splunk folder](https://github.com/pingidentity/pingidentity-devops-getting-started/tree/master/20-kubernetes/splunk)                                                                   |
| ImagePullSecrets (individual)                  | プライベート レジストリ認証のシークレットを提供する        | [image-pull-secrets-individual.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/image-pull-secrets-individual.yaml)                               | Replace stubs with your values |
| ImagePullSecrets (global)                      | プライベート レジストリ認証用のグローバル シークレットを提供する | [image-pull-secrets-global.yaml](https://raw.githubusercontent.com/pingidentity/pingidentity-devops-getting-started/master/30-helm/image-pull-secrets-global.yaml)                                       | Replace stubs with your values |

<!--
## To Deploy

```shell
helm upgrade --install myping pingidentity/ping-devops \
     -f <HTTP link to yaml>
```

## Uninstall

```shell
helm uninstall myping
```
-->

## デプロイ方法

```shell
helm upgrade --install myping pingidentity/ping-devops \
     -f <HTTP link to yaml>
```

## アンインストール

```shell
helm uninstall myping
```
