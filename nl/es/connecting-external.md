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

# Conexión de una aplicación externa
{: #connecting-external-app}

Todos los nuevos despliegues de {{site.data.keyword.composeForElasticsearch_full}} solo aceptan conexiones seguras TLS/SSL (`https://`) con el respaldo del certificado de Let's Encrypt.

Puede conectarse a Elasticsearch de varias formas, según el controlador que utilice. {{site.data.keyword.composeForElasticsearch}} utiliza el formato de URI para mostrar mensajes, que se formatean del siguiente modo:

```text
https://[username]:[password]@[host]:[port]/
```

Puede encontrar su serie de conexión en la página *Visión general* del servicio {{site.data.keyword.composeForElasticsearch}}.

Los ejemplos de este apartado cubren Node, Go, Java, Ruby y Python. Estos ejemplos configuran una conexión segura con {{site.data.keyword.composeForElasticsearch}} y llaman a la API del clúster Elasticsearch para realizar una comprobación básica del estado, que le muestra el comportamiento del clúster. Para familiarizarse con la API de Elasticsearch, consulte la guía de [consulta](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html) de Elasticsearch para Elasticsearch 2.4.

Encontrará el código completo de este ejemplo y de los siguientes en [github.com/compose-ex/elasticsearchconns.](https://github.com/compose-ex/elasticsearchconns).

## Node y Elasticsearch

### Instalación del cliente
{: #installing-client-node}

Cree su proyecto y luego instale el paquete [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) con el mandato `npm install elasticsearch --save`. Con este paquete instalado, podrá escribir el código para conectar con el despliegue.

### Creación de la conexión
{: #creating-connection-node}

En primer lugar, deberá ejecutar `requiere` sobre la biblioteca `elasticsearch` que ha instalado en la carpeta `node_modules` del proyecto y que ha guardado como dependencia en el archivo `package.json`.

```javascript
const elasticsearch = require('elasticsearch');
```

El paquete de elasticsearch ofrece un prototipo de cliente que se utiliza para crear una conexión con Elasticsearch:

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Series de conexión de Compose
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

Empiece creando una variable `client` y una conexión utilizando el prototipo de `Cliente` de la biblioteca de `elasticsearch`. Entre otros parámetros, adquiere una clave `host` con un valor de matriz que debe contener los URL de las series de conexión de la página Visión general.

El objeto cliente implementa la [API wide](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html). Sin embargo, en este ejemplo se utiliza la API para consultar el estado del clúster mediante la llamada `cluster.health`.

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

La llamada devuelve un objeto Javascript con detalles sobre el estado del clúster. El código mostrará esta información, cerrará el cliente y saldrá.

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

## Go y Elasticsearch

### Instalación del cliente
{: #installing-client-go}

Hay ciertos controladores que funcionan con el lenguaje Go. En este ejemplo se utiliza [Elastic](https://github.com/olivere/elastic); consulte la documentación y los ejemplos del sitio de [Elastic](https://olivere.github.io/elastic/) y de [GoDocs](https://godoc.org/gopkg.in/olivere/elastic.v3) para Elastic. En el momento de la publicación de esta guía, Compose da soporte a Elasticsearch 2.4.0, lo que significa que tiene que utilizar la versión 3.0 del paquete de Elastic. 

Para obtener el paquete de Elastic, ejecute `go get gopkg.in/olivere/elastic.v3` en el terminal.

En este código de ejemplo, todo el código está en la función `main`.  En primer lugar, cree un `cliente` e insertaremos las series de conexión en el método `SetURL`.

```go
package main

import (
			"fmt"
			"log"
  		// v3 for Elasticsearch 2.x
			"gopkg.in/olivere/elastic.v3"
)

func main() {
  		// crear un cliente y añadir las series de conexión de Compose
			client, err := elastic.NewClient(
				elastic.SetURL("https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113", "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113"),
				elastic.SetSniff(false),
			)
			if err != nil {
				log.Fatal(err)
			}
```

Después de configurar la conexión y de crear el `cliente`, puede llamar a su método `ClusterHealth` para configurar una solicitud del estado del clúster. Al invocar el método `Do` en el resultado se ejecutará esta solicitud. El método devuelve una estructura `Health` (estado) y muestra los resultados en su terminal. 

```go
// crear una variable que guarde el resultado
		// de la consulta ejecutada de estado del clúster y muestre el resultado
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

## Java y Elasticsearch

### Instalación del cliente
{: #installing-client-java}

En el siguiente ejemplo, el cliente es [Jest](https://github.com/searchbox-io/Jest), que le proporciona un cliente REST HTTP sencillo para Java. Puede seguir la guía de instalación y ver ejemplos de código en el [repositorio de Github](https://github.com/searchbox-io/Jest/tree/master/jest).

### Creación de una conexión
{: #creating-connection-java}

En el ejemplo, todo el código está contenido en el método `main`. En primer lugar, añada ` BasicConfigurator.configure();` de la biblioteca Log4j de Apache para mostrar el proceso de conexión en la consola. Si no lo añade, se puede conectar igualmente al despliegue, pero recibirá un aviso que le indicará que utilice Log4j. 

```java
public class ElasticsearchConnect {
    public static void main(String[] args) throws IOException {

      	// muestra el proceso de conexión
        BasicConfigurator.configure();

      	// inicio de los métodos de biblioteca Jest
        JestClientFactory factory = new JestClientFactory();
        factory.setHttpClientConfig(new HttpClientConfig
                .Builder(
                  Arrays.asList(
        // Series de conexión de Compose
                    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113",
                    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113"))
                .multiThreaded(true)
                .build());
```
A continuación, creará un nuevo `JestClientFactory`. El valor `factory` le proporciona el método `setHttpClientConfig` para configurar el cliente. Utilice `Arrays.asList` en el método `Builder` de Jest para crear una matriz que contenga las series de conexión de Compose Elasticsearch. A continuación, invoque el método `build` para crear la conexión. 

```java
        JestClient client = factory.getObject();
        Health health = new Health.Builder().build();
        JestResult result = client.execute(health);

        // mostrar la salida de la comprobación de estado del clúster Elasticsearch
        System.out.printf("\n\n<------ CLUSTER HEALTH ------>\n%s\n\n", result.getJsonObject());
				// cierre de la conexión
        client.shutdownClient();
    }
}
```

Una vez creada la conexión, puede crear una instancia `JestClient` desde el objeto factory de la conexión `factory.getObject()`. Se utiliza `JestClient` para invocar el método `execute` en cualquier consulta Elasticsearch que cree. En este ejemplo se utiliza `build` para crear una consulta `Health` para consultar el estado del clúster utilizando las clases del generador de Elasticsearch. 

Tras crear la consulta, utilice `JestResult` para obtener los documentos y mostrarlos en el terminal como objeto JSON y luego cerrar el cliente con `shutdownClient`. 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby y Elasticsearch

### Instalación del cliente
{: #installing-client-ruby}

Para utilizar Elasticsearch con Ruby, instale [Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api) `gem install elasticsearch`. Con esto instalado ya podrá ejecutar `require` sobre la biblioteca en el archivo de Ruby. 

### Creación de una conexión
{: #creating-connection-ruby}

Primero deberá ejecutar `requiere` sobre la biblioteca `elasticsearch` en el archivo de Ruby.

```ruby
require 'elasticsearch'
```

Luego cree un nuevo cliente, definiendo una variable y asignándola al constructor `ElasticSearch::Client`. Dentro del constructor, utilice el argumento `urls` y coloque allí las series de conexión. Dispone de otros argumentos, como `host`, `port`, `user` y `password`, que le permite analizar la serie de conexión, pero se aceptará la serie de conexión entera.

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

A continuación, para ver el estado del clúster Elasticsearch, solo tendrá que utilizar la conexión `client` que ha creado y utilizar las clases y los métodos que ofrece la [API de Elasticsearch](http://www.rubydoc.info/gems/elasticsearch-api). Este ejemplo muestra un hash de los resultados del estado del clúster en el terminal.

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python y Elasticsearch

### Instalación del cliente
{: #installing-client-python}

Para utilizar Elasticsearch con Python, debe instalar la biblioteca mediante `pip install elasticsearch` y luego debe importarla en el proyecto de Elasticsearch. Con esto instalado, deberá `importar` la biblioteca en el proyecto de Python.

### Creación de una conexión
{: #creating-connection-python}

Al igual que la conexión Ruby, primero tendrá que importar Elasticsearch en el archivo del proyecto:

```python
from elasticsearch import Elasticsearch
```

A continuación, defina una variable y asígnela a la clase `Elasticsearch` que contiene una matriz con las series de conexión. La clase le permitirá definir `hosts` como una matriz o como una sola serie de conexión, o también como un diccionario que incluya `host` y `port`. También tiene argumentos para definir opciones de rastreo y opciones de SSL/TLS.

```python
es = Elasticsearch(
  # Series de conexión de Compose
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

Para ver el estado del clúster, lo único que debe utilizar es la clase `cluster` para invocar el método `health`, que se muestra en el terminal mediante la función `print`. Encontrará otras clases y métodos disponibles en [Documentación de la API de Elasticsearch](http://elasticsearch-py.readthedocs.io/en/master/api.html).

```python
print(es.cluster.health())
```

Con este mandato se mostrará un diccionario del estado del clúster en el terminal.

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## Conexión a Elasticsearch en la línea de mandatos

Para conectar el despliegue de Elasticsearch desde la línea de mandatos, {{site.data.keyword.composeForElasticsearch}} proporciona los mandatos de curl para las series de conexión en el panel _Series de conexión_ de la página *Visión general* del servicio {{site.data.keyword.composeForElasticsearch}}.

La salida que se mostrará en el terminal será parecida a la siguiente:
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```
