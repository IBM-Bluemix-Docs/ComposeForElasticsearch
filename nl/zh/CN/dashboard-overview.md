---

Copyright:
  years: 2017,2018
lastupdated: "2017-12-05"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 仪表板概述

您可以从服务仪表板管理 {{site.data.keyword.composeForElasticsearch_full}} 服务。

_概述_页面显示有关 {{site.data.keyword.cloud}} Compose 数据库的信息。此概述包含基本的标识信息和当前资源使用情况。您还将找到一个用于连接字符串的部分，您可以将这些连接字符串与工具一起使用，也可以使用工具来连接到数据库。

## 部署详细信息

_部署详细信息_面板显示服务的详细信息。

![部署详细信息](./images/elastic_search-deployment-details.png "“部署详细信息”面板的视图")

### 类型

服务所提供的数据库类型，以及服务所使用的数据库版本。如果有更新的数据库版本可用，那么会显示通知以及指向服务仪表板中[升级版本](/docs/services/ComposeForElasticsearch/dashboard-settings.html#upgrade-version)部分的链接。

### 名称

服务的内部标识。

### 使用情况

数据库的大小和服务套餐所提供的存储量。


## 连接字符串

连接字符串可由某些客户机库使用，并包含其他库连接所需的所有信息。您可以在[连接外部应用程序](/docs/services/ComposeForElasticsearch/connecting-external.html)中找到如何使用“连接字符串”连接到服务的信息。

您将在_连接字符串_面板的其他选项卡中找到服务的每个“连接字符串”。

### HTTPS

这是 URI 格式的连接字符串，可供某些客户机库使用，并包含其他库连接所需的所有信息。您可以在[连接外部应用程序](/docs/services/ComposeForElasticsearch/connecting-external.html)中找到如何使用“连接字符串”进行连接的信息。

### 运行状况

可用于找出 Elasticsearch 集群的运行状况的示例调用。

## 实例管理 API

您可以通过 {{site.data.keyword.cloud_notm}} Compose API 来管理 {{site.data.keyword.composeForElasticsearch}} 服务。

### 基础端点

基础端点由服务所在的区域和服务实例标识组成。基础端点将位于每个端点的开头。

### 部署标识

部署标识对于大多数调用都是必需的，用于标识特定的部署实例。

### 参考

有关在所有 {{site.data.keyword.cloud_notm}} Compose 服务上使用 {{site.data.keyword.cloud_notm}} Compose API 的更多文档和参考信息，请参阅 [{{site.data.keyword.cloud_notm}} Compose API](https://www.compose.com/articles/the-ibm-cloud-compose-api/)。
