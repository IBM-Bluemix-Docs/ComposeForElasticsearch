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

# 升級 Elasticsearch
{: #upgrading}

為了使您的服務保持安全且最新，將會發表作為 {{site.data.keyword.composeForElasticsearch_full}} 基礎的新版本 Elasticsearch 引擎。建議您在提供新版本時便升級。提供新版本的 {{site.data.keyword.composeForElasticsearch}} 時，在_概觀_ 頁面上會出現通知。

## 次要版本
在幾乎所有情況下，將可以就地進行次要版本升級。您將能夠升級 Elasticsearch 而不必移轉任何資料，且能夠維持最短的關閉時間。您可以從服務的_設定_ 標籤升級，方法是從下拉功能表選取您要升級到的版本。

## 主要版本
目前有三個 Elasticsearch 主要版本可供 {{site.data.keyword.composeForElasticsearch}} 使用：2.4.6、5.x 及 6.x。目前可以：
- 從 Elasticsearch 5.x 移轉至 6.x
- 從 Elasticsearch 2.x 移轉至 5.x

在所有主要版本之間移轉的處理程序會使用最近或隨需應變備份，以將資料還原到新的部署。這個處理程序能提供許多優點：

- 原始資料庫會維持執行，正式作業工作則可以不間斷。
- 您可以在正式作業之外測試新資料庫，並處理任何應用程式不相容。
- 整個處理程序可以在任意位置重新執行。
- 全新還原能減少不需要的舊版本資料庫構件帶入新資料庫的可能性。

### 移轉考量

大部分主要版本會包含可能影響 Elasticsearch 使用的部分變更。尤其是移轉至 Elasticsearch 6.x。在升級之前、測試新版本時，以及取消佈建舊版本之前，請謹記這些事情。

#### 5.x 版以前的索引
Elasticsearch 通常支援讀取在先前主要版本中建立的索引。這表示在 Elasticsearch 5.x 中建立的任何索引都可以由 Elasticsearch 6.x 讀取。

如果您有在 Elasticsearch 2.x 建立的索引，它們在 Elasticsearch 6.x 中**無法**讀取。那些索引將需要在 Elasticsearch 5.x 中重新檢索，然後才能移轉至 6.x。如果您的資料包含在 Elasticsearch 2.x 建立的索引，而您試圖移轉至 Elasticsearch 6.x，則節點將無法啟動，移轉將會失敗。

[重新檢索](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html)可以在 Elasticsearch 5.x 中就地執行，然後才透過 [Reindex API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html) 移轉至 6.x。
{: .tip}

#### 索引對映

針對每個索引建立多個對映類型的功能在 Elasticsearch 6.x 中已經移除。在 Elasticsearch 5.x 中建立的任何多索引對映將持續運作，但 Elasticsearch 6.x 只支援每個索引建立單一對映類型。

對映類型將在 Elasticsearch 7.x 及更新版本中移除。

#### 其他變更

岔斷變更的完整文件可以在 [Elasticsearch 文件](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/breaking-changes-6.0.html)找到。

### 移轉至新版本

若要移轉至新版本的 Elasticsearch，您將需要 [{{site.data.keyword.cloud_notm}} CLI](https://console.{DomainName}/docs/cli/index.html#overview)。處理程序相當類似於[透過 CLI 還原備份](./dashboard-backups.html#restoring-via-cli)，只除了您將使用 JSON 中的 "db_version" 欄位來指定您要升級到哪個版本。完整指令為：

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

例如，將執行 5.x 的 {{site.data.keyword.composeForElasticsearch}} 服務還原至執行 Elasticsearch 6.2.2 的新服務，看起來如下：

```
ibmcloud service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
```
