---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# バージョン

デプロイ可能バージョン| 推奨バージョン
----------|-----------
2.4.6、5.6.9、6.2.2 | 6.2.2
{: caption="表 1. Elasticsearch バージョン" caption-side="top"}

## 推奨バージョン

通常、推奨バージョンは、{{site.data.keyword.composeForElasticsearch}} で使用可能な Redis データベースの最新バージョンです。 推奨バージョンは、カタログ・ページ上のバージョン・セレクターのデフォルトです。API の呼び出しでバージョンを指定しない場合は、自動的にこのバージョンがプロビジョンされます。

### 新しい推奨バージョンのプロトコル

新しいバージョンが入手可能になると、そのリリースが発表され、デプロイメントでその新しいバージョンを使用できるようになります。 リリース日以降、最新バージョンを入手できる期間が約 7 日間ありますが、これは推奨バージョンではありません。 ユーザーはこの期間に新しいバージョンをデプロイしたりテストしたりできますが、引き続き現行バージョンも使用できます。 さらにこの期間に、Compose エンジニアが新しいバージョンで発生した問題を発見して修正することもできます。 この 7 日間の期間の終わりに、新しいバージョンが推奨バージョンとして設定されるか、この変更に関する新しい日付が発表されます。

プロビジョニングで使用可能なバージョンのリストは、{{site.data.keyword.composeForMongoDB}} の[カタログ・ページ](https://console.{DomainName}/catalog/services/compose-for-mongodb)にあります。

ご使用の {{site.data.keyword.composeForElasticsearch}} サービスで使用できるバージョンの現行リストを取得するには、
[GET /2016-07/deployments/:id/versions](https://apidocs.compose.com/v1.0/reference#2016-07-get-deployments-versions) エンドポイントを使用できます。

