---

copyright:
  years: 2016,2018
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an {{site.data.keyword.cloud_notm}} application
{: #ibmcloud-cf-app}

To connect an {{site.data.keyword.cloud}} application to your service, use the service credentials that are created when the service is provisioned.

## Connecting with the 'Hello World' sample app

The [sample app](https://github.com/IBM-Cloud/compose-elasticsearch-helloworld-nodejs) demonstrates how to use Node.js to connect to a {{site.data.keyword.composeForElasticsearch}} service by using the provided credentials. The application creates, reads from, and writes to an Elasticsearch index.

Download the sample app and follow the instructions in the readme file. Then, in your application details page in {{site.data.keyword.cloud}}, click **View APP** to view the contents of the *examples* index.

## Credentials

Field Name|Description
----------|-----------
`uri`|The URI to be used when connecting to the service. Includes the schema (`https:`), admin user name and password, host name of server and port number to connect to.
`uri_direct_1`|A secondary URI that can be used when connecting to the service. Formatted as for `uri`.
`uri_health`|A `curl` command that requests the cluster health from the first haproxy.
`uri_health_1`|A `curl` command that requests the cluster health from the second haproxy.
`ca_certificate_base64` `(optional)`|A base64 encoded, self-signed certificate that is used to confirm that an application is connecting to the appropriate server. The certificate is not present on services that have a Let's Encrypt certificate. You need to decode the key before you can use it, as shown in the sample application. You need to decode the key before you can use it, as shown in the sample application.
`deployment_id`|An internal identifier for the service as created within Compose.
`db_type`|The type of database that is offered by the service; in this case `elastic_search`.
`name`|The database deployment name.
{: caption="Table 1. Compose for Elasticsearch credentials" caption-side="top"}

**Note:** Two `haproxy` portals provide access to the Elasticsearch cluster. Both `uri` and `uri_direct_1` can be used to connect to the cluster. In your applications, switch between `uri` and `uri_direct_1` to manage responses to connection failures.
