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

# Connecting an external application
{: #connecting-external-app}

All new {{site.data.keyword.composeForElasticsearch_full}} deployments accept only TLS/SSL (`https://`) secured connections that are backed with a Let's Encrypt certificate.

There are a number of ways to connect to Elasticsearch, depending on which driver you are using. {{site.data.keyword.composeForElasticsearch}} uses the URI format to display messages, which is formatted as:

```text
https://[username]:[password]@[host]:[port]/
```

You'll find your connection string on the *Overview* page of your {{site.data.keyword.composeForElasticsearch}} service.

The examples here cover Node, Go, Java, Ruby and Python. These examples set up a secure connection to {{site.data.keyword.composeForElasticsearch}} then call the Elasticsearch Cluster API to do a basic health check, which will tell you how your cluster is doing. To familiarize yourself with Elasticsearch's API, we suggest that you look at the Elasticsearch [reference](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html) for Elasticsearch 2.4.

The full code for this and subsequent examples can be found at [github.com/compose-ex/elasticsearchconns.](https://github.com/compose-ex/elasticsearchconns).

## Node and Elasticsearch

### Installing the Client

Create your project then install the [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) package with `npm install elasticsearch --save`. With that installed you can write the code to connect to your deployment.

### Creating the Connection

First, you will need to `require` the `elasticsearch` library that you have installed in your project's `node_modules` folder and saved as a dependency in the `package.json` file.

```javascript
const elasticsearch = require('elasticsearch');
```

The elasticsearch package offers a Client prototype which we use to create a connection to Elasticsearch:

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Compose connection strings
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

We start by creating a variable `client` and a connection using the `elasticsearch` library's `Client` prototype. This takes, among other parameters, a `host` key with an array value which should contain your connection strings URLs from the Overview page.

The client object implements the [wide API](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html). In this example tought, we will simply use that API to query the health of the cluster by means of the `cluster.health` call.

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

The call returns a Javascript object with details of the cluster's health. The code then prints that, closes the client and exits.

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

There are a few drivers that work with the Go language. We will be using [Elastic](https://github.com/olivere/elastic) for this example - see the documentation and examples on the [Elastic](https://olivere.github.io/elastic/) site and [GoDocs](https://godoc.org/gopkg.in/olivere/elastic.v3) for Elastic. As of writing, Compose supports Elasticsearch 2.4.0, which means that you have to use version 3.0 of the Elastic package. 

To get the Elastic package, run `go get gopkg.in/olivere/elastic.v3` in your terminal.

In the code example, we placed all of the code in the `main` function.  First, we will create a `client` and insert the connection strings into the `SetURL` method.

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

After setting up the connection and creating the `client`, we can call its `ClusterHealth` method to set up a request for the cluster health. Invoking the `Do` method on the result of that executes that request. This returns a `Health` struct and prints the results into your terminal. 

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

The client we will be using in the following example is [Jest](https://github.com/searchbox-io/Jest) which provides you with an easy HTTP REST client for Java. You can follow their installation guide and view code examples on their [Github repository](https://github.com/searchbox-io/Jest/tree/master/jest).

### Creating a connection

In the example all the code is contained within the `main` method. First, we add ` BasicConfigurator.configure();` from Apache's Log4j library to show the connection process in the console. If you do not add it, you will still connect to your deployment but you will receive a warning telling you to use Log4j. 

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
Next, you will create a new `JestClientFactory`. The `factory` will provide you with a `setHttpClientConfig` method to configure your client. Use `Arrays.asList` within Jest's `Builder` method to create an array containing both of your Compose Elasticsearch connection strings. Then you will invoke the `build` method to create the connection. 
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

Once the connection has been built, you can create a `JestClient` instance from the connection factory object `factory.getObject()`. The `JestClient` will be used to invoke the `execute` method on any Elasticsearch queries that you build. In this example, we `build` a `Health` query to look at the health of the cluster using Elasticsearch's builder classes. 

Once you have built a query, use `JestResult` to get the documents and print it to the terminal as a JSON object then close down the client with `shutdownClient`. 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby and Elasticsearch

### Installing the client
To use Elasticsearch with Ruby, install the [Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api) `gem install elasticsearch`. With that installed you will be able to `require` the library in your Ruby file. 

### Creating a connection

First, you will have to `require` the `elasticsearch` library into your Ruby file.

```ruby
require 'elasticsearch'
```

Then create a new client by defining a variable and assigning it to the `ElasticSearch::Client` constructor. Within the constructor, use the `urls` argument and place your connection strings there. Other arguments such as `host`, `port`, `user` and `password` are available and allow you to parse your connection string, but it will accept the entire connection string.

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

Then to view the health of your Elasticsearch cluster, you will simply use the connection `client` you created and use the classes and methods provided by the [Elasticsearch API](http://www.rubydoc.info/gems/elasticsearch-api). For this example we are printing a hash of the results of the cluster's health to the terminal.

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python and Elasticsearch

### Installing the client

Using Elasticsearch with Python will require you to install the library using `pip install elasticsearch` and then importing it into your Elasticsearch project. With that installed you will `import` the library into your Python project.

### Creating a connection

Like the Ruby connection, you will first need to import Elasticsearch into your project file:

```python
from elasticsearch import Elasticsearch
```

Next, define a variable and assign it to the `Elasticsearch` class which contains an array with your connection strings. The class will allow you to define `hosts` as an array or as a single connection string, or as a dictionary including a `host` and `port`. It also has arguments to set sniffing options and SSL/TLS options.

```python
es = Elasticsearch(
  # Compose connection strings
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

Then to print the cluster's health all you need to use is the `cluster` class to invoke the `health` method, which is printed on the terminal using the `print` function. Other classes and methods that are available are found in Python's Elasticsearch API [documentation}(http://elasticsearch-py.readthedocs.io/en/master/api.html).

```python
print(es.cluster.health())
```

This will result in printing a dictionary of your cluster's health to the terminal.

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## Connecting to Elasticsearch on the Command Line

To connect to your Elasticsearch deployment from the command line, {{site.data.keyword.composeForElasticsearch}} provides you with the curl commands for your connection strings in the _Connection Strings_ panel of the *Overview* page of your {{site.data.keyword.composeForElasticsearch}} service.

The output in your terminal will look similar to this:
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```