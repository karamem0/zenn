---
title: "Microsoft Graph"
---

# Microsoft Graph

Microsoft Graph を使用することで開発者は Microsoft 365 のデータにアクセスすることができます。Microsoft Graph としては **Microsoft Graph API**、**Microsoft Graph コネクタ**、**Microsoft Graph データ接続**の 3 つのコンポーネントが提供されています。

## Microsoft Graph API

Microsoft Graph API を使用することで Microsoft 365 のデータの取得や設定をすることができます。Microsoft Graph API は OData をサポートする REST API として実装されています。詳細については次のセクションで説明します。

## Microsoft Graph コネクタ

Microsoft Graph コネクタを使用することで外部のデータを Microsoft Graph に取り込み Microsoft Search API を使用してアクセスできるようになります。以前のバージョンの SharePoint をご存じの方であれば SharePoint エンタープライズ検索で検索先として外部のサイトを指定できるのと同じだといえばイメージがつかみやすいかもしれません。Microsoft Graph コネクタでは Azure SQL Database、Oracle Database、Salesforce、ServiceNow、Azure DevOps などをデータ ソースとして指定し Microsoft Graph に統合することができます。

## Microsoft Graph データ接続

Microsoft Graph データ接続を使用することで Microsoft 365 のデータを Azure にエクスポートすることができます。Azure Data Factory によって Azure Storage にコピーされたデータは分析や機械学習などに利用することができます。ただし、日本リージョンに対してはまだ提供されていません。

# Microsoft Graph API

Microsoft Graph API の特長はデータ同士がつながりを持つことです。ここでの **Graph** とはソーシャル グラフのことであり、個人や組織で表されるノードが関係性を表すエッジでつながることを示します。Microsoft 365 が提供するサービス群を個々のサービスとして考えるのではなく、個人から見たつながりとして統合的に考えることになります。

## サービス

Microsoft Graph API が提供するサービスには以下のようなものがあります。

|カテゴリー|機能|サービス|
|-|-|-|
|ID およびアクセス管理|ユーザー、グループ、ディレクトリ ロール、デバイス、テナント、ID リスク、アクセス レビュー|Azure AD|
|生産性|メール、予定表、メモ、連絡先|Outlook|
||ファイル|SharePoint、OneDrive|
||ブックとグラフ|Excel|
||To Do タスク|To Do|
|コラボレーション|コミュニケーション、チームワーク|Microsoft Teams、Skype for Business|
||サイトとリスト|SharePoint|
||タスクとプラン|Planner|
|インテリジェンス|ユーザーと職場の分析、ドキュメントの分析、プロファイル|Delve、MyAnalytics|
|デバイスおよびアプリ|クラウド印刷|ユニバーサル印刷|
||デバイスとアプリ|Intune|
||クラウド PC|Windows 365|
||デバイス更新プログラム|Windows Update|
||マルチ テナント管理|Microsoft 365 Lighthouse|
||サービス正常性と通信|-|
|セキュリティ|セキュリティ|-|
|クロスデバイス エクスペリエンス|クロスデバイス エクスペリエンス|-|
|レポート|レポート|Microsoft Teams、OneDrive、Outlook、SharePoint、Skype for Business、Yammer|
|教育|教育|-|
|ビジネス アプリケーション|予約|Microsoft Bookings|
||財務|Dynamics 365 Business Central|

## アクセス許可

前のチャプターでも説明した通り、Azure AD の仕組みを使う Microsoft Graph API は委任されたアクセス許可とアプリケーションのアクセス許可を提供します。

委任されたアクセス許可の場合は、Microsoft Graph API を呼び出すユーザーとして (つまり Azure AD にログインしたユーザーとして) 実行されます。このとき、対象のユーザーにアクセス許可がなかったり、ライセンスが付与されていない場合は、データが取得できないか失敗します。サービスごとに要件は異なりますが、例としては以下のようなものがあります。

- 他人の予定表を検索したい場合は、Microsoft Graph API の `Calendars.Read.Shared` だけではなく、対象の予定表が共有されている必要があります。また共有レベルによっては取得できない項目があります。
- 自分の OneDrive を検索したい場合は、Microsoft Graph API の `Files.Read` だけではなく、OneDrive のライセンスが付与され、個人用サイトが作成されている必要があります。

アプリケーションのアクセス許可の場合は、ユーザーではないのでそのような制限はなく、すべてのリソースにアクセスすることができます。ただし、アプリケーションのアクセス許可は非常に強い権限を持つため、取り扱いには注意する必要があります。

## URL 形式

Microsoft Graph API は REST API であるため呼び出しには URL を指定します。Microsoft Graph API の基本的な URL の形式を以下に示します。

```
{{http-method}} https://graph.microsoft.com/{{version}}/{{resource-path}}?{{query-parameters}}
```

### HTTP メソッド

Microsoft Graph API では CRUD 操作を実現するために HTTP メソッドを使用します。

|メソッド名|説明|
|-|-|
|GET|リソースからデータを読み取ります。|
|POST|リソースを作成します。リソースのメソッドを実行します。|
|PATCH|リソースの特定の値を書き込みます。|
|PUT|リソースのすべての値を書き込みます。|
|DELETE|リソースを削除します。|

### バージョン

バージョンは `v1.0` または `beta` です。運用環境では `v1.0` を使用することが推奨されます。`beta` にはプレビュー段階の新しい API が含まれますが、開発目的にのみ使用し、運用環境では使用しないことが推奨されます。`beta` でテストされた新しい API は `v1.0` として提供され一般公開となりますが、このとき API に変更がある可能性があるため注意してください。

### リソース パス

リソース パスはリソースを特定するために使用される階層構造です。「ユーザーの予定表にある予定を取得したい」といったときには、リソース パスは `/users/{{user-id}}/calendars/{{calendar-id}}/events/{{event-id}}` のようになります。一部の API では ID の代わりに他のキーを使うことができます (例えば `/users/{{user-id}}` で ID の代わりに userPrincipalName を使用することができます)。

特別なパスとしてサインインしているユーザーにアクセスするための `/me` があります。`/me` は `/users/{{signin-user-id}}` と同等です。`/me` が使用できるのは委任されたアクセス許可のみです。アプリケーションのアクセス許可では使用できません。

### クエリ パラメーター

`?` 記号のあとに `key=value` の形式でクエリ パラメーターを指定することができます。複数のパラメーターがある場合は `&` で区切ります。値は Base64 形式でエンコードするようにしてください。Microsoft Graph API では OData クエリ オプションによる汎用的なパラメーターの指定ができるほか、API によっては追加のパラメーターを要求するものもあります。

## クエリ

Microsoft Graph API は OData クエリ オプションを使用することができます。

|名前|説明|
|-|-|
|`$count`|リソースの件数を取得します。|
|`$expand`|関連リソースを取得します。|
|`$format`|結果のメディア形式を指定します。|
|`$filter`|リソースの結果をフィルターします。|
|`$orderby`|リソースの結果を並べ替えます。|
|`$search`|リソースの結果を検索します。|
|`$select`|リソースのプロパティをフィルターします。|
|`$skip`|リソースの結果の最初のデータをスキップします。|
|`$top`|リソースの結果の件数を指定します。|

ただしすべての API が完全なクエリ オプションを提供するわけではありません。API によっては[高度なクエリ機能](https://docs.microsoft.com/ja-jp/graph/aad-advanced-queries)として ConsistencyLevel ヘッダーを指定しなければならない場合もあります。API でクエリ オプションが使用できるかどうかは必ずドキュメントを確認してください。

クエリ パラメーターを使用する最大のメリットはパフォーマンスを最適化することができる点です。`$filter` および `$select` を使用すると結果のデータ量を減らすことができます。`$expand` を使用すると要求の回数を減らすことができます。

## ページング

Microsoft Graph API ではリスト データを取得するときに、一定以上のデータがある場合はすべての結果を返却せず、応答本文に最初の結果に含めて `@odata.nextLink` を送ることで、次の結果にアクセスできるようにしています。すべての結果を取得するためには `@odata.nextLink` が含まれなくなるまで要求を繰り返します。Microsoft Graph SDK では `PageIterator` によりページングを提供します。

## バッチ要求

Microsoft Graph API を使用すると非常に多数の要求が発生することがあります。これはパフォーマンスに大きな影響を与えるので、バッチ要求を使用することで、要求の回数を減らすことができます。バッチ要求はリソース パスとして `/$batch` を指定し、要求本文に JSON 形式で実際の要求を指定します。`/$batch` によって受け取った要求は既定では任意の順序で実行されますが、`dependsOn` プロパティにより順序を指定することもできます。それぞれの要求は独立しており、他の要求の結果のデータを別の要求のパラメーターとして渡すことはできません。要求 URL の長さの制限を回避するためにバッチ要求を使用することがあります。

## スロットリング

Microsoft Graph API に対して大量の要求を行った場合に HTTP 429 エラーが発生することがあります。これは大量の要求に対して API を実行せず失敗することで、 Microsoft Graph API のサービス全体に影響を与えないようにするための仕組みです。サービスごとのしきい値は[Microsoft Graph 調整ガイド](https://docs.microsoft.com/ja-jp/graph/throttling)で公開されています。HTTP 429 エラーが発生したときには `Retry-After` で指定された秒数だけ待機して再試行するようにする必要があります。Microsoft Graph SDK では `RetryHandler` によりスロットリングの再試行を提供します。

## ツール

Microsoft Graph API を実行するためのツールとして Graph エクスプローラーと Postman があります。

### Graph エクスプローラー

[Graph エクスプローラー](https://developer.microsoft.com/ja-jp/graph/graph-explorer)は Microsoft Graph API への要求の作成を確認するための Web アプリです。サンプル データで要求を実行することができるほか、サインインして自分のデータで要求を実行することもできます。ただし、サインインした場合は、サンプルではなく本当のデータに対して操作が行われるため、実行には十分に注意してください。API によっては追加のアクセス許可が必要になることもありますが、API に必要なアクセス許可を確認でき、アクセス許可を付与することができます。

### Postman

[Postman](https://www.postman.com) は Web API を開発するためのツールです。マイクロソフトの製品ではありません。Web アプリとデスクトップ アプリが提供されています。Postman では自分で登録した Azure AD アプリケーションで動作させることができます。また委任されたアクセス許可だけでなくアプリケーションのアクセス許可で実行することもできます。

# Microsoft Graph API を使った開発

## Microsoft Graph SDK

Microsoft Graph API へのアクセスを HTTP で呼び出すことで自分のアプリケーションに組み込むことも可能ですが、現実的には HTTP の呼び出しをライブラリ化した SDK を使用するのが一般的です。前のセクションでも触れましたが、Microsoft Graph SDK は再試行処理、セキュリティで保護されたリダイレクト、透過的な認証、ペイロード圧縮、ページング、およびバッチ リクエストの作成などの機能が組み込まれており、開発者は簡単に高品質なアプリケーションを作成することができます。

Microsoft Graph SDK は .NET、PowerShell、JavaScript、Java、Go、PHP、Python といった複数の言語向けに提供されています。

## Microsoft Graph TypeScript Types

## Microsoft Graph Toolkit
