---

copyright:
  years: 2016,2017
lastupdated: "2017-10-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Getting started with Compose for Elasticsearch
{: #getting-started-with-compose-for-elasticsearch}

{{site.data.keyword.composeForElasticsearch_full}} combines the power of a full text search engine with the indexing strengths of a JSON document database. Together they create a powerful tool for rich data analysis on large volumes of data. With Elasticsearch, your searching can be scored for exactness, letting you dig through your data set for those close matches and near misses that you might be missing.
{:shortdesc}

**Note:** Any Compose service instances that were provisioned before 14 September 2016 that are still active can still be used and directly accessed at [https://www.compose.com/](https://www.compose.com). Any Compose service instance that is provisioned from this point forward is directly accessed and used within your {{site.data.keyword.Bluemix_notm}} account.

## Creating a Compose for Elasticsearch service instance

[Create a {{site.data.keyword.composeForElasticsearch}} instance](https://console.bluemix.net/catalog/services/compose-for-elasticsearch/).

When you create an instance of the service, ensure that you choose both a name for your service and a credential name. Leave the service unbound; you can connect an application to your service later by using the credentials that are provided when the service is provisioned. The various credential values are listed in the *Available credentials* section.

When you provision your {{site.data.keyword.composeForElasticsearch}} instance you can choose the *Standard* or *Enterprise* plans. With the *Enterprise* plan, you can provision your {{site.data.keyword.composeForElasticsearch}} instance into an available {{site.data.keyword.composeEnterprise}} cluster. {{site.data.keyword.composeEnterprise}} provides the security and isolation required by enterprise compliance and uses dedicated networking to ensure the performance of the deployed databases. See the [Compose Enterprise documentation](../ComposeEnterprise/index.html) for more details.

## Managing Compose for Elasticsearch

You can manage your service from the service dashboard. Here you can find information about your {{site.data.keyword.Bluemix_notm}} Compose database and how to connect to it. You can also:

- manage your backups
- allocate more resources for your service 
- use whitelists to restrict access to your databases.

For more information, see [Settings](./dashboard-settings.html).

## Connecting to Compose for Elasticsearch

You can connect to your service using the credentials that are created along with the service, or with the connection strings and command line that are provided in the *Overview* tab of your service dashboard.

## Connecting a {{site.data.keyword.Bluemix_notm}} application to Compose for Elasticsearch

To connect a {{site.data.keyword.Bluemix_notm}} application to your service, use the credentials that are created along with the service. You can find information on how to connect a {{site.data.keyword.Bluemix_notm}} application to an {{site.data.keyword.composeForElasticsearch}} service in [Connecting a {{site.data.keyword.Bluemix_notm}} Application](./connecting-bluemix-app.html).

## Connecting to Compose for Elasticsearch from outside {{site.data.keyword.Bluemix_notm}}

If you want to connect to {{site.data.keyword.composeForElasticsearch}} from outside {{site.data.keyword.Bluemix_notm}}, you can use the provided connection strings or command line. You can find information on how to connect in [Connecting an external application](./connecting-external.html).
