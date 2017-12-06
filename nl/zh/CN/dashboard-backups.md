---

copyright:
  years: 2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 管理备份
{: #backups}

您可以从服务仪表板的*备份*页面创建和下载备份。可以使用安排的备份和手动备份。

## 查看现有备份

数据库的每日备份会自动安排。要查看现有备份：

1. 浏览到服务仪表板。
2. 单击选项卡中的**备份**以打开_备份_页面。此时将显示可用备份列表：

  ![可用备份](./images/elastic_search-backups-show.png "可用备份列表")

单击相应的行以展开任何可用备份的选项。

![备份选项](./images/elastic_search-backups-options.png "备份选项") 

## 创建手动备份

除了已安排的备份，您还可以手动创建备份。要创建手动备份，请执行以下步骤以查看现有备份，然后单击可用备份列表上方的**现在备份**。此时将显示一条消息，通知您已启动备份，并且“暂挂”备份将添加到可用备份列表中。

## 备份内容

{{site.data.keyword.composeForElasticsearch}} 备份将与 Elasticsearch API 中的快照实用程序一起使用。该过程以非阻塞方式在整个集群上执行，因此在运行该过程时，可继续正常执行所有建立索引和搜索操作。时间点备份是在创建快照时进行的。

## 复原备份
要将备份复原到新服务实例，请执行以下步骤以查看现有备份，然后单击相应的行以展开要下载的备份的选项。单击**复原**按钮。此时将显示一条消息，通知您已启动复原。新服务实例将自动命名为“elasticsearch-restore-[timestamp]”，并在供应启动时显示在仪表板上。
