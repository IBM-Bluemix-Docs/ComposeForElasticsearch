---

Copyright:
  Years: 2017
lastupdated: "2017-09-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ダッシュボードの概要

サービス・ダッシュボードから {{site.data.keyword.composeForElasticsearch_full}} サービスを管理できます。

{{site.data.keyword.cloud}} Compose データベースに関する情報は_「概要」_ページに表示されます。概要には、必要不可欠な識別情報と現在のリソース使用率が含まれます。ツールで使用する接続ストリングのセクションもあります。接続ストリングを使用してツールからデータベースに接続することもできます。

## デプロイメントの詳細 (Deployment Details)

_「デプロイメントの詳細 (Deployment Details)」_パネルには、サービスの詳細が表示されます。

![「デプロイメントの詳細 (Deployment Details)」](./images/elastic_search-deployment-details.png "「デプロイメントの詳細 (Deployment Details)」パネルのビュー")

### タイプ

サービスによって提供されるデータベースのタイプ、およびサービスが使用するデータベースのバージョン。

### 名前

サービスの内部 ID。

### 使用法

データベース・サイズとご使用のサービス・プランで利用可能なストレージ容量。


## 接続ストリング (Connection Strings)

接続ストリングはいくつかのクライアント・ライブラリーで使用でき、他のライブラリーが接続するために必要なすべての情報を含んでいます。接続ストリングを使用してサービスに接続する方法については、[外部アプリケーションの接続](./connecting-external.html)を参照してください。

_「接続ストリング」_パネルの各タブで、サービスで使用できるそれぞれの接続ストリングを確認できます。

### HTTPS

いくつかのクライアント・ライブラリーで使用できる URI フォーマットの接続ストリングで、他のライブラリーが接続するために必要なすべての情報を含んでいます。接続ストリングを使用して接続する方法については、[外部アプリケーションの接続](./connecting-external.html)を参照してください。

### 正常性

Elasticsearch クラスターの正常性を調べるために使用できる呼び出し例。
