---
title: FAQs
---

<!--
# Frequently asked questions
-->

# よくある質問と答え

<!--
### AWS

<details>
  <summary>What storage option should I use for container volumes on EKS?</summary>

Ping recommends the use of EBS volumes for container volumes on EKS.  EFS is not supported. For more information, please visit <a href="https://devops.pingidentity.com/reference/awsStorage/">AWS Storage Considerations</a>.
</details>
-->

### AWS

<details>
  <summary>EKS 上のコンテナ ボリュームにはどのストレージ オプションを使用する必要がありますか?</summary>

Ping では、EKS 上のコンテナー ボリュームとして EBS ボリュームの使用を推奨します。 EFSはサポートされていません。詳細については、<a href="https://devops.pingidentity.com/reference/awsStorage/">AWS ストレージに関する考慮事項</a>を参照してください。
</details>

<!--
### Docker Images

<details>
  <summary>I see Ping product container images hosted in Iron Bank. What are the differences between these images and those found on Docker Hub?</summary>

<a href="https://docs-ironbank.dso.mil/">Iron Bank</a> is a container image repository intended to host images for those environments requiring additional security, such as for FedRAMP certification and similar situations. Ping does not build these images, but rather they are created by a Ping partner. These images contain the same product code as found on Docker Hub; however, the OS and JDK used in building the container images are chosen by the partner as per their requirements.  You can have full confidence in these images.  If you encounter a problem related to an image provided through Iron Bank, you can open a ticket through your normal Ping support channels, indicating that it is an Iron Bank image in question.
</details>


<details>
  <summary>What OS and Java versions are included in Ping Docker images?</summary>

The operating system (OS) shims used for our images are Alpine and Red Hat UBI.  The UBI-based images are intended for Openshift deployments, while Alpine should be used in most other situations. For more information on the choice of Alpine, please visit <a href="https://devops.pingidentity.com/docker-images/imageSupport/#supported-os-shim">Supported OS Shim</a>.  The Java version currently included in our images is OpenJDK 17 and the distribution used is <a href="https://bell-sw.com/libericajdk/">BellSoft Liberica.</a>
</details>

<details>
  <summary>When are new Ping product Docker images released?</summary>

Typically, Docker images are released on a monthly basis during the first full week of the month.  The images are tagged YYMM, with the month indicating the complete month prior.  So, tag "2303", representing the work from March 2023, would be released in early April.  As we mature our processes, the frequency and timing of these images will more closely align with product releases.
</details>

<details>
  <summary>How can I be informed when new images are available?</summary>

You can watch the <a href="https://github.com/pingidentity/pingidentity-docker-builds/">docker-builds GitHub repository</a> for the Ping Identity product line. Select the "custom" option to receive notification when a release occurs.  Releases in the docker-builds repository correspond to the publishing of images in Docker Hub.
</details>

<details>
  <summary>What are the latest Ping product versions available as Docker images?</summary>

The latest Ping product images are tagged with <mark><b>{RELEASE}-{PRODUCT VERSION}</b></mark>. You can find more information about our latest product images by consulting the <a href="https://devops.pingidentity.com/docker-images/productVersionMatrix/">Product Version matrix</a>.
</details>

<details>
  <summary>Do the images come as product only or combined with an OS layer?</summary>

The DevOps program uses <mark><b>Alpine</b></mark> as its base OS shim for all images. For more information please visit <a href="https://devops.pingidentity.com/docker-images/imageSupport/#supported-os-shim">Supported OS Shim</a>.
</details>

<details>
  <summary>I have created a custom product installation. If we require a specific image, can that be supplied by Ping?
</summary>

We do not provide custom images, but you are welcome to build the image locally with your customized bits. For more information, see <a href="https://devops.pingidentity.com/how-to/buildLocal/">Build Local Images</a>.<br>

It is important to note using a custom image might affect support options and timing.
</details>
-->

### Docker Images

<details>
  <summary>Ping Docker イメージにはどのような OS と Java のバージョンが含まれていますか?</summary>

イメージに使用されているオペレーティング システム (OS) シムは、Alpine と Red Hat UBI です。 UBI ベースのイメージは Openshift 導入を目的としていますが、Alpine は他のほとんどの状況で使用する必要があります。 Alpine の選択の詳細については、<a href="https://devops.pingidentity.com/docker-images/imageSupport/#supported-os-shim">サポートされる OS シム</a>を参照してください。現在イメージに含まれている Java バージョンは OpenJDK 11 で、使用されているディストリビューションは <a href="https://bell-sw.com/libericajdk/">BellSoft Liberica</a> です。
</details>

<details>
  <summary>新しい Ping 製品の Docker イメージはいつリリースされますか?</summary>

通常、Docker イメージは毎月、その月の最初の 1 週間にリリースされます。画像には YYMM というタグが付けられ、月は完全な前月を示します。したがって、2023 年 3 月の作品を表すタグ「2303」は、4 月上旬にリリースされることになります。プロセスが成熟するにつれて、これらのイメージの頻度とタイミングは製品リリースとより密接に一致するようになります。
</details>

<details>
  <summary>新しいイメージime-ziが利用可能になったときに通知を受け取るにはどうすればよいですか?</summary>

Ping Identity 製品ラインの <a href="https://github.com/pingidentity/pingidentity-docker-builds/">docker-builds GitHub リポジトリ</a>を確認できます。リリースが発生したときに通知を受け取るには、「カスタム」オプションを選択します。 docker-builds リポジトリのリリースは、Docker Hub でのイメージの公開に対応します。
</details>

<details>
  <summary>Docker イメージとして利用できる最新の Ping 製品バージョンは何ですか?</summary>

最新の Ping 製品イメージには <mark><b>{RELEASE}-{PRODUCT VERSION}</b></mark> のタグが付けられています。最新の製品イメージの詳細については、 <a href="https://devops.pingidentity.com/docker-images/productVersionMatrix/">Product Version matrix</a>を参照してください。
</details>

<details>
  <summary>イメージは製品としてのみ提供されますか、それとも OS レイヤーと組み合わせて提供されますか?</summary>

DevOps プログラムは、すべてのイメージのベース OS シムとして <mark><b>Alpine</b></mark> を使用します。詳細については、<a href="https://devops.pingidentity.com/docker-images/imageSupport/#supported-os-shim">サポートされている OS シム</a>を参照してください。
</details>

<details>
  <summary>カスタム製品インストールを作成しました。特定のイメージが必要な場合、Ping から提供できますか?</summary>

カスタム イメージは提供しませんが、カスタマイズしたビットを使用してローカルでイメージを構築することはできます。詳細については、<a href="https://devops.pingidentity.com/how-to/buildLocal/">Build Local Images</a>を参照してください。

カスタム イメージを使用すると、サポート オプションとタイミングに影響を与える可能性があることに注意することが重要です。
</details>

<!--
### Container Operations

<details>
  <summary>How do files move around when the container starts up?</summary>

To find out how our files are moved at start up, please visit <a href="https://devops.pingidentity.com/reference/config/#file-flowchart-example">File Flowchart</a>.
</details>

<details>
  <summary>How do I turn off the calls to the Message of the Day (MOTD)?</summary>

Set the environment variable in PingBase to: <mark><b>MOTD_URL=""</b></mark>
<p>For more information about the PingBase environment variables, please visit <a href="https://devops.pingidentity.com/docker-images/pingbase/">PingBase</a>.</p>
</details>

<details>
  <summary>How do I get more verbosity in log outputs?</summary>

Set the environment variables in PingBase to: <mark><b>VERBOSE=“true”</b></mark>
<p>For more information about the PingBase environment variables, please visit <a href="https://devops.pingidentity.com/docker-images/pingbase/">PingBase</a></p>
</details>
-->

### コンテナの運用

<details>
  <summary>コンテナーの起動時にファイルはどのように移動するのでしょうか?</summary>

起動時にファイルがどのように移動されるかを確認するには、<a href="https://devops.pingidentity.com/reference/config/#file-flowchart-example">File Flowchart</a>を参照してください。
</details>

<details>
  <summary>今日のメッセージ (MOTD) への通話をオフにするにはどうすればよいですか?</summary>
PingBase の環境変数を <mark><b>MOTD_URL=""</b></mark> に設定します。

<p>PingBase 環境変数の詳細については、<a href="https://devops.pingidentity.com/docker-images/pingbase/">PingBase</a> を参照してください。</p>
</details>

<details>
  <summary>ログ出力をより詳細にするにはどうすればよいですか?</summary>

PingBase の環境変数を <mark><b>VERBOSE="true"</b></mark> に設定します。

<p>PingBase 環境変数の詳細については、<a href="https://devops.pingidentity.com/docker-images/pingbase/">PingBase</a> を参照してください。</p>
</details>

<!--
### Orchestration / Helm / Kubernetes

<details>
  <summary>How can I be informed when a new release of the Helm charts are available?</summary>

You can watch the <a href="https://github.com/pingidentity/helm-charts/">Ping helm-charts GitHub repository</a>. Select the "custom" option to receive notification when a release occurs.  As with the product Docker images, the Helm charts are usually updated once a month.
</details>

<details>
  <summary>Kubernetes has dropped direct integration support for Docker. Does this change impact Ping product containers?</summary>

<p>No. The underlying container runtime has not caused problems with our images.  Please let us know if you encounter errors.  The <mark><b>CRI-O</b></mark> and <mark><b>containerd</b></mark> runtimes have been tested without any known issues.</p>

For more background:<br>

<br>&emsp;The Kubernetes blog post on Docker removal is <a href="https://kubernetes.io/blog/2022/02/17/dockershim-faq/">here</a>.</br>
<br>&emsp;An excellent write up of how it looks is on this  <a href="https://kodekloud.com/blog/kubernetes-removed-docker-what-happens-now/">page</a>.</br>
</details>

<details>
  <summary>My container environment is not allowed to make any external calls to services such as Github or Docker <br> Hub. Can I still use Ping Identity containers? </br> </summary>

<p>Yes. This practice is common in production scenarios. To use Ping Identity containers in this situation:</p>

<br>&emsp;1. Use an <a href="https://devops.pingidentity.com/how-to/existingLicense">Existing License</a>.</br>
<br>&emsp;2. Use an empty remote profile <mark><b>SERVER_PROFILE_URL=""</b></mark>.  Optionally, you can build your profile into the image, visit <a href="https://devops.pingidentity.com/how-to/profiles/">Server Profiles</a> for more information.</br>
<br>&emsp;3. Turn off license verification with <mark><b>MUTE_LICENSE_VERIFICATION="true"</b></mark>.</br>
<br>&emsp;4. Turn off calls to the Message of the Day (MOTD) with <mark><b>MOTD_URL=""</b></mark>.</br>
</details>

<details>
  <summary>How do we run the console and engines in a container environment?</summary>

The helm chart supports instantiating both consoles and engines.  Ingress to the consoles would have to be laid out for UI access.
<p>For more information about the Ping's Helm Charts, please visit <a href="https://helm.pingidentity.com/">Ping Helm</a></p>
</details>

<details>
  <summary>Can I use Podman instead of Docker?</summary>

Yes, just like Docker, you will be able to use Podman for container orchestration.
</details>

<details>
  <summary>Why does Ping recommand K8s vs docker?</summary>

<br>&emsp;1. Docker or a pure container solution like ECS by itself is generally not as robust or resilient as a K8s environment. While managed Docker services like ECS provide some of the functionality of Kubernetes, you are locked into that provider and you would have a different experience at Google, Azure, or another cloud provider. Kubernetes, even managed services like EKS, provides more flexibility and portability.</br>
<br>&emsp;2. It is the model we use for our SaaS offerings, so internal teams at Ping are more familiar with this model.</br>
<br>&emsp;3. Orchestration among multiple applications and services is native to Kubernetes, a bit of an add-on with Container-only services.</br>
<br>&emsp;4. Workload management using Kubernetes native objects, such as Horizontal Pod Autoscaling, Node scaling and so on.</br>
<br>&emsp;5. Management through Infrastructure-as-Code principles using Helm Charts and Values files.</br>
</details>
-->

### オーケストレーション / Helm / Kubernetes

<details>
  <summary>Helm チャートの新しいリリースが利用可能になったときに通知を受け取るにはどうすればよいですか?</summary>

<a href="https://github.com/pingidentity/helm-charts/">Ping helm-charts GitHub リポジトリ</a>を確認できます。リリースが発生したときに通知を受け取るには、「カスタム」オプションを選択します。製品の Docker イメージと同様、Helm チャートは通常、月に 1 回更新されます。
</details>

<details>
  <summary>Kubernetes は、Docker の直接統合サポートを終了しました。この変更は Ping 製品コンテナに影響しますか?</summary>

<p>いいえ。基盤となるコンテナー ランタイムによってイメージに問題が発生することはありません。エラーが発生した場合はお知らせください。<mark><b>CRI-O</b></mark> および <mark><b>containerd</b></mark> ランタイムはテストされており、既知の問題はありません。</p>

詳しい背景については:<br>

<p>&emsp;Docker の削除に関する Kubernetes ブログ投稿は<a href="https://kubernetes.io/blog/2022/02/17/dockershim-faq/">ここ</a>にあります。</p>

<p>&emsp;それがどのように見えるかについての優れた記事は、この<a href="https://kodekloud.com/blog/kubernetes-removed-docker-what-happens-now/">ページ</a>にあります。</p>
</details>

<details>
  <summary>私のコンテナ環境では、Github や Docker Hub などのサービスへの外部呼び出しを行うことができません。 Ping Identity コンテナは引き続き使用できますか?</summary>

<p>はい。この方法は実稼働シナリオでは一般的です。この状況で Ping Identity コンテナを使用するには:</p>

<p>&emsp;1. <a href="https://devops.pingidentity.com/how-to/existingLicense">既存のライセンス</a>を使用する</p>
<p>&emsp;2. 空のリモートプロファイル <mark><b>SERVER_PROFILE_URL=""</b></mark>を使用します。オプションで、イメージにプロファイルを組み込むことができます。詳細については、<a href="https://devops.pingidentity.com/how-to/profiles/">Server Profiles</a>にアクセスしてください。</p>
<p>&emsp;3. <mark><b>MUTE_LICENSE_VERIFICATION="true"</b></mark> を指定してライセンス検証をオフにします。</p>
<p>&emsp;4. <mark><b>MOTD_URL=""</b></mark> を使用して、今日のメッセージ (MOTD) への呼び出しをオフにします。</p>
</details>

<details>
  <summary>コンテナ環境でコンソールとエンジンを実行するにはどうすればよいですか?</summary>

Helm チャートは、コンソールとエンジンの両方のインスタンス化をサポートしています。コンソールへの入力は、UI アクセス用にレイアウトする必要があります。

<p>Ping の Helm チャートの詳細については、<a href="https://helm.pingidentity.com/">Ping Helm</a> を参照してください。</p>
</details>

<details>
  <summary>Docker の代わりに Podman を使用できますか?</summary>

はい、Docker と同じように、コンテナ オーケストレーションに Podman を使用できるようになります。
</details>

<details>
  <summary>なぜ Ping は docker ではなく K8s を推奨するのでしょうか?</summary>

<p>&emsp;1. Docker や ECS のような純粋なコンテナ ソリューション自体は、通常、K8s 環境ほど堅牢でも復元力もありません。 ECS などのマネージド Docker サービスは Kubernetes の機能の一部を提供しますが、ユーザーはそのプロバイダーにロックされており、Google、Azure、または別のクラウド プロバイダーでは異なるエクスペリエンスが得られます。 Kubernetes は、EKS のようなマネージド サービスであっても、より高い柔軟性と移植性を提供します。</p>
<p>&emsp;2. これは当社の SaaS サービスに使用しているモデルであるため、Ping の内部チームはこのモデルに精通しています。</p>
<p>&emsp;3. 複数のアプリケーションとサービス間のオーケストレーションは Kubernetes にネイティブであり、コンテナ専用サービスのちょっとしたアドオンです。</p>
<p>&emsp;4. 水平Pod自動スケーリング、Node スケーリングなどの Kubernetes ネイティブ オブジェクトを使用したワークロード管理。</p>
<p>&emsp;5. Helm チャートと値ファイルを使用したコードとしてのインフラストラクチャの原則による管理。</p>
</details>

<!--
### Configuration and Server Profile

<details>
  <summary>How do I customize a container?</summary>

There are many ways to customize the container for a Ping product. For example, you can create a customized server profile to save a configuration.
<p>To find more ways on how to customize a container, see <a href="https://devops.pingidentity.com/reference/config/#customizing-the-containers">Customizing Containers</a>.</p>
</details>

<details>
  <summary>How do I save product configurations?</summary>

In order to save configurations, create a server profile and store in a server profile repository.  This repository can be used to pass the configuration into the runtime environment. For help with creating a custom server profile, visit <a href="https://devops.pingidentity.com/how-to/profiles/">Server Profiles</a>.
<p></p>

<p><b>Examples of how to get the profile data from the different products:</b></p>


&emsp; <a href="https://devops.pingidentity.com/how-to/buildPingFederateProfile/">PingFederate</a> Profile
```
curl -k https://localhost:9999/pf-admin-api/v1/bulk/export?includeExternalResources=false \
-u administrator:2FederateM0re \
-H 'X-XSRF-Header: PingFederate' \
-o data.json
```
&emsp; PingAccess Profile
```
curl -k https://localhost:9000/pa-admin-api/v3/config/export \
-u administrator:2FederateM0re \
-H "X-XSRF-Header: PingAccess" \
-o data.json
```
&emsp; <a href="https://devops.pingidentity.com/how-to/buildPingDirectoryProfile/">PingDirectory</a> Profile
```
kubectl exec -it pingdirectory-0 \
-- manage-profile generate-profile \
--profileRoot /tmp/pd.profile
```
</details>

<details>
  <summary>What should be in my server profile?</summary>

For more information about what information should be in the server profile consist, please visit <a href="https://devops.pingidentity.com/how-to/containerAnatomy/">Container Anatomy</a> and <a href="https://devops.pingidentity.com/reference/profileStructures/">Profile Structures</a>.
</details>

<details>
  <summary>Does my server profile have to be hosted on Github?</summary>

No, it can be any <a href="https://devops.pingidentity.com/how-to/profiles/#using-your-github-repository">Public</a> or <a href="https://devops.pingidentity.com/how-to/privateRepos/">Private</a> git repository.
<p>You are also able to use a <a href="https://devops.pingidentity.com/how-to/profiles/#using-local-directories">Local Directory</a> as your repository, which is convenient for testing and development.</p>
</details>
-->

### 構成とサーバープロファイル

<details>
  <summary>コンテナをカスタマイズするにはどうすればよいですか?</summary>

Ping 製品のコンテナをカスタマイズするには、さまざまな方法があります。たとえば、カスタマイズされたサーバー プロファイルを作成して構成を保存できます。

<p>コンテナをカスタマイズするその他の方法については、<a href="https://devops.pingidentity.com/reference/config/#customizing-the-containers">Customizing Containers</a>を参照してください。
</details>

<details>
  <summary>製品構成を保存するにはどうすればよいですか?</summary>

構成を保存するには、サーバー プロファイルを作成し、サーバー プロファイル リポジトリに保存します。このリポジトリを使用して、構成をランタイム環境に渡すことができます。カスタム サーバー プロファイルの作成に関するヘルプについては、<a href="https://devops.pingidentity.com/how-to/profiles/">Server Profiles</a>を参照してください。

<p></p>

<p><b>さまざまな製品からプロファイル データを取得する方法の例:</b></p>


&emsp; <a href="https://devops.pingidentity.com/how-to/buildPingFederateProfile/">PingFederate</a> Profile
```
curl -k https://localhost:9999/pf-admin-api/v1/bulk/export?includeExternalResources=false \
-u administrator:2FederateM0re \
-H 'X-XSRF-Header: PingFederate' \
-o data.json
```
&emsp; PingAccess Profile
```
curl -k https://localhost:9000/pa-admin-api/v3/config/export \
-u administrator:2FederateM0re \
-H "X-XSRF-Header: PingAccess" \
-o data.json
```
&emsp; <a href="https://devops.pingidentity.com/how-to/buildPingDirectoryProfile/">PingDirectory</a> Profile
```
kubectl exec -it pingdirectory-0 \
-- manage-profile generate-profile \
--profileRoot /tmp/pd.profile
```
</details>

<details>
  <summary>サーバー プロファイルには何を含めるべきですか?</summary>

サーバー プロファイルにどのような情報を含める必要があるかについて詳しくは、<a href="https://devops.pingidentity.com/how-to/containerAnatomy/">Container Anatomy</a> と <a href="https://devops.pingidentity.com/reference/profileStructures/">Profile Structures</a>を参照してください。
</details>

<details>
  <summary>私のサーバー プロファイルは Github でホストする必要がありますか?</summary>

いいえ、<a href="https://devops.pingidentity.com/how-to/profiles/#using-your-github-repository">Public</a> or <a href="https://devops.pingidentity.com/how-to/privateRepos/">Private</a>の任意の git リポジトリにすることができます。

<p>テストや開発に便利な<a href="https://devops.pingidentity.com/how-to/profiles/#using-local-directories">Local Directory</a>をリポジトリとして使用することもできます。</p>
</details>

<!--
### Product related

<details>
  <summary>How do I access various product consoles?</summary>

For a Helm-deployed stack, there are two basic ways you can access the consoles.
<p></p>

<p>1. PortForward to the pod to access with localhost.</p>
<p>&emsp; <mark><b>kubectl port-forward &#60;podName&#62; &#60;containerPort&#62;:&#60;localPort&#62;</b></mark></p>
2. Using Helm, add the ingress definition in the yaml file in order to access the container with a URL. See <a href="https://devops.pingidentity.com/deployment/deployHelmLocalIngress/#create-ingresses">Creating Ingresses</a>. You must have an ingress controller in your cluster for the ingress to work.
</details>

<details>
  <summary>How do I use an existing license?</summary>

You can mount the license in the container's <mark><b>opt/in</b></mark> directory. Please see <a href="https://devops.pingidentity.com/how-to/existingLicense/">using existing licenses</a> for more information.
</details>

<details>
  <summary>Where do I get a license?  How do I obtain a trial license?</summary>
<p></p>
The DevOps team at Ping is not responsible for issuing supported product licenses.  We provide a temporary license through the DevOps program. <a href="https://devops.pingidentity.com/how-to/devopsRegistration/">After signing up</a>, you can use the provided credentials to get a short-term license to use in evaluating Ping products running in containers.
<p></p>
If you want to use Ping products in production environments, you are required to purchase a valid license. <a href="https://www.pingidentity.com/en/company/contact-sales.html">Contact our sales department</a> for more information.
</details>

<details>
  <summary>How do I turn off the license verification?</summary>

Set the environment variable in PingBase to: <mark><b>MUTE_LICENSE_VERIFICATION="true"</b></mark>
<p>For more information about the PingBase environment variables, please visit <a href="https://devops.pingidentity.com/docker-images/pingbase/">PingBase</a>.</p>
</details>
-->

### 製品関連

<details>
  <summary>さまざまな製品コンソールにアクセスするにはどうすればよいですか?</summary>

Helm でデプロイされたスタックの場合、コンソールにアクセスするには 2 つの基本的な方法があります。
<p></p>

<p>1. localhost でアクセスするためにPodに PortForward します。</p>
<p>&emsp; <mark><b>kubectl port-forward &#60;podName&#62; &#60;containerPort&#62;:&#60;localPort&#62;</b></mark></p>
2. Helm を使用して、URL でコンテナにアクセスするための ingress 定義を yaml ファイルに追加します。 <a href="https://devops.pingidentity.com/deployment/deployHelmLocalIngress/#create-ingresses">Creating Ingress</a>を参照してください。 Ingress が機能するには、クラスター内に Ingress コントローラーが必要です。
</details>

<details>
  <summary>既存のライセンスを使用するにはどうすればよいですか?</summary>

You can mount the license in the container's <mark><b>opt/in</b></mark> directory. Please see <a href="https://devops.pingidentity.com/how-to/existingLicense/">using existing licenses</a> for more information.
ライセンスはコンテナの <mark><b>opt/in</b></mark> ディレクトリにマウントできます。詳細については、<a href="https://devops.pingidentity.com/how-to/existingLicense/">using existing licenses</a>を参照してください。
</details>

<details>
  <summary>ライセンスはどこで取得できますか?試用ライセンスを取得するにはどうすればよいですか?</summary>
<p></p>
Ping の DevOps チームは、サポートされている製品ライセンスを発行する責任を負いません。 DevOps プログラムを通じて一時ライセンスを提供します。<a href="https://devops.pingidentity.com/how-to/devopsRegistration/">サインアップ後</a>、提供された資格情報を使用して、コンテナー内で実行されている Ping 製品の評価に使用する短期ライセンスを取得できます。
<p></p>
運用環境で Ping 製品を使用する場合は、有効なライセンスを購入する必要があります。詳細については、<a href="https://www.pingidentity.com/en/company/contact-sales.html">当社の営業部門にお問い合わせ</a>ください。
</details>

<details>
  <summary>ライセンス認証をオフにするにはどうすればよいですか?</summary>

PingBase の環境変数を <mark><b>MUTE_LICENSE_VERIFICATION="true"</b></mark> に設定します。
<p>PingBase 環境変数の詳細については、<a href="https://devops.pingidentity.com/docker-images/pingbase/">PingBase</a> を参照してください。</p>
</details>

<!--
### Troubleshoot

<details>
  <summary>How do I run Collect-Support-Data in the devops environment?</summary>

You will need to modify the liveness probe to always exit 0 and the readiness probe to always exit 1. These changes will give you enough time to capture the CSD without it crashing or trying to serve live traffic.
<p>For more information about the Collect-Support-Data, please visit <a href="https://support.pingidentity.com/s/article/collect-support-data-tool">CSD</a>.</p>
</details>

<details>
  <summary>How much overhead memory and CPU is needed to run the Collect-Support-Data tool?</summary>

By default, this value is set to 1GB. You would need to add additional memory (1GB to 2GB) to the heap for the server. In terms of CPU, the CSD uses whatever is available.
<p>For more information about the Collect-Support-Data, please visit <a href="https://support.pingidentity.com/s/article/collect-support-data-tool">CSD</a>.</p>
</details>
-->

### トラブルシュート

<details>
  <summary>Devops 環境で Collect-Support-Data を実行するにはどうすればよいですか?</summary>

liveness プローブを常に 0 で終了するように変更し、readiness プローブを常に 1 で終了するように変更する必要があります。これらの変更により、CSD がクラッシュしたり、ライブ トラフィックを処理しようとしたりすることなく、CSD をキャプチャするのに十分な時間が得られます。

<p>Collect-Support-Dataの詳細については、<a href="https://support.pingidentity.com/s/article/collect-support-data-tool">CSD</a>を参照してください。</p>
</details>

<details>
  <summary>Collect-Support-Data ツールを実行するには、どのくらいのオーバーヘッド メモリと CPU が必要ですか?</summary>

デフォルトでは、この値は 1GB に設定されています。サーバーのヒープに追加のメモリ (1GB ～ 2GB) を追加する必要があります。 CPU に関しては、CSD は利用可能なものはすべて使用します。
<p>Collect-Support-Dataの詳細については、<a href="https://support.pingidentity.com/s/article/collect-support-data-tool">CSD</a>を参照してください。</p>
</details>
