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

# バックアップの管理
{: #backups}

サービス・ダッシュボードの_「管理」_ページの_「バックアップ」_タブから、バックアップの作成とリストアを行えます。日次、週次、月次、オンデマンドでのバックアップを使用できます。 これらは、以下のスケジュールに従って保持されます。

バックアップ・タイプ|保持スケジュール
----------|-----------
日次|日次バックアップは 7 日間保持されます
週次|週次バックアップは 4 週間保持されます
月次|月次バックアップは 3 カ月保持されます
オンデマンド|オンデマンド・バックアップは 1 つ保持されます。 保持されるバックアップは、常に最新のオンデマンド・バックアップです。
{: caption="表 1. バックアップ保持スケジュール" caption-side="top"}

## 既存のバックアップの表示

データベースの日次バックアップは自動的にスケジュールされます。 既存のバックアップを表示するには、以下のようにします。

1. サービス・ダッシュボードにナビゲートします。
2. タブ内の**「バックアップ (Backups)」**をクリックして、_「バックアップ (Backups)」_ページを開きます。 選択可能バックアップのリストが表示されます。

  ![選択可能バックアップ](./images/elastic_search-backups-show.png "選択可能バックアップのリスト。")

対応する行をクリックして、選択可能バックアップのオプションを展開します。
  ![バックアップ・オプション](./images/elastic_search-backups-options.png "バックアップのオプション。") 

### API を使用した既存のバックアップの表示

バックアップのリストを `GET /2016-07/deployments/:id/backups` エンドポイントで取得できます。サービス・インスタンス ID を含むファウンデーション・エンドポイントと、デプロイメント ID は、両方ともサービスの_「概要」_に表示されます。例: 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```  

## 手動バックアップの作成

スケジュールされたバックアップだけでなく、バックアップを手作業で作成することができます。 手動バックアップを作成するには、既存のバックアップを表示するためのステップに従った後、選択可能バックアップのリストの上にある**「今すぐバックアップ (Back up now)」**をクリックします。 バックアップが開始されたことを知らせるメッセージが表示され、選択可能バックアップのリストに「処理中」のバックアップが追加されます。

### API を使用したバックアップの作成

backups エンドポイントに POST 要求 `POST /2016-07/deployments/:id/backups` を送信して、手動でバックアップを開始できます。この要求はただちに戻り、実行中のバックアップのレシピ ID と情報を返します。
バックアップを使用するには、バックアップが完了したかどうかを backups エンドポイントで確認し、バックアップ ID を見つける必要があります。`GET /2016-07/deployments/:id/backups/` を使用します。

## バックアップのリストア

新しいサービス・インスタンスにバックアップをリストアするには、既存のバックアップを表示する手順を実行してから、対応する行をクリックして、リストアするバックアップのオプションを展開表示します。**「リストア (Restore)」**ボタンをクリックします。 復元が開始されたことを示すメッセージが表示されます。 新しいサービス・インスタンスに自動的に「elasticsearch-restore-[timestamp]」という名前が付けられ、プロビジョニングが始まるとダッシュボードに表示されます。

### {{site.data.keyword.cloud_notm}} CLI を使用したリストア

{{site.data.keyword.cloud_notm}} CLI を使用して、実行中の Elasticsearch サービスのバックアップを新しい Elasticsearch サービスにリストアするには、次の手順を実行します。 
1. 必要に応じて、[CLI をダウンロードしてインストールします](https://console.bluemix.net/docs/cli/index.html#overview)。 
2. サービスの_「バックアップ」_ページで、リストアするバックアップを見つけ、バックアップ ID をコピーします。  
  **または**  
`GET /2016-07/deployments/:id/backups` を使用して、Compose API でバックアップとその ID を見つけます。ファウンデーション・エンドポイントとサービス・インスタンス ID は、両方ともサービスの _「概要」_に表示されます。例: 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
応答には、そのサービス・インスタンスに使用可能なすべてのバックアップのリストが含まれています。リストアするバックアップを選択し、その ID をコピーします。

3. 適切なアカウントと資格情報を使用してログインします。`bx login` (または、すべてのログイン・オプションを表示するには、`bx login -help` を使用)

4. 組織とスペースに切り替えます (`bx target -o "$YOUR_ORG" -s "YOUR_SPACE"`)。

5. `service create` コマンドを使用して新規サービスをプロビジョンし、リストアするソース・サービスと特定のバックアップを JSON オブジェクトで指定します。例:
``` 
bx service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
_「SERVICE」_フィールドは compose-for-elasticsearch、_「PLAN」_フィールドはご使用の環境に応じて「Standard」または「Enterprise」のいずれかでなければなりません。_SERVICE\_INSTANCE\_NAME_ は、新規サービスの名前を入力する場所です。_source\_service\_instance\_id_ は、バックアップのソースのサービス・インスタンス ID です。`bx cf service DISPLAY_NAME --guid` を実行して取得できます。ここで、_DISPLAY\_NAME_ はバックアップ元のサービスの名前です。 
  
  また、エンタープライズ・ユーザーは、`"cluster_id": "$CLUSTER_ID"` パラメーターを使用して、デプロイ先のクラスターも JSON オブジェクトに指定する必要があります。
  

### 新規バージョンへのマイグレーション

一部のメジャー・バージョンのアップグレードは、現在実行中のデプロイメントでは使用できません。アップグレードしたバージョンを実行する新規サービスをプロビジョンしてから、バックアップを使用してそのサービスにデータをマイグレーションする必要があります。このプロセスは、アップグレード先のバージョンを指定することを除いて、前述のバックアップのリストアと同じです。

``` 
bx service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

例えば、古いバージョンの {{site.data.keyword.composeForElasticsearch}} サービスを Elasticsearch 6.2.2 を実行する新規サービスにリストアする場合は、次のようにします。
```
bx service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
