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

# 仪表板概述

您可以从服务仪表板管理 {{site.data.keyword.composeForElasticsearch_full}} 服务。

_概述_页面显示有关 {{site.data.keyword.cloud}} Compose 数据库的信息。此概述包含基本的标识信息和当前资源使用情况。您还将找到一个用于连接字符串的部分，您可以将这些连接字符串与工具一起使用，也可以使用工具来连接到数据库。

## 部署详细信息

_部署详细信息_面板显示服务的详细信息。

![部署详细信息](./images/elastic_search-deployment-details.png "“部署详细信息”面板的视图")

### 类型

服务所提供的数据库类型，以及服务所使用的数据库版本。

### 名称

服务的内部标识。

### 使用情况

数据库的大小和服务套餐所提供的存储量。


## 连接字符串

连接字符串可由某些客户机库使用，并包含其他库连接所需的所有信息。您可以在[连接外部应用程序](./connecting-external.html)中找到如何使用“连接字符串”连接到服务的信息。

您将在_连接字符串_面板的其他选项卡中找到服务的每个“连接字符串”。

### HTTPS

这是 URI 格式的连接字符串，可供某些客户机库使用，并包含其他库连接所需的所有信息。您可以在[连接外部应用程序](./connecting-external.html)中找到如何使用“连接字符串”进行连接的信息。

### 运行状况

可用于找出 Elasticsearch 集群的运行状况的示例调用。
