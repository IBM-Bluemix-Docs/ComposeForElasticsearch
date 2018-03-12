---

copyright:
  years: 2017, 2018
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 外部アプリケーションの接続
{: #connecting-external-app}

新しい {{site.data.keyword.composeForElasticsearch_full}} デプロイメントはすべて、Let's Encrypt 証明書に裏付けられた TLS/SSL (`https://`) セキュア接続のみ受け入れます。

Elasticsearch に接続する方法はいくつかあり、どのドライバーを使用するかによって異なります。 {{site.data.keyword.composeForElasticsearch}} は URI フォーマットを使用してメッセージを表示します。このフォーマットは次のとおりです。

```text
https://[username]:[password]@[host]:[port]/
```

接続ストリングは {{site.data.keyword.composeForElasticsearch}} サービスの*「概要」*ページに表示されます。

ここでの例は、Node、Go、Java、Ruby、Python をカバーします。 これらの例は、{{site.data.keyword.composeForElasticsearch}} へのセキュア接続をセットアップした後、Elasticsearch クラスター API を呼び出して基本的なヘルス・チェックを行います。ヘルス・チェックからクラスターの状況が通知されます。 Elasticsearch の API をよく理解するには、Elasticsearch 2.4 の Elasticsearch [リファレンス](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html)を調べることをお勧めします。

この例と後続の例の全コードは、[github.com/compose-ex/elasticsearchconns](https://github.com/compose-ex/elasticsearchconns) に掲載されています。

## Node と Elasticsearch

### クライアントのインストール

プロジェクトを作成した後に、`npm install elasticsearch --save` を使用して [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) パッケージをインストールします。 パッケージがインストールされたら、デプロイメントに接続するためのコードを書くことができます。

### 接続の作成

まず、プロジェクトの `node_modules` フォルダーにインストールして `package.json` ファイルに従属関係として保存した `elasticsearch` ライブラリーを `require` する必要があります。

```javascript
const elasticsearch = require('elasticsearch');
```

elasticsearch パッケージは、Elasticsearch への接続を作成するために使用する Client プロトタイプを提供します。

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Compose connection strings
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

`elasticsearch` ライブラリーの `Client` プロトタイプを使用して変数 `client` と接続を作成することから始めます。 そのためにはさまざまなパラメーターを必要としますが、特に `host` キーの配列値には、「概要」ページの接続ストリング URL を含める必要があります。

クライアント・オブジェクトは、[ワイド API](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html) を実装します。 ただし、この例では、`cluster.health` 呼び出しでクラスターの正常性を照会するためにだけ、その API を使用します。

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

この呼び出しは、クラスターの正常性の詳細を含む Javascript オブジェクトを返します。 次にコードはそれを表示し、クライアントを閉じてから終了します。

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

## Go と Elasticsearch

### クライアントのインストール

Go 言語でいくつかのドライバーを使用できます。 この例では、[Elastic](https://github.com/olivere/elastic) を使用します。[Elastic](https://olivere.github.io/elastic/) サイトと Elastic 用 [GoDoc](https://godoc.org/gopkg.in/olivere/elastic.v3) に掲載されている文書と例を参照してください。 この資料を作成している時点では、Compose は Elasticsearch 2.4.0 をサポートしています。つまり、バージョン 3.0 の Elastic パッケージを使用する必要があるということになります。 

Elastic パッケージを取得するには、端末で `go get gopkg.in/olivere/elastic.v3` を実行します。

コード例では、コードはすべて `main` 関数に置かれています。  まず、`client` を作成し、接続ストリングを `SetURL` メソッドに挿入します。

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

接続をセットアップして `client` を作成したら、その `ClusterHealth` メソッドを呼び出して、クラスターの正常性を確認する要求をセットアップできます。 その結果に対して `Do` を呼び出すと、その要求が実行されます。 これによって `Health` 構造体が返され、結果が端末に表示されます。 

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

## Java と Elasticsearch

### クライアントのインストール

次の例で使用するクライアントは、Java 用の簡単な HTTP REST クライアントを提供する [Jest](https://github.com/searchbox-io/Jest) です。 そのインストール・ガイドに従うとともに、[Github リポジトリー](https://github.com/searchbox-io/Jest/tree/master/jest)にあるコード例を参照してください。

### 接続の作成

この例では、`main` メソッド内にすべてのコードが含まれています。 まず、Apache の Log4j ライブラリーにある `BasicConfigurator.configure();` を追加して、接続プロセスをコンソールに表示します。 追加しなくてもデプロイメントに接続されますが、Log4j を使用するようにという警告を受け取ります。 

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
次に、新しい `JestClientFactory` を作成します。 この `factory` は、クライアントを構成するための `setHttpClientConfig` メソッドを提供します。 Jest の `Builder` メソッド内で `Arrays.asList` を使用して、両方の Compose Elasticsearch 接続ストリングを含む配列を作成します。 次に、`build` メソッドを呼び出して接続を作成します。 
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

接続が作成されたら、接続ファクトリー・オブジェクト `factory.getObject()` から `JestClient` インスタンスを作成できます。 `JestClient` は、作成する Elasticsearch 照会に対して `execute` メソッドを呼び出すために使用されます。 この例では、Elasticsearch の builder クラスを使用して、クラスターの正常性を調べるための `Health` 照会を `build` します。 

照会を作成したら、`JestResult` を使用してドキュメントを取得し、それを JSON オブジェクトとして端末に表示してから、`shutdownClient` を使用してクライアントを閉じます。 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby と Elasticsearch

### クライアントのインストール
Ruby で Elasticsearch を使用するには、[Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api) をインストールします (`gem install elasticsearch`)。 それがインストールされたら、Ruby ファイルの中にライブラリーを `require` できるようになります。 

### 接続の作成

まず、Ruby ファイルの中に `elasticsearch` ライブラリーを `require` する必要があります。

```ruby
require 'elasticsearch'
```

次に、変数を定義してそれを `ElasticSearch::Client` コンストラクターに割り当てることによって、新しいクライアントを作成します。 コンストラクター内で、`urls` 引数を使用して、接続ストリングをそこに置きます。 `host`、`port`、`user`、`password` などの他の引数を使用でき、接続ストリングを解析できますが、接続ストリング全体を受け入れます。

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

次に、Elasticsearch クラスターの正常性を調べます。作成した接続 `client` と、[Elasticsearch API](http://www.rubydoc.info/gems/elasticsearch-api) が提供するクラスとメソッドを使用するだけです。 この例では、クラスターの正常性の結果のハッシュを端末に表示します。

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python と Elasticsearch

### クライアントのインストール

Python で Elasticsearch を使用するには、`pip install elasticsearch` を使用してライブラリーをインストールして、それを Elasticsearch プロジェクトにインポートする必要があります。 ライブラリーがインストールされたら、Python プロジェクトに `import` します。

### 接続の作成

Ruby 接続と同様に、まずプロジェクト・ファイルに Elasticsearch をインポートする必要があります。

```python
from elasticsearch import Elasticsearch
```

次に、変数を定義して、接続ストリングの配列を含む `Elasticsearch` クラスに割り当てます。 このクラスで、`hosts` を配列として、または単一の接続ストリングとして、あるいは `host` と `port` を含むディクショナリーとして定義できます。 スニッフィング・オプションと SSL/TLS オプションを設定するための引数もあります。

```python
es = Elasticsearch(
  # Compose connection strings
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

次に、クラスターの正常性を表示するために使用する必要があるのは、`health` メソッドを呼び出す `cluster` クラスだけです。正常性は `print` 関数を使用して端末に表示されます。 使用可能なその他のクラスとメソッドについては、Python の Elasticsearch API [資料}(http://elasticsearch-py.readthedocs.io/en/master/api.html) を参照してください。

```python
print(es.cluster.health())
```

これにより、クラスターの正常性のディクショナリーが端末に表示されます。

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## コマンド・ラインでの Elasticsearch への接続

コマンド・ラインから Elasticsearch デプロイメントに接続できるように、{{site.data.keyword.composeForElasticsearch}} では、{{site.data.keyword.composeForElasticsearch}} サービスの *「概要」*ページの_「接続ストリング (Connection Strings)」_パネルに、接続ストリングのための curl コマンドが用意されています。

端末での出力は次のようになります。
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```
