---

copyright:
  years: 2016, 2018
lastupdated: "2017-07-13"

keywords: elasticsearch, compose

subcollection: compose-for-elasticsearch

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting an external application
{: #external-app}

All new {{site.data.keyword.composeForElasticsearch_full}} deployments accept only TLS/SSL (`https://`) secured connections that are backed with a Let's Encrypt certificate.

You can connect to Elasticsearch in a number of ways, depending on which driver you are using. {{site.data.keyword.composeForElasticsearch}} uses the URI format to display messages:

```text
https://[username]:[password]@[host]:[port]/
```

You can find your connection string on the *Overview* page of your {{site.data.keyword.composeForElasticsearch}} service.

The examples here cover Node, Go, Java, Ruby, and Python. They set up a secure connection to {{site.data.keyword.composeForElasticsearch}}, then call the Elasticsearch Cluster API to do a basic health check. To familiarize yourself with the Elasticsearch API, look at the [reference documentation](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html) for Elasticsearch 2.4.

You can find the full code for this and other examples at [github.com/compose-ex/elasticsearchconns](https://github.com/compose-ex/elasticsearchconns).
{: tip}

## Node and Elasticsearch

### Installing the Client
{: #installing-client-node}

Create your project, then install the [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) package with `npm install elasticsearch --save`. With that installed you can write the code to connect to your deployment.

### Creating the Connection
{: #creating-connection-node}

First, you need to `require` the `elasticsearch` library that you have installed in your project's `node_modules` folder and saved as a dependency in the `package.json` file.

```javascript
const elasticsearch = require('elasticsearch');
```

The elasticsearch package offers a Client prototype, which you use to create a connection to Elasticsearch:

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Compose connection strings
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

Start by creating a variable `client` and a connection by using the `elasticsearch` library's `Client` prototype. This takes, among other parameters, a `host` key with an array value, which should contain your connection strings URLs from the Overview page.

The client object implements the [wide API](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html). In this example though, you use that API to query the health of the cluster by using the `cluster.health` call.

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

The call returns a JavaScript object with details of the cluster's health, prints the results, closes the client and exits.

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

## Go and Elasticsearch

### Installing the client
{: #installing-client-go}

There are a few drivers that work with the Go language. This example uses [Elastic](https://github.com/olivere/elastic) - see the documentation and examples on the [Elastic](https://olivere.github.io/elastic/) site and [GoDocs](https://godoc.org/gopkg.in/olivere/elastic.v3) for Elastic. As of writing, Compose supports Elasticsearch 2.4.0, which means that you must use version 3.0 of the Elastic package. 

To get the Elastic package, run `go get gopkg.in/olivere/elastic.v3` in your terminal.

In the code example, all of the code is in the `main` function. First, create a `client` and insert the connection strings into the `SetURL` method.

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

After you set up the connection and creating the `client`, you can call its `ClusterHealth` method to set up a request for the cluster health. Invoking the `Do` method on the result of that executes the request. The method returns a `Health` struct and prints the results into your terminal. 

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

## Java and Elasticsearch

### Installing the client
{: #installing-client-java}

The client in the following example is [Jest](https://github.com/searchbox-io/Jest), which provides you with an easy HTTP REST client for Java. You can follow their installation guide and view code examples on their [GitHub repository](https://github.com/searchbox-io/Jest/tree/master/jest).

### Creating a connection
{: #creating-connection-java}

In the example all the code is contained within the `main` method. First, add ` BasicConfigurator.configure();` from Apache's Log4j library to show the connection process in the console. If you do not add it, you can still connect to your deployment but you receive a warning to use Log4j. 

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
Next, create a new `JestClientFactory`. The `factory` provides you with a `setHttpClientConfig` method to configure your client. Use `Arrays.asList` within Jest's `Builder` method to create an array that contains both of your Compose Elasticsearch connection strings. Then, you invoke the `build` method to create the connection. 

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

After the connection has been built, you can create a `JestClient` instance from the connection factory object `factory.getObject()`. The `JestClient` is used to invoke the `execute` method on any Elasticsearch queries that you build. This example uses `build` to build a `Health` query to look at the health of the cluster by using Elasticsearch's builder classes. 

After you have built a query, use `JestResult` to get the documents and print it to the terminal as a JSON object then close down the client with `shutdownClient`. 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby and Elasticsearch

### Installing the client
{: #installing-client-ruby}

To use Elasticsearch with Ruby, install the [Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api) `gem install elasticsearch`. With that installed you will be able to `require` the library in your Ruby file. 

### Creating a connection
{: #creating-connection-ruby}

First, you have to `require` the `elasticsearch` library into your Ruby file.

```ruby
require 'elasticsearch'
```

Then, create a new client by defining a variable and assigning it to the `ElasticSearch::Client` constructor. Within the constructor, use the `urls` argument and place your connection strings there. Other arguments such as `host`, `port`, `user, and `password` are available and allow you to parse your connection string, but it accepts the entire connection string.

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

Then, to view the health of your Elasticsearch cluster, you use the connection `client` you created and use the classes and methods that are provided by the [Elasticsearch API](http://www.rubydoc.info/gems/elasticsearch-api). This example prints a hash of the results of the cluster's health to the terminal.

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python and Elasticsearch

### Installing the client
{: #installing-client-python}

Using Elasticsearch with Python requires you to install the library by using `pip install elasticsearch` and then importing it into your Elasticsearch project. With that installed you `import` the library into your Python project.

### Creating a connection
{: #creating-connection-python}

Like the Ruby connection, you first need to import Elasticsearch into your project file:

```python
from elasticsearch import Elasticsearch
```

Next, define a variable and assign it to the `Elasticsearch` class, which contains an array with your connection strings. The class allows you to define `hosts` as an array or as a single connection string, or as a dictionary including a `host` and `port`. It also has arguments to set sniffing options and SSL/TLS options.

```python
es = Elasticsearch(
  # Compose connection strings
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

Then, to print the cluster's health all you need to use is the `cluster` class to invoke the `health` method, which is printed on the terminal by using the `print` function. Other classes and methods that are available are found in Python's [Elasticsearch API documentation](http://elasticsearch-py.readthedocs.io/en/master/api.html).

```python
print(es.cluster.health())
```

This prints a dictionary of your cluster's health to the terminal.

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## Connecting to Elasticsearch on the Command Line

To connect to your Elasticsearch deployment from the command line, {{site.data.keyword.composeForElasticsearch}} provides you with the curl commands for your connection strings in the _Connection Strings_ panel of the *Overview* page of your {{site.data.keyword.composeForElasticsearch}} service.

The output in your terminal will look similar to this:
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```