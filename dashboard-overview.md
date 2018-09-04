---

Copyright:
  years: 2016,2018
lastupdated: "2018-05-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Dashboard Overview

You can manage your {{site.data.keyword.composeForElasticsearch_full}} service from the service dashboard.

The _Overview_ page shows you information about your {{site.data.keyword.cloud}} Compose database. The overview includes essential identifying information and current resource usage. You can also find a section for connection strings that you can use to connect to your database.

## Deployment Details

The _Deployment Details_ pane shows details of your service.

![Deployment Details](./images/elastic_search-deployment-details.png "A view of the Deployment Details pane")

### Type

The type of database that is offered by the service, and the database version that your service uses. If a more recent database version is available, a notification is displayed, together with a link to the [Upgrade version](/docs/services/ComposeForElasticsearch/dashboard-settings.html#upgrade-version) section of your service dashboard.

### ID

An internal identifier for the service.

### Usage

The size of your database and the amount of storage that is provided by your service plan.

## Recent Tasks

Making administrative changes to your service (such as scaling, or taking a manual backup) starts a task. While the task is running, the _Recent Tasks_ panel shows the task name and a progress bar.

## Connection Strings

Connection Strings can be used by some client libraries. They contain all the information that is needed for other libraries to connect. You can find out how to use a Connection String to connect to your service in [Connecting an external application](/docs/services/ComposeForElasticsearch/connecting-external.html).

You can find each Connection String for your service in a different tab in the _Connection Strings_ pane.

### HTTPS

A URI-formatted connection string that can be used by some client libraries. It contains all the information that is needed for other libraries to connect. You can find out how to use the Connection String to connect in [Connecting an external application](/docs/services/ComposeForElasticsearch/connecting-external.html).

### Health

An example call that you can use to find out the health of the Elasticsearch cluster.

## Instance Administration API

You can manage your {{site.data.keyword.composeForElasticsearch}} service through the {{site.data.keyword.cloud_notm}} Compose API.

For more documentation and reference for using the {{site.data.keyword.cloud_notm}} Compose API, across all {{site.data.keyword.cloud_notm}} Compose services, read [The {{site.data.keyword.cloud_notm}} Compose API](https://www.compose.com/articles/the-ibm-cloud-compose-api/).

### Foundation Endpoint

The foundation endpoint is composed of the region the service resides in and the service instance ID. It is at the start of every endpoint.

### Deployment ID

The deployment ID is necessary for most calls, and identifies the specific deployment instance.
