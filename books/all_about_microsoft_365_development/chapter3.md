---
title: "Microsoft ID プラットフォーム"
---

# Azure Active Directory

Azure Active Directory (以下、Azure AD) は ID 管理のためのクラウド ベースのプラットフォーム (IDaaS) です。Microsoft 365 や Azure の土台となるべき重要なサービスであるため、はじめに Azure AD がどのようなものかを理解する必要があります。

Azure AD には Free、Office 365、Premium P1、Premium P2 の 4 つのエディションがあり、順にできることが増えていきます。Premium P1 は Microsoft 365 E3 に含まれており、Premium P2 は Microsoft 365 E5 に含まれていますが、単体でライセンスを購入することもできます。Microsoft 365 開発という観点ではほぼ Azure AD Free の範囲で開発できるため、あまり気にすることはありませんが、ライセンスの違いがあることは理解しておいたほうがいいでしょう。なお、Microsoft 365 開発者プログラムに参加すれば、Microsoft 365 E5 相当のライセンスが無料で入手できますので、Azure AD Premium P2 の機能を試すことができます。

## Azure AD と Microsoft 365 および Azure

Azure AD は Azure という名前がついているため、Azure のサービスと思われがちですが、厳密には異なります。はじめて Microsoft 365 にサインアップするときに、`onmicrosoft.com` のドメインで終わるテナントを作成する必要がありますが、このときに Azure AD のディレクトリも合わせて作成されます。Azure のサブスクリプションの契約がなく、Microsoft 365 だけを契約しているテナントでも Azure AD は存在するということです。Microsoft 365 管理センターから管理できるユーザーおよびグループは、Azure ポータルから管理できるユーザーおよびグループと同じものです。こう考えると、Azure AD は Azure のサービスではなく、独立したサービスであるということがわかると思います。

Azure ではロール ベースのアクセス制御 (RBAC) によって Azure リソースを管理します。ユーザーやグループに対して所有者や共同作成者などのロールを割り当てます。Microsoft 365 ではロールによって Microsoft 365 を管理します。ユーザーやグループに対してグローバル管理者やライセンス管理者などのロールを割り当てます。これらは異なる概念です。たとえばグローバル管理者を割り当てられたユーザーが同じテナントにある Azure のサブスクリプションを操作できるわけではありません。

## Azure AD のセキュリティ プリンシパル

Azure AD のセキュリティ プリンシパルとは Azure や Microsoft 365 に対するアクセス許可を割り当て可能な対象のことを指します。Azure AD ではユーザー、グループまたはサービス プリンシパルにアクセス許可を割り当てることができます。

### ユーザー

ユーザーは Azure AD にプロファイルを持つ個人を表します。ユーザーにアクセス許可を付与するとユーザーはリソースにアクセスできるようになります。Microsoft 365 のライセンスの付与もユーザーに対して行われます。

ユーザーは Microsoft 365 管理センターの**ユーザー**またはAzure ポータルの**ユーザー**で確認することができます。

### グループ

グループは複数のユーザーのコレクションです。Azure AD で作成できるグループにはいくつかの種類があります。

- セキュリティ グループは Azure や Microsoft 365 のロールを割り当てることができます。
- 配布グループは電子メール アドレスを持ちメンバーとなるユーザーにメールを送信することができます。
- メールが有効なセキュリティ グループはセキュリティ グループと配布グループの両方の性質を合わせ持ちます。
- Microsoft 365 グループはメールボックス、予定表、サイトを持ちメンバーの共同作業をサポートします。

グループは Microsoft 365 管理センターの**チームとグループ**またはAzure ポータルの**グループ**で確認することができます。

### サービス プリンシパル

サービス プリンシパルはアプリケーションまたはサービスによって使用される ID です。ユーザーと異なり、ログインしたりライセンスを付与したりすることはできませんが、ユーザーの代わりにリソースにアクセスしたり、サービス プリンシパルそのものとしてリソースにアクセスすることができます。

サービス プリンシパルは Windows の `LOCAL SERVICE` や `NETWORK SERVICE`、あるいは SharePoint のサーバー ファーム アカウントと同じようなものと考えることができます。ただし異なるのは、あくまでこれらはサービスから使用されるユーザーですが、Azure AD の場合は明確に区別されているという点です。

サービス プリンシパルは Azure ポータルの**エンタープライズ アプリケーション**で確認することができます。

#### アプリケーション

アプリケーションは Azure AD アプリケーションのテナントにおけるローカル インスタンスです。Azure AD アプリケーションとの違いについては後述します。

#### マネージド ID

マネージド ID は Azure リソースそのものに割り当てられる ID です。これまで SQL Database や Blob Storage などへのアクセスには、パスワードやシークレットのようなものが使われてきましたが、これらは漏洩するとセキュリティ上の問題が発生する可能性がありました。これを防ぐために、パスワードやシークレットによって認証するのではなく、Azure リソースそのものを信頼するというのが マネージド ID の仕組みです。

#### レガシー

レガシー アプリケーションは旧来のアプリケーションであり互換性のために維持されています。

# Azure AD アプリケーション

Microsoft 365 のような Azure AD で保護されたアプリケーションの API を使用するアプリケーションを作成する必要があるとします。このようなアプリケーションの API を使用するためには Azure AD に対して認証および承認する必要があります。Azure AD では Azure AD アプリケーションによって、他の Azure AD アプリケーションへのアクセスや、他の Azure AD アプリケーションからのアクセスを許可することができます。

**アプリケーション A** を呼び出す**アプリケーション B** を作成する場合、

1. **アプリケーション B** という Azure AD アプリケーションを作成する
2. **アプリケーション B** に**アプリケーション A** の API へのアクセス許可を追加する
3. **アプリケーション B** に対して認証/承認する

という流れになります。

逆に作成したアプリケーションの API を他のアプリケーションに公開することもできます。API にはスコープを設定することができ、スコープにしたがって特定の API のみを許可するように構成することができます。

**アプリケーション C** の API を公開する場合、

1. **アプリケーション C** という Azure AD アプリケーションを作成する
2. **アプリケーション C** の API のスコープを追加する

という流れになります。たとえばすでに**アプリケーション D** が**アプリケーション C** の API を使用することがわかっている場合、

3. **アプリケーション D** が**アプリケーション C** の API を使用することを承認する

ということも可能です。

## Azure AD アプリケーションとサービス プリンシパル

Azure AD アプリケーションとサービス プリンシパルは切り離せない関係にあるためその違いを理解することは重要です。

### Azure AD アプリケーション

Azure AD アプリケーションは認証の方法、使用する API、公開する API などの構成を定義するためのオブジェクトです。

Azure AD アプリケーションはシングル テナント アプリケーションとマルチ テナント アプリケーションが選択できます。シングル テナント アプリケーションはそのアプリケーションを作成したテナントのみで使用できます。マルチ テナント アプリケーションは任意のテナントで使用できます。マルチ テナント アプリケーションはサードパーティのベンダー (ISV) によって提供されるアプリケーションを想定しており、マイクロソフト パートナー ネットワーク (MPN) に参加している必要があります。このため Azure AD アプリケーションはすべてのテナントにおいてユニークです。たとえば SharePoint Online の Azure AD アプリケーション ID はどのテナントにおいても常に `00000003-0000-0000-c000-000000000000` です。

### サービス プリンシパル

サービス プリンシパルは Azure AD アプリケーションのそれぞれのテナントにおけるインスタンスとなるオブジェクトです。Azure AD アプリケーションを使用するユーザー、アプリケーション プロキシ、条件付きアクセス許可などを設定することができます。

サービス プリンシパルは Azure AD アプリケーションをもとに作成されます。特定のテナントで見ると 1 つの Azure AD アプリケーションからなるサービス プリンシパルは 1 つですが、マルチ テナント アプリケーションでは複数のテナントで同じ Azure AD アプリケーションからなる異なるオブジェクト ID のサービス プリンシパルが存在することになります。

サービス プリンシパルは Azure PowerShell の `New-AzADServicePrincipal` や Azure CLI の `az ad sp create` を使って作成することもできますが、多くの場合は API のアクセス許可の同意をする際に自動的に作成されます。Azure AD アプリケーションを作成しただけではサービス プリンシパルは作成されないという点にも注意が必要です。

## Azure AD アプリケーションを作成できるユーザー

既定ではすべてのユーザーが Azure AD アプリケーションを作成することができます。ただし、アプリケーションの API には管理者の同意が必要なものがあり、同意がされなければそれらの API を使用することはできません。たとえば Microsoft Graph のユーザー API では `User.Read`、`User.ReadWrite` および `User.ReadBasic.All` は管理者の同意を必要としませんが、それ以外のアクセス許可は管理者の同意を必要とします。

管理者の同意を付与できるのは**グローバル管理者**または**アプリケーション管理者**です。管理者の同意は Azure ポータルから行えるほか、同意エンドポイントにアクセスすることで行うこともできます。

```
https://login.microsoftonline.com/{{tenant-id}}/v2.0/adminconsent?client_id={{client_id}}&scope={{scope}}&redirect_uri={{redirect_uri}}
```

ユーザーが Azure AD アプリケーションを作成できないようにすることもできます。これは**ユーザー設定**から変更できます。

# OAuth

OAuth は RFC で定義される承認のプロトコルです。現在の最新は OAuth 2.0 であり、Azure AD も OAuth 2.0 を実装します。以下、単に OAuth と呼ぶ場合は OAuth 2.0 を指すものとします。

## ロール

OAuth では 4 つのロールが定義されています。それぞれのロールと Azure AD のオブジェクトまたはサービスをマッピングすると以下のようになります。

|ロール|説明|オブジェクトまたはサービス|
|-|-|-|
|リソース オーナー|リソースに対するアクセス許可を持つオブジェクト|Azure AD ユーザーまたはサービス プリンシパル|
|リソース サーバー|リソースを提供するサーバー|API を提供する Azure AD アプリケーション|
|クライアント|リソース オーナーに変わってリソースにアクセスするアプリケーション|API を使用する Azure AD アプリケーション|
|認証サーバー|アクセス トークンを発行するサーバー|Azure AD (`login.microsoftonline.com`)|

## フロー

私たちは Web ブラウザー、デスクトップまたはモバイルなどのさまざまな種類の方法によってアプリケーションを使用します。それぞれの特性に合わせて承認のフローが存在します。

### フローの種類

Azure AD では以下の OAuth のフローをサポートします。

#### Authorization Code フロー

[Authorization Code フロー](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/v2-oauth2-auth-code-flow)はリソース オーナーが Web ブラウザーで認証されたときに返却される認証コードを使用してアクセス トークンを取得するフローです。Web アプリケーションやデスクトップ アプリケーションの場合に使用します。

1. `https://login.microsoftonline.com/{{tenant-id}}/oauth2/v2.0/authorize` にリダイレクト
2. Azure AD での認証後、パラメーターで渡したコールバック URL に**認証コード**を含めてリダイレクト
3. `https://login.microsoftonline.com/{{tenant-id}}/oauth2/v2.0/token` に**認証コード**および**証明書またはクライアント シークレット**を含めて POST リクエスト
4. POST レスポンスの JSON から**アクセス トークン**を取得

POST リクエストには証明書またはクライアント シークレットを指定するか、パブリック クライアント アプリケーションにする必要があります。パブリック クライアント アプリケーションについては後述します。

#### Implicit Grant フロー

[Implicit Grant フロー](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/v2-oauth2-implicit-grant-flow)はリソース オーナーが Web ブラウザーで認証されたときにリダイレクト URL からアクセス トークンを取得するフローです。シングル ページ アプリケーション (SPA) を含む JavaScript のみで動作するアプリケーションの場合に使用します。

1. `https://login.microsoftonline.com/{{tenant-id}}/oauth2/v2.0/authorize` にリダイレクト
2. Azure AD での認証後、パラメーターで渡したコールバック URL に**アクセス トークン**を含めてリダイレクト

セキュリティの問題があるため Proof Key for Code Exchange by OAuth Public Clients (PKCE) を使用することが推奨されています。

#### Resource Owner Password Credentials フロー

[Resource Owner Password Credentials フロー](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/v2-oauth-ropc)はリソース オーナーのパスワードを使用してアクセス トークンを取得するフローです。

1. `https://login.microsoftonline.com/{{tenant-id}}/oauth2/v2.0/token` に**パスワード**を含めて POST リクエスト
2. POST レスポンスの JSON から**アクセス トークン**を取得

パスワードをリクエストに含めることはセキュリティの問題があるため推奨されていません。また多要素認証を使用するアカウントでは使用できません。

#### Client Credentials フロー

[Client Credentials フロー](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow)はクライアントが直接的にリソースにアクセスするフローです。このフローではリソース オーナーを必要としません。バックグラウンドで動作するアプリケーションの場合に使用します。

1. `https://login.microsoftonline.com/{{tenant-id}}/oauth2/v2.0/token` に**証明書またはクライアント シークレット**を含めて POST リクエスト
2. POST レスポンスの JSON から**アクセス トークン**を取得

#### Device Code フロー

[Device Code フロー](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/v2-oauth2-device-code)はデバイス コードを入力することによって別のデバイスで認証しアクセス トークンを取得するフローです。Web ブラウザーを表示することができないデバイスで動作するアプリケーションの場合に使用します。

1. `https://login.microsoftonline.com/{{tenant-id}}/oauth2/v2.0/devicecode` に POST リクエスト
2. POST レスポンスの JSON から**認証 URL およびデバイス コード**を取得
3. 別のデバイスで認証 URL にアクセスしデバイス コードを入力して認証
4. `https://login.microsoftonline.com/{{tenant-id}}/oauth2/v2.0/token` に定期的に POST リクエスト (ポーリング)
5. POST レスポンスの JSON から**アクセス トークン**を取得

#### On-Behalf-Of フロー

[On-Behalf-Of フロー](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow)はすでに取得済みのアクセス トークンを使用して別のリソースのアクセス トークンを取得するフローです。サービスが別のサービスを呼び出す場合に使用します。

#### SAML Bearer Assertion フロー

[SAML Bearer Assertion フロー](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/v2-saml-bearer-assertion)は Active Directory フェデレーション サービス (ADFS) によって発行された SAML トークンを使ってアクセス トークンを取得するフローです。

### フローの選択

アプリケーションの種類によって利用できるフローは異なります。適切なフローを選択する必要があります。

|アプリケーション|フロー|備考|
|-|-|-|
|Web アプリケーション|Authorization Code フロー||
|SPA アプリケーション|Implicit Grant フロー (PKCE を推奨)||
|Web API アプリケーション|On-Behalf-Of フロー||
|デスクトップ アプリケーション|Authorization Code フロー||
|モバイル アプリケーション|Authorization Code フロー||
|コンソール アプリケーション|Authorization Code フロー|UI が表示できる場合|
||Device Code フロー|UI が表示できない場合|
|サービス アプリケーション|Client Credentials フロー||

Azure AD アプリケーションは複数のフローをサポートします。アプリケーションを Web とデスクトップで展開するような場合に Azure AD アプリケーションをそれぞれ作成する必要はありません。

アクセス トークンを取得するときには証明書またはクライアント シークレットをリクエストに含める必要がありますが、デスクトップ アプリケーションやモバイル アプリケーションの場合は、セキュリティ上の問題からこれらの情報を含めることができません。そのため Azure AD アプリケーションを[パブリック クライアント アプリケーション](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/msal-client-applications)として構成します。

フローはクライアント (API の実行側) によって行われます。クライアントによるフローの実行をサポートするために [Microsoft Authentication Library (MSAL)](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/msal-overview) が提供されています。MSAL は .NET、Java および JavaScript などの複数の言語をサポートしています。

## アクセス許可

Azure AD のアクセス許可には**委任されたアクセス許可**と**アプリケーションのアクセス許可**の 2 種類が存在します。

### 委任されたアクセス許可

委任されたアクセス許可はリソース オーナーがクライアントにアクセス許可の一部または全部を委任する方法です。委任されたアクセス許可の場合、実際のアクセス許可はクライアントに委任されたアクセス許可とリソース オーナーが持っているアクセス許可の最小範囲で動作します。つまり、そもそもリソース オーナーがアクセスできないコンテンツにはアクセスできません。Client Credentials 以外のフローは委任されたアクセス許可になります。

### アプリケーションのアクセス許可

アプリケーションのアクセス許可はクライアントが直接的にリソースにアクセスする方法です。アプリケーションのアクセス許可は非常に強い権限が与えられるため、取り扱いには注意しなければなりません。Client Credentials フローのみがアプリケーションのアクセス許可をサポートします。

## Json Web Token (JWT)

Azure AD の OAuth のフローで返却されるアクセス トークンは Json Web Token (JWT) という形式になっています。JWT はヘッダー、ペイロード、および署名の 3 つのセクションから構成されています。ヘッダーには署名に関するデータ、ペイロードにはクレームのセットが含まれます。署名は Azure AD から提供される公開鍵によって検証することができます。公開鍵は `https://login.microsoftonline.com/{{tenant-id}}/.well-known/openid-configuration` に含まれる `jwks_uri` によって取得することができます。

トラブルシューティングをする上で JWT のクレームの意味を知ることは非常に重要です。JWT は [jwt.ms](https://jwt.ms) や [jwt.io](https://jwt.io) といったサイトで簡単にデコードすることができます。特に確認するべきクレームを以下に示します。

|名前|説明|
|-|-|
|aud|リソース サーバーとなる Azure AD アプリケーションの ID または URI|
|iss|認証サーバーの URL|
|exp|トークンの有効期限|
|appid|クライアントとなる Azure AD アプリケーションの ID|
|scp|トークンのスコープ (アクセス許可)|
|oid|ユーザー ID|
|tid|テナント ID|

JWT を検証する必要があるのはリソース サーバー (API の提供側) です。JWT の検証をサポートするために [Microsoft Identity Web 認証ライブラリ](https://docs.microsoft.com/ja-jp/azure/active-directory/develop/microsoft-identity-web)が提供されています。

# OpenID Connect (OIDC)

OpenID Connect (OIDC) は OpenID Foundation によって定義される認証のプロトコルです。OAuth 2.0 がベースになっているため OAuth の仕組みがわかれば簡単に利用できるようになっています。OAuth と異なるのは、OAuth が**承認**のプロトコルであるのに対して OIDC は**認証**のプロトコルであるという点です。そのため、OIDC ではリソース サーバーは存在せず、クライアントと認証サーバーのみのやりとりになります。Azure AD でも OIDC をサポートします。

Azure AD での認証を有効にするもっとも簡単な方法は [Azure App Service の認証と承認](https://docs.microsoft.com/ja-jp/azure/app-service/overview-authentication-authorization)を使用することです。この機能は EasyAuth と呼ばれることもあります。アプリケーション開発者はノーコードで Azure AD での認証を実装することができるため、静的サイトや既存の API などをコードの変更なく保護したい場合に有効です。Windows および Linux で利用できますが、実装アーキテクチャが異なるため、注意が必要です。サイト全体を保護することができるほか、ルートに `authorization.json` を配置することで特定の URL のみを保護することも可能です。

# まとめ

認証や承認はシステムの根幹に関わる非常に重要な要素です。ユーザーが安全に利用できるようにするために特に注意しなければならず、安易に実装して脆弱性を生み出すことのないようにしなければなりません。OAuth を低レイヤーで実装することは検証や理解のためとしては推奨しますが、実稼働のアプリケーションでは提供されているライブラリを使用しましょう。
