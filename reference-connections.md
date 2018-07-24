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

# Connection Configuration
{: #connection-configuration}

{{site.data.keyword.composeForElasticsearch_full}} database connections are managed by 2 HAProxy portals. Each portal has 64 MB of memory.

The two portals allow for applications to maintain connectivity if one of the portals becomes unreachable. Failover at the client side is the responsibility of the application designer. Some Elasticsearch drivers handle multiple connection strings and graceful failover automatically.

If you are not working with a driver that completely handles failover errors, take the following steps to handle errors yourself.

* Work with a driver that accepts multiple endpoints in its connection configuration.
* Ensure that your application reacts to errors appropriately when it to reads from or writes to the database. You can configure your application to react to errors in different ways, including the following:
  + Log the error and reconnect.
  + Reconnect and retry the operation that caused the error.
  + Queue the failed operation and schedule a reconnection.

## Encryption in Transit

All {{site.data.keyword.composeForElasticsearch}} HAProxy portals are TLS/SSL enabled and support TLS version 1.2. The certificates for the service are Let's Encrypt certificates.

## Connection Limits

The two HAProxy portals have a connection limit of 2000 connections per portal. If you exceed the connection limit your service can become unresponsive. You can manage connections from your application through your driver's connection pooling feature.

## Proxy Timeouts

The HAProxy portals manage the lifecycle of connections between the server and client. Timeout values are detailed here as a reference guide for driver writers and those who have an interest in the low-level activity of {{site.data.keyword.composeForElasticsearch}} database connections.

Setting | Value | Applies
----------|-----------|-----------
client | 1m | When the client is expect to acknowledge or send data.
connect | 9s | When a connection is being made between the proxy and the server.
server | 2m | When the server is expected to acknowledge or send data.
check | 5s | As a secondary timeout check when a connection is being made between the proxy and the server.
http-request | 5s | The maximum allowed time to wait for a complete HTTP request.
{: caption="Table 1. Elasticsearch HAProxy timeouts" caption-side="top"}
