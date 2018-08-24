---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# 连接配置
{: #connection-configuration}

{{site.data.keyword.composeForElasticsearch_full}} 数据库连接通过 2 个 HAProxy 门户网站进行管理。每个门户网站有 64 MB 内存。

这两个门户网站支持应用程序在其中一个门户网站变为不可访问时保持连接。客户机端故障转移由应用程序设计人员负责。某些 Elasticsearch 驱动程序会自动处理多个连接字符串和正常故障转移。

如果未使用可完全处理故障转移错误的驱动程序，请执行以下步骤自行处理错误。

* 使用在其连接配置中接受多个端点的驱动程序。
* 确保应用程序在读写数据库时针对错误做出正确反应。可以将应用程序配置为以不同方式对错误做出反应，包括以下方式：
  + 记录错误并重新连接。
  + 重新连接并重试导致错误的操作。
  + 将失败操作排队并安排重新连接。

## 传输中加密

所有 {{site.data.keyword.composeForElasticsearch}} HAProxy 门户网站都已启用 TLS/SSL，并且支持 TLS V1.2。用于服务的证书是 Let's Encrypt 证书。

## 连接限制

两个 HAProxy 门户网站的连接限制为每个门户网站 2000 个连接。如果超出连接限制，那么服务可能会变为无响应。可以通过驱动程序的连接池功能来管理应用程序的连接。

## 代理超时

HAProxy 门户网站可管理服务器与客户机之间连接的生命周期。在此详细描述的超时值供驱动程序作者以及对 {{site.data.keyword.composeForElasticsearch}} 数据库连接低级别活动有兴趣的人员作为参考指南。

设置|值|适用情况
----------|-----------|-----------
client|1 分钟|客户机应该确认或发送数据的时间。
connect|9 秒|在代理与服务器之间建立连接的时间。
server|2 分钟|服务器应该确认或发送数据的时间。
check|5 秒|作为辅助超时，是指检查代理与服务器之间是否建立连接的时间。
http-request|5 秒|允许等待完整 HTTP 请求的最长时间。
{: caption="表 1. Elasticsearch HAProxy 超时" caption-side="top"}
