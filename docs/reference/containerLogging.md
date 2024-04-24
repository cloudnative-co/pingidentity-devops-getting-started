---
title: Container Logging
---

<!--
# Container Logging

This document provides an outline of how logging is handled in containerized environments.  Please refer to the provided links at the end of this page for details on implementing a logging solution for your deployments.

!!! info "Splunk Example"
    While providing examples for all logging solutions is impractical, there is an example for using Splunk on this portal [here](../how-to/splunkLogging.md).

-->

# コンテナのロギング

このドキュメントでは、コンテナ化された環境でロギングがどのように処理されるかについて概要を説明します。導入環境にログ ソリューションを実装する方法の詳細については、このページの最後にあるリンクを参照してください。

!!! info "Splunkの例"
    すべてのロギング ソリューションの例を提供することは現実的ではありませんが、このポータルでの Splunk の使用例が[ここ](../how-to/splunkLogging.md)にあります。

## 課題ステートメント

コンテナ化されたデプロイメント モデルでは、コンテナ (または Kubernetes のポッド) が一時的なものになることが予想されます。さらに、コンテナーでのアプリケーションのログ記録の標準的な方法では、ストリーミング ログの手段として `stdout` を使用し、場合によっては `stderr` を使用します。 Ping 製品コンテナはこの慣行に従っています。その結果、コンテナーまたはポッドのライフサイクルの外にログが保持されることはありません。特に、構成ミスやエラーが原因でポッドが失敗しているかクラッシュループに陥っている場合、ポッドが再起動しようとするたびにクラッシュに関する情報を提供する可能性のあるログが失われるため、原因をトラブルシューティングすることは不可能です。**したがって、ログがコンテナの外部に保存されるようにすることが重要です。**

!!! error "見逃した場合に備えて"
    クラッシュなどの何らかの理由でコンテナが停止した場合、他の場所に保存されていないコンテナのログ情報はすべて失われます。

<!--
## Viewing logs

In a Kubernetes deployment, you can view the streaming logs (stdout/stderr) of a container in a pod by issuing the `kubectl logs` command.  This function is useful for quickly examining logs from operational containers.
-->

## ログの表示

Kubernetes デプロイメントでは、`kubectl logs` コマンドを発行することで、ポッド内のコンテナーのストリーミング ログ (stdout/stderr) を表示できます。この機能は、運用中のコンテナのログを素早く調べるのに役立ちます。

<!--
## Persisting logs

Because you cannot rely on the logs from the container itself for long-term use, you must implement some means of storing the logging information apart from the container itself.
-->

## ログの永続化

長期間使用する場合はコンテナー自体のログに依存できないため、コンテナー自体とは別にログ情報を保存する何らかの手段を実装する必要があります。

<!--
## Logging sidecar

In the Kubernetes model, a common method of maintaining logs is to use the [sidecar model](../deployment/deployK8sUtilitySidecar.md). A logging sidecar is included in the pod and configured to grab the stdout/stderr streams from the application container and persist them to the logging service. Many vendors provide a Docker image for this sidecar that contains the agent for their product. In addition, they usually provide support for configuring the container to connect to their service and format the logs for consumption, such as through environment variables, Kubernetes ConfigMaps, or other means.

Advantages:

- Logs can be sent to different locations at the same time using multiple sidecars
- Access to the cluster node is not required - particularly useful for hosted Kubernetes environments
- No update to the application is required, assuming it dumps logging information to stdout/stderr

Disadvantage:

- Additional resources are required for running the extra container(s), though they tend to be lightweight
-->

## ロギングサイドカー

Kubernetes モデルでは、ログを維持する一般的な方法は[sidecar model](../deployment/deployK8sUtilitySidecar.md)を使用することです。ロギング サイドカーはポッドに含まれており、アプリケーション コンテナから stdout/stderr ストリームを取得してロギング サービスに永続化するように構成されています。多くのベンダーは、自社製品のエージェントを含むこのサイドカー用の Docker イメージを提供しています。さらに、通常は、環境変数、Kubernetes ConfigMap、またはその他の手段を介して、サービスに接続し、消費するためにログをフォーマットするためのコンテナーの構成のサポートも提供します。

利点:

- 複数のサイドカーを使用してログを異なる場所に同時に送信できます
- クラスター ノードへのアクセスは必要ありません。ホストされた Kubernetes 環境では特に便利です。
- ログ情報を stdout/stderr にダンプする場合、アプリケーションを更新する必要はありません。

欠点:

- 追加のコンテナを実行するには追加のリソースが必要ですが、コンテナは軽量である傾向があります。 

<!--
## The TAIL_LOG_FILES environment variable

Many Ping products were designed and built for a server-deployed implementation.  As a result, they write log information to files (the old model for logging), rather than to `stdout`.  To ease containerization, an environment variable (**`TAIL_LOG_FILES`**) is included in the Docker images and this variable is fed to a function that streams these files to `stdout` as they are written.

While Ping includes key log files as defaults, this variable can be modified.  You can add additional log files to this variable to include them in the `stdout` stream.  See [each product Dockerfile](https://github.com/pingidentity/pingidentity-docker-builds) for the default value of this variable for the product in question.
-->

## TAIL_LOG_FILES 環境変数

多くの Ping 製品は、サーバー配備の実装向けに設計および構築されています。その結果、ログ情報は標準出力ではなくファイル (古いログ モデル) に書き込まれます。コンテナ化を容易にするために、環境変数 (**`TAIL_LOG_FILES`**) が Docker イメージに含まれており、この変数は、これらのファイルが書き込まれるときに `stdout` にストリーミングする関数に供給されます。

Ping にはデフォルトとして主要なログ ファイルが含まれていますが、この変数は変更できます。この変数に追加のログ ファイルを追加して、`stdout` ストリームに含めることができます。当該製品のこの変数のデフォルト値については、[各製品のDockerfile](https://github.com/pingidentity/pingidentity-docker-builds) を参照してください。

<!--
## References

The list below is not intended to be comprehensive but should provide a good starting point for understanding how logging works and what you can do to retain logs from your deployments.  

!!! note "Examples only"
    Any vendor listed here should not be considered an endorsement or recommendation by Ping Identity for that service. Refer to the documentation for the image in question for further assistance.

- [Kubernetes Logging Documentation](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
- [Docker Logging Documentation](https://docs.docker.com/config/containers/logging/)
- Docker Hub images, listed alphabetically:
    - [AWS Cloudwatch](https://hub.docker.com/r/amazon/cloudwatch-agent)
    - [Datadog](https://hub.docker.com/r/datadog/agent)
    - [Fluentd](https://hub.docker.com/_/fluentd)
    - [Graylog](https://hub.docker.com/u/graylog)
    - [Rsyslog](https://hub.docker.com/u/rsyslog)
    - [Sematext](https://hub.docker.com/u/sematext)
    - [Splunk Forwarder](https://hub.docker.com/r/splunk/universalforwarder/)
    - [Sumologic](https://hub.docker.com/r/sumologic/collector)
-->

## 参考文献

以下のリストは包括的なものではありませんが、ロギングの仕組みと、展開からのログを保持するために何ができるかを理解するための良い出発点となります。

!!! note "例のみ"
    ここにリストされているベンダーは、Ping Identity によるそのサービスの承認または推奨とみなされるべきではありません。詳細については、問題のイメージのドキュメントを参照してください。

- [Kubernetes Logging Documentation](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
- [Docker Logging Documentation](https://docs.docker.com/config/containers/logging/)
- Docker Hub images, listed alphabetically:
    - [AWS Cloudwatch](https://hub.docker.com/r/amazon/cloudwatch-agent)
    - [Datadog](https://hub.docker.com/r/datadog/agent)
    - [Fluentd](https://hub.docker.com/_/fluentd)
    - [Graylog](https://hub.docker.com/u/graylog)
    - [Rsyslog](https://hub.docker.com/u/rsyslog)
    - [Sematext](https://hub.docker.com/u/sematext)
    - [Splunk Forwarder](https://hub.docker.com/r/splunk/universalforwarder/)
    - [Sumologic](https://hub.docker.com/r/sumologic/collector)
