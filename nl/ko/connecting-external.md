---

copyright:
  years: 2016, 2018
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 외부 애플리케이션 연결
{: #connecting-external-app}

모든 신규 {{site.data.keyword.composeForElasticsearch_full}} 배치는 Let's Encrypt 인증서로 백업되는 TLS/SSL(`https://`) 보안 연결만 허용합니다.

사용 중인 드라이버에 따라 Elasticsearch에 연결하는 여러 가지 방법이 있습니다. {{site.data.keyword.composeForElasticsearch}}에서는 URI 형식을 사용하여 다음과 같이 형식화된 메시지를 표시합니다.

```text
https://[username]:[password]@[host]:[port]/
```

{{site.data.keyword.composeForElasticsearch}} 서비스의 *개요* 페이지에서 연결 문자열을 찾을 수 있습니다.

여기에 나오는 예제에서는 Node, Go, Java, Ruby 및 Python에 대해 다룹니다. 이러한 예제에서는 {{site.data.keyword.composeForElasticsearch}}에 대한 보안 연결을 설정한 후 Elasticsearch 클러스터 API를 호출하여 클러스터가 어떻게 수행되고 있는지를 알리는 기본 상태 검사를 수행합니다. Elasticsearch API에 익숙해지려면 Elasticsearch 2.4에 대한 Elasticsearch [참조서](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html)를 보는 것이 좋습니다.

이에 대한 전체 코드와 후속 예제는 [github.com/compose-ex/elasticsearchconns](https://github.com/compose-ex/elasticsearchconns)에서 찾을 수 있습니다.

## Node 및 Elasticsearch

### 클라이언트 설치

프로젝트를 작성한 후 `npm install elasticsearch --save`를 사용하여 [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) 패키지를 설치하십시오. 설치가 되면 코드를 작성하여 배치에 연결할 수 있습니다.

### 연결 작성

먼저, 프로젝트의 `node_modules` 폴더에 설치하고 `package.json` 파일에 종속 항목으로 저장한 `elasticsearch` 라이브러리를 `require`해야 합니다.

```javascript
const elasticsearch = require('elasticsearch');
```

elasticsearch 패키지는 Elasticsearch에 대한 연결을 작성하는 데 사용하는 Client 프로토타입을 제공합니다.

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Compose connection strings
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

`elasticsearch` 라이브러리의 `Client` 프로토타입을 사용하여 `client` 변수와 연결을 작성하여 시작합니다. 이는 여러 매개변수 중에서 개요 페이지의 연결 문자열 URL을 포함해야 하는 배열 값이 있는 `host` 키를 받아들입니다.

클라이언트 오브젝트는 [광범위한 API](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html)를 구현합니다. 이 예제에서 알려준 대로 단순히 해당 API를 사용하여 `cluster.health` 호출을 통해 클러스터의 상태를 조회합니다.

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

이 호출은 Javascript 오브젝트를 클러스터 상태의 세부사항과 함께 리턴합니다. 그런 다음, 코드가 이를 인쇄하고 클라이언트를 닫은 후 종료합니다.

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

## Go 및 Elasticsearch

### 클라이언트 설치

Go 언어에 대해 작업하는 몇 가지 드라이버가 있습니다. 이 예제에서는 [Elastic](https://github.com/olivere/elastic)을 사용합니다. [Elastic](https://olivere.github.io/elastic/) 사이트 및 [GoDocs](https://godoc.org/gopkg.in/olivere/elastic.v3)에서 Elastic에 대한 문서와 예제를 참조하십시오. 이 문서가 작성된 시점을 기준으로 Compose는 Elasticsearch 2.4.0을 지원합니다. 즉, 버전 3.0의 Elastic 패키지를 사용해야 합니다. 

Elastic 패키지를 얻으려면 터미널에서 `go get gopkg.in/olivere/elastic.v3`을 실행하십시오.

코드 예제에서는 모든 코드를 `main` 함수에 배치했습니다.  먼저, `client`를 작성하고 `SetURL` 메소드에 연결 문자열을 삽입합니다.

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

연결을 설정하고 `client`를 작성한 후 `ClusterHealth` 메소드를 호출하여 클러스터 상태에 대한 요청을 설정할 수 있습니다. 이 메소드의 결과에서 `Do` 메소드를 호출하면 해당 요청이 실행됩니다. 이는 `Health` 구조체를 리턴하고 결과를 터미널에 인쇄합니다. 

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

## Java 및 Elasticsearch

### 클라이언트 설치

다음 예제에서 사용 중인 클라이언트는 간단한 Java용 HTTP REST 클라이언트를 제공하는 [Jest](https://github.com/searchbox-io/Jest)입니다. 해당 설치 안내서에 따르고 [Github 저장소](https://github.com/searchbox-io/Jest/tree/master/jest)에서 코드 예제를 볼 수 있습니다.

### 연결 작성

이 예제에서 모든 코드는 `main` 메소드 내에 포함됩니다. 먼저, 콘솔에 연결 프로세스를 표시하기 위해 Apache Log4j 라이브러리의 `BasicConfigurator.configure();`를 추가합니다. 이를 추가하지 않으면 계속 배치에 연결하지만 Log4j를 사용하도록 알리는 경고가 수신됩니다. 

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
다음으로, 새로운 `JestClientFactory`를 작성합니다. `factory`는 클라이언트를 구성하기 위한 `setHttpClientConfig` 메소드를 제공합니다. Jest의 `Builder` 메소드 내에 있는 `Arrays.asList`를 사용하여 두 개의 Compose Elasticsearch 연결 문자열을 모두 포함하는 배열을 작성하십시오. 그런 다음 `build` 메소드를 호출하여 연결을 작성합니다. 
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

연결이 빌드되면 연결 팩토리 오브젝트 `factory.getObject()`에서 `JestClient` 인스턴스를 작성할 수 있습니다. `JestClient`는 빌드하는 Elasticsearch 조회에 대한 `execute` 메소드를 호출하는 데 사용됩니다. 이 예제에서는 Elasticsearch의 빌더 클래스를 사용하여 클러스터의 상태를 보기 위해 `Health` 조회를 `build`합니다. 

조회를 빌드한 후 `JestResult`를 사용하여 문서를 가져와서 터미널에 JSON 오브젝트로 인쇄한 후 `shutdownClient`로 클라이언트를 닫으십시오. 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby 및 Elasticsearch

### 클라이언트 설치
Elasticsearch를 Ruby와 함께 사용하려면 [Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api)을 설치하십시오(`gem install elasticsearch`). 설치가 되었으면 라이브러리를 Ruby 파일에 `require`할 수 있습니다. 

### 연결 작성

먼저, `elasticsearch`를 Ruby 파일로 `require`해야 합니다.

```ruby
require 'elasticsearch'
```

그런 다음, 변수를 정의하고 `ElasticSearch::Client` 생성자에 지정하여 새 클라이언트를 작성하십시오. 생성자 내에서 `urls` 인수를 사용하고 연결 문자열을 거기에 배치하십시오. `host`, `port`, `user` 및 `password`와 같은 다른 인수가 사용 가능하며 연결 문자열을 구문 분석할 수 있도록 하지만 전체 연결 문자를 허용합니다.

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

그런 다음, Elasticsearch 클러스터의 상태를 보려면 작성한 연결 `client`를 사용하고 [Elasticsearch API](http://www.rubydoc.info/gems/elasticsearch-api)에서 제공되는 클래스와 메소드를 사용합니다. 이 예제에서는 클러스터 상태의 결과에 대한 해시를 터미널에 인쇄합니다.

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python 및 Elasticsearch

### 클라이언트 설치

Elasticsearch를 Python과 함께 사용하려면 `pip install elasticsearch`를 사용하여 라이브러리를 설치한 후 Elasticsearch 프로젝트로 가져와야 합니다. 설치가 되었으면 라이브러리를 Python 프로젝트로 `import`합니다.

### 연결 작성

Ruby 연결과 마찬가지로 먼저 Elasticsearch를 프로젝트 파일로 가져와야 합니다.

```python
from elasticsearch import Elasticsearch
```

다음으로 변수를 정의하여 연결 문자열이 있는 배열이 포함된 `Elasticsearch` 클래스에 지정하십시오. 이 클래스를 사용하면 `hosts`를 배열 또는 단일 연결 문자열이나 `host` 및 `port`를 포함하는 사전으로 정의할 수 있습니다. 또한 검색 옵션 및 SSL/TLS 옵션을 설정하기 위한 인수가 있습니다.

```python
es = Elasticsearch(
  # Compose connection strings
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

그런 다음, 클러스터의 상태를 인쇄하려면 `cluster` 클래스만 사용하면 됩니다. 이 클래스는 `print` 함수를 사용하여 터미널에 인쇄되는 `health` 메소드를 호출합니다. 사용 가능한 다른 클래스 및 메소드는 다음 Python Elasticsearch API 문서에서 찾을 수 있습니다. http://elasticsearch-py.readthedocs.io/en/master/api.html

```python
print(es.cluster.health())
```

이 결과로 클러스터 상태의 사전이 터미널에 인쇄됩니다.

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## 명령행에서 Elasticsearch에 연결

명령행에서 Elasticsearch 배치에 연결할 수 있도록 {{site.data.keyword.composeForElasticsearch}}는 {{site.data.keyword.composeForElasticsearch}} 서비스의 *개요* 페이지에 있는 _연결 문자열_ 패널에서 연결 문자열에 대한 curl 명령을 제공합니다.

터미널의 출력은 다음과 유사합니다.
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```
