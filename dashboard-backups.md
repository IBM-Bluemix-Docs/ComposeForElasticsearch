---

copyright:
  years: 2016,2018
lastupdated: "2018-04-19"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Managing backups
{: #backups}

You can create and restore backups from the _Backups_ tab of the _Manage_ page of your service dashboard. Daily, weekly, monthly, and on-demand backups are available. They are retained according to the following schedule:

Backup type|Retention schedule
----------|-----------
Daily|Daily backups are retained for 7 days
Weekly|Weekly backups are retained for 4 weeks
Monthly|Monthly backups are retained for 3 months
On-demand|One on-demand backup is retained. The retained backup is always the most recent on-demand backup.
{: caption="Table 1. Backup retention schedule" caption-side="top"}

## Viewing existing backups

Daily backups of your database are automatically scheduled. You can view your existing backups from your service dashboard.

1. Navigate to your service Dashboard.
2. Click **Backups** in the tabs to open the _Backups_ page. A list of available backups is shown:

  ![Available backups](./images/elastic_search-backups-show.png "A list of available backups.")

Click the corresponding row to expand the options for any available backup.
  ![Backup Options](./images/elastic_search-backups-options.png "Options for a backup.") 

### Using the API to view existing backups

A list of backups is available at the `GET /2016-07/deployments/:id/backups` endpoint. The Foundation Endpoint with the service instance ID and the deployment ID are shown in the service's _Overview_. For example: 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```  

## Creating a manual backup

To create a manual backup, follow the steps to view existing backups, then click **Back up now** above the list of available backups. A message is displayed to let you know that a backup has started, and a 'pending' backup is added to the list of available backups.

### Using the API to create a backup

Send a POST request to the backups endpoint to initiate a manual backup: `POST /2016-07/deployments/:id/backups`. It returns immediately with the recipe ID and information about the backup as it is running. Before using the backup you need to check the backups endpoint to verify that the backup has finished, and find the `backup_id` value before using it.

```
GET /2016-07/deployments/:id/backups/
```

## Restoring a backup

1. Follow the steps to view existing backups.
2. Click the corresponding row to expand the options for the backup you want to restore.
3. Click the **Restore** button. A message is displayed to let you know that a restore has started. The new service instance appears on your dashboard when provisioning starts, and has the generated name `elasticsearch-restore-[timestamp]`.

### Restoring via the {{site.data.keyword.cloud_notm}} CLI

Use the following steps to restore a backup from a running Elasticsearch service to a new Elasticsearch service using the {{site.data.keyword.cloud_notm}} CLI. 
1. If you need to, [download and install the CLI](https://console.{DomainName}/docs/cli/index.html#overview). 
2. Find the backup that you would like to restore from on the _Backups_ page on your service and copy the backup ID.  
  **Or**  
  Use the `GET /2016-07/deployments/:id/backups` to find a backup and its ID through the Compose API. The Foundation Endpoint and the service instance ID are both shown in the service's _Overview_. For example: 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
  The response includes a list of all available backups for that service instance. Pick the backup you want to restore from and copy its ID.

3. Log in with the appropriate account and credentials. `bx login` (or `bx login -help` to see all the login options).

4. Switch to your Organization and Space `bx target -o "$YOUR_ORG" -s "YOUR_SPACE"`

5. Use the `service create` command to provision a new service, and provide the source service and the specific backup that you are restoring in a JSON object. For example:
``` 
bx service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
  The _SERVICE_ field should be `compose-for-elasticsearch`, and _PLAN_ field should be either Standard or Enterprise depending on your environment. _SERVICE\_INSTANCE\_NAME_ is where you will put the name for your new service. The _source\_service\_instance\_id_ is the service instance id of the source of the backup; it can be obtained by running `bx cf service DISPLAY_NAME --guid` where _DISPLAY\_NAME_ is the name of the service the backup is from. 
  
  Enterprise users also need to specify which cluster to deploy to in the JSON object with the `"cluster_id": "$CLUSTER_ID"` parameter.
  

### Migrating to a New Version

Some major version upgrades are not available in the current running deployment. You will need to provision a new service that is running the upgraded version, and then migrate your data into it using a backup. This process is the same a restoring a backup above, except you will specify the version you would like to upgrade to.

``` 
bx service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

For example, restoring an older version of a {{site.data.keyword.composeForElasticsearch}} service to a new service running Elasticsearch 6.2.2 looks like this:

```
bx service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
```

