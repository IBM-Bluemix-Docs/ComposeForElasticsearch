---

copyright:
  years: 2016,2018
lastupdated: "2018-07-25"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Elasticsearch のアップグレード
{: #upgrading}

サービスを安全で最新の状態に保つため、{{site.data.keyword.composeForElasticsearch_full}} の基盤となる Elasticsearch エンジンの新バージョンがリリースされます。新バージョンを入手できるようになったら、アップグレードすることをお勧めします。新バージョンの {{site.data.keyword.composeForElasticsearch}} を入手できるようになったら、_「概要」_ページに通知が表示されます。

## マイナー・バージョン
ほとんどすべての場合、マイナー・バージョンのアップグレードはインプレースで使用できます。データをマイグレーションすることなく最小のダウン時間で Elasticsearch をアップグレードできます。アップグレードは、サービスの_「設定」_タブのドロップダウン・メニューからアップグレード先のバージョンを選択することによって行えます。

## メジャー・バージョン
現在のところ、{{site.data.keyword.composeForElasticsearch}} で使用できる Elasticsearch のバージョンとしては、2.4.6、5.x、6.x という 3 つのメジャー・バージョンがあります。現在、次のような選択肢があります。
- Elasticsearch 5.x から 6.x にマイグレーションする
- Elasticsearch 2.x から 5.x にマイグレーションする

異なるメジャー・バージョン間で行うすべてのマイグレーション・プロセスにおいて、新しいデプロイメントにデータをリストアするための、最新バックアップまたはオンデマンド・バックアップが使用されます。このプロセスには、いくつかの利点があります。

- 元のデータベースが実行されたままなので、実動作業を中断せずに実行できる。
- 実動とは別に新規データベースをテストして、アプリケーションの非互換性がないかを確認できる。
- 任意の時点でプロセス全体をやり直すことができる。
- 新規の状態でリストアされるので、古いバージョンのデータベースに存在する不必要な成果物が新規データベースに持ち越される可能性が減る。

### マイグレーションにおける考慮事項

ほとんどのメジャー・バージョンには、Elasticsearch の使用方法に影響を与える可能性がある変更が含まれています。Elasticsearch 6.x へのマイグレーションでは、この点が顕著です。アップグレードの前、新規バージョンのテスト中、古いバージョンのプロビジョニング解除の前には、この点を念頭に置いてください。

#### 5.x より前の索引
通常、Elasticsearch は、直前のメジャー・バージョンで作成された索引の読み取りをサポートします。つまり、Elasticsearch 5.x で作成された索引は Elasticsearch 6.x で読み取ることができます。

Elasticsearch 2.x で作成された索引がある場合、Elasticsearch 6.x で読み取ることは**できません**。そのような索引は、Elasticsearch 6.x にマイグレーションする前に 5.x で再索引付けする必要があります。Elasticsearch 2.x で作成された索引がデータに含まれる場合、Elasticsearch 6.x にマイグレーションしようとすると、ノードの開始に失敗してマイグレーションに失敗します。

Elasticsearch 6.x にマイグレーションする前に、[再索引付け API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html) を使用して 5.x で行う[再索引付けはインプレースで行えます](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html)。
{: .tip}

#### 索引マッピング

索引ごとに複数のマッピング・タイプを作成する機能は、Elasticsearch 6.x で除去されました。Elasticsearch 5.x で作成された複数の索引マッピングは引き続き機能しますが、Elasticsearch 6.x では索引ごとに 1 つのマッピング・タイプの作成しかサポートしません。

マッピング・タイプは Elasticsearch 7.x 以上では除去されます。

#### その他の変更

互換性を破る変更に関する文書全体は、[Elasticsearch docs](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/breaking-changes-6.0.html) から入手できます。

### 新規バージョンへのマイグレーション

新しいバージョンの Elasticsearch にマイグレーションするには、[{{site.data.keyword.cloud_notm}} CLI](https://console.{DomainName}/docs/cli/index.html#overview) が必要です。このプロセスは、[CLI を使用したバックアップのリストア](./dashboard-backups.html#restoring-via-cli)に非常に類似していますが、JSON の「db_version」フィールドを使用してアップグレード先のバージョンを指定する点は異なります。コマンド全体は次のようになります。

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

例えば、5.x を実行する {{site.data.keyword.composeForElasticsearch}} サービスを Elasticsearch 6.2.2 を実行する新規サービスにリストアする場合は、次のようにします。

```
ibmcloud service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
```
