---
title: Deploy an Example Stack
---
<!--
# Deploy an Example Stack

!!! note "Video Demonstration"
    A video demonstration of this example is available [here](https://videos.pingidentity.com/detail/videos/devops/video/6313575361112/getting-started-walkthrough).

!!! note "Versions Used"
    This example was written using Docker Desktop with Kubernetes enabled on the Mac platform using the Apple M4 chip.  The Docker Desktop version used for this guide was `4.39.0 (184744)`, which includes Docker Engine `v28.0.1` and Kubernetes `v1.32.2`.  The ingress-nginx controller version was `1.12.1`, deployed from Helm chart version `4.12.1`.

!!! note "Kubernetes Services Kubernetes versus Server-Deployed Applications"

    If you are new to Kubernetes-based deployments, there is a distinct difference when running under Kubernetes compared to running applications on servers.  In a server model, many applications typically run on the same server, and you can access any of them using the same host. For example, many on-premise deployments of PingFederate also include the PingDataConsole, hosted on the same server.

    Under Kubernetes, however, each application that requires external access is associated with a `service`.  A service is a fixed endpoint in the cluster that routes traffic to a given application.  So, in this example, there are distinct service endpoints for PingFederate, PingDataConsole, and the other products.  

    In this demo, these service endpoints are load balanced using the Nginx ingress controller. By adding entries to the `/etc/hosts` file, you can access them using typical URL entries.

The Ping Identity Helm [Getting Started](https://helm.pingidentity.com/getting-started/) page has instructions on getting your environment configured for using the Ping Helm charts.

For more examples, see [Helm Chart Example Configurations](../deployment/deployHelm.md).

For more information on Helm with Ping products, see [Ping Identity DevOps Helm Charts](https://helm.pingidentity.com).
-->

# サンプルスタックをデプロイする

!!! note "ビデオデモンストレーション"
    この例のビデオ デモンストレーションは、[ここ](https://videos.pingidentity.com/detail/videos/devops/video/6313575361112/getting-started-walkthrough)から入手できます。

!!! note "使用されるバージョン"
    この例は、Mac プラットフォームで Kubernetes が有効になっている Docker Desktop を使用して作成されました。このガイドで使用されたバージョンは `4.27.1 (136059)` で、これには Docker Engine `v25.0.2` と Kubernetes `v1.29.1` が含まれています。 ingress-nginx コントローラーのバージョンは 1.9.6 で、Helm チャート バージョン `4.9.1` からデプロイされました。

!!! note "Kubernetesとサーバーにデプロイされたアプリケーション"
    Kubernetes ベースのデプロイメントを初めて使用する場合、Kubernetes で実行する場合と、サーバー上でアプリケーションを実行する場合には明確な違いがあります。サーバー モデルでは、通常、多くのアプリケーションが同じサーバー上で実行され、同じホストを使用してどのアプリケーションにもアクセスできます。たとえば、PingFederate の多くのオンプレミス展開には、同じサーバー上でホストされる PingDataConsole も含まれています。

    ただし、Kubernetes では、外部アクセスを必要とする各アプリケーションはサービスに関連付けられます。サービスは、トラフィックを特定のアプリケーションにルーティングするクラスター内の固定エンドポイントです。したがって、この例では、PingFederate、PingDataConsole、およびその他の製品に個別のサービス エンドポイントがあります。

    このデモでは、これらのサービス エンドポイントは、Nginx Ingress コントローラーを使用して負荷分散されます。 /etc/hosts ファイルにエントリを追加すると、一般的な URL エントリを使用してアクセスできるようになります。

Ping Identity Helm [Getting Started](https://helm.pingidentity.com/getting-started/)ページには、Ping Helm チャートを使用するように環境を構成する手順が記載されています。

その他の例については、[Helm チャートの構成例](../deployment/deployHelm.md)を参照してください。

Ping 製品を使用した Helm の詳細については、[Ping Identity DevOps Helm チャート](https://helm.pingidentity.com)を参照してください。

<!--
## What You Will Do

After using Git to clone the `pingidentity-devops-getting-started` repository, you will use Helm to deploy a sample stack to a Kubernetes cluster.
-->

## あなたがやること

Git を使用して `pingidentity-devops-getting-started` リポジトリのクローンを作成した後、Helm を使用してサンプル スタックを Kubernetes クラスターにデプロイします。

<!--
## Prerequisites

* Register for the Ping DevOps program and install/configure `pingctl` with your User and Key
* Install [Git](https://git-scm.com/downloads)
* Follow the instructions on the helm [Getting Started](https://helm.pingidentity.com/getting-started/) page up through updating to the latest charts to ensure you have the latest version of our charts
* Access to a Kubernetes cluster. You can enable Kubernetes in Docker Desktop for a simple cluster, which was the cluster used for this guide (on the Mac platform).
-->

## 前提条件

* Ping DevOps プログラムに登録し、ユーザーとキーを使用して `pingctl` をインストール/構成します。
* [Git](https://git-scm.com/downloads)をインストールする。
* helmの[Getting Started](https://helm.pingidentity.com/getting-started/)ページの指示に従って最新のchartsに更新し、chartsの最新バージョンを確実に入手してください。
* Kubernetes クラスターへのアクセス。 Docker Desktop で単純なクラスターに対して Kubernetes を有効にすることができます。このクラスターは、このガイド (Mac プラットフォーム上) で使用されたクラスターです。

<!--
## Clone the `getting-started` repository

1. Clone the `pingidentity-devops-getting-started` repository to your local `${PING_IDENTITY_DEVOPS_HOME}` directory.

    !!! note "The `${PING_IDENTITY_DEVOPS_HOME}` environment variable was set by running `pingctl config`."

    ```sh
    cd "${PING_IDENTITY_DEVOPS_HOME}"
    git clone \
      https://github.com/pingidentity/pingidentity-devops-getting-started.git
    ```
-->

## `getting-started` リポジトリのクローン

1. `pingidentity-devops-getting-started` リポジトリのクローンをローカルの `${PING_IDENTITY_DEVOPS_HOME}` ディレクトリに作成します。

    !!! note "`${PING_IDENTITY_DEVOPS_HOME}` 環境変数は、`pingctl config` を実行することによって設定されている"

    ```sh
    cd "${PING_IDENTITY_DEVOPS_HOME}"
    git clone \
        https://github.com/pingidentity/pingidentity-devops-getting-started.git
    ```

<!--
## Deploy the example stack

1. Deploy the example stack of our product containers.

    !!! warning "Initial Deployment"
        For this guide, avoid making changes to the `everything.yaml` file to ensure a successful first-time deployment.

    1. Create a namespace for running the stack in your Kubernetes cluster.  

         ```sh
         # Create the namespace
         kubectl create ns pinghelm

         # Set the kubectl context to the namespace
         kubectl config set-context --current --namespace=pinghelm

         # Confirm
         kubectl config view --minify | grep namespace:
         ```

    1. Deploy the ingress controller to Docker Desktop:

         ```sh
         helm upgrade --install ingress-nginx ingress-nginx \
         --repo https://kubernetes.github.io/ingress-nginx \
         --namespace ingress-nginx --create-namespace
         ```

    1. To wait for the Nginx ingress to reach a healthy state, run the following command.  You can also observe the pod status using k9s or by running `kubectl get pods --namespace ingress-nginx`. You should see one controller pod running when the ingress controller is ready.  This command should exit after no more than 90 seconds or so, depending on the speed of your computer:

        ```sh
        kubectl wait --namespace ingress-nginx \
          --for=condition=ready pod \
          --selector=app.kubernetes.io/component=controller \
          --timeout=90s
        ```

    1. Create a secret in the namespace you will be using to run the example (pinghelm) using the `pingctl` utility. This secret will obtain an evaluation license based on your Ping DevOps username and key:

         ```sh
         pingctl k8s generate devops-secret | kubectl apply -f -
         ```

    1.  This example will use the Helm release name `demo` and DNS domain suffix `*pingdemo.example` for accessing applications.  Add all expected hosts to `/etc/hosts`:

        ```sh
        echo '127.0.0.1 demo-pingaccess-admin.pingdemo.example demo-pingaccess-engine.pingdemo.example demo-pingauthorize.pingdemo.example demo-pingauthorizepap.pingdemo.example demo-pingdataconsole.pingdemo.example demo-pingdelegator.pingdemo.example demo-pingdirectory.pingdemo.example demo-pingfederate-admin.pingdemo.example demo-pingfederate-engine.pingdemo.example demo-pingcentral.pingdemo.example' | sudo tee -a /etc/hosts > /dev/null
        ```

    1. To install the chart, go to your local `"${PING_IDENTITY_DEVOPS_HOME}"/pingidentity-devops-getting-started/30-helm` directory and run the command shown here.  In this example, the release (deployment into Kubernetes by Helm) is called `demo`, forming the prefix for all objects created. The `ingress-demo.yaml` file configures the ingresses for the products to use the **_ping-local_** domain:

        ```sh
        helm upgrade --install demo pingidentity/ping-devops -f everything.yaml -f ingress-demo.yaml
        ```

        The latest product Docker images are automatically downloaded if they have not previously been pulled from [Docker Hub](https://hub.docker.com/u/pingidentity/).

        Sample output:

         ```text
         Release "demo" does not exist. Installing it now.
         NAME: demo
         LAST DEPLOYED: Tue Apr  1 09:14:20 2025
         NAMESPACE: pinghelm
         STATUS: deployed
         REVISION: 1
         TEST SUITE: None
         NOTES:
         #-------------------------------------------------------------------------------------
         # Ping DevOps
         #
         # Description: Ping Identity helm charts - 03/03/2025
         #-------------------------------------------------------------------------------------
         #
         #           Product          tag   typ  #  cpu R/L   mem R/L  Ing
         #    --------------------- ------- --- -- --------- --------- ---
         #    global                2502              0/0       0/0     √
         #
         #  √ pingaccess-admin      2502    sts  1    0/2     1Gi/4Gi   √
         #  √ pingaccess-engine     2502    dep  1    0/2     1Gi/4Gi   √
         #  √ pingauthorize         2502    dep  1    0/2    1.5G/4Gi   √
         #    pingauthorizepap
         #    pingcentral
         #  √ pingdataconsole       2502    dep  1    0/2    .5Gi/2Gi   √
         #    pingdatasync
         #    pingdelegator
         #  √ pingdirectory         2502    sts  1  50m/2     2Gi/8Gi   √
         #    pingdirectoryproxy
         #  √ pingfederate-admin    2502    dep  1    0/2     1Gi/4Gi   √
         #  √ pingfederate-engine   2502    dep  1    0/2     1Gi/4Gi   √
         #    pingintelligence
         #
         #    ldap-sdk-tools
         #    pd-replication-timing
         #    pingtoolkit
         #
         #-------------------------------------------------------------------------------------
         # To see values info, simply set one of the following on your helm install/upgrade
         #
         #    --set help.values=all         # Provides all (i.e. .Values, .Release, .Chart, ...) yaml
         #    --set help.values=global      # Provides global values
         #    --set help.values={ image }   # Provides image values merged with global
         #-------------------------------------------------------------------------------------
         ```

        As you can see, PingAccess Admin and Engine, PingData Console, PingDirectory, PingAuthorize, and the PingFederate Admin and Engine are deployed from the provided `everything.yaml` values file.

        It will take several minutes for all components to become operational.

     1. To display the status of the deployed components, you can use [k9s](https://k9scli.io/) or issue the corresponding commands shown here:

           * Display the services (endpoints for connecting) by running `kubectl get service --selector=app.kubernetes.io/instance=demo`

           ```text
           NAME                            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                   AGE
           demo-pingaccess-admin           ClusterIP   10.105.30.25     <none>        9090/TCP,9000/TCP         6m33s
           demo-pingaccess-admin-cluster   ClusterIP   None             <none>        <none>                    6m33s
           demo-pingaccess-engine          ClusterIP   10.100.1.136     <none>        3000/TCP                  6m33s
           demo-pingauthorize              ClusterIP   10.101.98.228    <none>        443/TCP                   6m33s
           demo-pingauthorize-cluster      ClusterIP   None             <none>        1636/TCP                  6m33s
           demo-pingdataconsole            ClusterIP   10.103.181.27    <none>        8443/TCP                  6m33s
           demo-pingdirectory              ClusterIP   10.106.174.162   <none>        443/TCP,389/TCP,636/TCP   6m33s
           demo-pingdirectory-cluster      ClusterIP   None             <none>        1636/TCP                  6m33s
           demo-pingfederate-admin         ClusterIP   10.96.52.217     <none>        9999/TCP                  6m33s
           demo-pingfederate-cluster       ClusterIP   None             <none>        7600/TCP,7700/TCP         6m33s
           demo-pingfederate-engine        ClusterIP   10.103.84.196    <none>        9031/TCP                  6m33s
           ```

           * To view the pods, run `kubectl get pods --selector=app.kubernetes.io/instance=demo` - you will need to run this at intervals until all pods have started (** Running ** status):

           ```text
           NAME                                        READY   STATUS    RESTARTS   AGE
           demo-pingaccess-admin-0                    1/1     Running   0          7m7s
           demo-pingaccess-engine-59cfb85b9d-7l6tz    1/1     Running   0          7m7s
           demo-pingauthorize-5696dd6b67-hsxnw        1/1     Running   0          7m7s
           demo-pingdataconsole-56b75f9ffb-wld5k      1/1     Running   0          7m7s
           demo-pingdirectory-0                       1/1     Running   0          7m7s
           demo-pingfederate-admin-67cdb47bb4-h88zr   1/1     Running   0          7m7s
           demo-pingfederate-engine-d9889b494-pdhv8   1/1     Running   0          7m7s
           ```

           * To see the ingresses you will use to access the product, run `kubectl get ingress`. If the ingress controller is configured properly, you should see `localhost` as the address as shown here:

           ```text
           NAME                       CLASS    HOSTS                                     ADDRESS     PORTS     AGE
           demo-pingaccess-admin      nginx   demo-pingaccess-admin.pingdemo.example      localhost   80, 443   7m28s
           demo-pingaccess-engine     nginx   demo-pingaccess-engine.pingdemo.example     localhost   80, 443   7m28s
           demo-pingauthorize         nginx   demo-pingauthorize.pingdemo.example         localhost   80, 443   7m28s
           demo-pingdataconsole       nginx   demo-pingdataconsole.pingdemo.example       localhost   80, 443   7m28s
           demo-pingdirectory         nginx   demo-pingdirectory.pingdemo.example         localhost   80, 443   7m28s
           demo-pingfederate-admin    nginx   demo-pingfederate-admin.pingdemo.example    localhost   80, 443   7m28s
           demo-pingfederate-engine   nginx   demo-pingfederate-engine.pingdemo.example   localhost   80, 443   7m28s
           ```

        !!! error "Address must be localhost"
            If the ingress controller is working properly, the ingress definitions will all report the ADDRESS column as `localhost` as shown above.  If you do not see this entry, then you will not be able to access the services later.  This problem is due to a known error with Docker Desktop and the embedded virtual machine (VM) used on the Mac and Windows platform in combination with the ingress controller. To correct the problem, uninstall the chart as instructed at the bottom of this page and restart Docker Desktop.  Afterward, you can re-run the helm command to install the Ping products as instructed above.  The [issue appears to be related to a stale networking configuration](https://github.com/kubernetes/ingress-nginx/issues/7686) under the covers of Docker Desktop.


           * To see everything tied to the helm release run `kubectl get all --selector=app.kubernetes.io/instance=demo`:

           ```text
           NAME                                            READY   STATUS    RESTARTS   AGE
           pod/demo-pingaccess-admin-0                    1/1     Running   0          8m23s
           pod/demo-pingaccess-engine-59cfb85b9d-7l6tz    1/1     Running   0          8m23s
           pod/demo-pingauthorize-5696dd6b67-hsxnw        1/1     Running   0          8m23s
           pod/demo-pingdataconsole-56b75f9ffb-wld5k      1/1     Running   0          8m23s
           pod/demo-pingdirectory-0                       1/1     Running   0          8m23s
           pod/demo-pingfederate-admin-67cdb47bb4-h88zr   1/1     Running   0          8m23s
           pod/demo-pingfederate-engine-d9889b494-pdhv8   1/1     Running   0          8m23s
           
           NAME                                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                   AGE
           service/demo-pingaccess-admin           ClusterIP   10.105.30.25     <none>        9090/TCP,9000/TCP         8m23s
           service/demo-pingaccess-admin-cluster   ClusterIP   None             <none>        <none>                    8m23s
           service/demo-pingaccess-engine          ClusterIP   10.100.1.136     <none>        3000/TCP                  8m23s
           service/demo-pingauthorize              ClusterIP   10.101.98.228    <none>        443/TCP                   8m23s
           service/demo-pingauthorize-cluster      ClusterIP   None             <none>        1636/TCP                  8m23s
           service/demo-pingdataconsole            ClusterIP   10.103.181.27    <none>        8443/TCP                  8m23s
           service/demo-pingdirectory              ClusterIP   10.106.174.162   <none>        443/TCP,389/TCP,636/TCP   8m23s
           service/demo-pingdirectory-cluster      ClusterIP   None             <none>        1636/TCP                  8m23s
           service/demo-pingfederate-admin         ClusterIP   10.96.52.217     <none>        9999/TCP                  8m23s
           service/demo-pingfederate-cluster       ClusterIP   None             <none>        7600/TCP,7700/TCP         8m23s
           service/demo-pingfederate-engine        ClusterIP   10.103.84.196    <none>        9031/TCP                  8m23s
           
           NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
           deployment.apps/demo-pingaccess-engine     1/1     1            1           8m23s
           deployment.apps/demo-pingauthorize         1/1     1            1           8m23s
           deployment.apps/demo-pingdataconsole       1/1     1            1           8m23s
           deployment.apps/demo-pingfederate-admin    1/1     1            1           8m23s
           deployment.apps/demo-pingfederate-engine   1/1     1            1           8m23s
           
           NAME                                                 DESIRED   CURRENT   READY   AGE
           replicaset.apps/demo-pingaccess-engine-59cfb85b9d    1         1         1       8m23s
           replicaset.apps/demo-pingauthorize-5696dd6b67        1         1         1       8m23s
           replicaset.apps/demo-pingdataconsole-56b75f9ffb      1         1         1       8m23s
           replicaset.apps/demo-pingfederate-admin-67cdb47bb4   1         1         1       8m23s
           replicaset.apps/demo-pingfederate-engine-d9889b494   1         1         1       8m23s
           
           NAME                                     READY   AGE
           statefulset.apps/demo-pingaccess-admin   1/1     8m23s
           statefulset.apps/demo-pingdirectory      1/1     8m23s
           ```

           * To view logs, look at the logs for the deployment of the product in question.  For example:

           ```text
            kubectl logs -f deployment/demo-pingfederate-admin
           ```
-->

## サンプルスタックをデプロイする

1. 製品コンテナのサンプル スタックをデプロイします。

    !!! warning "初回のデプロイ"
        このガイドでは、初回のデプロイメントを確実に成功させるために、`everything.yaml` ファイルに変更を加えることは避けてください。

    1. Kubernetes クラスターでスタックを実行するための名前空間を作成します。

         ```sh
         # namespaceを作成する
         kubectl create ns pinghelm

         # kubectl コンテキストをnamespaceに設定します
         kubectl config set-context --current --namespace=pinghelm

         # 確認する
         kubectl config view --minify | grep namespace:
         ```

    1. Ingress コントローラーを Docker Desktop にデプロイします。

         ```sh
         helm upgrade --install ingress-nginx ingress-nginx \
         --repo https://kubernetes.github.io/ingress-nginx \
         --namespace ingress-nginx --create-namespace
         ```

    1. Nginx Ingress が正常な状態に達するまで待機するには、次のコマンドを実行します。k9s を使用するか、`kubectl get pods --namespace ingress-nginx` を実行してポッドのステータスを観察することもできます。ingress コントローラーの準備が完了すると、1 つのコントローラー podが実行されているのが表示されます。コンピュータの速度に応じて、このコマンドは 90 秒程度で終了します。

        ```sh
        kubectl wait --namespace ingress-nginx \
          --for=condition=ready pod \
          --selector=app.kubernetes.io/component=controller \
          --timeout=90s
        ```

    1. `pingctl` ユーティリティを使用して、サンプル (pinghelm) を実行するために使用する名前空間にシークレットを作成します。このシークレットは、Ping DevOps ユーザー名とキーに基づいて評価ライセンスを取得します。

         ```sh
         pingctl k8s generate devops-secret | kubectl apply -f -
         ```

    1. この例では、アプリケーションにアクセスするために Helm リリース名`demo`と DNS ドメイン サフィックス `*pingdemo.example` を使用します。予想されるすべてのホストを `/etc/hosts` に追加します。

        ```sh
        echo '127.0.0.1 demo-pingaccess-admin.pingdemo.example demo-pingaccess-engine.pingdemo.example demo-pingauthorize.pingdemo.example demo-pingauthorizepap.pingdemo.example demo-pingdataconsole.pingdemo.example demo-pingdelegator.pingdemo.example demo-pingdirectory.pingdemo.example demo-pingfederate-admin.pingdemo.example demo-pingfederate-engine.pingdemo.example demo-pingcentral.pingdemo.example' | sudo tee -a /etc/hosts > /dev/null
        ```

    1. チャートをインストールするには、ローカルの `"${PING_IDENTITY_DEVOPS_HOME}"/pingidentity-devops-getting-started/30-helm` ディレクトリに移動し、ここに示されているコマンドを実行します。この例では、リリース (Helm による Kubernetes へのデプロイメント) は `demo` と呼ばれ、作成されたすべてのオブジェクトのプレフィックスを形成します。 `ingress-demo.yaml` ファイルは、 **_ping-local_**ドメインを使用するように製品の Ingress を構成します。

        ```sh
        helm upgrade --install demo pingidentity/ping-devops -f everything.yaml -f ingress-demo.yaml
        ```

        最新の製品 Docker イメージは、以前に [Docker Hub](https://hub.docker.com/u/pingidentity/) から取得されていない場合、自動的にダウンロードされます。

        出力の例:

         ```text
         NAME: demo
         LAST DEPLOYED: Tue Feb  6 13:04:07 2024
         NAMESPACE: pinghelm
         STATUS: deployed
         REVISION: 1
         TEST SUITE: None
         NOTES:
         #-------------------------------------------------------------------------------------
         # Ping DevOps
         #
         # Description: Ping Identity helm charts - 02/05/2024
         #-------------------------------------------------------------------------------------
         #
         #           Product          tag   typ  #  cpu R/L   mem R/L  Ing
         #    --------------------- ------- --- -- --------- --------- ---
         #    global                2401              0/0       0/0     √
         #
         #  √ pingaccess-admin      2401    sts  1    0/2     1Gi/4Gi   √
         #  √ pingaccess-engine     2401    dep  1    0/2     1Gi/4Gi   √
         #  √ pingauthorize         2401    dep  1    0/2    1.5G/4Gi   √
         #    pingauthorizepap
         #    pingcentral
         #  √ pingdataconsole       2401    dep  1    0/2    .5Gi/2Gi   √
         #    pingdatasync
         #    pingdelegator
         #  √ pingdirectory         2401    sts  1  50m/2     2Gi/8Gi   √
         #    pingdirectoryproxy
         #  √ pingfederate-admin    2401    dep  1    0/2     1Gi/4Gi   √
         #  √ pingfederate-engine   2401    dep  1    0/2     1Gi/4Gi   √
         #    pingintelligence
         #
         #    ldap-sdk-tools
         #    pd-replication-timing
         #    pingtoolkit
         #
         #-------------------------------------------------------------------------------------
         # To see values info, simply set one of the following on your helm install/upgrade
         #
         #    --set help.values=all         # Provides all (i.e. .Values, .Release, .Chart, ...) yaml
         #    --set help.values=global      # Provides global values
         #    --set help.values={ image }   # Provides image values merged with global
         #-------------------------------------------------------------------------------------
         ```

        ご覧のとおり、PingAccess Admin と Engine、PingData Console、PingDirectory、PingAuthorize、および PingFederate Admin と Engine は、提供された `everything.yaml` 値ファイルからデプロイされます。

        すべてのコンポーネントが動作可能になるまでに数分かかります。

     1. デプロイされたコンポーネントのステータスを表示するには、[k9s](https://k9scli.io/) を使用するか、次に示す対応するコマンドを発行します。

           * `kubectl get service --selector=app.kubernetes.io/instance=demo` を実行して、サービス (接続用のエンドポイント) を表示します。

           ```text
           NAME                            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                   AGE
           demo-pingaccess-admin           ClusterIP   10.106.227.103   <none>        9090/TCP,9000/TCP         15m
           demo-pingaccess-admin-cluster   ClusterIP   None             <none>        <none>                    15m
           demo-pingaccess-engine          ClusterIP   10.108.102.245   <none>        3000/TCP                  15m
           demo-pingauthorize              ClusterIP   10.110.95.132    <none>        443/TCP                   15m
           demo-pingauthorize-cluster      ClusterIP   None             <none>        1636/TCP                  15m
           demo-pingdataconsole            ClusterIP   10.97.81.22      <none>        8443/TCP                  15m
           demo-pingdirectory              ClusterIP   10.102.91.214    <none>        443/TCP,389/TCP,636/TCP   15m
           demo-pingdirectory-cluster      ClusterIP   None             <none>        1636/TCP                  15m
           demo-pingfederate-admin         ClusterIP   10.99.145.24     <none>        9999/TCP                  15m
           demo-pingfederate-cluster       ClusterIP   None             <none>        7600/TCP,7700/TCP         15m
           demo-pingfederate-engine        ClusterIP   10.108.240.203   <none>        9031/TCP                  15m
           ```

           * To view the pods, run `kubectl get pods --selector=app.kubernetes.io/instance=demo` - you will need to run this at intervals until all pods have started (** Running ** status):
           * ポッドを表示するには、`kubectl get pods --selector=app.kubernetes.io/instance=demo` を実行します。すべてのポッドが開始される (**Running** ステータス) まで、これを一定の間隔で実行する必要があります。

           ```text
           NAME                                        READY   STATUS    RESTARTS   AGE
           demo-pingaccess-admin-0                     1/1     Running   0          7m29s
           demo-pingaccess-engine-cf9987bb5-npspz      1/1     Running   0          7m52s
           demo-pingauthorize-8bdfd4fd8-j82zg          1/1     Running   0          7m43s
           demo-pingdataconsole-7c875985d4-mfjbq       1/1     Running   0          17m
           demo-pingdirectory-0                        1/1     Running   0          7m38s
           demo-pingfederate-admin-5786787dfd-5b5s5    1/1     Running   0          7m36s
           demo-pingfederate-engine-5ff6546f4f-7jfnt   1/1     Running   0          7m31s
           ```

           * 製品へのアクセスに使用する ingress を確認するには、`kubectl get ingress` を実行します。イングレス コントローラーが正しく構成されている場合は、次のようにアドレスとして `localhost` が表示されるはずです。

           ```text
           NAME                       CLASS    HOSTS                                     ADDRESS     PORTS     AGE
           demo-pingaccess-admin      nginx   demo-pingaccess-admin.pingdemo.example      localhost   80, 443   9m50s
           demo-pingaccess-engine     nginx   demo-pingaccess-engine.pingdemo.example     localhost   80, 443   9m50s
           demo-pingauthorize         nginx   demo-pingauthorize.pingdemo.example         localhost   80, 443   9m50s
           demo-pingdataconsole       nginx   demo-pingdataconsole.pingdemo.example       localhost   80, 443   9m50s
           demo-pingdirectory         nginx   demo-pingdirectory.pingdemo.example         localhost   80, 443   9m50s
           demo-pingfederate-admin    nginx   demo-pingfederate-admin.pingdemo.example    localhost   80, 443   9m50s
           demo-pingfederate-engine   nginx   demo-pingfederate-engine.pingdemo.example   localhost   80, 443   9m50s
           ```

        !!! error "アドレスはローカルホストである必要があります"
            ingress コントローラーが適切に動作している場合、ingress定義はすべて、上記のように ADDRESS 列を `localhost` として報告します。このエントリが表示されない場合は、後でサービスにアクセスできなくなります。この問題は、Docker Desktop と、Mac および Windows プラットフォームでingress コントローラーと組み合わせて使用​​される組み込み仮想マシン (VM) の既知のエラーが原因です。問題を解決するには、このページの下部の指示に従ってチャートをアンインストールし、Docker Desktop を再起動します。その後、helm コマンドを再実行して、上記の手順に従って Ping 製品をインストールできます。[この問題は、Docker Desktop の背後にある古いネットワーク構成に関連しているようです](https://github.com/kubernetes/ingress-nginx/issues/7686)。


           * Helm リリースに関連するすべてを確認するには、`kubectl get all --selector=app.kubernetes.io/instance=demo` を実行します。

           ```text
           NAME                                            READY   STATUS    RESTARTS   AGE
           pod/demo-pingaccess-admin-0                     1/1     Running   0          107m
           pod/demo-pingaccess-engine-cf9987bb5-npspz      1/1     Running   0          107m
           pod/demo-pingauthorize-8bdfd4fd8-j82zg          1/1     Running   0          107m
           pod/demo-pingdataconsole-7c875985d4-mfjbq       1/1     Running   0          116m
           pod/demo-pingdirectory-0                        1/1     Running   0          107m
           pod/demo-pingfederate-admin-5786787dfd-5b5s5    1/1     Running   0          107m
           pod/demo-pingfederate-engine-5ff6546f4f-7jfnt   1/1     Running   0          107m
           
           NAME                                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                   AGE
           service/demo-pingaccess-admin           ClusterIP   10.96.234.72     <none>        9090/TCP,9000/TCP         116m
           service/demo-pingaccess-admin-cluster   ClusterIP   None             <none>        <none>                    116m
           service/demo-pingaccess-engine          ClusterIP   10.106.190.217   <none>        3000/TCP                  116m
           service/demo-pingauthorize              ClusterIP   10.104.246.123   <none>        443/TCP                   116m
           service/demo-pingauthorize-cluster      ClusterIP   None             <none>        1636/TCP                  116m
           service/demo-pingdataconsole            ClusterIP   10.96.166.28     <none>        8443/TCP                  116m
           service/demo-pingdirectory              ClusterIP   10.107.42.8      <none>        443/TCP,389/TCP,636/TCP   116m
           service/demo-pingdirectory-cluster      ClusterIP   None             <none>        1636/TCP                  116m
           service/demo-pingfederate-admin         ClusterIP   10.105.26.94     <none>        9999/TCP                  116m
           service/demo-pingfederate-cluster       ClusterIP   None             <none>        7600/TCP,7700/TCP         116m
           service/demo-pingfederate-engine        ClusterIP   10.99.223.48     <none>        9031/TCP                  116m
           
           NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
           deployment.apps/demo-pingaccess-engine     1/1     1            1           116m
           deployment.apps/demo-pingauthorize         1/1     1            1           116m
           deployment.apps/demo-pingdataconsole       1/1     1            1           116m
           deployment.apps/demo-pingfederate-admin    1/1     1            1           116m
           deployment.apps/demo-pingfederate-engine   1/1     1            1           116m
           
           NAME                                                  DESIRED   CURRENT   READY   AGE
           replicaset.apps/demo-pingaccess-engine-cf9987bb5      1         1         1       116m
           replicaset.apps/demo-pingauthorize-8bdfd4fd8          1         1         1       116m
           replicaset.apps/demo-pingdataconsole-7c875985d4       1         1         1       116m
           replicaset.apps/demo-pingfederate-admin-5786787dfd    1         1         1       116m
           replicaset.apps/demo-pingfederate-engine-5ff6546f4f   1         1         1       116m
           
           NAME                                     READY   AGE
           statefulset.apps/demo-pingaccess-admin   1/1     116m
           statefulset.apps/demo-pingdirectory      1/1     116m
           ```

           * ログを表示するには、問題の製品の展開のログを確認します。例えば以下のようにします。

           ```text
            kubectl logs -f deployment/demo-pingfederate-admin
           ```

2. これらは、製品の管理コンソールにサインオンするための URL と資格情報です。

    !!! note "証明書"
        この例では、ブラウザで受け入れられるか、キーストアに追加される必要がある自己署名証明書を使用します。

    ingressを設定すると、次の URL で製品にアクセスできます。

    | 製品                                                                           | 接続の詳細                                                                                                                                                                                                                                            |
    | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | [PingFederate](https://demo-pingfederate-admin.pingdemo.example/pingfederate/app) | <ul> <li>URL: [https://demo-pingfederate-admin.pingdemo.example/pingfederate/app](https://demo-pingfederate-admin.pingdemo.example/pingfederate/app)</li><li>Username: administrator</li><li>Password: 2FederateM0re</li></ul>                                |
    | [PingDirectory](https://demo-pingdataconsole.pingdemo.example/console)            | <ul><li>URL: [https://demo-pingdataconsole.pingdemo.example/console](https://demo-pingdataconsole.pingdemo.example/console)</li><li>Server: ldaps://demo-pingdirectory-cluster:1636</li><li>Username: administrator</li><li>Password: 2FederateM0re</li></ul> |
    | [PingAccess](https://demo-pingaccess-admin.pingdemo.example/)                     | <ul><li>URL: [https://demo-pingaccess-admin.pingdemo.example/](https://demo-pingaccess-admin.pingdemo.example/)</li><li>Username: administrator</li><li>Password: 2FederateM0re</li></ul>                                                                     |
    | [PingAuthorize](https://demo-pingdataconsole.pingdemo.example/console)            | <ul><li>URL: [https://demo-pingdataconsole.pingdemo.example/console](https://demo-pingdataconsole.pingdemo.example/console)</li><li>Server: ldaps://demo-pingauthorize-cluster:1636</li><li>Username: administrator</li><li>Password: 2FederateM0re</li></ul> |

3. 完了したら、helm のアンインストール コマンドを実行して、`demo` コンポーネントを削除できます。

    ```sh
    helm uninstall demo
    ```

<!--
## Next Steps

Now that you have deployed a set of our product images using the provided chart, you can move on to deployments using configurations that more closely reflect use cases to be explored.  Refer to the [helm examples](../deployment/deployHelm.md)) page for other typical deployments.

!!! warning "Container logging"
    Maintaining logs in a containerized model is different from the typical server-deployed application.  See [this page](../reference/containerLogging.md) for additional details.
-->

## 次のステップ

提供されたチャートを使用して一連の製品イメージをデプロイしたので、検討するユースケースをより厳密に反映した構成を使用したデプロイに進むことができます。他の一般的なデプロイメントについては、[helm examples](../deployment/deployHelm.md)のページを参照してください。

!!! warning "コンテナのロギング"
    コンテナ化モデルでのログの管理は、サーバーにデプロイされた一般的なアプリケーションとは異なります。詳細については、[このページ](../reference/containerLogging.md)を参照してください。
