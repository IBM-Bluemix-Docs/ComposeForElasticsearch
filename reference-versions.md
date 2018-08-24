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

# Versions

Deployable Versions| Preferred Version
----------|-----------
2.4.6, 5.6.9, 6.2.2 | 6.2.2
{: caption="Table 1. Elasticsearch versions" caption-side="top"}

You can find the list of available versions on the {{site.data.keyword.composeForElasticsearch}} [catalog page](https://console.{DomainName}/catalog/services/compose-for-elasticsearch).

## Preferred Version

The preferred version is typically the newest version of Redis that is available for {{site.data.keyword.composeForElasticsearch}}. The preferred version is the default in the version selector on the catalog page, and it's also the version that is automatically provisioned if no version is specified in the API call.

### New Preferred Version Protocol

When a new version is made available, its release is announced and the new version is made available for deployment. After the release date there is an approximately 7-day window where the newest version is available, but is not yet the preferred version. This window allows users to deploy and test the new version while still having the current version available to them. At the end of the 7-day window, the new version is set as the preferred version, or a new date for this change is announced.


To get a current list of available versions for your {{site.data.keyword.composeForElasticsearch}} service, you can use the 
`GET /2016-07/deployments/:id/versions` endpoint. For more information, see the [API documentation](https://apidocs.compose.com/v1.0/reference#2016-07-get-deployments-versions)

