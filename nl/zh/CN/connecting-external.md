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

# 连接外部应用程序
{: #connecting-external-app}

所有新的 {{site.data.keyword.composeForElasticsearch_full}} 部署仅接受使用 Let's Encrypt 证书支持的 TLS/SSL (`https://`) 安全连接。

根据您使用的驱动程序，有多种方法可连接到 Elasticsearch。{{site.data.keyword.composeForElasticsearch}} 使用 URI 格式来显示格式化为以下内容的消息：

```text
https://[username]:[password]@[host]:[port]/
```

您将在 {{site.data.keyword.composeForElasticsearch}} 服务的*概述*页面上找到连接字符串。

此处的示例涵盖 Node、Go、Java、Ruby 和 Python。这些示例设置 {{site.data.keyword.composeForElasticsearch}} 的安全连接，然后调用 Elasticsearch 集群 API 来执行基本的运行状况检查，这将告诉您集群如何运行。为了熟悉 Elasticsearch 的 API，我们建议您查看 Elasticsearch 2.4 的 Elasticsearch [参考](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html)。

此示例和后续示例的完整代码可在 [github.com/compose-ex/elasticsearchconns](https://github.com/compose-ex/elasticsearchconns) 中找到。

## Node 和 Elasticsearch

### 安装客户机

创建项目，然后使用 `npm install elasticsearch --save` 安装 [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) 软件包。通过该安装，您可以编写代码以连接到您的部署。

### 创建连接

首先，您需要 `require` 在项目 `node_modules` 文件夹中安装并在 `package.json` 文件中存储为依赖关系的 `elasticsearch` 库。

```javascript
const elasticsearch = require('elasticsearch');
```

elasticsearch 软件包提供了一个用于创建 Elasticsearch 连接的 Client 原型：

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Compose connection strings
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

首先，通过使用 `elasticsearch` 库的 `Client` 原型来创建变量 `Client` 和连接。这会在其他参数间采用一个 `host` 键，其数组值应该包含“概述”页面中的连接字符串 URL。

客户机对象实现[广泛 API](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html)。在此示例中，我们将仅使用该 API 通过 `cluster.health` 调用来查询集群的运行状况。

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

此调用会返回包含集群运行状况详细信息的 Javascript 对象。然后，该代码将打印该信息，关闭客户机并退出。

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

## Go 和 Elasticsearch

### 安装客户机

有一些驱动程序使用 Go 语言。我们将为此示例使用 [Elastic](https://github.com/olivere/elastic) - 请参阅 [Elastic](https://olivere.github.io/elastic/) 站点和 [GoDocs](https://godoc.org/gopkg.in/olivere/elastic.v3)（针对 Elastic）上的文档和示例。在编写本文时，Compose 支持 Elasticsearch 2.4.0，这意味着您必须使用 Elastic 软件包 V3.0。 

要获取 Elastic 软件包，请在终端中运行 `go get gopkg.in/olivere/elastic.v3`。

在代码示例中，将所有代码都放在 `main` 函数中。首先，我们将创建一个 `client`，并将连接字符串插入到 `SetURL` 方法中。

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

设置连接并创建 `client` 后，可以调用其 `ClusterHealth` 方法来设置对集群运行状况的请求。对请求执行的结果调用 `Do` 方法。这将返回 `Health` 结构，并将结果打印到您的终端。 

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

## Java 和 Elasticsearch

### 安装客户机

我们在以下示例中使用的客户机是 [Jest](https://github.com/searchbox-io/Jest)，它为您提供了一个用于 Java 的简单 HTTP REST 客户机。您可以遵循其 [Github 存储库](https://github.com/searchbox-io/Jest/tree/master/jest) 上的安装指南并查看代码示例。

### 创建连接

在该示例中，所有代码都包含在 `main` 方法中。首先，从 Apache 的 Log4j 库中添加 `BasicConfigurator.configure();` 以在控制台中显示连接进程。如果您未进行添加，那么您仍会连接到您的部署，但是您将收到警告，告诉您使用 Log4j。 

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
接下来，您将创建新的 `JestClientFactory`。`factory` 将为您提供 `setHttpClientConfig` 方法来配置客户机。在 Jest 的 `Builder` 方法中使用 `Arraws.asList` 以创建包含您的两个 Compose Elasticsearch 连接字符串的数组。然后，您将调用 `build` 方法来创建连接。
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

一旦构建了连接，就可以从连接工厂对象 `factory.getObject()` 创建 `JestsClient` 实例。`JestClient` 将用于对您构建的任何 Elasticsearch 查询调用 `execute` 方法。在此示例中，我们使用 Elasticsearch 的构建器类 `build` `Health` 查询，以查看集群的运行状况。 

构建查询后，使用 `JestResult` 获取文档并以 JSON 对象的方式将其打印到终端，然后使用 `shutdownClient` 关闭客户机。 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby 和 Elasticsearch

### 安装客户机
要将 Elasticsearch 与 Ruby 配合使用，请安装 [Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api) `gem install elasticsearch`。通过该安装，您能够在 Ruby 文件中 `require` 库。 

### 创建连接

首先，您必须在 Ruby 文件中 `require` `elasticsearch` 库。

```ruby
require 'elasticsearch'
```

然后，通过定义变量并将其指定给 `ElasticSearch::Client` 构造函数来创建新的客户机。在构造函数内，使用 `urls` 参数并将连接字符串放在此处。其他参数（例如 `host`、`port`、`user` 和 `password`）可用，并允许您解析连接字符串，但它将接受整个连接字符串。

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

然后，要查看 Elasticsearch 集群的运行状况，只需使用您创建的连接 `client`，并使用 [Elasticsearch API](http://www.rubydoc.info/gems/elasticsearch-api) 提供的类和方法。对于此示例，我们是在将集群运行状况结果的散列打印到终端。

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python 和 Elasticsearch

### 安装客户机

使用 Elasticsearch 搭配 Python 需要您通过 `pip install elasticsearch` 安装库，然后将其导入 Elasticsearch 项目中。安装后，您会将该库 `import` 到 Python 项目中。

### 创建连接

与 Ruby 连接类似，您首先需要将 Elasticsearch 导入到项目文件中：

```python
from elasticsearch import Elasticsearch
```

接下来，定义一个变量并将其分配给包含带有连接字符串的数组的 `Elasticsearch` 类。此类允许您将 `hosts` 定义为数组或单个连接字符串，或定义为包含 `host` 和 `port` 的字典。它还具有用于设置嗅探选项和 SSL/TLS 选项的自变量。

```python
es = Elasticsearch(
  # Compose connection strings
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

然后，要打印集群的运行状况，您需要使用的是 `cluster` 类来调用 `health` 方法，使用 `print` 函数可在终端上打印该方法。其他可用的类和方法可在 Python 的 Elasticsearch API [documentation}(http://elasticsearch-py.readthedocs.io/en/master/api.html) 中找到。

```python
print(es.cluster.health())
```

这会将集群的运行状况字典打印到终端。

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## 在命令行上连接到 Elasticsearch

要从命令行连接到 Elasticsearch 部署，{{site.data.keyword.composeForElasticsearch}} 会在 {{site.data.keyword.composeForElasticsearch}} 服务的*概述*页面的_连接字符串_面板中为您的连接字符串提供 curl 命令。

终端中的输出类似于以下内容：
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```
