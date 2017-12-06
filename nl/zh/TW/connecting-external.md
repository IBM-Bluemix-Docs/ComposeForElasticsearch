---

copyright:
  years: 2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 連接外部應用程式
{: #connecting-external-app}

所有新的 {{site.data.keyword.composeForElasticsearch_full}} 部署只接受以 Let's Encrypt 憑證支持的 TLS/SSL (`https://`) 安全連線。

有多種方式可用來連接至 Elasticsearch，視您使用的驅動程式而定。{{site.data.keyword.composeForElasticsearch}} 使用 URI 格式來顯示訊息，其格式如下：

```text
https://[username]:[password]@[host]:[port]/
```

您將在 {{site.data.keyword.composeForElasticsearch}} 服務的*概觀* 頁面中找到連線字串。

這裡的範例涵蓋 Node、Go、Java、Ruby 及 Python。這些範例會設定 {{site.data.keyword.composeForElasticsearch}} 的安全連線，然後呼叫 Elasticsearch 叢集 API 來執行基本的性能檢查，這會告訴您叢集的執行方式。若要熟悉 Elasticsearch 的 API，建議您參閱 Elasticsearch 2.4 的 Elasticsearch [參考資料](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html)。

您可以在 [github.com/compose-ex/elasticsearchconns](https://github.com/compose-ex/elasticsearchconns) 找到這個範例和後續範例的完整程式碼。

## Node 及 Elasticsearch

### 安裝用戶端

建立專案，然後使用 `npm install elasticsearch --save` 來安裝 [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) 套件。安裝完成後，您就可以撰寫程式碼來連接至部署。

### 建立連線

首先，您將必須要求 (`require`) 您已在專案的 `node_modules` 資料夾中安裝的 `elasticsearch` 程式庫，並儲存為 `package.json` 檔案中的相依關係。

```javascript
const elasticsearch = require('elasticsearch');
```

Elasticsearch 套件提供一個 Client 原型，用來建立與 Elasticsearch 的連線：

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Compose connection strings
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

我們從使用 `elasticsearch` 程式庫的 `Client` 原型來建立變數 `client` 及連線開始。其中，此參數會採用 `host` 索引鍵，而其具有的陣列值應該包含來自「概觀」頁面的連線字串 URL。

client 物件會實作[廣泛的 API](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html)。在此範例中，我們只會使用該 API，透過 `cluster.health` 呼叫來查詢叢集的性能。

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

呼叫會傳回 Javascript 物件，其中包含叢集性能的詳細資料。程式碼接著會將其印出，然後關閉用戶端並結束。

```shell
{ cluster_name: 'latest-elasticsearch',
  status: 'green',
  timed_out: false,
  number_of_nodes: 3,
  number_of_data_nodes: 3,
  active_primary_shards: 21,
  active_shards: 57,
  relocating_shards: 0,
  initializing_shards: 0,
  unassigned_shards: 0,
  delayed_unassigned_shards: 0,
  number_of_pending_tasks: 0,
  number_of_in_flight_fetch: 0,
  task_max_waiting_in_queue_millis: 0,
  active_shards_percent_as_number: 100 }
```

## Go 及 Elasticsearch

### 安裝用戶端

有一些使用 Go 語言的驅動程式。我們將對此範例使用 [Elastic](https://github.com/olivere/elastic) - 請參閱 [Elastic](https://olivere.github.io/elastic/) 網站及 [GoDocs](https://godoc.org/gopkg.in/olivere/elastic.v3) 上 Elastic 的文件及範例。撰寫時，Compose 支援 Elasticsearch 2.4.0，這表示您必須使用 3.0 版的 Elastic 套件。 

若要取得 Elastic 套件，請在終端機中執行 `go get gopkg.in/olivere/elastic.v3`。

在程式碼範例中，我們已將所有程式碼放在 `main` 函數中。首先，我們將建立 `client`，然後將連線字串插入 `SetURL` 方法中。

```go
package main

import (
			"fmt"
			"log"
  		// v3 for Elasticsearch 2.x
			"gopkg.in/olivere/elastic.v3"
)

func main() {
  		// create a client and add Compose connection strings
			client, err := elastic.NewClient(
				elastic.SetURL("https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113", "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113"),
				elastic.SetSniff(false),
			)
			if err != nil {
				log.Fatal(err)
			}
```

在設定連線並建立 `client` 之後，我們可呼叫其 `ClusterHealth` 方法來設定叢集性能的要求。由於執行該要求，將會呼叫 `Do` 方法。這會傳回 `Health` 結構，並將結果列印到終端機。 

```go
// create a variable that stores the result 
  		// of the executed cluster health query and prints the result
			health, err := client.ClusterHealth().Do()
			if err != nil {
				log.Fatal(err)
			}
			fmt.Printf("<------ Cluster Health ------>\n%+v\n", health)

}
```

```shell
<------ Cluster Health ------>
&{ClusterName:latest-elasticsearch Status:green TimedOut:false NumberOfNodes:3 NumberOfDataNodes:3 ActivePrimaryShards:21 ActiveShards:57 RelocatingShards:0 InitializingShards:0 UnassignedShards:0 DelayedUnassignedShards:0 NumberOfPendingTasks:0 NumberOfInFlightFetch:0 TaskMaxWaitTimeInQueueInMillis:0 ActiveShardsPercentAsNumber:100 ValidationFailures:[] Indices:map[]}
```

## Java 及 Elasticsearch

### 安裝用戶端

我們將在下列範例中使用的用戶端是 [Jest](https://github.com/searchbox-io/Jest)，它提供您一個適用於 Java 的簡單 HTTP REST 用戶端。您可以遵循其安裝手冊，並檢視其 [Github 儲存庫](https://github.com/searchbox-io/Jest/tree/master/jest)上的程式碼範例。

### 建立連線

在範例中，所有程式碼都包含在 `main` 方法內。首先，從 Apache 的 Log4j 程式庫新增 ` BasicConfigurator.configure();`，以在主控台中顯示連線處理程序。如果您未新增它，仍將連接至您的部署，但會收到警告，告訴您要使用 Log4j。 

```java
public class ElasticsearchConnect {
    public static void main(String[] args) throws IOException {

      	// shows connection process
        BasicConfigurator.configure();

      	// start of Jest library methods
        JestClientFactory factory = new JestClientFactory();
        factory.setHttpClientConfig(new HttpClientConfig
                .Builder(
                  Arrays.asList(
        // Compose connection strings
                    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113", 
                    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113"))
                .multiThreaded(true)
                .build());
```
然後，您將建立新的 `JestClientFactory`。`factory` 將提供 `setHttpClientConfig` 方法來配置您的用戶端。在 Jest 的 `Builder` 方法內使用 `Arrays.asList`，以建立包含兩個 Compose Elasticsearch 連線字串的陣列。然後，您將呼叫 `build` 方法來建立連線。
```java
        JestClient client = factory.getObject();
        Health health = new Health.Builder().build();
        JestResult result = client.execute(health);

        // prints output of Elasticsearch cluster health check
        System.out.printf("\n\n<------ CLUSTER HEALTH ------>\n%s\n\n", result.getJsonObject());
				// shuts down the connection
        client.shutdownClient();
    }
}
```

一旦建置了連線後，您就可以從 Connection Factory 物件 `factory.getObject()` 建立 `JestClient` 實例。`JestClient` 將用來在您建置的任何 Elasticsearch 查詢上呼叫 `execute` 方法。在此範例中，我們建置 (`build`) `Health` 查詢，以使用 Elasticsearch 的建置器類別來查看叢集的性能。 

一旦建置了查詢後，請使用 `JestResult` 來取得文件，並將它作為 JSON 物件列印到終端機，然後使用 `shutdownClient` 來關閉用戶端。 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby 及 Elasticsearch

### 安裝用戶端
若要使用 Elasticsearch 與 Ruby 搭配，請安裝 [Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api) `gem install elasticsearch`。安裝完成後，您就能夠在 Ruby 檔案中要求 (`require`) 程式庫。 

### 建立連線

首先，您將必須要求 (`require`) 將 `elasticsearch` 程式庫放入 Ruby 檔案中。

```ruby
require 'elasticsearch'
```

然後，定義變數並將它指派給 `ElasticSearch::Client` 建構子，以建立新的用戶端。在建構子內，使用 `urls` 引數，並將您的連線字串放在其中。有其他引數（例如 `host`、`port`、`user` 及 `password`）可供使用，並可讓您剖析連線字串，但它會接受整個連線字串。

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

然後，若要檢視 Elasticsearch 叢集的性能，您只需使用您建立的連線 `client`，並使用 [Elasticsearch API](http://www.rubydoc.info/gems/elasticsearch-api) 所提供的類別及方法。在此範例中，我們會將叢集性能結果的雜湊列印到終端機中。

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python 及 Elasticsearch

### 安裝用戶端

使用 Elasticsearch 與 Python 搭配，將需要您使用 `pip install elasticsearch` 來安裝程式庫，然後將它匯入至 Elasticsearch 專案。安裝完成後，您將程式庫匯入 (`import`) 至 Python 專案。

### 建立連線

類似於 Ruby 連線，您首先需要將 Elasticsearch 匯入至專案檔：

```python
from elasticsearch import Elasticsearch
```

接下來，定義一個變數，並將它指派給 `Elasticsearch` 類別，其中包含具有連線字串的陣列。此類別可讓您將 `hosts` 定義為陣列或單一連線字串，或定義為包含 `host` 及 `port` 的字典。它也具有用來設定吸入選項及 SSL/TLS 選項的引數。

```python
es = Elasticsearch(
  # Compose connection strings
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

然後，若要列印叢集的性能，您只需使用 `cluster` 類別來呼叫 `health` 方法，此方法會使用 `print` 函數在終端機上列印性能。其他可用的類別和方法可在 Python 的 Elasticsearch API [文件](http://elasticsearch-py.readthedocs.io/en/master/api.html) 中找到。

```python
print(es.cluster.health())
```

這將導致您將叢集性能的字典列印到終端機。

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## 在指令行上連接至 Elasticsearch

為了從指令行連接至 Elasticsearch 部署，在 {{site.data.keyword.composeForElasticsearch}} 服務的*概觀* 頁面的_連線字串_ 畫面中，{{site.data.keyword.composeForElasticsearch}} 提供您連線字串的 curl 指令。

終端機中的輸出將如下所示：
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```
