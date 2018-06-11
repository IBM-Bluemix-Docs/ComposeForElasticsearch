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

# Connessione a un'applicazione esterna
{: #connecting-external-app}

Tutte le nuove distribuzioni {{site.data.keyword.composeForElasticsearch_full}} accettano solo le connessioni sicure (`https://`) TLS/SSL garantite da un certificato Let's Encrypt.

Esistono diversi modi per collegarsi a Elasticsearch, a seconda del driver che stai utilizzando. {{site.data.keyword.composeForElasticsearch}} utilizza il formato URI per visualizzare i messaggi, che è formattato come:

```text
https://[username]:[password]@[host]:[port]/
```

Troverai la tua stringa di connessione nella pagina *Overview* del tuo servizio {{site.data.keyword.composeForElasticsearch}}.

Gli esempi qui presenti coprono Node, Go, Java, Ruby e Python. Questi esempi configurano una connessione sicura a {{site.data.keyword.composeForElasticsearch}} quindi richiamano un'API cluster Elasticsearch per eseguire un controllo di integrità di base, che ti fornirà informazioni su cosa sta facendo il cluster. Per prendere familiarità con l'API Elasticsearch, ti suggeriamo di controllare Elasticsearch [reference](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html) per Elasticsearch 2.4.

Il codice completo per questo e i seguenti esempi può essere trovato all'indirizzo [github.com/compose-ex/elasticsearchconns.](https://github.com/compose-ex/elasticsearchconns).

## Node e Elasticsearch

### Installazione del client

Crea il tuo progetto e quindi installa il pacchetto [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) con `npm install elasticsearch --save`. Dopo l'installazione puoi scrivere il codice per il collegamento alla tua distribuzione.

### Creazione della connessione

Per prima cosa, dovrai utilizzare `require` per richiedere la libreria `elasticsearch` che hai installato nella tua cartella `node_modules` del progetto e salvata come una dipendenza nel file `package.json`.

```javascript
const elasticsearch = require('elasticsearch');
```

Il pacchetto elasticsearch offre un prototipo client che utilizziamo per creare una connessione a Elasticsearch:

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Stringhe connessione Compose
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

Iniziamo creando un variabile `client` e una connessione utilizzando il prototipo `Client` della libreria `elasticsearch`. Questo richiede, insieme ad altri parametri, una chiave `host` con un valore array che dovrebbe contenere i tuoi URL delle stringhe di connessione dalla pagina della panoramica.

L'oggetto client implementa la [wide API](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html). In questo esempio, utilizziamo semplicemente questa API per eseguire una query per l'integrità del cluster tramite una chiamata `cluster.health`.

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

La chiamata restituisce un oggetto Javascript con i dettagli sull'integrità del cluster. Il codice lo stampa, chiude il client ed esce.

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

## Go e Elasticsearch

### Installazione del client

Esistono alcuni driver che utilizzano il linguaggio Go. Noi utilizzeremo [Elastic](https://github.com/olivere/elastic) per questo esempio - consulta la documentazione e gli esempi nel sito [Elastic](https://olivere.github.io/elastic/) e [GoDocs](https://godoc.org/gopkg.in/olivere/elastic.v3) per Elastic. Come per la scrittura, Compose supporta Elasticsearch 2.4.0, il che significa che devi utilizzare la versione 3.0 del pacchetto Elastic. 

Per ottenere il pacchetto Elastic, esegui `go get gopkg.in/olivere/elastic.v3` nel tuo terminale.

Nell'esempio di codice, abbiamo inserito tutto il codice nella funzione `main`.  Per prima cosa, creiamo un `client` e inseriamo le stringhe di connessione nel metodo `SetURL`.

```go
package main

import (
			"fmt"
			"log"
  		// v3 per Elasticsearch 2.x
			"gopkg.in/olivere/elastic.v3"
)

func main() {
  		// crea un client e aggiungi le stringhe di connessione Compose
			client, err := elastic.NewClient(
				elastic.SetURL("https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113", "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113"),
				elastic.SetSniff(false),
			)
			if err != nil {
				log.Fatal(err)
			}
```

Dopo aver configurato la connessione e aver creato il `client`, possiamo richiamare il rispettivo metodo `ClusterHealth` per configurare una richiesta di integrità del cluster. Richiamare il metodo `Do` in modo che esegua tale richiesta. Questo metodo restituisce la struttura `Health` e stampa i risultati nel tuo terminale. 

```go
// crea una variabile che archivia il risultato
  		// della query di integrità del cluster eseguita e stampa il risultato
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

## Java e Elasticsearch

### Installazione del client

Il client che stiamo utilizzando nel seguente esempio è [Jest](https://github.com/searchbox-io/Jest) che ti fornisce un client REST HTTP semplice per Java. Puoi seguire il loro manuale di installazione e visualizzare gli esempi di codice nel loro [Github repository](https://github.com/searchbox-io/Jest/tree/master/jest).

### Creazione di una connessione

Nell'esempio tutto il codice è contenuto nel metodo `main`. Per prima cosa, aggiungiamo ` BasicConfigurator.configure();` dalla libreria Log4j di Apache per visualizzare il processo di connessione nella console. Se non la aggiungi, continuerai a collegarti alla tua distribuzione ma riceverai un'avvertenza di utilizzo di Log4j. 

```java
public class ElasticsearchConnect {
    public static void main(String[] args) throws IOException {

      	// mostra il processo di connessione
        BasicConfigurator.configure();

      	// avvia i metodi della libreria Jest
        JestClientFactory factory = new JestClientFactory();
        factory.setHttpClientConfig(new HttpClientConfig
                .Builder(
                  Arrays.asList(
        // Stringhe di connessione Compose
                    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113",
                    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113"))
                .multiThreaded(true)
                .build());
```
Successivamente, creerai un nuovo `JestClientFactory`. `factory` ti fornirà un metodo `setHttpClientConfig` per configurare il tuo client. Utilizza `Arrays.asList` all'interno del metodo `Builder` di Jest per creare un array che contiene entrambe le tue stringhe di connessione Compose Elasticsearch. Quindi richiamerai il metodo `build` per creare la connessione. 
```java
        JestClient client = factory.getObject();
        Health health = new Health.Builder().build();
        JestResult result = client.execute(health);

        // stampa l'output del controllo di integrità del cluster Elasticsearch
        System.out.printf("\n\n<------ CLUSTER HEALTH ------>\n%s\n\n", result.getJsonObject());
				// disattiva la connessione
        client.shutdownClient();
    }
}
```

Dopo aver creato la connessione, puoi creare una istanza `JestClient` dall'oggetto factory di connessione `factory.getObject()`. `JestClient` sarà utilizzato per richiamare il metodo `execute` per tutte le query Elasticsearch che hai creato. In questo esempio, `creiamo` una query `Health` per ricercare l'integrità del cluster utilizzando le classi del builder di Elasticsearch. 

Dopo aver creato una query, utilizza `JestResult` per ottenere i documenti e stamparli nel terminale come un oggetto JSON, quindi chiudi il client con `shutdownClient`. 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby e Elasticsearch

### Installazione del client
Per utilizzare Elasticsearch con Ruby, installa [Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api) `gem install elasticsearch`. Dopo l'installazione potrai eseguire `require` per richiedere la libreria nel tuo file Ruby. 

### Creazione di una connessione

Per prima cosa, dovrai utilizzare `require` per richiedere la libreria `elasticsearch` nel tuo file Ruby.

```ruby
require 'elasticsearch'
```

Quindi crea un nuovo client definendo una variabile e assegnandola al constructor `ElasticSearch::Client`. All'interno del constructor, utilizza l'argomento `urls` e inserisci qui le tue stringhe di connessione. Sono disponibili altri argomenti come `host`, `port`, `user` e `password` che ti permettono di analizzare la tua stringa di connessione, ma accetterà la stringa di connessione completa.

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

Per visualizzare l'integrità del tuo cluster Elasticsearch, dovrai semplicemente utilizzare la connessione `client` che hai creato e utilizzare le classi e i metodi forniti da [Elasticsearch API](http://www.rubydoc.info/gems/elasticsearch-api). Per questo esempio stiamo stampando un hash dei risultati dell'integrità del cluster nel terminale.

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python e Elasticsearch

### Installazione del client

Utilizzando Elasticsearch con Python dovrai installare la libreria utilizzando `pip install elasticsearch` e quindi importarla nel tuo progetto Elasticsearch. Dopo l'installazione dovrai `importare` la libreria nel tuo progetto Python.

### Creazione di una connessione

Come per la connessione Ruby, dovrai prima importare Elasticsearch nel tuo file del progetto:

```python
from elasticsearch import Elasticsearch
```

Successivamente, definisci una variabile e assegnala alla classe `Elasticsearch` che contiene un array con le tue stringhe di connessione. La classe di permetterà di definire `hosts` come un array o una sola stringa di connessione o un dizionario che include `host` e `port`. Dispone inoltre di argomenti per configurare le opzioni sniffer e SSL/TLS.

```python
es = Elasticsearch(
  # Stringhe di connessione Compose
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

Successivamente, per stampare l'integrità del cluster dovrai utilizzare la classe `cluster` per richiamare il metodo `health`, che viene stampato nel terminale utilizzando la funzione `print`. Le altre classi e metodi disponibili si trovano nella Elasticsearch API di Python [documentazione}(http://elasticsearch-py.readthedocs.io/en/master/api.html).

```python
print(es.cluster.health())
```

Questo comando stamperà un dizionario dell'integrità del tuo cluster nel terminale.

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## Collegamento a Elasticsearch nella riga di comando

Per collegarti alla tua distribuzione Elasticsearch dalla riga di comando, {{site.data.keyword.composeForElasticsearch}} ti fornisce i comandi curl per le tue stringhe di connessione nel pannello _Connection Strings_ della pagina *Overview* del tuo servizio {{site.data.keyword.composeForElasticsearch}}.

L'output nel tuo terminale sarà simile al seguente:
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```
