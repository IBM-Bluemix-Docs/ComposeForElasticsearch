---

copyright:
  years: 2016,2017
lastupdated: "2017-10-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.composeForElasticsearch}} の概説
{: #getting-started-with-compose-for-elasticsearch}

{{site.data.keyword.composeForElasticsearch_full}} は、フルテキスト検索エンジンの能力と JSON 文書データベースの索引作成の強みを組み合わせたものです。この組み合わせにより、大量のデータの多様な分析を可能にする、強力なツールを生み出します。Elasticsearch を利用することで、検索時の一致度をスコア化できるため、データ・セットを探索していて見逃しかねない類似一致やニアミスを検出できるようになります。
{:shortdesc}

**注:** 2016 年 9 月 14 日より前にプロビジョンされた、現在もアクティブな Compose サービス・インスタンスは引き続き使用可能で、[https://www.compose.com/](https://www.compose.com) から直接アクセスできます。その時点以降にプロビジョンされた Compose サービス・インスタンスは、{{site.data.keyword.cloud}} アカウント内で直接アクセスして使用されます。

## Compose for Elasticsearch サービス・インスタンスの作成

[{{site.data.keyword.composeForElasticsearch}} インスタンスを作成します](https://console.bluemix.net/catalog/services/compose-for-elasticsearch/)。

サービスのインスタンスを作成するときは、必ずサービスの名前と資格情報名の両方を選択してください。サービスをバインドしないでおきます。サービスをプロビジョンするときに指定される資格情報を使用して、アプリケーションをサービスに接続できます。*使用可能な資格情報*セクションには、各種の資格情報値がリストされています。

{{site.data.keyword.composeForElasticsearch}} インスタンスをプロビジョンするときに、*「標準」*プランか*「エンタープライズ (Enterprise)」*プランかを選択できます。*「エンタープライズ (Enterprise)」*プランを選択した場合は、使用可能な {{site.data.keyword.composeEnterprise}} クラスターに {{site.data.keyword.composeForElasticsearch}} インスタンスをプロビジョンできます。{{site.data.keyword.composeEnterprise}} は、企業コンプライアンスで要求されるセキュリティーと分離を提供し、専用ネットワーキングを使用してデプロイ済みデータベースのパフォーマンスを確保します。詳しくは、[Compose Enterprise 文書](../ComposeEnterprise/index.html)を参照してください。

## Compose for Elasticsearch の管理

サービス・ダッシュボードからサービスを管理できます。ダッシュボードには {{site.data.keyword.cloud}} Compose データベースとそれへの接続方法に関する情報が示されます。また、以下の操作を行うこともできます。

- バックアップを管理する。
- サービスに割り振るリソースを増やす 
- ホワイトリストを使用してデータベースへのアクセスを制限する。

詳しくは、[設定](./dashboard-settings.html)を参照してください。

## {{site.data.keyword.composeForElasticsearch}} への接続

サービスに接続するには、サービスと一緒に作成された資格情報を使用するか、サービス・ダッシュボードの*「概要」*タブに表示される接続ストリングとコマンド・ラインを使用します。

## {{site.data.keyword.composeForElasticsearch}} への {{site.data.keyword.cloud_notm}} アプリケーションの接続

{{site.data.keyword.cloud_notm}} アプリケーションをサービスに接続するには、サービスと一緒に作成された資格情報を使用します。{{site.data.keyword.composeForElasticsearch}} サービスに {{site.data.keyword.cloud_notm}} アプリケーションを接続する方法は、[{{site.data.keyword.cloud_notm}} アプリケーションの接続](./connecting-bluemix-app.html)に記載されています。

## {{site.data.keyword.cloud_notm}} 外からの {{site.data.keyword.composeForElasticsearch}} への接続

{{site.data.keyword.cloud_notm}} 外から {{site.data.keyword.composeForElasticsearch}} に接続する場合は、示された接続ストリングまたはコマンド・ラインを使用できます。接続方法については、[外部アプリケーションの接続](./connecting-external.html)を参照してください。
