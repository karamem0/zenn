---
title: "Microsoft 365 開発の概要"
---

# Microsoft 365 とは

Microsoft 365 開発をはじめる上で、その前に Microsoft 365 とは何かを理解しておくことは重要です。Microsoft 365 は、マイクロソフトによって提供される、個人、職場、または学校での生産性を高め、コラボレーションを容易にし、セキュリティを高めるための、クラウド サービスの総称です。Microsoft 365 は以下のサービスから構成されています。なお、Microsoft 365 のプランによっては提供されないものもありますので、詳しくはマイクロソフトのドキュメントをご確認ください。

- Office 365
- Windows
- Enterprise Mobility + Security

それぞれのサービスについてもう少し細かく見ていきましょう。

## Office 365

Office 365 にはチーム ワークによる生産性を高めるためのさまざまなサービスが含まれています。

|製品名|説明|
|-|-|
|Office アプリ|Excel、Word、PowerPoint を使ってドキュメントを作成する。|
|Exchange (Outlook)|メールでコミュニケーションする。予定表によりスケジュールを管理する。|
|SharePoint|ファイルやナレッジを共有するためのポータルを提供する。|
|OneDrive|個人のファイルを管理し共有する。|
|Microsoft Teams|チームのコラボレーションを最適化するためのチャット、ビデオ会議、通話、およびファイル共有を提供する。|
|Planner|プロジェクトのタスクを管理する。|
|Yammer|組織の同僚とさまざまなトピックでアイデアを交換する。|

ここに紹介した以外にも多くのサービスが存在します。

## Windows

Windows はシェア率がトップのデスクトップ向けの OS です。現在は Windows 11 が最新版として提供されています。Microsoft 365 では Windows 10/11 Enterprise のライセンスが提供されます。

## Enterprise Mobility + Security

Enterprise Mobility + Security には組織でのセキュリティを保つためのさまざまなサービスが含まれています。

|製品名|説明|
|-|-|
|Azure Active Directory|組織に所属するユーザーの資格情報やアクセス許可を管理する。|
|Azure Information Protection|データを分類、ラベル付け、および保護する。|
|Intune|組織に接続するデバイスを管理する。|

ここに紹介した以外にも多くのサービスが存在します。

# Microsoft 365 のプラン

Microsoft 365 のプランとしては個人向け、中小企業向け、大規模企業向けのほか、特定の団体 (教育機関、政府機関、非営利団体) 向けのプランがあります。なおライセンスの詳しい情報については必ずマイクロソフトの公式サイトやセールス担当にお問い合わせください。

## 個人向け

[個人向け](https://www.microsoft.com/ja-jp/microsoft-365/buy/compare-all-microsoft-365-products)の Microsoft 365 はマイクロソフト アカウント (MSA) に適用されます。組織アカウントでは利用できません。サブスクリプションとして提供される Microsoft 365 Personal と一回限りの購入となる Office Home & Business 2021 および Office Personal 2021 があります。

## 中小企業向け

[中小企業向け](https://www.microsoft.com/ja-jp/microsoft-365/business/compare-all-microsoft-365-business-products)の Microsoft 365 プランは 300 ユーザーまでの企業に対して提供されています。300 ユーザーを超える場合は大企業向けのプランを購入する必要があります。

- Microsoft 365 Apps for business
- Microsoft 365 Business Basic
- Microsoft 365 Business Standard
- Microsoft 365 Business Premium

## 大企業向け

[大企業向け](https://www.microsoft.com/ja-jp/microsoft-365/compare-microsoft-365-enterprise-plans)の Microsoft 365 プランには一般の従業員向けの E シリーズと現場担当者向けの F シリーズが存在します。

- Microsoft 365 E3
- Microsoft 365 E5
- Microsoft 365 F3

# Microsoft 365 開発とは

これまで見た通り Microsoft 365 にはさまざまなサービスが存在します。しかし、組織によっては、これらのサービスではフィットせず、組織にカスタマイズされた機能を使いたいということもあるでしょう。たとえば、以下のようなユース ケースが考えられます。

- 自社開発した予約システムでスケジュールをユーザーの Outlook 予定表に連携したい。
- Excel で収集したフォームのデータをデータベースに連携したい。
- SharePoint に自社開発した API のデータを表示させたい。

これらの要件に応えるため、Microsoft 365 にはカスタム開発を行うための API や SDK が提供されています。

[Microsoft 365 開発者プログラム](https://developer.microsoft.com/ja-jp/microsoft-365/dev-program)では、以下の 6 つのテクノロジーが定義されています。

|テクノロジー名|説明|
|-|-|
|Microsoft ID プラットフォーム|Azure AD で保護されたアプリケーションを開発するためのフレームワーク|
|Microsoft Graph|Microsoft 365 の統合された API|
|SharePoint Framework|SharePoint を拡張するためのフレームワーク|
|Microsoft Teams|Microsoft Teams を拡張するためのフレームワーク|
|Office アドイン|Office アプリを拡張するためのフレームワーク|
|アダプティブ カード|リッチ コンテンツを簡単に定義するためのフレームワーク|

以降の章ではそれぞれのテクノロジーについて解説していきますが、それぞれなにができるのかを簡単に確認していきましょう。

## Microsoft ID プラットフォーム

Azure AD には、ユーザーやグループを管理するだけでなく、組織で使用するアプリケーションを管理する機能があります。マイクロソフトやサードパーティーが提供するアプリケーション (Office 365 のサービスもマイクロソフトが提供する Azure AD アプリケーションのひとつです) のほかに、組織が開発したアプリケーションも Azure AD アプリケーションとすることができます。ユーザーは Azure AD の資格情報でアプリケーションにサインインすることができるようになります。アプリケーションはユーザーの資格情報を管理する必要がないため、セキュリティ リスクになることを防ぐことができます。また、Azure AD アプリケーションから別の Azure AD アプリケーションへのアクセスも行うことができます。これを実現するため、Azure AD では OAuth や OpenID Connect の仕組みが使われています。

マイクロソフトからは Azure AD での OAuth をサポートするための SDK である MSAL (Microsoft Authentication Library) を提供しています。なお、かつて ADAL (Azure Active Directory Authentication Library) を提供していましたが、こちらは現在ではサポートされておりません。

## Microsoft Graph

Microsoft Graph は Microsoft 365 のデータへのアクセスを提供します。その中でも Microsoft Graph API は Microsoft 365 の統合された API エンドポイントです。これまでは、それぞれのサービスごとに API が提供され、サービスごとの連携も難しかったのですが、Microsoft Graph API はサービスの枠を超えてシームレスにアクセスすることができるようになりました。たとえば、以下のようなことが実現できます。

- 私の同僚がよく使っているファイルを検索する
- 私のマネージャーに会議開催依頼を送信する
- 私の所属する Microsoft 365 グループのタスクを取得する

Microsoft Graph API は REST API で提供されているため、HTTP プロトコルを使ってアクセスできますが、簡単にアクセスできるための SDK も提供されています。

## SharePoint Framework

SharePoint Framework は SharePoint を拡張するための新しい方法です。SharePoint Framework では Node.js や TypeScript といったテクノロジーを使用して開発することができます。開発したパッケージはテナントまたはサイト コレクションに展開することができ、コードはクライアント側で実行されます。また、SharePoint Framework の Web パーツは Microsoft Teams のタブとしても表示することができます。

SharePoint Framework のライブラリは npm で提供されています。また SharePoint Framework を簡単にはじめるためのジェネレーターが提供されています。

## Microsoft Teams

Microsoft Teams はチーム作業の生産性を最大化するための製品ですが、カスタム アプリをインストールすることによって、カスタムの機能を追加することができます。チームやグループ チャットにカスタムのタブを追加する、メッセージを拡張する、ボットを追加する、コネクタによりメッセージを送信する、会議のエクスペリエンスを向上させるなど、さまざまなカスタマイズをすることができます。

Microsoft Teams のライブラリは npm で提供されています。また Visual Studio や Visual Studio Code の拡張機能や CI/CD のための CLI などを含む Teams Framework (TeamsFx) が提供されています。

## Office アドイン

Office アドインは Excel、Word、PowerPoint および Outlook といった Office アプリを拡張するための新しい方法です。Office アドインでは HTML や JavaScript などのテクノロジーを使用して開発することができます。Office アドインは Web 版の Office またはデスクトップ版の Office アプリのいずれでも実行できます。

Office アドインのライブラリは npm で提供されています。また Office アドインを簡単にはじめるためのジェネレーターが提供されています。

## アダプティブ カード

アダプティブ カードはプラットフォームに依存しない UI 表示のためのフレームワークです。アダプティブ カードは JSON 形式で記述することができ、Microsoft Teams、Outlook および Bot Framework などで表示することができます。またカスタムのアプリケーションにアダプティブ カードを表示することもでき、レンダリングのための SDK が提供されています。

# Microsoft 365 開発のテクノロジー

これらの Microsoft 365 開発のテクノロジーは個別に存在するものではありません。SharePoint Framework や Microsoft Teams の開発をするときに、Microsoft ID プラットフォームによる OAuth の仕組みを理解することは必要ですし、Microsoft Graph を使ってさまざまな操作をすることもあります。Microsoft Viva コネクションのダッシュボードでは SharePoint Framework とアダプティブ カードを使ってカードを表示します。最適なソリューションを実現するには、どれかひとつのみを理解すればいいというのではなく、広い視野を持って理解しなければなりません。

また、いずれのテクノロジーにおいても最新の Web フロントエンド技術が使われていることは注目すべき点であるといえるでしょう。かつてのマイクロソフト技術は C# (.NET Framework) や XML などが使われてきました。これがマイクロソフトの方針の転換により、他のオープンソース プロジェクトを積極的に採用するようになりましたし、マイクロソフト自身の製品もオープンソース化するという流れが進みました。Microsoft 365 開発も Node.js や JSON を使うようになっています。これまで C# で Exchange、SharePoint、または Office 開発をしてきた開発者にとっては、今まで培ってきたスキルが使えなくなることに不安を感じることもあるかもしれませんが、Web フロントエンド技術を理解することで、ほかにも応用が効くようになると前向きに捉えていただきたいと思っています。

開発技術の変化は DevOps の観点においてもメリットがあります。かつての SharePoint ソリューション開発ではアプリのビルドに SharePoint をインストールした Windows サーバーを用意しなければならず、CI/CD が困難でした。現在の SharePoint Framework は Azure DevOps や GitHub を使って簡単にビルドすることができます。ビルド環境が Windows である必要もありません。モダンなアプリケーション開発においては、アプリケーションは作ったら終わりではなく、環境の変化に合わせて常にアップデートしていく必要があります。Microsoft 365 開発においても DevOps は非常に重要ですので、ぜひ実践していただければと思います。

# Microsoft 365 開発のベスト プラクティス

Microsoft 365 開発をはじめるとなったときに、その決定に至るまでには、何かしらの理由があったものと思います。ただ、一般的にアプリケーション開発は大変な労力もかかりますし、必ずしも成功するとも限りません。もし熟慮の末の決定でなかったとすれば、Microsoft 365 開発をすることが妥当かどうかを判断するために、以下のチェック リストを確認していただければと思います。

- 標準で提供されている機能で要件は実現できないでしょうか。業務に合わせてアプリを作るのではなく、アプリに合わせて業務を変えるべきです。
- サードパーティーが提供している製品で要件は実現できないでしょうか。車輪の再発明をする必要はありません。
- Power Platform または Dynamics 365 で要件は実現できないでしょうか。Power Platform および Dynamics 365 はビジネス アプリケーションを作成するのに適しています。後述するように、Microsoft 365 に付属する Power Platform では向いていない場合もありますが、ライセンスを購入することで利用できるモデル駆動型アプリやビジネス プロセス フローを使うことで、問題が解決する可能性があります。

# Microsoft 365 開発と Power Platform との比較

Microsoft 365 をカスタマイズするために Power Platform を使う方法もあります。Power Platform は以下の製品で構成されています。

|製品名|説明|
|-|-|
|Power Apps|ビジネス アプリケーションの作成|
|Power Automate|ビジネス フローの自動化|
|Power BI|データの可視化|
|Power Virtual Agents|ボットの作成|

Power Platform は Microsoft 365 のソリューションではありませんが、Microsoft 365 との親和性が高いです。Microsoft 365 のライセンスがあれば Microsoft 365 の範囲内で利用することができるため、Power Apps や Power Automate を使ってアプリケーションを構築することもできます。ただし、Power Platform はあくまで市民開発者向けのソリューションであり、本格的なアプリケーションの作成には向いていません。Power Platform でよくある問題点としては以下のようなものがあります。

- アプリやフローがユーザーに紐づけられるため、ユーザーが退職したときに、アプリケーションが使用できなくなる可能性がある。
- アプリやフローが呼び出す API の実行回数に制限がある。
- アプリやフローのパフォーマンスが悪くデータの取得に時間がかかる。
- アプリのデザインのカスタマイズに限界がある。

このうち API の実行回数やパフォーマンスについてはライセンスを購入することで解決するものもあります。しかし SharePoint や Microsoft Teams と統合された UI を提供したい、または複雑な処理を必要とするような場合には Microsoft 365 開発を選択する必要があります。

# Microsoft 365 開発者プログラム

最後に [Microsoft 365 開発者プログラム](https://developer.microsoft.com/ja-jp/microsoft-365/dev-program)について触れておきましょう。

Microsoft 365 開発者プログラムに参加すると、25 ユーザーの Microsoft 365 E5 開発者ライセンスを無料で 90 日間利用することができます。この期限は使用している限り無期限に 90 日ごとに更新されます。また Visual Studio のサブスクライバーは Visual Studio のサブスクリプションの有効期限にあわせて Microsoft 365 開発者プログラムを利用できます。Visual Studio のサブスクリプションの特典に Azure のサブスクリプションも含まれますので、Microsoft 365 と Azure を使った開発環境を構築することができます。

注意点として、Microsoft 365 E5 開発者ライセンスは既存のテナントに追加することはできず、新しいテナントを作成する必要があります。すでに Azure のサブスクリプションがあるような場合、そのテナントに Microsoft 365 E5 開発者ライセンスを追加することはできません。同じテナントで使いたい場合は、Microsoft 365 開発者プログラムで作成したテナントに Azure のサブスクリプションを移動する必要があります。

すぐに開発をはじめたい人のためのサンプル データ パックが提供されています。英語になってしまうのが残念ではありますが、サンプル データを用意することなく開発をはじめられるのでぜひ活用してみてください。
