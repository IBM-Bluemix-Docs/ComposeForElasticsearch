---

copyright:
  years: 2016,2018
lastupdated: "2018-05-04"

subcollection: compose-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# About {{site.data.keyword.composeForElasticsearch}}
{: #about}

{{site.data.keyword.composeForElasticsearch_full}} combines the power of a full text search engine with the indexing strengths of a JSON document database. Together they create a powerful tool for rich data analysis on large volumes of data. Elasticsearch can score your searches for exactness, so you can dig through your data set for those close matches and near misses that you might be missing.
{:shortdesc}

**Note:** Any Compose service instances that were provisioned before 14 September 2016, and which are still active, can be directly accessed at [https://www.compose.com/](https://www.compose.com). Any Compose service instance that is provisioned from this point forward is directly accessed and used from your {{site.data.keyword.cloud}} account.

## Creating a {{site.data.keyword.composeForElasticsearch}} service instance

You can create a {{site.data.keyword.composeForElasticsearch}} service from the [{{site.data.keyword.composeForElasticsearch}} page](https://{DomainName}/catalog/services/compose-for-elasticsearch/) in the {{site.data.keyword.cloud_notm}} catalog.

Choose a service name, and a region, organization, and space to provision the service in. You can use the **Select a database version** field to choose from the available database versions.

When you provision your {{site.data.keyword.composeForElasticsearch}} instance, you can choose the *Standard* or *Enterprise* plans. With the *Enterprise* plan, you can provision your {{site.data.keyword.composeForElasticsearch}} instance into an available {{site.data.keyword.composeEnterprise}} cluster. {{site.data.keyword.composeEnterprise}} provides the security and isolation that is required by enterprise compliance and uses dedicated networking to ensure the performance of the deployed databases. See the [{{site.data.keyword.composeEnterprise}} documentation](/docs/services/ComposeEnterprise/index.html) for more details.

## Managing {{site.data.keyword.composeForElasticsearch}}

You can manage your service from the service dashboard. Here you can find information about your {{site.data.keyword.cloud}} Compose database and how to connect to it. You can also:

- Manage your backups
- Allocate more resources for your service 
- Use whitelists to restrict access to your databases.

For more information, see [Settings](/docs/services/ComposeForElasticsearch?topic=compose-for-elasticsearch-dashboard-settings).

{{site.data.keyword.composeForElasticsearch}} relies on Cloud Foundry roles to manage access to the service. Only users with the Developer role can see or use the service dashboard. For more information about Cloud Foundry roles, see the [Cloud Foundry access](/docs/iam?topic=iam-cfaccess#cfaccess) and the [Managing Cloud Foundry access](/docs/iam?topic=iam-mngcf#mngcf) pages.
{: tip}

## Connecting to {{site.data.keyword.composeForElasticsearch}}

You can connect to your service by using the credentials that are created along with the service, or with the connection strings and command line that are provided in the *Overview* tab of your service dashboard.

## Connecting an {{site.data.keyword.cloud_notm}} application to {{site.data.keyword.composeForElasticsearch}}

To connect an {{site.data.keyword.cloud_notm}} application to your service, use the credentials that are created along with the service. You can find information on how to connect an {{site.data.keyword.cloud_notm}} application to a {{site.data.keyword.composeForElasticsearch}} service in [Connecting an {{site.data.keyword.cloud_notm}} Application](/docs/services/ComposeForElasticsearch?topic=compose-for-elasticsearch-ibmcloud-cf-app).

## Connecting to {{site.data.keyword.composeForElasticsearch}} from outside {{site.data.keyword.cloud_notm}}

If you want to connect to {{site.data.keyword.composeForElasticsearch}} from outside {{site.data.keyword.cloud_notm}}, you can use the provided connection strings or command line. You can find information on how to connect in [Connecting an external application](/docs/services/ComposeForElasticsearch?topic=compose-for-elasticsearch-external-app).
