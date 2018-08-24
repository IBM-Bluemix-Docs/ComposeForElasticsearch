---

copyright:
  years: 2016,2018
lastupdated: "2018-07-25"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Upgrading Elasticsearch
{: #upgrading}

To keep your service secure and up-to-date, new versions of the Elasticsearch engine underlying your {{site.data.keyword.composeForElasticsearch_full}} will be released. You should upgrade as the new versions become available.

When a new version of {{site.data.keyword.composeForElasticsearch}} is available, a notification appears on the _Overview_ page.

## Upgrading to a new minor version

In almost all cases, you can make minor version upgrades to Elasticsearch without migrating any data and with minimal downtime. You can upgrade from the _Settings_ tab of your service by selecting the version that you would like to upgrade to from the drop-down menu.

## Upgrading to a new major version

There are three major versions of Elasticsearch available for {{site.data.keyword.composeForElasticsearch}}: 2.4.6, 5.x, and 6.x. YOu can migrate from Elasticsearch 2.x to 5.x, and from Elasticsearch 5.x to 6.x.

The process to migrate between all major versions uses a recent or on-demand backup to restore the data into a new deployment. This process provides a number of advantages:

- The original database stays running, and production work can continue uninterrupted.
- You can test the new database out of production and act on any application incompatibilities.
- The entire process can be rerun at any point.
- A fresh restoration reduces the likelihood that unneeded artifacts of the older version of the database are carried over to the new database.

### Migration Considerations

Most major versions include changes that might affect how you use Elasticsearch. This is especially true for migrating to Elasticsearch 6.x. Keep these considerations in mind before you upgrade, while you are testing the new version, and before you deprovision your old version.

#### Pre-5.x Indices

Elasticsearch usually supports reading indices that were created in the previous major version. Any indices that are created in Elasticsearch 5.x are readable by Elasticsearch 6.x.

If you have indices that were created in Elasticsearch 2.x, they are **not** readable in Elasticsearch 6.x. You will need to reindex those indices in Elasticsearch 5.x before migrating to 6.x. If your data contains indices that were made in Elasticsearch 2.x and you attempt to migrate to Elasticsearch 6.x, the nodes will fail to start and the migration will fail.

[Re-indexing can be done in-place](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html) in Elasticsearch 5.x before migrating to 6.x through the [Reindex API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html).
{: tip}

#### Index mappings

The ability to create multiple mapping types per index has been removed in Elasticsearch 6.x. Any multiple index mappings that are created in Elasticsearch 5.x will continue to work, but Elasticsearch 6.x supports creation of a single mapping type only per index.

Mapping types will be removed in Elasticsearch 7.x.

#### Other changes

Full documentation of breaking changes can be found in the [Elasticsearch docs](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/breaking-changes-6.0.html).

### Migrating to a New Version

To migrate to a new version of Elasticsearch, you need the [{{site.data.keyword.cloud_notm}} CLI](https://console.{DomainName}/docs/cli/index.html#overview). The process is similar to [restoring backup through the CLI](./dashboard-backups.html#restoring-via-cli), except that you will use the "db_version" field in the JSON to specify which version you are upgrading to. The full command is as follows.

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

For example, restoring a {{site.data.keyword.composeForElasticsearch}} service that is running 5.x to a new service that is running Elasticsearch 6.2.2 is as follows.

```
ibmcloud service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
```
