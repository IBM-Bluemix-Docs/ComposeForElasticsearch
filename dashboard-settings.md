---

Copyright:
  Years: 2017
lastupdated: "2017-10-20"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Settings
These features allow you to adapt your {{site.data.keyword.composeForElasticsearch_full}} service to better suit your needs and requirements.

## Upgrade Version
If there is a new version of the database available, a drop-down menu appears, allowing you to select which version you would like to upgrade to. Otherwise, your service is on the newest version available, and the panel displays the current version information.

![The Version panel](./images/elastic_search-version-show.png "The Version panel")


## Scale Resources

If your service needs additional storage, or you want to reduce the amount of storage allocated to your service, you can do this by scaling resources.

1. Navigate to your service's dashboard overview page.
2. In the _Deployment Details_ pane, click **Scale Resources**. The Scale Resouces page opens.

    ![The Scale Resources panel](./images/elastic_search-scale-show.png "The Scale Resources panel")

3. Adjust the slider to raise or lower the storage allocated to the {{site.data.keyword.composeForElasticsearch}} service. Move the slider to the left to reduce the amount of storage, or move it to the right to increase the storage.
4. Click **Scale Deployment** to trigger the rescaling and return to the dashboard overview. The _Recipes_ pane contains details of the rescaling job that is now in progress. When the scaling is complete the _Deployment Details_ pane updates to show the current usage and the new value for the available storage.


## Using Whitelists

If you want to restrict access to your databases, you can whitelist specific IP addresses or ranges of IP addresses on your service. When there are no IP addresses in the whitelist, the whitelist is disabled and the deployment will accept connections from any system on the internet.

![Whitelisting IPs](./images/elastic_search-whitelist-show.png "The whitelist fields.")

### IP Addresses
The *IP* field can take a single complete IPv4 address or IPv6 address with or without a netmask. Without a netmask, incoming connections must come from exactly that IP address. 

Although the IP entry allows for IPv6, no Compose deployments are currently available to IPv6 networking and so these addresses cannot be filtered on.
{: .tip}

### Netmasks
To allow a connection from a specified range of IP addresses, use a netmask. The IP address must be fully specified when using a netmask. That means entering, for example, `192.168.1.0/24` rather than `192.168.1/24`.

### Description
The *Description* can be any user-significant text for identifying the whitelist entry - a customer name, project identifier or employee number, for example. The description field is required.

### Compose Services
Whitelist entries are automatically added to Compose's servers to allow them to connect.

### Removal
To remove an IP address or netmask from the Whitelist, click the *Remove* entry displayed next to it.
When all entries on the whitelist are removed, the whitelist will be disabled and all IP addresses will be accepted by the TCP access portals.
