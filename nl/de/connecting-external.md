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

# Externe Anwendung verbinden
{: #connecting-external-app}

Alle neuen {{site.data.keyword.composeForElasticsearch_full}}-Bereitstellungen akzeptieren nur mit TLS/SSL (`https://`) gesicherte Verbindungen, die durch ein Zertifikat von Let's Encrypt unterstützt werden.

Je nach dem verwendeten Treiber gibt es verschiedene Möglichkeiten, eine Verbindung zu Elasticsearch herzustellen. {{site.data.keyword.composeForElasticsearch}} zeigt Nachrichten im URI-Format an, das das folgende Format aufweist:

```text
https://[username]:[password]@[host]:[port]/
```

Sie finden Ihre Verbindungszeichenfolge auf der Seite *Übersicht* Ihres {{site.data.keyword.composeForElasticsearch}}-Service.

Die hier aufgeführten Beispiele gelten für Node, Go, Java, Ruby und Python. Mit diesen Beispielen wird eine sichere Verbindung zu {{site.data.keyword.composeForElasticsearch}} eingerichtet und dann Cluster-API von Elasticsearch aufgerufen, um eine grundlegende Statusprüfung durchzuführen, die Auskunft über den Zustand Ihres Clusters gibt. Es empfiehlt sich, die Elasticsearch-[Referenzinformationen](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html) für Elasticsearch 2.4 zu lesen, um sich mit der Elasticsearch-API vertraut zu machen.

Den vollständigen Code des nachstehenden und des darauffolgenden Beispiels finden Sie unter [github.com/compose-ex/elasticsearchconns](https://github.com/compose-ex/elasticsearchconns).

## Node und Elasticsearch

### Client installieren

Erstellen Sie Ihr Projekt und installieren Sie das [`elasticsearch`](https://www.npmjs.com/package/elasticsearch)-Paket mit dem Befehl `npm install elasticsearch --save`. Wenn das Paket installiert ist, können Sie den Code für die Verbindung zu Ihrer Bereitstellung schreiben.

### Verbindung erstellen

Als Erstes müssen Sie den Befehl `require` für die `elasticsearch`-Bibliothek absetzen, die Sie im Ordner `node_modules` Ihres Projekt installiert und als Abhängigkeit in der Datei `package.json` gespeichert haben.

```javascript
const elasticsearch = require('elasticsearch');
```

Das elasticsearch-Paket bietet einen Prototyp namens 'Client', mit dem eine Verbindung zu Elasticsearch erstellt wird:

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Compose connection strings
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

Zuerst werden mit dem Prototyp `Client` der `elasticsearch`-Bibliothek die Variable `client` und eine Verbindung erstellt. Diese nimmt neben anderen Parametern den Schlüssel `host` mit einem Array-Wert an, der Ihre Verbindungszeichenfolgen-URLs der Übersichtsseite enthalten sollte.

Das Clientobjekt implementiert die [Wide API](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html). Im vorliegenden Beispiel wird die API einfach zur Abfrage des Status des Clusters verwendet, und zwar durch den Aufruf von `cluster.health`.

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

Der Aufruf gibt ein JavaScript-Objekt mit Details zum Status des Clusters zurück. Dann druckt der Code diese Angaben, schließt den Client und wird beendet.

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

## Go und Elasticsearch

### Client installieren

Es gibt einige Treiber, die die Sprache Go nutzen. Für dieses Beispiel wird [Elastic](https://github.com/olivere/elastic) verwendet - siehe die Dokumentation und die Beispiele auf der Site von [Elastic](https://olivere.github.io/elastic/) und unter [GoDocs](https://godoc.org/gopkg.in/olivere/elastic.v3) for Elastic. Derzeit wird Elasticsearch 2.4.0 von Compose unterstützt. Sie müssen also Version 3.0 des Elastic-Pakets verwenden. 

Führen Sie zum Abrufen des Elastic-Pakets den Befehl `go get gopkg.in/olivere/elastic.v3` in Ihrem Terminal aus.

Im Codebeispiel ist der gesamte Code in der Funktion `main` enthalten.  Als Erstes wird ein `Client` erstellt. Dann werden die Verbindungszeichenfolgen in die Methode `SetURL` eingefügt.

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

Nach dem Einrichten der Verbindung und der Erstellung des `Clients` wird dessen Methode `ClusterHealth` aufgerufen, um eine Abfrage des Clusterstatus einzurichten. Mit dem Aufruf der Methode `Do` für das Ergebnis wird diese Abfrage ausgeführt. Dabei wird das Struct `Health` zurückgegeben und die Ergebnisse werden in Ihr Terminal gedruckt. 

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

## Java und Elasticsearch

### Client installieren

Im folgenden Beispiel wird der Client [Jest](https://github.com/searchbox-io/Jest) verwendet. Dieser stellt Ihnen einen einfachen HTTP REST-Client für Java bereit. Sie können sich nach dessen Installationshandbuch richten und in dessen [Github-Repository](https://github.com/searchbox-io/Jest/tree/master/jest) Codebeispiele anzeigen.

### Verbindung erstellen

Im vorliegenden Beispiel ist der gesamte Code in der Methode `main` enthalten. Als Erstes wird ` BasicConfigurator.configure();` von der Apache-Bibliothek Log4j hinzugefügt, um den Verbindungsprozess in der Konsole anzuzeigen. Auch wenn Sie dieses Element nicht hinzufügen, können Sie eine Verbindung zu Ihrer Bereitstellung herstellen. Sie erhalten jedoch die Warnung, dass Sie Log4j verwenden sollen. 

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
Als Nächstes erstellen Sie ein neues Element `JestClientFactory`. Das Element `factory` stellt Ihnen die Methode `setHttpClientConfig` zum Konfigurieren Ihres Clients bereit. Erstellen Sie mithilfe von `Arrays.asList` in der Jest-Methode `Builder` ein Array, das Ihre beiden Compose-Elasticsearch-Verbindungszeichenfolgen enthält. Anschließend rufen Sie die Methode `build` auf, um die Verbindung zu erstellen. 
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

Sobald die Verbindung erstellt wurde, können Sie aus dem Objekt `factory.getObject()` der Verbindungsfactory eine `JestClient`-Instanz erstellen. Mit `JestClient` wird die Methode `execute` für alle Elasticsearch-Abfragen aufgerufen, die Sie erstellt haben. Im vorliegenden Beispiel wird mit dem Befehl `build` eine `Health`-Abfrage erstellt, um mithilfe der Builderklassen von Elasticsearch den Status des Clusters zu bestimmen. 

Verwenden Sie `JestResult`, sobald Sie eine Abfrage erstellt haben, um die Dokumente abzurufen und als JSON-Objekt in das Terminal zu drucken. Schließen Sie den Client anschließend mit `shutdownClient`. 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby und Elasticsearch

### Client installieren
Installieren Sie zur Verwendung von Elasticsearch mit Ruby die Funktion [Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api) `gem install elasticsearch`. Nach der Installation können Sie den Befehl `require` für die Bibliothek in Ihrer Ruby-Datei absetzen. 

### Verbindung erstellen

Als Erstes müssen Sie mit `require` die `elasticsearch`-Bibliothek in Ihre Ruby-Datei integrieren.

```ruby
require 'elasticsearch'
```

Dann erstellen Sie einen neuen Client, indem Sie eine Variable definieren und sie dem Konstruktor `ElasticSearch::Client` zuweisen. Verwenden Sie im Konstruktor das Argument `urls` und platzieren Sie Ihre Verbindungszeichenfolge hier. Es sind andere Argumente wie `host`, `port`, `user` und `password` verfügbar, mit denen Sie Ihre Verbindungszeichenfolge parsen können. Er nimmt jedoch die gesamte Verbindungszeichenfolge an.

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

Um dann den Status Ihres Elasticsearch-Cluster anzuzeigen, verwenden Sie einfach den erstellten Verbindungs`client` und die von der [Elasticsearch-API](http://www.rubydoc.info/gems/elasticsearch-api) bereitgestellten Klassen und Methoden. Im vorliegenden Beispiel wird ein Hashwert der Ergebnisse des Clusterstatus in das Terminal gedruckt.

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python und Elasticsearch

### Client installieren

Zur Verwendung von Elasticsearch mit Python müssen Sie die Bibliothek mit dem Befehl `pip install elasticsearch` installieren und sie in Ihr Elasticsearch-Projekt importieren. Nach dieser Installation `importieren` Sie die Bibliothek in Ihr Python-Projekt.

### Verbindung erstellen

Wie bei der Ruby-Verbindung müssen Sie als Erstes Elasticsearch in Ihre Projektdatei importieren:

```python
from elasticsearch import Elasticsearch
```

Definieren Sie danach eine Variable und weisen Sie sie der `Elasticsearch`-Klasse zu, die ein Array mit Ihren Verbindungszeichenfolgen enthält. Die Klasse gibt Ihnen die Möglichkeit, `Hosts` als ein Array oder als einzelne Verbindungszeichenfolge bzw. als Wörterverzeichnis zu definieren, in den ein `Host` und ein `Port` eingebunden sind. Außerdem enthält sie Argumente zum Festlegen von Optionen zum Ausspionieren und für SSL/TLS.

```python
es = Elasticsearch(
  # Compose connection strings
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

Um dann den Clusterstatus zu drucken, müssen Sie lediglich mit der `Cluster`klasse die Methode `health` aufzurufen, die mithilfe der Funktion `print` auf dem Terminal gedruckt wird. Weitere verfügbare Klassen und Methoden finden Sie in der [Dokumentation} zur Elasticsearch-API von Python (http://elasticsearch-py.readthedocs.io/en/master/api.html).

```python
print(es.cluster.health())
```

Damit wird ein Wörterverzeichnis des Status Ihres Clusters in das Terminal gedruckt.

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## Verbindung zu Elasticsearch in der Befehlszeile herstellen

Um über die Befehlszeile eine Verbindung zu Ihrer Elasticsearch-Bereitstellung herzustellen, stellt {{site.data.keyword.composeForElasticsearch}} in der Anzeige _Verbindungszeichenfolgen_ der Seite *Übersicht* Ihres {{site.data.keyword.composeForElasticsearch}}-Service die Curl-Befehle für Ihre Verbindungszeichenfolgen bereit.

Die Ausgabe in Ihrem Terminal ähnelt der folgenden:
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```
