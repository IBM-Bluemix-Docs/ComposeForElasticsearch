---

copyright:
  years: 2016,2018
lastupdated: "2017-10-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 关于 {{site.data.keyword.composeForElasticsearch}}
{: #about-compose-for-elasticsearch}

{{site.data.keyword.composeForElasticsearch_full}} 将全文搜索引擎的强大功能和 JSON 文档数据库的建立索引优势结合在一起。如此一来，创建出强大的工具，可对大量数据执行富数据分析。使用 Elasticsearch，可对您搜索的正确性进行评分，让您深度挖掘您的数据集，以获得那些您可能遗漏的近似匹配项和接近遗漏项。
{:shortdesc}

**注：**在 2016 年 9 月 14 日之前供应的任何仍处于活动状态的 Compose 服务实例仍可通过 [https://www.compose.com/](https://www.compose.com) 使用和直接访问。从此处供应的任何 Compose 服务实例都可在 {{site.data.keyword.cloud}} 帐户中直接访问和使用。

## 创建 {{site.data.keyword.composeForElasticsearch}} 服务实例

您可以在 {{site.data.keyword.cloud_notm}}“目录”的 [{{site.data.keyword.composeForElasticsearch}} 页面](https://console.{DomainName}/catalog/services/compose-for-elasticsearch/)中创建 {{site.data.keyword.composeForElasticsearch}} 服务。

选择服务名称以及要在其中供应该服务的区域、组织和空间。可以使用**选择数据库版本**字段从可用数据库版本中进行选择。

供应 {{site.data.keyword.composeForElasticsearch}} 实例时，可以选择*标准*或*企业*套餐。使用*企业*套餐，您可以将 {{site.data.keyword.composeForElasticsearch}} 实例供应到可用的 {{site.data.keyword.composeEnterprise}} 集群中。{{site.data.keyword.composeEnterprise}} 提供企业合规性所需的安全性和隔离，并使用专用网络来确保已部署的数据库的性能。有关更多详细信息，请参阅 [Compose Enterprise 文档](../ComposeEnterprise/index.html)。

## 管理 {{site.data.keyword.composeForElasticsearch}}

您可以从服务仪表板管理服务。您可以在此找到有关 {{site.data.keyword.cloud}} Compose 数据库以及如何连接到该数据库的信息。您还可以：

- 管理备份
- 为服务分配更多资源 
- 使用白名单来限制对数据库的访问。

有关更多信息，请参阅[设置](./dashboard-settings.html)。

## 连接到 {{site.data.keyword.composeForElasticsearch}}

您可以使用随服务一起创建的凭证，或者使用服务仪表板*概述*选项卡中提供的连接字符串和命令行，来连接到服务。

## 将 {{site.data.keyword.cloud_notm}} 应用程序连接到 {{site.data.keyword.composeForElasticsearch}}

要将 {{site.data.keyword.cloud_notm}} 应用程序连接到服务，请使用随服务一起创建的凭证。您可以在[连接 {{site.data.keyword.cloud_notm}} 应用程序](./connecting-bluemix-app.html)中查找有关如何将 {{site.data.keyword.cloud_notm}} 应用程序连接到 {{site.data.keyword.composeForElasticsearch}} 服务的信息。

## 从外部 {{site.data.keyword.cloud_notm}} 连接到 {{site.data.keyword.composeForElasticsearch}}

如果要从外部 {{site.data.keyword.cloud_notm}} 连接到 {{site.data.keyword.composeForElasticsearch}}，可以使用提供的连接字符串或命令行。您可以在[连接外部应用程序](./connecting-external.html)中找到有关如何连接的信息。
