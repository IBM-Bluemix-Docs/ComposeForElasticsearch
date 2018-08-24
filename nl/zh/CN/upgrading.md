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

# 升级 Elasticsearch
{: #upgrading}

为保持服务安全且最新，将发布作为 {{site.data.keyword.composeForElasticsearch_full}} 基础的新版本 Elasticsearch 引擎。建议在新版本可用时进行升级。在新版本的 {{site.data.keyword.composeForElasticsearch}} 可用时，将在_概述_页面上显示一个通知。

## 次版本
在几乎所有情况下，次版本升级都将就地提供。您能够升级 Elasticsearch，而不必迁移任何数据，并且停机时间最短。您可以在服务的_设置_选项卡中，通过从下拉菜单中选择要升级到的版本进行升级。

## 主版本
目前有三个主版本的 Elasticsearch 可用于 {{site.data.keyword.composeForElasticsearch}}：2.4.6、5.x 和 6.x。目前可以：
- 从 Elasticsearch 5.x 迁移到 6.x
- 从 Elasticsearch 2.x 迁移到 5.x。

所有主版本之间的迁移过程都会使用最新备份或按需备份，将数据复原到新部署中。此过程具有诸多优点：

- 原始数据库保持运行，并且可以不中断生产工作。
- 可以在生产环境外部测试新数据库，并对任何应用程序不兼容性采取行动。
- 可在任意点重新运行整个过程。
- 全新复原可降低将旧版数据库中不需要工件迁移到新数据库的可能性。

### 迁移注意事项

大多数主版本都会包含一些可能影响 Elasticsearch 使用方式的更改。在迁移到 Elasticsearch 6.x 时尤其如此。请在升级前、测试新版本期间以及取消供应旧版本之前，牢记这些事项。

#### 5.x 前的索引
Elasticsearch 通常支持读取在先前主版本中创建的索引。这意味着 Elasticsearch 6.x 可读取在 Elasticsearch 5.x 中创建的任何索引。

如果您有在 Elasticsearch 2.x 中创建的索引，那么在 Elasticsearch 6.x 中**不**可读取这些索引。这些索引需要在 Elasticsearch 5.x 中进行重建，然后再迁移到 6.x。如果数据包含在 Elasticsearch 2.x 中生成的索引，并且尝试迁移到 Elasticsearch 6.x，那么节点将无法启动并且迁移会失败。

迁移到 6.x 之前，可以通过 [重建索引 API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html) 在 Elasticsearch 5.x 中[就地重建索引](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html)。
{: .tip}

#### 索引映射

Elasticsearch 6.x 中除去了为每个索引创建多个映射类型的功能。在 Elasticsearch 5.x 中创建的任何多索引映射将继续生效，但是 Elasticsearch 6.x 仅支持为每个索引创建单个映射类型。

在 Elasticsearch 7.x 以及更高版本中，将除去映射类型。

#### 其他更改

有关重大更改的完整文档，请访问 [Elasticsearch 文档](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/breaking-changes-6.0.html)。

### 迁移到新版本

要迁移到新版本的 Elasticsearch，您需要 [{{site.data.keyword.cloud_notm}} CLI](https://console.{DomainName}/docs/cli/index.html#overview)。此过程非常类似于[通过 CLI 复原备份](./dashboard-backups.html#restoring-via-cli)，只是在此过程中将在 JSON 中使用“db_version”字段来指定要升级到的版本。完整命令为：

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

例如，将运行 5.x 的 {{site.data.keyword.composeForElasticsearch}} 服务复原到运行 Elasticsearch 6.2.2 的新服务类似于以下示例：

```
ibmcloud service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
```
