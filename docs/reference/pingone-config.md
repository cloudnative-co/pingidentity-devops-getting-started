---
title: PingOne Worker App and User Config
---

<!--
# PingOne Worker App and User Config

## PingOne Worker App Configuration

To manage PingOne resources using credentials other than your own, you are required to have a PingOne Worker App.

You have 3 options to authenticate to PingOne from pingctl:

* [Authorization Code (w/ PKCE) Flow](#authorization-code-w-pkce-flow-settings) (Recommended and most secure) - Via a PingOne Admin User
* [Implicit Flow](#implicit-flow-settings) - Via a PingOne Admin User
* [Client Credentials Flow](#client-credentials-flow-settings) (Easiest, but most insecure, as a user isn't required)

Additionally, you must set up the proper [roles for your Worker App](#worker-app-roles-settings)
-->

# PingOne Worker App と User Config

## PingOne Worker App の設定

自分以外の資格情報を使用して PingOne リソースを管理するには、PingOne Worker App が必要です。

pingctl から PingOne に対して認証するには 3 つのオプションがあります。

* [認証コード (PKCE 付き) フロー](#authorization-code-w-pkce-flow-settings) (推奨され、最も安全) - PingOne 管理者ユーザー経由
* [暗黙的フロー](#implicit-flow-settings) - PingOne 管理者ユーザー経由
* [クライアント資格情報フロー](#client-credentials-flow-settings) (最も簡単ですが、ユーザーが必要ないため最も安全ではありません)

さらに、[ワーカー アプリに適切なロールを設定](#worker-app-roles-settings)する必要があります。

<!--
### Authorization Code (w/ PKCE) Flow Settings

The following shows an example of a Worker App setup for Authorization Code (w/ PKCE) Flow:

![](images/pingone-worker-app-authorization_code.png)
-->

### 認証コード（PKCSつき）フローの設定

以下に、認可コード (PKCE あり) フローのワーカー アプリ設定の例を示します。

![](images/pingone-worker-app-authorization_code.png)

<!--
### Implicit Flow Settings

The following shows an example of a Worker App setup for Implicit Flow:

![](images/pingone-worker-app-implicit.png)
-->

### 暗黙的フローの設定

以下に、暗黙的フローのワーカー アプリ設定の例を示します。

![](images/pingone-worker-app-implicit.png)

<!--
### Client Credentials Flow Settings

The following shows an example of a Worker App setup for Client Credentials Flow:

![](images/pingone-worker-app-client-credentials.png)
-->

### クライアント資格情報フローの設定

以下に、クライアント認証情報フローのワーカー アプリ設定の例を示します。

![](images/pingone-worker-app-client-credentials.png)

<!--
### Worker App Roles Settings

The following shows an example of the minimum roles required.  Typically, these are set up by default.

![](images/pingone-worker-app-roles.png)
-->

### Worker Appのロール設定

以下に、クライアント認証情報フローのワーカー アプリ設定の例を示します。

![](images/pingone-worker-app-roles.png)

<!--
## PingOne User Config

When using Authorization Code or Implicit Flows, you need to log in with an Administrative user to use the
Worker App.

The most important item is to add the proper administrative roles to the user.  The following shows
an example of this:

![](images/pingone-user-roles.png)
-->

## PingOne User Config

認可コードまたは暗黙的フローを使用する場合、ワーカー アプリを使用するには管理ユーザーでログインする必要があります。

最も重要な項目は、適切な管理役割をユーザーに追加することです。以下にその例を示します。

![](images/pingone-user-roles.png)
