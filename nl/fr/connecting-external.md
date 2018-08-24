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

# Connexion d'une application externe
{: #connecting-external-app}

Tous les nouveaux déploiements {{site.data.keyword.composeForElasticsearch_full}} acceptent uniquement les connexions sécurisées TLS/SSL (`https://`) doublées d'un certificat Let's Encrypt.

Vous pouvez vous connecter à Elasticsearch de nombreuses manières, selon le pilote que vous utilisez. Pour afficher des messages, {{site.data.keyword.composeForElasticsearch}} utilise le format URI qui se présente comme suit :

```text
https://[username]:[password]@[host]:[port]/
```

Vous trouverez la chaîne de connexion sur la page *Vue d'ensemble* de votre service {{site.data.keyword.composeForElasticsearch}}.

Les exemples donnés ici couvrent Node, Go, Java, Ruby et Python. Ces exemples configurent une connexion sécurisée à {{site.data.keyword.composeForElasticsearch}}, puis appellent l'API cluster Elasticsearch pour effectuer un diagnostic d'intégrité basique, qui vous indique l'état de votre cluster. Pour vous familiariser avec l'API d'Elasticsearch, nous vous conseillons de consulter le [guide de référence](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html) d'Elasticsearch 2.4.

L'intégralité du code pour cet exemple et les suivants se trouve à l'adresse [github.com/compose-ex/elasticsearchconns.](https://github.com/compose-ex/elasticsearchconns).

## Node et Elasticsearch

### Installation du client
{: #installing-client-node}

Créez votre projet, puis installez le package [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) avec `npm install elasticsearch --save`. Une fois ce package installé, vous pouvez écrire le code pour la connexion à votre déploiement.

### Création de la connexion
{: #creating-connection-node}

Tout d'abord, vous devez demander (commande `require`) la bibliothèque `elasticsearch` que vous avez installée dans le dossier `node_modules` de votre projet et sauvegardée en tant que dépendance dans le fichier `package.json`.

```javascript
const elasticsearch = require('elasticsearch');
```

Le package elasticsearch offre un prototype Client que vous utilisez pour créer une connexion à Elasticsearch:

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Compose connection strings
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

Commencez par créer une variable `client` et une connexion à l'aide du prototype `Client` de la bibliothèque `elasticsearch`. Cela nécessite, entre autres paramètres, une clé `host` avec une valeur de tableau qui doit contenir vos adresses URL de chaîne de connexion issues de la page Vue d'ensemble.

L'objet client implémente l'[API élargie](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html). Dans cet exemple, vous utilisez simplement cette API pour demander l'état de santé du cluster au moyen de l'appel `cluster.health`.

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

L'appel renvoie un objet Javascript avec des détails sur l'état de santé du cluster. Ensuite, le code imprime ces informations, ferme le client et quitte.

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

## Go et Elasticsearch

### Installation du client
{: #installing-client-go}

Quelques pilotes fonctionnent avec le langage Go. Cet exemple utilise [Elastic](https://github.com/olivere/elastic) (voir la documentation et les exemples sur le site [Elastic](https://olivere.github.io/elastic/) et dans [GoDocs](https://godoc.org/gopkg.in/olivere/elastic.v3) for Elastic. Au moment de la rédaction du présent document, Compose prend en charge Elasticsearch 2.4.0, ce qui signifie que vous devez utiliser la version 3.0 du package Elastic. 

Pour obtenir le package Elastic, exécutez `go get gopkg.in/olivere/elastic.v3` sur votre terminal.

Dans l'exemple, l'intégralité du code se trouve dans la fonction `main`.  Créez d'abord un `client` et insérer les chaînes de connexion dans la méthode `SetURL`.

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

Après avoir défini la connexion et créé le `client`, vous pouvez appeler sa méthode `ClusterHealth` afin de définir une requête de l'état de santé du cluster. L'appel de la méthode `Do` sur le résultat obtenu exécute cette requête. La méthode renvoie un élément struct `Health` et affiche les résultats sur votre terminal. 

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

## Java et Elasticsearch

### Installation du client
{: #installing-client-java}

Le client utilisé dans l'exemple suivant est [Jest](https://github.com/searchbox-io/Jest), qui vous offre un client HTTP REST pour Java facile. Vous pouvez suivre le guide d'installation des clients et afficher les exemples de code dans leur [référentiel Github](https://github.com/searchbox-io/Jest/tree/master/jest).

### Création d'une connexion
{: #creating-connection-java}

Dans l'exemple, l'intégralité du code est contenu dans la méthode `main`. Pour commencer, ajoutez ` BasicConfigurator.configure();` à partir de la bibliothèque Log4j d'Apache pour afficher le processus de connexion sur la console. Si vous n'ajoutez pas cet élément, vous pouvez tout de même vous connecter à votre déploiement, mais vous recevrez un avertissement vous demandant d'utiliser Log4j. 

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
Ensuite, vous créerez une nouvelle fabrique `JestClientFactory`. La `fabrique` vous fournit une méthode `setHttpClientConfig` pour configurer votre client. Utilisez `Arrays.asList` dans la méthode `Builder` de Jest pour créer un tableau contenant vos deux chaînes de connexion Compose Elasticsearch. Ensuite, appelez la méthode `build` pour créer la connexion. 

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

Une fois la connexion générée, vous pouvez créer une instance `JestClient` à partir de l'objet fabrique de connexions `factory.getObject()`. L'instance `JestClient` est utilisée pour appeler la méthode `execute` sur toute les requêtes Elasticsearch que vous générez. Cet exemple utilise `build` pour générer une requête `Health` afin de connaître l'état de santé du cluster en utilisant les classe du générateur d'Elasticsearch. 

Après avoir généré une requête, utilisez `JestResult` pour obtenir les documents et les afficher sur le terminal en tant qu'objet JSON, puis fermez le client avec `shutdownClient`. 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby et Elasticsearch

### Installation du client
{: #installing-client-ruby}

Pour utiliser Elasticsearch avec Ruby, installez [Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api) `gem install elasticsearch`. Une fois cet élément installé, vous pourrez demander (`require`) la bibliothèque dans votre fichier Ruby. 

### Création d'une connexion
{: #creating-connection-ruby}

D'abord, vous devez demander la bibliothèque `elasticsearch` avec la commande `require` dans votre fichier Ruby.

```ruby
require 'elasticsearch'
```

Créez ensuite un nouveau client en définissant une variable que vous affectez au constructeur `ElasticSearch::Client`. Dans le constructeur, utilisez l'argument `urls` et placez-y vos chaînes de connexion. D'autres arguments, tels que `host`, `port`, `user` et `password`, vous permettent d'analyser votre chaîne de connexion, mais l'intégralité de la chaîne de connexion sera acceptée.

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

Ensuite, pour visualiser l'état de santé de votre cluster Elasticsearch, il suffit d'utiliser le `client` de connexion que vous avez créé et d'utiliser les classes et méthodes fournies par l'[API Elasticsearch](http://www.rubydoc.info/gems/elasticsearch-api). Cet exemple affiche un hachage des résultats de l'état de santé du cluster sur le terminal.

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python et Elasticsearch

### Installation du client
{: #installing-client-python}

L'utilisation d'Elasticsearch avec Python nécessite d'installer la bibliothèque à l'aide de `pip install elasticsearch` et de l'importer dans le projet Elasticsearch. Une fois cette installation effectuée vous importerez (`import`) la bibliothèque dans votre projet Python.

### Création d'une connexion
{: #creating-connection-python}

Comme pour la connexion Ruby, vous devez d'abord importer Elasticsearch dans votre fichier de projet :

```python
from elasticsearch import Elasticsearch
```

Ensuite, définissez une variable et affectez-la à la classe `Elasticsearch` qui contient un tableau de vos chaînes de connexion. La classe vous permettra de définir `hosts` en tant que tableau ou chaîne de connexion unique, ou en tant que dictionnaire incluant `host` et `port`. Elle dispose également d'arguments pour définir des options de détection et des options SSL/TLS.

```python
es = Elasticsearch(
  # Compose connection strings
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

Ensuite, pour afficher l'état de santé du cluster, il vous suffit d'utiliser la classe du `cluster` pour appeler la méthode `health`, affichée sur le terminal à l'aide de la fonction `print`. D'autres classes et méthodes sont disponibles dans la [documentation de l'API Elasticsearch](http://elasticsearch-py.readthedocs.io/en/master/api.html) de Python.

```python
print(es.cluster.health())
```

Ce code se traduit par l'affichage d'un dictionnaire de l'état de santé du cluster sur le terminal.

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## Connexion à Elasticsearch sur la ligne de commande

Pour la connexion à votre déploiement Elasticsearch à partir de la ligne de commande, {{site.data.keyword.composeForElasticsearch}} fournit les commandes curl pour vos chaînes de connexion dans le panneau _Chaînes de connexion_ de la page *Vue d'ensemble* de votre service {{site.data.keyword.composeForElasticsearch}}.

La sortie sur votre terminal est similaire à la suivante :
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```
