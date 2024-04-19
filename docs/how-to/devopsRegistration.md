---
title: Ping Identity DevOps Registration
---

<!--
!!! info "Getting Support"
    The team responsible for the Ping DevOps program does not have access to the user account system on the Ping Identity website.  If you have trouble with your account and are unable to follow these instructions to enroll, the issue is probably with your credentials in our system.  Please contact your sales representative at [Ping Identity Support](https://support.pingidentity.com/s/).

# Ping Identity DevOps Registration

Registering for Ping Identity's DevOps Program provides you with credentials that enable you to easily deploy and evaluate Ping Identity products using trial licenses automatically using tools and platforms like Helm or Kubernetes.

To register for the DevOps Program:

* Make sure you have a registered account with Ping Identity.  If you're not sure, click the link to [Sign On](https://www.pingidentity.com/en/account/sign-on.html) and follow the instructions to access your account.
  * If you don't have an account, create one [here](https://www.pingidentity.com/en/account/register.html).
  * When signing on, select **Support and Community** in the **Select Account** list.
  * After you're signed on, you're directed to your profile [page](https://support.pingidentity.com/s/).
  * In the right-side menu, click **Register for DevOps Program**.
  ![Register for DevOps](../images/DEVOPS_REGISTRATION.png)

A confirmation message will be shown and the DevOps credentials will be forwarded to the email address associated with your Ping Identity account.

!!! info "Saving Credentials"
    When you receive your key, follow the instructions below for saving these with the `pingctl` utility.

Example:

* `PING_IDENTITY_DEVOPS_USER=jsmith@example.com`
* `PING_IDENTITY_DEVOPS_KEY=e9bd26ac-17e9-4133-a981-d7a7509314b2`
-->

!!! info "サポートを得る"
    Ping DevOpsプログラムの責任者チームは、Ping Identity Webサイトのユーザーアカウントシステムにアクセスできません。アカウントに問題があり、登録するためにこれらの指示に従うことができない場合、問題はおそらくシステムの資格情報にあります。 [Ping Identity Support](https://support.pingidentity.com/s/)の営業担当者にお問い合わせください。

# Ping Identity DevOps Registration

Ping Identity の DevOps プログラムに登録すると、Helm や Kubernetes などのツールやプラットフォームを使用して試用ライセンスを自動的に使用して、Ping Identity 製品を簡単にデプロイおよび評価できる認証情報が提供されます。

DevOps プログラムに登録するには:

* Ping Identity に登録済みのアカウントがあることを確認してください。よくわからない場合は、[Sign On](https://www.pingidentity.com/en/account/sign-on.html)のリンクをクリックし、指示に従ってアカウントにアクセスしてください。
  * アカウントをお持ちでない場合は、[ここ](https://www.pingidentity.com/en/account/register.html)で作成してください。
  * サインオンするときに、 **アカウントの選択** リストで **サポートとコミュニティ** を選択します。
  * サインオンすると、プロフィール [ページ](https://support.pingidentity.com/s/)に移動します。
  * In the right-side menu, click **Register for DevOps Program**.
  * 右側のメニューで、 **DevOps プログラムに登録** をクリックします。
  ![Register for DevOps](../images/DEVOPS_REGISTRATION.png)

確認メッセージが表示され、DevOps 認証情報が Ping Identity アカウントに関連付けられた電子メール アドレスに転送されます。

!!! info "認証情報の保存"
    キーを受け取ったら、以下の手順に従って `pingctl` ユーティリティを使用してキーを保存します。

例:

* `PING_IDENTITY_DEVOPS_USER=jsmith@example.com`
* `PING_IDENTITY_DEVOPS_KEY=e9bd26ac-17e9-4133-a981-d7a7509314b2`

<!--
## Saving Your DevOps User and Key

The recommended way to save your DevOps User/Key is to use the Ping Identity DevOps utility `pingctl`.

!!! info "pingctl setup"
    You can find installation instructions for `pingctl` in the [pingctl Tool](../tools/pingctlUtil.md) document.

To save your DevOps credentials, run `pingctl config` and supply your credentials when prompted.

When `pingctl` is installed and configured, it places your DEVOPS USER/KEY into a Ping Identity property file found at
`~/.pingidentity/config`  with the following variable names set (see the following example).

```text
PING_IDENTITY_DEVOPS_USER=jsmith@example.com
PING_IDENTITY_DEVOPS_KEY=e9bd26ac-17e9-4133-a981-d7a7509314b2
```

After you've configured these settings, you can view them with the `pingctl info` command (credential values are masked by default, use `pingctl info -v` to show unmasked).
-->

## DevOps ユーザーとキーの保存

DevOps ユーザー/キーを保存する推奨方法は、Ping Identity DevOps ユーティリティ `pingctl` を使用することです。

!!! info "pingctl セットアップ"
    `pingctl` のインストール手順については、[pingctl Tool](../tools/pingctlUtil.md)のドキュメントを参照してください。

DevOps 認証情報を保存するには、`pingctl config` を実行し、プロンプトが表示されたら認証情報を入力します。

`pingctl` をインストールして構成すると、DEVOPS USER/KEY が、次の変数名が設定された `~/.pingidentity/config` にある Ping Identity プロパティ ファイルに配置されます (次の例を参照)。

```text
PING_IDENTITY_DEVOPS_USER=jsmith@example.com
PING_IDENTITY_DEVOPS_KEY=e9bd26ac-17e9-4133-a981-d7a7509314b2
```

これらの設定を構成した後、`pingctl info` コマンドを使用してそれらを表示できます (資格情報の値はデフォルトでマスクされています。マスクされていないことを表示するには `pingctl info -v` を使用します)。

<!--
## Resending your DevOps User and Key

If you have misplaced or lost your DevOps User/Key, there are two convenient ways to recover it.

* If you have configured `pingctl`, the `PING_IDENTITY_DEVOPS_USER` and `PING_IDENTITY_DEVOPS_KEY` can be printed by entering the following command:
    ```text
    pingctl info -v
    ```

* If you did not save the credentials in the `pingctl` tool, you can recover your credentials by logging in to your Ping Identity account.
  * Navigate to [Sign On](https://www.pingidentity.com/en/account/sign-on.html) and follow the instructions to access your account.
  * When signing on, select **Support and Community** in the **Select Account** list.
  * After you're signed on, you're directed to your profile [page](https://support.pingidentity.com/s/).
  * In the right-side menu, click **Register for DevOps Program** again.  A confirmation message will be shown and the same DevOps credentials will be resent to the email address associated with your Ping Identity account.
-->

## DevOps ユーザーとキーの再送信

DevOps ユーザー/キーを置き忘れたり紛失した場合、それを回復する便利な方法が 2 つあります。

* `pingctl` を構成している場合は、次のコマンドを入力して `PING_IDENTITY_DEVOPS_USER` と `PING_IDENTITY_DEVOPS_KEY` を出力できます。
    ```text
    pingctl info -v
    ```

* 資格情報を `pingctl` ツールに保存しなかった場合は、Ping Identity アカウントにログインすることで資格情報を回復できます。
    * [Sign On](https://www.pingidentity.com/en/account/sign-on.html)に移動し、指示に従ってアカウントにアクセスします。
    * サインオンするときに、 **アカウントの選択** リストで **サポートとコミュニティ** を選択します。
    * サインオンすると、プロフィール [ページ](https://support.pingidentity.com/s/)に移動します。
    * 右側のメニューで、もう一度 **DevOps プログラムに登録** をクリックします。確認メッセージが表示され、同じ DevOps 資格情報が Ping Identity アカウントに関連付けられた電子メール アドレスに再送信されます。
