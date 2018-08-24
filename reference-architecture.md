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

# Architecture 
{: #architecture}


## Replication
An {{site.data.keyword.composeForElasticsearch_full}} service consists of three data nodes in a cluster. You select a region where the service is deployed, and the three data nodes are spread over the region's availability zones. The replica count is set by default to 1, giving you two copies of your data. Elasticsearch balances your replicas across the cluster automatically. If a data node becomes unreachable, your cluster can continue to operate normally.
   
## Disk Encryption

All {{site.data.keyword.composeForElasticsearch}} services have encryption at rest. Your data resides on servers that have volume-level encryption enabled. 

Because {{site.data.keyword.composeForElasticsearch}} is a managed service, it's possible for {{site.data.keyword.cloud}} Compose operators to see data. In addition to the disk encryption, we recommend that you encrypt personal information before storing it in the database or use extensions or features to enable encryption on the database itself. While these methods might impact usability or performance, it's good practice to ensure that personal information is protected with encryption.

## Auto-scaling

Resources are scaled automatically based on the total disk storage use of the Elasticsearch databases. Memory usage is based on a ratio of provisioned storage at a ratio of 1:10. These resources are bundled as units.

Auto-scaling is designed to respond to the short-to-medium term trends of your database. Every hour, your service is checked and if it is running short on disk space, then more units are allocated to the deployment. Re-allocation happens once in every 24-hour period. If you are planning on running operations that might put a spike in the usual RAM usage, or any data operations that could overflow your allotted storage, you should manually scale your service's resources up first to avoid hitting any resource limits that would affect cluster operations.

Auto-scaling does not scale down deployments when disk/memory usage shrinks. The resources provisioned to your databases will remain for your future needs, or until you scale down your deployment manually.
{: tip}