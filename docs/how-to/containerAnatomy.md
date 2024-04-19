---
title: Server Profile Deployment
---
<!--
# Deployment

Any configuration that is deployed with one of our product containers can be considered a "server profile". A profile typically looks like a set of files.

You can use profiles in these ways:

* Pull at startup.
* Build into the image.
* Mount as a container volume.
-->

# デプロイメント

当社の製品コンテナのいずれかで展開される構成はすべて、「サーバー プロファイル」と見なすことができます。通常、プロファイルは一連のファイルのように見えます。

プロファイルは次の方法で使用できます。

* 起動時にpullする
* buildしてイメージに組み込む
* コンテナにボリュームとしてマウントする

<!--
## Pull at startup

Pass a Github-based URL and path as environment variables that point to a server profile.

Pros:

* Easily sharable, inherently source-controlled

Cons:

* Adds download time at container startup

For profiles pulled at startup, the image uses the following variables to clone the repo at startup and pull the profile into the container:

* `SERVER_PROFILE_URL` - The git URL with the server profile.
* `SERVER_PROFILE_PATH` - The location from the base of the URL with the specific server profile.
  This allows for several products server profile to be housed in the same git repo.
* `SERVER_PROFILE_BRANCH` (optional) - If other than the default branch (usually master or main), allows
  for specifying a different branch.  Example might be a user's development branch before merging into master.

Although there is additional customizable functionality, this is the most common way that profiles are provided to containers because it is easy to provide a known starting state as well as track changes over time.  For more information, see [Private Github Repos](privateRepos.md).
-->

## 起動時にpullする

Github ベースの URL とパスを、サーバー プロファイルを指す環境変数として渡します。

Pros:

* 簡単に共有でき、本質的にソース管理されている

Cons:

* コンテナ起動時にダウンロード時間が増える

起動時にpullされるプロファイルの場合、イメージは次の変数を使用して起動時にリポジトリのクローンを作成し、プロファイルをコンテナーにpullします。

* `SERVER_PROFILE_URL` - サーバー プロファイルを含む git URL
* `SERVER_PROFILE_PATH` - 特定のサーバー プロファイルを含む URL のベースからの場所。これにより、複数の製品サーバー プロファイルを同じ git リポジトリに格納できるようになります。
* `SERVER_PROFILE_BRANCH` (オプション) - デフォルトのブランチ (通常はmasterまたはmain) 以外の場合、別のブランチを指定できます。例としては、masterにmergeする前のユーザーの開発ブランチが挙げられます。

追加のカスタマイズ可能な機能もありますが、既知の開始状態を提供したり、経時的な変化を追跡したりするのが簡単であるため、これがプロファイルをコンテナーに提供する最も一般的な方法です。詳細については、[Private Github Repos](privateRepos.md)を参照してください。

<!--
## Build into the image

Build your own image from one of our Docker images and copy the profile files in.

Pros:

* No download at startup, and no egress required

Cons:

* Tedious to build images when making iterative changes

Building a profile into the image is useful when you have no access to the Github repository or if you're often spinning containers up and down.

For example, if you made a Dockerfile at this location: https://github.com/pingidentity/pingidentity-server-profiles/tree/master/baseline, the relevant entries might look similar to this:

```shell
FROM: pingidentity/pingfederate:edge
COPY pingfederate/. /opt/in/.
```
-->

## buildしてイメージに組み込む

Docker イメージの 1 つから独自のイメージを構築し、プロファイル ファイルをコピーします。

Pros:

* 起動時にダウンロードがなく、egressも必要ありません

Cons:

* 反復的な変更を行うときにイメージを構築するのは面倒です

イメージにプロファイルを構築すると、Github リポジトリにアクセスできない場合、またはコンテナーを頻繁に起動・終了場合に役立ちます。

たとえば、次の場所に Dockerfile を作成した場合: https://github.com/pingidentity/pingidentity-server-profiles/tree/master/baseline、関連するエントリは次のようになります。

```shell
FROM: pingidentity/pingfederate:edge
COPY pingfederate/. /opt/in/.
```

<!--
## Mount as a Docker volume

Using `docker-compose` you can bind-mount a host file system location to a location in the container.

Pros:

* Most iterative. There's no download time, and you can see the file system while you are working in the container.

Cons:

* There's no great way to do this in Kubernetes or other platform orchestration tools.

Mount the profile as a Docker volume when you're developing a server profile and you want to be able to quickly make changes to the profile and spin up a container against it.

For example, if you have a profile in same directory as your `docker-compose.yaml` file, you can add a bind-mount volume to /opt/in like this:

```sh
volumes:
   - ./pingfederate:/opt/in
```
-->

## コンテナにボリュームとしてマウントする

`docker-compose` を使用すると、ホスト ファイル システムの場所をコンテナ内の場所にバインド マウントできます。

Pros:

* 最も反復的なもの。ダウンロードに時間がかからず、コンテナで作業しながらファイル システムを確認できます。

Cons:

* Kubernetes やその他のプラットフォーム オーケストレーション ツールでこれを行う優れた方法はありません。

サーバー プロファイルを開発するときに、プロファイルを Docker ボリュームとしてマウントし、プロファイルにすばやく変更を加えて、プロファイルに対してコンテナーをスピンアップできるようにしたい場合は、プロファイルを Docker ボリュームとしてマウントします。

たとえば、`docker-compose.yaml` ファイルと同じディレクトリにプロファイルがある場合は、次のようにバインドマウント ボリュームを /opt/in に追加できます。

```sh
volumes:
   - ./pingfederate:/opt/in
```
