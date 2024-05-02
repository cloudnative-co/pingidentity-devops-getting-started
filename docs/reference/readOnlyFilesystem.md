---
title: Running Ping product images with a read-only root filesystem requirement
---
<!--
# Running product containers with a read-only root filesystem

## Overview

In some environments, there is a requirement that the container filesystem be read-only.  Our product images are maturing to support this capability natively in the future.  In the meantime, this guide will explain the overall concepts and provide an example with PingDirectory.  The other product images can operate in a similar manner.  

!!! warning "Example only"
    This guide is intended to provide an example implementation of solving this problem; your situation might require a different approach.

!!! note "Ping image exploration"
    An excellent starting point for understanding what goes on with our containers as they are instantiated can be found in [this video](https://videos.pingidentity.com/detail/videos/devops/video/6314748082112/ping-product-docker-image-exploration).  It is highly recommended that you take the time to view it prior to working through this guide.
-->

# 読み取り専用のルートファイルシステムで製品コンテナを実行する

## 概要

一部の環境では、コンテナ ファイル システムが読み取り専用である必要があります。当社の製品イメージは、将来的にこの機能をネイティブにサポートできるよう成熟しつつあります。それまでの間、このガイドでは全体的な概念を説明し、PingDirectory の例を示します。他の製品画像も同様に操作できます。

!!! warning "例のみ"
    このガイドは、この問題を解決するための実装例を提供することを目的としています。状況によっては別のアプローチが必要になる場合があります。

!!! note "Ping イメージ探索"
    コンテナーがインスタンス化されるときにコンテナーで何が起こっているかを理解するための優れた出発点は、[このビデオ](https://videos.pingidentity.com/detail/videos/devops/video/6314748082112/ping-product-docker-image-exploration)にあります。このガイドを読み進める前に、時間をかけて参照することを強くお勧めします。

<!--
### High-level process

In Ping product containers, the layered approach of bringing the environment and configuration parameters into the container at launch requires merging of files from server profiles and possibly other locations before the product is launched.  This process means that files are modified at runtime, and a read-only root filesystem blocks this action.  The overall approach is to use [emptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) volumes to overlay the directories that need modification, allowing the hook scripts to run as normal against the volume rather than the container filesystem.  In order to get everything necessary for the scripts in place, an init container (using the same image as the product container) is launched and the files necessary are copied to the shared volume before starting the product container.
-->

### 高度なプロセス

Ping 製品コンテナでは、起動時に環境と構成パラメータをコンテナに取り込む多層アプローチでは、製品の起動前にサーバー プロファイルや場合によっては他の場所からファイルをマージする必要があります。このプロセスは、ファイルが実行時に変更されることを意味し、読み取り専用のルート ファイルシステムがこのアクションをブロックします。全体的なアプローチは、[emptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) ボリュームを使用して変更が必要なディレクトリをオーバーレイし、フック スクリプトがコンテナ ファイル システムではなくボリュームに対して通常どおり実行できるようにすることです。スクリプトに必要なものをすべて適切に配置するために、製品コンテナを開始する前に、init コンテナ (製品コンテナと同じイメージを使用) が起動され、必要なファイルが共有ボリュームにコピーされます。

<!--
## Prerequisites

- [kustomize](https://kubectl.docs.kubernetes.io/installation/kustomize/) CLI utility to serve as a [post-renderer](https://helm.sh/docs/topics/advanced/#post-rendering) for Helm.
-->

## 前提条件

- [kustomize](https://kubectl.docs.kubernetes.io/installation/kustomize/) CLI ユーティリティを使用して、Helm の[post-renderer](https://helm.sh/docs/topics/advanced/#post-rendering)として機能します。


<!--
## File explanation

In the **30-helm/read-only-filesystem** folder of the [Getting Started repository](https://github.com/pingidentity/pingidentity-devops-getting-started) is a values file and kustomize directory.  First, we will explore the the `pd-values.yaml` file; inline comments explain what is going on:

<details>
  <summary>pd-values.yaml</summary>

```yaml
initContainers:
  pd-init:
    name: runtime-init
    # CHANGEMETAG TO VERSION NEEDED
    # Init container uses the same image as the product container and therefore versions much match
    image: pingidentity/pingdirectory:CHANGEMETAG
    env:
      # Override the startup command so the product is not launched in the init container
      - name: STARTUP_COMMAND
        value: "ls"
      # Use a name different from /opt/staging for holding the copied files from the product image into the emptyDir volume
      - name: STAGING_DIR
        value: "/opt/handoff"
      # Just in case there is a .env we will need
      - name: CONTAINER_ENV
        value: "/opt/handoff/.env"
      # Another flag for preventing the product from being launched
      - name: STARTUP_FOREGROUND_OPTS
        value: ""
    envFrom:
      # CHANGEMERELEASE TO MATCH HELM RELEASE NAME
      - configMapRef:
          name: CHANGEMERELEASE-global-env-vars
          optional: true
      - configMapRef:
          name: CHANGEMERELEASE-env-vars
          optional: true
      - configMapRef:
          name: CHANGEMERELEASE-pingdirectory-env-vars
      - secretRef:
          name: devops-secret
          optional: true
      - secretRef:
          name: CHANGEME-pingdirectory-git-secret
          optional: true
    volumeMounts:
      # emptyDir volume: /opt/staging will be copied from the init container to this volume
      # This volume will be mounted as /opt/staging in the product container
      - mountPath: /opt/handoff
        name: staging
        readOnly: false
      # The location for the license file varies by product
      # See https://devops.pingidentity.com/how-to/existingLicense/ for more information
      # The license file is required for the init container to operate
      - name: pingdirectory-license
        mountPath: "/opt/staging/pd.profile/server-root/pre-setup/PingDirectory.lic"
        subPath: PingDirectory.lic
      # Also an emptyDir
      - name: tmp
        mountPath: "/tmp"
        readOnly: false
      # Also an emptyDir
      - name: init-runtime
        mountPath: "/opt/out"
        readOnly: false
      # Mount the slightly modified versions of the bootstrap and start sequence scripts (see below)
      - mountPath: /opt/bootstrap.sh
        name: bootstrap
        readOnly: true
        subPath: bootstrap.sh
        defaultMode: 0555
      - mountPath: /opt/staging/hooks/10-start-sequence.sh
        name: init-start
        readOnly: true
        subPath: 10-start-sequence.sh
        defaultMode: 0555

volumes:
  # The 3 emptyDir volumes referenced above
  init-runtime:
    emptyDir: {}
  staging:
    emptyDir: {}
  tmp:
    emptyDir: {}
  # This secret is created from a license file
  pingdirectory-license:
    secret:
      secretName: pingdirectory-license
  # Make the modified bootstrap and start sequence scripts available as configMaps
  bootstrap:
    configMap:
      items:
      - key: bootstrap.sh
        path: bootstrap.sh
      name: bootstrap
  init-start:
    configMap:
      items:
      - key: 10-start-sequence.sh
        path: 10-start-sequence.sh
      name: init-start

configMaps:
  init-start:
    data:
      10-start-sequence.sh: |-
        #!/usr/bin/env sh
        echo "overwriting 10 hook"
        #!/usr/bin/env sh
        #
        # Ping Identity DevOps - Docker Build Hooks
        #
        # Called when it has been determined that this is the first time the container has
        # been run.
        #

        ##############################################################################
        ####### Prevent init container from starting the product normally.  ##########
        ####### These two lines are the only delta from the default script. ##########
        ##############################################################################
        if test ${STARTUP_FOREGROUND_OPTS} != "" ; then
          test "${VERBOSE}" = "true" && set -x

          # shellcheck source=./pingcommon.lib.sh
          . "${HOOKS_DIR}/pingcommon.lib.sh"

          echo "Initializing server for the first time"

          run_hook "17-check-license.sh"

          run_hook "18-setup-sequence.sh"
        fi
  bootstrap:
    data:
      bootstrap.sh: |-
        #!/usr/bin/env sh
        ######################################################################################################
        ####### Make a copy of everything under /opt/staging in the product image to /opt/handoff.  ##########
        ####### Primarily, this makes the hook scripts available in the emptyDir (writable) volume. ##########
        ####### This line is the only delta from the default script.                                ##########
        ######################################################################################################
        cp -r /opt/staging/* /opt/handoff
        test "${VERBOSE}" = "true" && set -x
        # shellcheck source=./staging/hooks/pingcommon.lib.sh
        . "${HOOKS_DIR}/pingcommon.lib.sh"

        _userID=$(id -u)
        _groupID=$(id -g)

        echo "### Bootstrap"
        if test "${_userID}" -eq 0; then
            echo_yellow "### Warning: running container as root user"
        else
            echo "### Using the default container user and group"

            _effectiveGroupName=$(awk 'BEGIN{FS=":"}$3~/^'"${_groupID}"'$/{print $1}' /etc/group)
            test -z "${_effectiveGroupName}" && _effectiveGroupName="undefined group"

            _effectiveUserName=$(awk 'BEGIN{FS=":"}$3~/^'"${_userID}"'$/{print $1}' /etc/passwd)
            test -z "${_effectiveUserName}" && _effectiveUserName="undefined user"

            echo "### Container user and group"
            echo "###     user : ${_effectiveUserName} (id: ${_userID})"
            echo "###     group: ${_effectiveGroupName} (id: ${_groupID})"
        fi

        # if the current process id is not 1, tini needs to register as sub-reaper
        if test $$ -ne 1; then
            _subReaper="-s"
        fi

        # shellcheck disable=SC2086,SC2048
        exec "${BASE}/tini" ${_subReaper} -- "${BASE}/entrypoint.sh" ${*}

pingdirectory:
  enabled: true
  envs:
    MUTE_LICENSE_VERIFICATION: "yes"
    ORCHESTRATION_TYPE: "NONE"
    VERBOSE: "true"
 # (Optional) Specify a particular tag by uncommenting these two lines and naming the tag to use.
 # Otherwise, you will get the latest from Docker Hub.
 # If a particular tag is used, be sure the init container tag matches above
 # image:
 #   tag: "2306"
  includeInitContainers:
  # Use the init container specification above at pod startup
    - pd-init
  # Share the volumes between the init container and the product container
  includeVolumes:
    - staging
    - tmp
    - pingdirectory-license
    - bootstrap
    - init-start
    - init-runtime
  volumeMounts:
    # The emptyDir mounted at /opt/handoff in the init container is mounted to /opt/staging here
    # Hook scripts and product startup will operate as with a read/write filesystem
    - mountPath: /opt/staging
      name: staging
      readOnly: false
    - name: pingdirectory-license
      mountPath: "/opt/staging/pd.profile/server-root/pre-setup/PingDirectory.lic"
      subPath: PingDirectory.lic
    - name: tmp
      mountPath: "/tmp"
      readOnly: false
```

</details>

!!! note "Kustomize"
    **Kustomize is used as the Ping helm charts do not support setting the readOnlyFileSystem value to the securityContext of a container at this time.**

In the **30-helm/read-only-filesystem/kustomize** subdirectory is a kustomize script and definition file.

The script simply runs kustomize:
```bash
#!/bin/bash

cat <&0 > kustomize/all.yaml

kustomize build kustomize && rm kustomize/all.yaml
```

The `kustomization.yaml` file sets the **securityContext** for the each container's root file system to read-only:

```yaml
resources:
  - all.yaml

patches:
  - target:
      group: apps
      version: v1
      kind: StatefulSet
    patch: |-
      - op: add
        path: /spec/template/spec/containers/0/securityContext
        value:
          readOnlyRootFilesystem: true
      - op: add
        path: /spec/template/spec/initContainers/0/securityContext
        value:
          readOnlyRootFilesystem: true
```
-->

## ファイルの説明

[Getting Started repository](https://github.com/pingidentity/pingidentity-devops-getting-started)の **30-helm/read-only-filesystem** フォルダーには、値ファイルと kustomize ディレクトリがあります。まず、`pd-values.yaml` ファイルを調べます。インラインコメントは何が起こっているかを説明します:

<details>
  <summary>pd-values.yaml</summary>

```yaml
initContainers:
  pd-init:
    name: runtime-init
    # CHANGEMETAG をバージョンに変更する
    # Init コンテナは製品コンテナと同じイメージを使用するため、バージョンはほぼ一致します。
    image: pingidentity/pingdirectory:CHANGEMETAG
    env:
      # 製品が init コンテナーで起動されないように起動コマンドをオーバーライドします。
      - name: STARTUP_COMMAND
        value: "ls"
      # 製品イメージからコピーしたファイルを emptyDir ボリュームに保持するには、/opt/staging とは異なる名前を使用します。
      - name: STAGING_DIR
        value: "/opt/handoff"
      # 念のため、.env が必要です
      - name: CONTAINER_ENV
        value: "/opt/handoff/.env"
      # 製品の起動を阻止するための別のフラグ
      - name: STARTUP_FOREGROUND_OPTS
        value: ""
    envFrom:
      # CHANGEMERELEASE をHelm リリース名に変更する
      - configMapRef:
          name: CHANGEMERELEASE-global-env-vars
          optional: true
      - configMapRef:
          name: CHANGEMERELEASE-env-vars
          optional: true
      - configMapRef:
          name: CHANGEMERELEASE-pingdirectory-env-vars
      - secretRef:
          name: devops-secret
          optional: true
      - secretRef:
          name: CHANGEME-pingdirectory-git-secret
          optional: true
    volumeMounts:
      # emptyDir volume: /opt/staging は init コンテナからこのボリュームにコピーされます
      # このボリュームは製品コンテナに /opt/staging としてマウントされます
      - mountPath: /opt/handoff
        name: staging
        readOnly: false
      # ライセンス ファイルの場所は製品によって異なります
      # 詳細については、https://devops.pingidentity.com/how-to/existingLicense/ を参照してください。
      # initコンテナが動作するにはライセンスファイルが必要です
      - name: pingdirectory-license
        mountPath: "/opt/staging/pd.profile/server-root/pre-setup/PingDirectory.lic"
        subPath: PingDirectory.lic
      # また、emptyDir
      - name: tmp
        mountPath: "/tmp"
        readOnly: false
      # また、emptyDir
      - name: init-runtime
        mountPath: "/opt/out"
        readOnly: false
      # わずかに変更されたバージョンのブートストラップをマウントし、シーケンス スクリプトを開始します (下記を参照)。
      - mountPath: /opt/bootstrap.sh
        name: bootstrap
        readOnly: true
        subPath: bootstrap.sh
        defaultMode: 0555
      - mountPath: /opt/staging/hooks/10-start-sequence.sh
        name: init-start
        readOnly: true
        subPath: 10-start-sequence.sh
        defaultMode: 0555

volumes:
  # 上記で参照した 3 つの emptyDir ボリューム
  init-runtime:
    emptyDir: {}
  staging:
    emptyDir: {}
  tmp:
    emptyDir: {}
  # このシークレットはライセンス ファイルから作成されます
  pingdirectory-license:
    secret:
      secretName: pingdirectory-license
  # 変更したブートストラップおよび開始シーケンス スクリプトを configMaps として利用できるようにします。
  bootstrap:
    configMap:
      items:
      - key: bootstrap.sh
        path: bootstrap.sh
      name: bootstrap
  init-start:
    configMap:
      items:
      - key: 10-start-sequence.sh
        path: 10-start-sequence.sh
      name: init-start

configMaps:
  init-start:
    data:
      10-start-sequence.sh: |-
        #!/usr/bin/env sh
        echo "overwriting 10 hook"
        #!/usr/bin/env sh
        #
        # Ping Identity DevOps - Docker Build Hooks
        #
        # Called when it has been determined that this is the first time the container has
        # been run.
        #

        ##############################################################################
        ####### Prevent init container from starting the product normally.  ##########
        ####### These two lines are the only delta from the default script. ##########
        ##############################################################################
        if test ${STARTUP_FOREGROUND_OPTS} != "" ; then
          test "${VERBOSE}" = "true" && set -x

          # shellcheck source=./pingcommon.lib.sh
          . "${HOOKS_DIR}/pingcommon.lib.sh"

          echo "Initializing server for the first time"

          run_hook "17-check-license.sh"

          run_hook "18-setup-sequence.sh"
        fi
  bootstrap:
    data:
      bootstrap.sh: |-
        #!/usr/bin/env sh
        ######################################################################################################
        ####### Make a copy of everything under /opt/staging in the product image to /opt/handoff.  ##########
        ####### Primarily, this makes the hook scripts available in the emptyDir (writable) volume. ##########
        ####### This line is the only delta from the default script.                                ##########
        ######################################################################################################
        cp -r /opt/staging/* /opt/handoff
        test "${VERBOSE}" = "true" && set -x
        # shellcheck source=./staging/hooks/pingcommon.lib.sh
        . "${HOOKS_DIR}/pingcommon.lib.sh"

        _userID=$(id -u)
        _groupID=$(id -g)

        echo "### Bootstrap"
        if test "${_userID}" -eq 0; then
            echo_yellow "### Warning: running container as root user"
        else
            echo "### Using the default container user and group"

            _effectiveGroupName=$(awk 'BEGIN{FS=":"}$3~/^'"${_groupID}"'$/{print $1}' /etc/group)
            test -z "${_effectiveGroupName}" && _effectiveGroupName="undefined group"

            _effectiveUserName=$(awk 'BEGIN{FS=":"}$3~/^'"${_userID}"'$/{print $1}' /etc/passwd)
            test -z "${_effectiveUserName}" && _effectiveUserName="undefined user"

            echo "### Container user and group"
            echo "###     user : ${_effectiveUserName} (id: ${_userID})"
            echo "###     group: ${_effectiveGroupName} (id: ${_groupID})"
        fi

        # if the current process id is not 1, tini needs to register as sub-reaper
        if test $$ -ne 1; then
            _subReaper="-s"
        fi

        # shellcheck disable=SC2086,SC2048
        exec "${BASE}/tini" ${_subReaper} -- "${BASE}/entrypoint.sh" ${*}

pingdirectory:
  enabled: true
  envs:
    MUTE_LICENSE_VERIFICATION: "yes"
    ORCHESTRATION_TYPE: "NONE"
    VERBOSE: "true"
 # (オプション) これら 2 行のコメントを解除し、使用するタグに名前を付けることで、特定のタグを指定します。
 # それ以外の場合は、Docker Hub から最新のものを入手します。
 # 特定のタグが使用されている場合は、init コンテナー タグが上記と一致していることを確認してください。
 # image:
 #   tag: "2306"
  includeInitContainers:
  # ポッドの起動時に上記の init コンテナ仕様を使用します
    - pd-init
  # 初期コンテナと製品コンテナの間でボリュームを共有する
  includeVolumes:
    - staging
    - tmp
    - pingdirectory-license
    - bootstrap
    - init-start
    - init-runtime
  volumeMounts:
    # initコンテナの/opt/handoffにマウントされたemptyDirは、ここでは/opt/stagingにマウントされます。
    # フック スクリプトと製品の起動は、読み取り/書き込みファイル システムと同様に動作します。
    - mountPath: /opt/staging
      name: staging
      readOnly: false
    - name: pingdirectory-license
      mountPath: "/opt/staging/pd.profile/server-root/pre-setup/PingDirectory.lic"
      subPath: PingDirectory.lic
    - name: tmp
      mountPath: "/tmp"
      readOnly: false
```

</details>

!!! note "Kustomize"
    **現時点では、Ping Helm チャートがコンテナーの securityContext への readOnlyFileSystem 値の設定をサポートしていないため、Kustomize が使用されます。**

**30-helm/read-only-filesystem/kustomize** サブディレクトリには、kustomize スクリプトと定義ファイルがあります。

スクリプトは単純に kustomize を実行します。

```bash
#!/bin/bash

cat <&0 > kustomize/all.yaml

kustomize build kustomize && rm kustomize/all.yaml
```

The `kustomization.yaml` file sets the **securityContext** for the each container's root file system to read-only:

```yaml
resources:
  - all.yaml

patches:
  - target:
      group: apps
      version: v1
      kind: StatefulSet
    patch: |-
      - op: add
        path: /spec/template/spec/containers/0/securityContext
        value:
          readOnlyRootFilesystem: true
      - op: add
        path: /spec/template/spec/initContainers/0/securityContext
        value:
          readOnlyRootFilesystem: true
```

<!--
## Process

To use the example files to deploy PingDirectory with a read-only root filesystem, follow the steps here:

1.  Generate a license file and create a secret.  If you have an existing license file, you can use it here:

    ```bash
    # Generate the license file
    pingctl license pingdirectory 9.2 > PingDirectory.lic

    # Create the secret to match the name in the values file:
    kubectl create secret generic pingdirectory-license --from-file=./PingDirectory.lic
    ```

1.  Update the **`30-helm/read-only-filesystem/pd-values.yaml`** file with the appropriate image tag and release name to be used with Helm.  Afterward, use Helm to deploy the release:

    ```bash
    # For this example, the release name of 'rofs' is used
    cd 30-helm/read-only-filesystem
    helm upgrade --install rofs pingidentity/ping-devops -f './pd-values.yaml' \
          --post-renderer kustomize/kustomize
    ```
-->

## プロセス

サンプル ファイルを使用して、読み取り専用のルート ファイル システムで PingDirectory を展開するには、次の手順に従います。

1. ライセンス ファイルを生成し、シークレットを作成します。既存のライセンス ファイルがある場合は、ここでそれを使用できます。

    ```bash
    # Generate the license file
    pingctl license pingdirectory 9.2 > PingDirectory.lic

    # Create the secret to match the name in the values file:
    kubectl create secret generic pingdirectory-license --from-file=./PingDirectory.lic
    ```

1. Helm で使用する適切なイメージ タグとリリース名を使用して **`30-helm/read-only-filesystem/pd-values.yaml`** ファイルを更新します。その後、Helm を使用してリリースをデプロイします。

    ```bash
    # For this example, the release name of 'rofs' is used
    cd 30-helm/read-only-filesystem
    helm upgrade --install rofs pingidentity/ping-devops -f './pd-values.yaml' \
          --post-renderer kustomize/kustomize
    ```

<!--
## Diagram

This image provides an overview of what happens.  It is best viewed in a separate tab:

![Read-Only Root Filesystem Example](../images/readOnlyFileSystem.png)

!!! note "/etc/motd"
    One issue we encountered is that the **`/etc/motd`** file can be modified at startup by hook scripts, but /etc/ is read-only.  We are exploring ways to address this in some way, but at this time, it appears one possible solution is to treat `/etc/` in the same manner as /opt/staging (copying to an emptyDir) if the `motd` file is to be updated.  However, `/etc` has many more files and directories and such a solution is not practical.  Baking it into a custom image is another possibility.
-->

## 図式

この画像は、何が起こるかの概要を示しています。別のタブで表示するのが最適です。

![Read-Only Root Filesystem Example](../images/readOnlyFileSystem-ja.png)

!!! note "/etc/motd"
    私たちが遭遇した問題の 1 つは、**`/etc/motd`** ファイルは起動時にフック スクリプトによって変更できるが、/etc/ は読み取り専用であることです。 私たちはこれに何らかの方法で対処する方法を模索していますが、現時点で考えられる解決策の 1 つは、`motd` ファイルを更新する場合に `/etc/` を /opt/staging (emptyDir にコピーする) と同じ方法で扱うことであるようです。 ただし、`/etc` にはさらに多くのファイルとディレクトリがあり、そのような解決策は現実的ではありません。 それをカスタム イメージにベイクすることも可能です。
