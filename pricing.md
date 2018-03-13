---

copyright:
  years: 2016,2018
lastupdated: "2018-01-11"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Pricing
{: #pricing}

## Base Configuration
An {{site.data.keyword.composeForElasticsearch_full}} service starts with three identical data nodes, each with 2GB of storage and 204MB of memory, which is equal to 2 units of resources. The service _includes_ replication and high-availablity, so each unit and the price per unit _includes_ the cost of the resources across all three nodes.

The base configuration also includes the two HAProxy capsules with 64MB each to support authentication, HTTPS, and IP whitelisting. 

### Cost
The base service configuration has a set price. Please consult the catalog tiles on {{site.data.keyword.cloud_notm}} for base pricing in your local currency. For example, the base price in US dollars is $36/month.


## Expansion Options
If you need additional storage or memory for your service, you can increase the resources allocated in a 10:1 ratio of disk storage to memory unit. Increasing the disk allocated to the deployment also increases the RAM allocated. A {{site.data.keyword.composeForElasticsearch}} unit consists of 1GB of storage and 102MB of memory, and each unit and the price per unit _includes_ the cost to increase the resources on all three nodes.

### Cost
Each additional unit (1GB storage/102MB memory) has a per unit price, which is listed in your local currency on the {{site.data.keyword.cloud_notm}} catalog tile for the service. In US dollars each additional unit costs $18. As the _total_ size of all your {{site.data.keyword.composeForElasticsearch}} services increases, the per unit price decreases, as shown in the tiered pricing table, below.

### Tiered Pricing
Number of Units|Tiered Discount
----------|-----------
2 - 9 units|base per unit price -- $18.00 USD/Unit
10 - 24 units|10% reduction -- $16.20 USD/Unit
25 - 49 units|20% reduction -- $14.40 USD/Unit
50 - 99 units|30% reduction -- $12.60 USD/Unit
100 - 499 units|40% reduction --$10.80 USD/Unit
500 - 999 units|50% reduction -- $9.00 USD/Unit
1,000 - 4,999 units|60% reduction -- $7.20 USD/Unit
5,000+ units|70% reduction -- $5.40 USD/Unit
{: caption="Table 1. {{site.data.keyword.composeForElasticsearch}} tiered pricing" caption-side="top"}

