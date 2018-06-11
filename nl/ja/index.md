---

copyright:
  years: 2016,2018
lastupdated: "2018-03-26"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.composeForElasticsearch}} の概要
{: #about-compose-for-elasticsearch}

{{site.data.keyword.composeForElasticsearch_full}} は、フルテキスト検索エンジンの能力と JSON 文書データベースの索引作成の強みを組み合わせたものです。 この組み合わせにより、大量のデータの多様な分析を可能にする、強力なツールを生み出します。 Elasticsearch を利用することで、検索時の一致度をスコア化できるため、データ・セットを探索していて見逃しかねない類似一致やニアミスを検出できるようになります。
{:shortdesc}

**注:** 2016 年 9 月 14 日より前にプロビジョンされた、現在もアクティブな Compose サービス・インスタンスは引き続き使用可能で、[https://www.compose.com/](https://www.compose.com) から直接アクセスできます。 その時点以降にプロビジョンされた Compose サービス・インスタンスは、{{site.data.keyword.cloud}} アカウント内で直接アクセスして使用されます。

## {{site.data.keyword.composeForElasticsearch}} サービス・インスタンスの作成

{{site.data.keyword.composeForElasticsearch}} サービスは、{{site.data.keyword.cloud_notm}} カタログの [{{site.data.keyword.composeForElasticsearch}} ページ](https://console.{DomainName}/catalog/services/compose-for-elasticsearch/)から作成できます。

サービス名、およびサービスをプロビジョンする地域、組織、スペースを選択します。 **「データベースのバージョンの選択 (Select a database version)」**フィールドを使用して、データベースの使用できるバージョンを選択できます。

{{site.data.keyword.composeForElasticsearch}} インスタンスをプロビジョンするときに、*「標準」*プランか*「エンタープライズ (Enterprise)」*プランかを選択できます。 *「エンタープライズ (Enterprise)」*プランを選択した場合は、使用可能な {{site.data.keyword.composeEnterprise}} クラスターに {{site.data.keyword.composeForElasticsearch}} インスタンスをプロビジョンできます。 {{site.data.keyword.composeEnterprise}} は、企業コンプライアンスで要求されるセキュリティーと分離を提供し、専用ネットワーキングを使用してデプロイ済みデータベースのパフォーマンスを確保します。 詳しくは、[Compose Enterprise 文書](../ComposeEnterprise/index.html)を参照してください。

## {{site.data.keyword.composeForElasticsearch}} の管理

サービス・ダッシュボードからサービスを管理できます。 ダッシュボードには {{site.data.keyword.cloud}} Compose データベースとそれへの接続方法に関する情報が示されます。 また、以下の操作を行うこともできます。

- バックアップを管理する。
- サービスに割り振るリソースを増やす 
- ホワイトリストを使用してデータベースへのアクセスを制限する。

詳しくは、[設定](./dashboard-settings.html)を参照してください。

{{site.data.keyword.composeForElasticsearch}} は、Cloud Foundry の役割を利用して、サービスへのアクセスを管理します。開発者役割を持つユーザーのみが、サービス・ダッシュボードを表示または使用できます。Cloud Foundry の役割について詳しくは、『[Cloud Foundry アクセス権限](https://console.bluemix.net/docs/iam/cfaccess.html#cfaccess)』および 『[Cloud Foundry のアクセス権限の管理](https://console.bluemix.net/docs/iam/mngcf.html#mngcf)』のページを参照してください。
{: .tip}

## {{site.data.keyword.composeForElasticsearch}} への接続

サービスに接続するには、サービスと一緒に作成された資格情報を使用するか、サービス・ダッシュボードの*「概要」*タブに表示される接続ストリングとコマンド・ラインを使用します。

## {{site.data.keyword.composeForElasticsearch}} への {{site.data.keyword.cloud_notm}} アプリケーションの接続

{{site.data.keyword.cloud_notm}} アプリケーションをサービスに接続するには、サービスと一緒に作成された資格情報を使用します。 {{site.data.keyword.composeForElasticsearch}} サービスに {{site.data.keyword.cloud_notm}} アプリケーションを接続する方法は、[{{site.data.keyword.cloud_notm}} アプリケーションの接続](./connecting-bluemix-app.html)に記載されています。

## {{site.data.keyword.cloud_notm}} 外からの {{site.data.keyword.composeForElasticsearch}} への接続

{{site.data.keyword.cloud_notm}} 外から {{site.data.keyword.composeForElasticsearch}} に接続する場合は、示された接続ストリングまたはコマンド・ラインを使用できます。 接続方法については、[外部アプリケーションの接続](./connecting-external.html)を参照してください。
