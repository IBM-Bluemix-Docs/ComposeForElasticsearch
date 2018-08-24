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

# 管理备份
{: #backups}

您可以从服务仪表板_管理_页面的_备份_选项卡创建和复原备份。有每日、每周、每月和按需备份可用。备份按照以下安排保留：

备份类型|保留安排
----------|-----------
每日|每日备份保留 7 天
每周|每周备份保留 4 周
每月|每月备份保留 3 个月
按需|保留一个按需备份。保留的备份始终是最新的按需备份。
{: caption="表 1. 备份保留安排" caption-side="top"}

## 查看现有备份

数据库的每日备份会自动安排。您可以在服务仪表板中查看现有备份。

1. 浏览到服务仪表板。
2. 单击选项卡中的**备份**以打开_备份_页面。此时将显示可用备份列表：

  ![可用备份](./images/elastic_search-backups-show.png "可用备份列表")

单击相应的行以展开任何可用备份的选项。
  ![备份选项](./images/elastic_search-backups-options.png "备份选项。") 

### 使用 API 查看现有备份

`GET /2016-07/deployments/:id/backups` 端点提供了备份列表。这将在服务的_概述_中显示基础端点以及服务实例标识和部署标识。例如： 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```  

## 创建手动备份

要创建手动备份，请执行以下步骤以查看现有备份，然后单击可用备份列表上方的**现在备份**。此时将显示一条消息，通知您已启动备份，并且“暂挂”备份将添加到可用备份列表中。

### 使用 API 创建备份

向备份端点发送 POST 请求以启动手动备份：`POST /2016-07/deployments/:id/backups`。该请求在运行时会立即返回诀窍标识以及有关备份的信息。使用备份之前，您需要检查备份端点以验证备份是否已完成，并在使用之前找到 `backup_id` 值。

```
GET /2016-07/deployments/:id/backups/
```

## 复原备份

1. 执行以下步骤以查看现有备份。
2. 单击相应的行以展开要复原的备份的选项。
3. 单击**复原**按钮。此时将显示一条消息，通知您已启动复原。新服务实例在供应启动时会显示在仪表板上，并且具有生成的名称 `elasticsearch-restore-[timestamp]`。

从备份复原时，会将数据复原到可用于 {{site.data.keyword.composeForElasticsearch}} 的最新次版本。您可以通过 {{site.data.keyword.cloud_notm}} CLI 复原数据并在要复原到的版本中进行发送，以覆盖此设置。

**注：**只能复原到可用于供应的版本。

### 通过 CLI 复原

通过 {{site.data.keyword.cloud_notm}} CLI，使用以下步骤将备份从正在运行的 Elasticsearch 服务复原到新的 Elasticsearch 服务。 
1. 如果需要，请[下载并安装 CLI](https://console.{DomainName}/docs/cli/index.html#overview)。 
2. 在服务的_备份_页面上查找要从其复原的备份，然后复制备份标识。  
  **或者**  
  通过 Compose API，使用 `GET /2016-07/deployments/:id/backups` 来查找备份及其标识。这将在服务的_概述_中显示基础端点和服务实例标识。例如： 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
响应包含该服务实例的所有可用备份的列表。选取要从其复原的备份并复制其标识。

3. 使用相应的帐户和凭证登录。`ibmcloud login`（或 `ibmcloud login -help` 以查看所有登录选项）。

4. 切换到您的组织和空间：`ibmcloud target -o "$YOUR_ORG" -s "YOUR_SPACE"`

5. 使用 `service create` 命令以供应新服务，并提供要在 JSON 对象中复原的源服务和特定备份。例如：
``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
  _SERVICE_ 字段应该为 `compose-for-elasticsearch`，_PLAN_ 字段应该为 Standard 或 Enterprise（取决于环境）。_SERVICE\_INSTANCE\_NAME_ 是放置新服务的名称的位置。_source\_service\_instance\_id_ 是备份源的服务实例标识；可通过运行 `ibmcloud cf service DISPLAY_NAME --guid` 获取该值，其中 _DISPLAY\_NAME_ 是备份源自的服务的名称。 

  如果需要指定要复原到的 Elasticsearch 版本，那么可使用可选的 JSON 参数“db_version”。此参数还用于[升级到主版本的 Elasticsearch](./upgrading.html)。
  
  企业用户还需要在 JSON 对象中使用 `"cluster_id": "$CLUSTER_ID"` 参数指定要部署到的集群。

