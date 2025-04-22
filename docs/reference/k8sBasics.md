---
title: Kubernetes Basics
---

<!--
# Kubernetes Basics

Although this document cannot cover all aspects of these tools, new Kubernetes users might find other technical documentation too involved for purposes of using Ping Identity images. This document aims to equip new users with helpful terminology in simple terms, with a focus on relevant commands.

!!! note
    This overview uses Ping Identity images and practices as a guide, but generally applies to any interactions in Kubernetes. With these assumptions, this document might feel incomplete or inaccurate to veterans. If you would like to contribute to this document, feel free to open a pull request!
-->

# Kubernetes の基本

このドキュメントではこれらのツールのすべての側面をカバーすることはできませんが、Kubernetes の新規ユーザーは、Ping Identity イメージを使用する目的では他の技術ドキュメントが複雑すぎると感じるかもしれません。このドキュメントは、関連するコマンドに焦点を当てて、新しいユーザーに役立つ用語を簡単な用語で提供することを目的としています。

!!! note
    この概要では、Ping Identity のイメージと実践をガイドとして使用しますが、一般的に Kubernetes でのあらゆる対話に当てはまります。こうした前提を踏まえると、退役軍人にとってこの文書は不完全または不正確に感じるかもしれません。このドキュメントに貢献したい場合は、お気軽にpull requestを開いてください！

<!--
## Kubernetes

### Terms

**Cluster** - The ice cube tray

You can consider a Kubernetes cluster as a set of resources into which you can deploy containers. A cluster can be as small as your local computer or as large as hundreds of virtual machines (VMs), called **nodes**, in a data center. Interaction with the cluster is through an API requiring authentication and role-based access control (RBAC) that allows the actions necessary within the cluster.

In a cloud provider Kubernetes cluster, such as Amazon Web Services (AWS) EKS, Azure AKS, or Google GKE, the cluster can span multiple Availability Zones (AZs), but only _one_ region. In AWS terms, a cluster can be in the region us-west-2 but have nodes in the AZs us-west-2a, us-west-2b, and us-west-2c. Kubernetes provides high availability by distributing applications with multiple instances of containers, called replicas, across available AZs.

**Nodes** - The individual ice cube spaces in the tray

The nodes are the pieces that provide allocatable resources, such as CPU and memory, and make up a cluster. Typically, these are VMs, and for example in AWS, they would be EC2 instances.

**Namespace** - A loosely defined slice of the cluster

Namespaces are intended to be a virtual delimiter for deploying grouped applications.  While it is possible for pods to communicate across namespaces, policies can be put in place with third-party services to prevent this communication.

!!! note
    You can allocate resource limits available to a namespace, but this is not required.

**Context** - A definition in your ~/.kube/config file that specifies the cluster and namespace where your `kubectl` commands will be executed.

**Deployments and Statefulsets** - The water that fills ice cube spots.

Applications are deployed as Deployments or Statefulsets. You can consider both of these objects as controllers that define and manage the following:

- Name of an application
- Number of instances (pods) of an application (replicas)
- Persistent storage

Though they are similar, Deployments differ from Statefulsets in a few fundamental ways.
-->

## Kubernetes

### 用語

**クラスター（Cluster）** - 製氷皿

Kubernetes クラスターは、コンテナーをデプロイできるリソースのセットとして考えることができます。クラスターは、ローカル コンピューターと同じくらい小さい場合もあれば、データ センター内のノードと呼ばれる数百の仮想マシン (VM) ほど大きい場合もあります。クラスターとの対話は、クラスター内で必要なアクションを可能にする認証とロールベースのアクセス制御 (RBAC) を必要とする API を通じて行われます。

アマゾン ウェブ サービス (AWS) EKS、Azure AKS、Google GKE などのクラウド プロバイダーの Kubernetes クラスターでは、クラスターは複数のアベイラビリティーゾーン (AZ) にまたがることができますが、リージョンは 1 つだけです。 AWS の用語では、クラスターはリージョン us-west-2 にありますが、ノードは AZ us-west-2a、us-west-2b、us-west-2c にあります。 Kubernetes は、レプリカと呼ばれるコンテナの複数のインスタンスを使用してアプリケーションを利用可能な AZ に分散することで高可用性を実現します。

**ノード（Nodes）** - トレイ内の個々の角氷スペース

ノードは、CPU やメモリなどの割り当て可能なリソースを提供し、クラスターを構成する部分です。通常、これらは VM であり、たとえば AWS では EC2 インスタンスになります。

**名前空間（Namespace）** - 大まかに定義されたクラスターのスライス

名前空間は、グループ化されたアプリケーションをデプロイするための仮想区切り文字となることを目的としています。ポッドが名前空間を越えて通信することは可能ですが、この通信を防ぐためにサードパーティのサービスを使用してポリシーを導入することができます。

!!! note
    名前空間に利用可能なリソース制限を割り当てることができますが、これは必須ではありません。

**コンテキスト（Context）** - `kubectl` コマンドが実行されるクラスターと名前空間を指定する ~/.kube/config ファイル内の定義。

**デプロイメントとステートフルセット（Deployments and Statefulsets）** - 氷皿を埋める水

アプリケーションは、デプロイメントまたはステートフルセットとしてデプロイされます。これらのオブジェクトは両方とも、次のものを定義および管理するコントローラーとして考えることができます。

- アプリケーションの名前
- アプリケーション (レプリカ; replicas) のインスタンス (ポッド; pods) の数
- 永続ストレージ

デプロイメントとステートフルセットは似ていますが、いくつかの基本的な点で異なります。

<!--
##### Deployments

* Deployments are typically used for stateless applications - if a pod is lost or removed, any other pod in the same deployment can take on the activity the lost pod was performing.
* Pod names are inconsequential because each pod is identical with no state information required.  As a result, the name of the pod does not matter and names are suffixed with a randomly generated string.
* The order in which pods are started is also inconsequential.  When starting a deployment all pods are launched at the same time.  
* When updating a deployment, you can cycle one, many, or all pods at the same time.
-->

##### デプロイメント（Deployments）

- デプロイメントは通常、ステートレス アプリケーションに使用されます。ポッドが紛失または削除された場合、同じデプロイメント内の他のポッドが、失われたポッドが実行していたアクティビティを引き継ぐことができます。
- 各ポッドは同一であり、状態情報は必要ないため、ポッド名は重要ではありません。その結果、ポッドの名前は重要ではなくなり、名前の末尾にはランダムに生成された文字列が付加されます。
- ポッドが開始される順序も重要ではありません。デプロイメントを開始すると、すべてのポッドが同時に起動されます。
- デプロイメントを更新するときは、1 つ、複数、またはすべてのポッドを同時に循環できます。

<!--
##### Statefulsets

StatefulSets are more structured in the manner in which the pods are handled.  

* StatefulSets - as the name implies - are used for applications in which a known state is required.  For example, many clustered products have an instance in the cluster that is considered the leader and all pods in the set need to know which pod is acting in this capacity.  A controlled scale-up and scale-down process is needed to maintain known state as application nodes or instances join or leave the cluster or are restarted.  
* Pod names are _sticky_ in that each pod in the StatefulSet has a known name, with each pod receving an ordinal indicator (unlike the random pod name found in Deployments).  For example, a StatefulSet will have pod names similar to:  `myping-pingdirectory-0`, `myping-pingdirectory-1`, and `myping-pingdirectory-2`
* Controlled startup with health priority: unlike a Deployment, a StatefulSet deploys the first instance (pod name appended with -0) and waits for it to be healthy before adding another to the group.
* Updates occur to instances in a rolling fashion, one-at-a-time, starting with the most recent pod (e.g., `myping-pingdirectory-2`) first.
* With a known Pod name, persistent storage can be maintained for each pod.  After persistent storage is created and assigned, the same storage object is provided to the same-named pod every time.
-->

##### ステートフルセット（Statefulsets）

ステートフルセット は、ポッドの処理方法がより構造化されています。

- ステートフルセット は、その名前が示すように、既知の状態が必要なアプリケーションに使用されます。たとえば、多くのクラスタ化された製品には、クラスタ内にリーダーとみなされるインスタンスがあり、セット内のすべてのポッドは、どのポッドがこの能力で動作しているかを認識する必要があります。アプリケーション ノードやインスタンスがクラスターに参加したり、クラスターから離脱したり、再起動したりする際に、既知の状態を維持するには、制御されたスケールアップおよびスケールダウンのプロセスが必要です。
- ポッド名は、ステートフルセット 内の各ポッドが既知の名前を持ち、各ポッドが序数インジケーターを受け取るという点で _固定的_ です (デプロイメント で見つかるランダムなポッド名とは異なります)。たとえば、ステートフルセット には、`myping-pingdirectory-0`、`myping-pingdirectory-1`、`myping-pingdirectory-2` のようなポッド名が付けられます。
- 健全性優先による起動の制御: デプロイメント とは異なり、ステートフルセット は最初のインスタンス (ポッド名に -0 が付加される) をデプロイし、それが健全になるまで待機してからグループに別のインスタンスを追加します。
- 更新は、最新のポッド (例: `myping-pingdirectory-2`) から開始して、一度に 1 つずつローリング方式でインスタンスに対して行われます。
- 既知のポッド名を使用すると、ポッドごとに永続ストレージを維持できます。永続ストレージが作成されて割り当てられた後は、毎回同じストレージ オブジェクトが同じ名前のポッドに提供されます。

<!--
**Pod** - The molecules that make up the water

A Deployment/StatefulSet specifies the _number_ of pods to run for a given application. For example, you can have a `pingfederate-engine` deployment that calls for three replicas with two CPUs and 2 GB of memory, but you cannot make one engine larger or smaller than the others.

Like a molecule, a pod can consist of just one container, or it can have multiple containers, called sidecars. For example, your pod can have a PingFederate container as the main process and a sidecar container, such as Splunk Universal Forwarder, to export logs. All containers in a pod, including these sidecars, share a namespace and IP address.

Pods are are considered disposable and by default do not persist any data.  To maintain state or data, external storage or a database of some kind is needed.
-->

**ポッド（Pod）** - 水を構成する分子

デプロイメント/ステートフルセット は、特定のアプリケーションに対して実行するポッドの数を指定します。たとえば、2 つの CPU と 2 GB のメモリを備えた 3 つのレプリカを必要とする `pingfederate-engine` のデプロイメントを行うことができますが、1 つのエンジンを他のエンジンより大きくしたり小さくしたりすることはできません。

分子と同様に、ポッドは 1 つのコンテナーのみで構成することも、サイドカーと呼ばれる複数のコンテナーで構成することもできます。たとえば、ポッドにはメインプロセスとして PingFederate コンテナーを設定し、ログをエクスポートするために Splunk Universal Forwarder などのサイドカー コンテナーを含めることができます。これらのサイドカーを含むポッド内のすべてのコンテナーは、名前空間と IP アドレスを共有します。

ポッドは使い捨てとみなされ、デフォルトではデータは保持されません。状態やデータを維持するには、外部ストレージまたは何らかのデータベースが必要です。

<!--
**PersistentVolume (PV)** and **PersistentVolumeClaim (PVC)** - A virtual external storage device or definition attached to a Pod

The PV is the storage object and PVC is the claim that a given pod makes for that storage. 
-->

**永続化ボリューム（PersistentVolume（PV））** and **永続化ボリューム要求（PersistentVolumeClaim（PVC））** - ポッドに接続された仮想外部ストレージ デバイスまたは定義

PV はストレージ オブジェクトであり、PVC は特定のポッドがそのストレージに対して行う要求です。

<!--
**Service** - A slim loadbalancer within the cluster

Pods can come and go, be disposed of or restarted.  Every time a Pod is started, it will receive an IP address which often changes. In order to access the application hosted in the Pod, a fixed, known location or address is required.

Services provide a single IP address and cluster-internal DNS resolution that is placed in front of Deployments and Statefulsets to distribute traffic. For service-to-service communication, such as PingFederate using PingDirectory as a user store, the application should be configured to point to a service name and port rather than the individual pods. Services are given fully-qualified domain names (FQDNs) in a cluster. Within the same namespace, services are accessible by their name (`https://myping-pingdirectory:443`), but across namespaces, you must be more explicit (`https://myping-pingdirectory.<namespace>:443`). A FQDN would be `https://myping-pingdirectory.<namespace>.svc.cluster.local`.
-->

**サービス（Service）** - クラスター内のスリムなロードバランサー

ポッドは行き来したり、破棄したり、再起動したりできます。 ポッドが起動されるたびに、頻繁に変更される IP アドレスを受け取ります。ポッドでホストされているアプリケーションにアクセスするには、固定された既知の場所またはアドレスが必要です。

サービスは、トラフィックを分散するためにデプロイメントおよびステートフルセットの前に配置される単一の IP アドレスとクラスター内部の DNS 解決を提供します。ユーザー ストアとして PingDirectory を使用する PingFederate などのサービス間通信の場合、アプリケーションは個々のポッドではなくサービス名とポートを指すように構成する必要があります。クラスター内のサービスには完全修飾ドメイン名 (FQDN) が与えられます。同じ名前空間内では、サービスは名前 (`https://myping-pingdirectory:443`) でアクセスできますが、名前空間を越えると、より明示的にする必要があります (`https://myping-pingdirectory.<namespace>:443`)。 FQDN は `https://myping-pingdirectory.<namespace>.svc.cluster.local` になります。

<!--
**Ingress** - A network definition used to expose an application external to the cluster. In order for an ingress to work, you need an Ingress Controller.

A common pattern is a deployment of Nginx pods fronted by a physical loadbalancer. The client application traffic hits the loadbalancer first, then is forwarded to Nginx. The header information (hostname and path) of the request is evaluated and forwarded to a corresponding application service in the cluster.

For example, suppose a PingFederate ingress has a host name of **myping-pingfederate-engine.pingdemo.example**. If a client application makes a request to `https://myping-pingfederate-engine.pingdemo.example/pf/heartbeat.ping`, the traffic flow of the request would be:

* Client -> LoadBalancer -> Nginx k8s Service -> Nginx Pod -> Pingfederate-engine k8s Service -> Pingfederate-engine pod
-->

**イングレス（Ingress）** - アプリケーションをクラスターの外部に公開するために使用されるネットワーク定義。イングレス が機能するには、イングレス コントローラーが必要です。

一般的なパターンは、物理ロードバランサーが前面にある Nginx ポッドのデプロイメントです。クライアント アプリケーションのトラフィックは、最初にロードバランサーに到達し、次に Nginx に転送されます。リクエストのヘッダー情報 (ホスト名とパス) が評価され、クラスター内の対応するアプリケーション サービスに転送されます。

たとえば、PingFederate イングレスのホスト名が **myping-pingfederate-engine.pingdemo.example** であるとします。クライアント アプリケーションが `https://myping-pingfederate-engine.pingdemo.example/pf/heartbeat.ping` にリクエストを行う場合、リクエストのトラフィック フローは次のようになります。

* Client -> LoadBalancer -> Nginx k8s Service -> Nginx Pod -> Pingfederate-engine k8s Service -> Pingfederate-engine pod

<!--

### Commands

To see which cluster and namespace you are using, use the [kubectx](https://github.com/ahmetb/kubectx#installation) tool.

Alternatively, you can run the following commands:

```shell
# Retrieve and set context
kubectl config get-contexts
kubectl config current-context
kubectl config use-context my-cluster-name

# Set Namespace
kubectl config set-context --current --namespace=<namespace>
```
-->

### コマンド

使用しているクラスターと名前空間を確認するには、[kubectx](https://github.com/ahmetb/kubectx#installation) ツールを使用します。

あるいは、次のコマンドを実行することもできます。

```shell
# Retrieve and set context
kubectl config get-contexts
kubectl config current-context
kubectl config use-context my-cluster-name

# Set Namespace
kubectl config set-context --current --namespace=<namespace>
```

<!--
#### Viewing resources

You can use [k9s](https://github.com/derailed/k9s), which is a UI designed to run in a terminal.

If you cannot use k9s, review the standard commands here.

You can run `kubectl get` for any [resource type](https://kubernetes.io/docs/reference/kubectl/overview/#resource-types), such as Pods, Deployments, Statefulsets, and PVCs. Many resources have short names:

- `po` = pods
- `deploy` = Deployments
- `sts` = Statefulsets
- `ing` = ingresses
- `pvc` = persistentvolumeclaims

The most common command is `get pods`:

```sh
kubectl get pods
```

To show anything that the container prints to `stdout`, use `logs`:

```sh
kubectl logs -f <pod-name>
```

To show the logs of a pod with multiple containers, specify the container for which you wish to view logs with the `-c` option:

```sh
kubectl logs -f <pod-name> -c <container-name>
```

To show the logs of a crashed pod (`RESTARTS != 0`):

```sh
kubectl logs -f <pod-name> --previous
```

To see available host names by ingress:

```sh
kubectl get ing
```
-->

#### リソースの表示

ターミナルで実行するように設計された UI である [k9s](https://github.com/derailed/k9s) を使用できます。

k9s を使用できない場合は、ここで標準コマンドを確認してください。

ポッド、デプロイメント、ステートフルセット、PVC などの任意の[resource type](https://kubernetes.io/docs/reference/kubectl/overview/#resource-types)に対して `kubectl get` を実行できます。多くのリソースには短い名前が付けられています。

- `po` = pods
- `deploy` = Deployments
- `sts` = Statefulsets
- `ing` = ingresses
- `pvc` = persistentvolumeclaims

最も一般的なコマンドは `get pods` です。

```sh
kubectl get pods
```

コンテナーが `stdout` に出力するものを表示するには、 `logs` を使用します。

```sh
kubectl logs -f <pod-name>
```

複数のコンテナーを含むポッドのログを表示するには、 `-c` オプションを使用してログを表示するコンテナーを指定します。

```sh
kubectl logs -f <pod-name> -c <container-name>
```

クラッシュしたポッドのログを表示するには (`RESTARTS != 0`):

```sh
kubectl logs -f <pod-name> --previous
```

利用可能なホスト名をイングレスで確認するには:

```sh
kubectl get ing
```

<!--
#### Debugging

When a pod crashes unexpectedly, you can mine information about the cause with the following commands.

To view logs of the crash:

```sh
kubectl logs -f <pod-name> --previous
```

To view the reason for exit:

```sh
kubectl describe pod <pod-name>
```

When looking at `describe`, there are two main sections of the output to note:

- lastState - shows the exit code and the reason for exit.
- Events - this list is most helpful when your pod is not being created. It might be stuck in pending state if:
  - There are not enough resources available for the pod to be created.
  - Something about the pod definition is incorrect, such as a missing volume or secret.

Common exit codes associated with containers are:

| Exit Code | Description |
| ------------- | ----------------------------------------------------------------------------------------------------- |
| Exit Code 0 | Absence of an attached foreground process |
| Exit Code 1 | Indicates failure due to application error |
| Exit Code 137 | Indicates failure as container received SIGKILL (manual intervention or ‘oom-killer’ [OUT-OF-MEMORY]) |
| Exit Code 139 | Indicates failure as container received SIGSEGV |
| Exit Code 143 | Indicates failure as a container received SIGTERM |
-->

#### デバッグ

ポッドが予期せずクラッシュした場合、次のコマンドを使用して原因に関する情報をマイニングできます。

クラッシュのログを表示するには:

```sh
kubectl logs -f <pod-name> --previous
```

終了の理由を表示するには:

```sh
kubectl describe pod <pod-name>
```

`describe` を見ると、出力には 2 つの主要なセクションがあることに注意してください。

- lastState - 終了コードと終了理由を示します
- Events - このリストは、ポッドが作成されていないときに最も役立ちます。次の場合、保留状態のままになる可能性があります。
    - ポッドの作成に使用できるリソースが不足しています。
    - ボリュームやシークレットが欠落しているなど、ポッド定義に関する何らかの不正確な点があります。

コンテナに関連する一般的な終了コードは次のとおりです。

| Exit Code | Description |
| ------------- | ----------------------------------------------------------------------------------------------------- |
| Exit Code 0 | アタッチされたフォアグラウンドプロセスの欠如 |
| Exit Code 1 | アプリケーションエラーによる失敗を示します |
| Exit Code 137 | コンテナが SIGKILL を受信したため失敗を示します (手動介入または 'oom-killer' [OUT-OF-MEMORY]) |
| Exit Code 139 | コンテナが SIGSEGV を受信したため失敗したことを示します |
| Exit Code 143 | コンテナが SIGTERM を受信したため失敗したことを示します |
