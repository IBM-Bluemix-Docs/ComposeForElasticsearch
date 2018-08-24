---
copyright:
  years: 2016,2018
lastupdated: "2018-06-11"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 体系结构 
{: #architecture}


## 复制
{{site.data.keyword.composeForElasticsearch_full}} 服务包含一个具有三个数据节点的集群。您可选择部署了该服务的区域，然后这三个数据节点会分布在该区域的可用性专区中。缺省情况下，副本计数设置为 1，这将为您提供数据的两个副本。Elasticsearch 会自动均衡集群中的副本。如果某个数据节点变为不可访问，集群仍可以继续正常运行。
   
## 磁盘加密

所有 {{site.data.keyword.composeForElasticsearch}} 服务都具有静态加密。数据位于启用了卷级别加密的服务器上。 

由于 {{site.data.keyword.composeForElastcisearch}} 是受管服务，因此 {{site.data.keyword.cloud}} Compose 操作员可以查看数据。除了磁盘加密外，还建议您在数据库中存储个人信息之前，对这些信息加密，或者使用扩展或功能启用对数据库本身的加密。虽然这些方法可能会影响可用性或性能，但确保个人信息受到加密保护是比较好的做法。

## 自动扩展

资源将根据 Elasticsearch 数据库的磁盘存储总使用量自动进行扩展。内存使用量基于按 1:10 比率供应的存储量。这些资源会捆绑为单元。

自动扩展旨在响应数据库的中短期趋势。系统每小时会检查一次服务，如果服务的磁盘空间不足，那么会向部署分配更多单元。每 24 小时会执行一次重新分配。如果计划运行占用很多 RAM 而可能导致平常 RAM 使用量出现峰值的操作，或者计划执行会溢出所分配存储量的任何数据操作，那么建议您先手动向上扩展服务的资源，以避免达到任何资源限制而影响到集群操作。

自动扩展不会向下扩展部署，即减少磁盘/内存使用量。向数据库供应的资源将保留以满足您的未来需求，或者直到您手动向下扩展部署为止。
{: .tip}
