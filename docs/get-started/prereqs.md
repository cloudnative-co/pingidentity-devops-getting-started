---
title: Prerequisites
---
<!--
# Prerequisites

In order to use our resources, you will need the following components, software, or other information.

## Product license

You must have a product license to run our images. You may either use an evaluation license or existing license.
-->

# 前提条件

当社のリソースを使用するには、次のコンポーネント、ソフトウェア、またはその他の情報が必要です。

## 製品ライセンス

イメージを実行するには、製品ライセンスが必要です。評価ライセンスまたは既存のライセンスを使用できます。

<!--
### Evaluation License

Generate an evaluation license obtained with a [valid DevOps user key](../how-to/devopsRegistration.md).  

When you register for Ping Identity's DevOps program, you are issued credentials that automate the process of retrieving an evaluation product license.
!!! note "DevOps User and Key"
    For more information about using your DevOps program user and key in various ways (including Kubernetes and with stand-alone containers) see this how-to guide: [Using Your Devops User and Key](../how-to/devopsUserKey.md)

!!! warning "Evaluation License"
    Evaluation licenses are short-lived (30 days) and **must not** be used in production deployments.

Evaluation licenses can only be used with images published in the last 90 days.  If you want to continue to use an image that was published more than 90 days ago, you must obtain a product license.
-->

### 評価ライセンス

[有効な DevOps ユーザー キー](../how-to/devopsRegistration.md)を使用して取得した評価ライセンスを生成します。

Ping Identity の DevOps プログラムに登録すると、評価製品ライセンスの取得プロセスを自動化する認証情報が発行されます。

!!! note "DevOps ユーザーとキー"
    さまざまな方法 (Kubernetes やスタンドアロン コンテナーを含む) での DevOps プログラム ユーザーとキーの使用の詳細については、このハウツー ガイドを参照してください: [Devops ユーザーとキーの使用](../how-to/devopsUserKey.md)

!!! warning "評価ライセンス"
    評価ライセンスは有効期間が短い (30 日間) ため、運用環境で**使用しないで**ください。

評価ライセンスは、過去 90 日間に公開されたイメージでのみ使用できます。 90 日以上前に公開されたイメージを引き続き使用したい場合は、製品ライセンスを取得する必要があります。

<!--
### Existing License

If you possess a product license for the product, you can use it with supported versions of the image (including those over 90 days old mentioned above) by following these instructions to [mount the product license](../how-to/existingLicense.md).

!!! note "Mount paths"
    The mount points and name of the license file vary by product.  The link above provides the proper location and name for these files.
-->

### 既存のライセンス

製品の製品ライセンスを所有している場合は、次の手順に従って[製品ライセンスをマウントする](../how-to/existingLicense.md)ことで、サポートされているバージョンのイメージ (上記の 90 日以上経過したバージョンを含む) でその製品を使用できます。

!!! note "マウントパス"
    マウント ポイントとライセンス ファイルの名前は製品によって異なります。上記のリンクには、これらのファイルの適切な場所と名前が記載されています。

<!--
## Local runtime environment

The initial example uses Kubernetes under Docker Desktop because it does not require a lot of configuration.

In order to try Ping products in a manner most similar to typical production installations, you should consider using a Kubernetes environment. [Kind](https://kind.sigs.k8s.io/) (**K**ubernetes **in** **D**ocker) provides a platform to get started with local Kubernetes development.  Instructions for setting up a Kind cluster are [here](../deployment/deployLocalK8sCluster.md).

Other local Kubernetes environments include [Rancher Desktop](https://rancherdesktop.io), [Docker Desktop](https://www.docker.com/products/docker-desktop/) with Kubernetes enabled, and [minikube](https://minikube.sigs.k8s.io/docs/).

!!! note "Rancher Desktop"
    Rancher Desktop is compatible with Linux, MacOS, and Windows (using WSL). It also supports the [docker container runtime](https://docs.rancherdesktop.io/preferences#container-runtime), which provides support for running docker commands without installing individual docker components or Docker Desktop.  

For running Docker Compose deployments of single products, any Docker Desktop installation or Linux system with Docker and `docker compose` installed can be used.
-->

## ローカルのランタイム環境

最初の例では、多くの構成が必要ないため、Docker Desktop で Kubernetes を使用します。

一般的な運用環境のインストールに最も近い方法で Ping 製品を試すには、Kubernetes 環境の使用を検討する必要があります。 [Kind](https://kind.sigs.k8s.io/) (**K**ubernetes **in** **D**ocker) は、ローカルの Kubernetes 開発を開始するためのプラットフォームを提供します。 Kind クラスターのセットアップ手順については、[こちら](../deployment/deployLocalK8sCluster.md)を参照してください。

他のローカル Kubernetes 環境には、[Rancher Desktop](https://rancherdesktop.io)、Kubernetes が有効になっている [Docker Desktop](https://www.docker.com/products/docker-desktop/)、[minikube](https://minikube.sigs.k8s.io/docs/) などがあります。

!!! note "Rancher Desktop"
    Rancher Desktop は、Linux、MacOS、および Windows (WSL を使用) と互換性があります。また、[docker コンテナー ランタイム](https://docs.rancherdesktop.io/preferences#container-runtime)もサポートしており、個々の docker コンポーネントや Docker Desktop をインストールせずに docker コマンドを実行するためのサポートを提供します。

単一製品の Docker Compose デプロイメントを実行するには、任意の Docker デスクトップ インストール、または Docker と `docker compose` がインストールされた Linux システムを使用できます。

<!--
## Applications / Utilities
* [Helm](https://helm.sh/docs/intro/install/) cli
* [kubectl](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
* [Homebrew](https://brew.sh) for package installation and management.  Homebrew can be used to install k9s, kubectl, helm, and other programs.
   ```sh
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
* [pingctl](../tools/pingctlUtil.md#installation)
   ```sh
   brew install pingidentity/tap/pingctl
   ```
## Recommended Additional Utilities

* [k9s](https://k9scli.io/)
    ```sh
    brew install derailed/k9s/k9s
    ```
* [kubectx](https://github.com/ahmetb/kubectx)
    ```sh
    brew install kubectx
    ```
* [docker-compose](https://docs.docker.com/compose/install/)
    ```sh
    brew install docker-compose
    ```

    !!! info docker-compose installation note
          Installing docker-compose is only necessary to deploy Docker containers when using Docker with Rancher Desktop. It is included with the Docker Desktop installation.
 See [Rancher preferences](https://docs.rancherdesktop.io/preferences#container-runtime) to switch from containerd to dockerd (moby).
-->

## Applications / Utilities

* [Helm](https://helm.sh/docs/intro/install/) cli
* [kubectl](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
* パッケージのインストールと管理用の [Homebrew](https://brew.sh)。 Homebrew は、k9s、kubectl、helm、およびその他のプログラムのインストールに使用できます。

   ```sh
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
* [pingctl](../tools/pingctlUtil.md#installation)
   ```sh
   brew install pingidentity/tap/pingctl
   ```

## 推奨される追加ユーティリティ

* [k9s](https://k9scli.io/)
    ```sh
    brew install derailed/k9s/k9s
    ```
* [kubectx](https://github.com/ahmetb/kubectx)
    ```sh
    brew install kubectx
    ```
* [docker-compose](https://docs.docker.com/compose/install/)
    ```sh
    brew install docker-compose
    ```

    !!! info "docker-compose インストールの注意事項" 
          docker-compose のインストールは、Rancher Desktop で Docker を使用する場合に Docker コンテナをデプロイする場合にのみ必要です。これは Docker Desktop インストールに含まれています。

 containerd から dockerd (moby) に切り替えるには、[Rancher の設定](https://docs.rancherdesktop.io/preferences#container-runtime)を参照してください。

<!--
## Configure the Environment

1. Open a terminal and create a local DevOps directory named `${HOME}/projects/devops`.

    !!! info Parent Directory
        ${HOME}/projects/devops is the parent directory for all examples referenced in our documentation.

2. Configure the environment as follows.

      ```sh
      pingctl config
      ```

      1. Respond to all configuration questions, accepting the defaults if uncertain. Settings for custom variables aren't needed initially but may be necessary for additional capabilities.
   
      2. All responses are captured in your local `~/.pingidentity/config` file. Allow the configuration script to source this file in your shell profile (for example, `~/.bash_profile` in a bash shell).
   
3. [Optional] Export configured pingctl variables as environment variables

      1. Modify your shell profile (for example, `~/.bash_profile` in a bash shell) so that the generated `source ~/.pingidentity/config` command is surrounded by `set -a` and `set +a` statements.

      ```sh
      set -a
      # Ping Identity - Added with 'pingctl config' on Fri Apr 22 13:57:04 MDT 2022
      test -f '${HOME}/.pingidentity/config' && source '${HOME}/.pingidentity/config'
      set +a
      ```

      2. Verify configured variables are exported in your environment.

            1. Restart your shell or source your shell profile.

            2. Run `env | grep 'PING'`

4. To display your environment settings, run:

      ```sh
      pingctl info
      ```

5. For more information on the options available for ```pingctl``` see [Configuration & Environment Variables](configVars.md).
-->

## 環境を構成する

1. ターミナルを開き、`${HOME}/projects/devops` という名前のローカル DevOps ディレクトリを作成します。

    !!! info "Parent Directory"
        `${HOME}/projects/devops` は、ドキュメントで参照されているすべての例の親ディレクトリです。

2. 以下のように環境を構築する

      ```sh
      pingctl config
      ```

      1. 設定に関するすべての質問に回答し、不明な場合はデフォルトを受け入れます。カスタム変数の設定は最初は必要ありませんが、機能を追加するために必要になる場合があります。
   
      2. すべての応答はローカルの `~/.pingidentity/config` ファイルにキャプチャされます。構成スクリプトがシェル プロファイルでこのファイルをソースできるようにします (たとえば、bash シェルの ~/.bash_profile)。
   
3. [オプション] 構成された pingctl 変数を環境変数としてエクスポートする

      1. シェル プロファイル (たとえば、bash シェルの `~/.bash_profile`) を変更して、生成された `source ~/.pingidentity/config` コマンドが `set -a` ステートメントと `set +a` ステートメントで囲まれるようにします。

      ```sh
      set -a
      # Ping Identity - Added with 'pingctl config' on Fri Apr 22 13:57:04 MDT 2022
      test -f '${HOME}/.pingidentity/config' && source '${HOME}/.pingidentity/config'
      set +a
      ```

      2. 構成された変数が環境にエクスポートされていることを確認します。

            1. シェルを再起動するか、sourceでシェル プロファイルを取得します。

            2. `env | grep 'PING'` を実行する。

4. 環境設定を表示するには、次を実行します。

      ```sh
      pingctl info
      ```

5. `pingctl` で使用できるオプションの詳細については、[構成変数と環境変数](configVars.md)を参照してください。
