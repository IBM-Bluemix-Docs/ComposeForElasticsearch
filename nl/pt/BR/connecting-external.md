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

# Conectando um aplicativo externo
{: #connecting-external-app}

Todas as novas implementações do {{site.data.keyword.composeForElasticsearch_full}} aceitam somente as conexões seguras TLS/SSL (`https://`) que são auxiliadas com um certificado Let's Encrypt.

É possível se conectar ao Elasticsearch de várias maneiras, dependendo do driver que estiver sendo usado. O {{site.data.keyword.composeForElasticsearch}} usa o formato de URI para exibir mensagens, que é formatado como:

```text
https://[username]:[password]@[host]:[port]/
```

É possível localizar sua sequência de conexões na página *Visão geral* do serviço {{site.data.keyword.composeForElasticsearch}}.

Os exemplos aqui cobrem Node, Go, Java, Ruby e Python. Esses exemplos configuram uma conexão segura com o {{site.data.keyword.composeForElasticsearch}} e, em seguida, chamam a API do Cluster do Elasticsearch para executar uma verificação de funcionamento básica, que informa como o cluster está sendo executado. Para se familiarizar com a API do Elasticsearch, examine a [referência](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/index.html) do Elasticsearch para o Elasticsearch 2.4.

o código integração para este exemplo e os subsequentes pode ser localizado em [github.com/compose-ex/elasticsearchconns](https://github.com/compose-ex/elasticsearchconns).

## Node e Elasticsearch

### Instalando o cliente
{: #installing-client-node}

Crie seu projeto, em seguida, instale o pacote [`elasticsearch`](https://www.npmjs.com/package/elasticsearch) com `npm install elasticsearch --save`. Com isso instalado, é possível gravar o código para conectar à sua implementação.

### Criando a conexão
{: #creating-connection-node}

Primeiro, será necessário `requerer` a biblioteca `elasticsearch` que você instalou na pasta `node_modules` de seu projeto e salvou como uma dependência no arquivo `package.json`.

```javascript
const elasticsearch = require('elasticsearch');
```

O pacote elasticsearch oferece um protótipo de Cliente que será usado para criar uma conexão com o Elasticsearch:

```javascript
const client = new elasticsearch.Client({
  hosts: [
    // Compose connection strings
    "https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/",
    "https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113/"]
});
```

Comece criando uma variável `client` e uma conexão usando o protótipo `Client` da biblioteca `elasticsearch`. Isso toma, entre outros parâmetros, uma chave `host` com um valor de matriz que deve conter suas URLs de sequências de conexões da página Visão geral.

O objeto do cliente implementa a [API ampla](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference-2-4.html). Neste exemplo, no entanto, você usará essa API para consultar o funcionamento do cluster por meio da chamada `cluster.health`.

```javascript
client.cluster.health((err, res) => {
  if (err) throw err;
  console.log(res);
  client.close();
  process.exit(0);
});
```

A chamada retorna um objeto Javascript com detalhes do funcionamento do cluster. O código imprime isso, fecha o cliente e sai.

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

### Instalando o cliente
{: #installing-client-go}

Existem alguns drivers que trabalham com a linguagem Go. Este exemplo usa [Elastic](https://github.com/olivere/elastic) - consulte a documentação e os exemplos no site [Elastic](https://olivere.github.io/elastic/) e [GoDocs](https://godoc.org/gopkg.in/olivere/elastic.v3) do Elastic. No momento em que isto é escrito, o Compose suporta o Elasticsearch 2.4.0, o que significa que você tem que usar a versão 3.0 do pacote Elastic. 

Para obter o pacote Elastic, execute `go get gopkg.in/olivere/elastic.v3` em seu terminal.

No exemplo de código, todo o código está na função `main`.  Primeiro, crie um `client` e insira as sequências de conexões no método `SetURL`.

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

Depois de configurar a conexão e criar o `client`, será possível chamar seu método `ClusterHealth` para configurar uma solicitação para o funcionamento do cluster. Chamando o método `Do` no resultado disso executa essa solicitação. O método retorna uma estrutura `Health` e imprime os resultados em seu terminal. 

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

## Java e Elasticsearch

### Instalando o cliente
{: #installing-client-java}

O cliente no exemplo a seguir é [Jest](https://github.com/searchbox-io/Jest), que fornece a você um cliente HTTP REST fácil para Java. É possível seguir o guia de instalação e visualizar exemplos de código em seu [Repositório Github](https://github.com/searchbox-io/Jest/tree/master/jest).

### Criando uma conexão
{: #creating-connection-java}

No exemplo, todo o código está contido no método `main`. Primeiro, inclua `BasicConfigurator.configure();` da biblioteca Log4j do Apache para mostrar o processo de conexão no console. Se não o incluir, ainda será possível se conectar à implementação, mas você receberá um aviso informando para usar Log4j. 

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
Em seguida, criaremos um novo `JestClientFactory`. O `factory` fornece a você um método `setHttpClientConfig` para configurar o cliente. Use `Arrays.asList` dentro do método `Builder` do Jest para criar uma matriz contendo ambas as sequências de conexões do Compose Elasticsearch. Em seguida, chame o método `build` para criar a conexão. 

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

Após a construção da conexão, será possível criar uma instância `JestClient` do objeto de connection factory `factory.getObject()`. O `JestClient` é usado para chamar o método `execute` em qualquer consulta do Elasticsearch que for construída. Esse exemplo usa `build` para construir uma consulta `Health` para examinar o funcionamento do cluster usando as classes do construtor do Elasticsearch. 

Depois construir uma consulta, use `JestResult` para obter os documentos e imprimi-los no terminal como um objeto JSON e, em seguida, encerre o cliente com `shutdownClient`. 

```shell
<------ CLUSTER HEALTH ------>
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```

## Ruby e Elasticsearch

### Instalando o cliente
{: #installing-client-ruby}

Para usar Elasticsearch com Ruby, instale o [Elasticsearch Ruby gem](https://github.com/elastic/elasticsearch-ruby/tree/master/elasticsearch-api) `gem install elasticsearch`. Com isso instalado, você poderá `requerer` a biblioteca em seu arquivo Ruby. 

### Criando uma conexão
{: #creating-connection-ruby}

Primeiro, você precisará `requerer` a biblioteca `elasticsearch` em seu arquivo Ruby.

```ruby
require 'elasticsearch'
```

Em seguida, crie um novo cliente definindo uma variável e designando-a ao construtor `ElasticSearch::Client`. No construtor, use o argumento `urls` e coloque suas sequências de conexões lá. Outros argumentos como `host`, `port`, `user` e `password` estão disponíveis e permitem que você analise a sua sequência de conexões, mas isso aceitará a sequência de conexões inteira.

```ruby
client = Elasticsearch::Client.new urls: 'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113/, https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10164/'
```

Em seguida, para visualizar o funcionamento de seu cluster do Elasticsearch, você simplesmente usará o `client` de conexão criado e usará as classes e os métodos fornecidos pela [API Elasticsearch](http://www.rubydoc.info/gems/elasticsearch-api). Esse exemplo imprime um hash dos resultados do funcionamento do cluster no terminal.

```ruby
p client.cluster.health
```

```shell
{"cluster_name"=>"latest-elasticsearch", "status"=>"green", "timed_out"=>false, "number_of_nodes"=>3, "number_of_data_nodes"=>3, "active_primary_shards"=>21, "active_shards"=>57, "relocating_shards"=>0, "initializing_shards"=>0, "unassigned_shards"=>0, "delayed_unassigned_shards"=>0, "number_of_pending_tasks"=>0, "number_of_in_flight_fetch"=>0, "task_max_waiting_in_queue_millis"=>0, "active_shards_percent_as_number"=>100.0}
```

## Python e Elasticsearch

### Instalando o cliente
{: #installing-client-python}

O uso do Elasticsearch com o Python requer a instalação da biblioteca usando `pip install elasticsearch` e, em seguida, sua importação para o projeto Elasticsearch. Com isso instalado, você `importará` a biblioteca para seu projeto Python.

### Criando uma conexão
{: #creating-connection-python}

Como a conexão Ruby, você precisará primeiro importar Elasticsearch para seu arquivo de projeto:

```python
from elasticsearch import Elasticsearch
```

Em seguida, defina uma variável e designe-a à classe `Elasticsearch` que contém uma matriz com suas sequências de conexões. A classe permitirá que você defina `hosts` como uma matriz ou como uma única sequência de conexões ou como um dicionário incluindo um `host` e `port`. Ela também tem argumentos para configurar as opções de procura e opções SSL/TLS.

```python
es = Elasticsearch(
  # Compose connection strings
    [
    'https://username:password@portal113-2.latest-elasticsearch.compose-3.composedb.com:10113',
    'https://username:password@portal164-1.latest-elasticsearch.compose-3.composedb.com:10113'
    ]
)
```

Em seguida, para imprimir o funcionamento do cluster, tudo o que você precisa usar é a classe `cluster` para chamar o método `health`, que é impresso no terminal usando a função `print`. Outras classes e métodos disponíveis são localizados na [documentação da API do Elasticsearch](http://elasticsearch-py.readthedocs.io/en/master/api.html) do Python.

```python
print(es.cluster.health())
```

Isso resultará na impressão de um dicionário do funcionamento de seu cluster para o terminal.

```shell
{u'status': u'green', u'number_of_nodes': 3, u'unassigned_shards': 0, u'number_of_pending_tasks': 0, u'number_of_in_flight_fetch': 0, u'timed_out': False, u'active_primary_shards': 21, u'task_max_waiting_in_queue_millis': 0, u'cluster_name': u'latest-elasticsearch', u'relocating_shards': 0, u'active_shards_percent_as_number': 100.0, u'active_shards': 57, u'initializing_shards': 0, u'number_of_data_nodes': 3, u'delayed_unassigned_shards': 0}
```

## Conectando-se ao Elasticsearch na linha de comandos

Para se conectar à sua implementação do Elasticsearch por meio da linha de comandos, o {{site.data.keyword.composeForElasticsearch}} fornece os comandos curl para suas sequências de conexões no painel _Sequências de conexões_ da página *Visão geral* de seu serviço {{site.data.keyword.composeForElasticsearch}}.

A saída em seu terminal será semelhante a esta:
```shell
{"cluster_name":"latest-elasticsearch","status":"green","timed_out":false,"number_of_nodes":3,"number_of_data_nodes":3,"active_primary_shards":21,"active_shards":57,"relocating_shards":0,"initializing_shards":0,"unassigned_shards":0,"delayed_unassigned_shards":0,"number_of_pending_tasks":0,"number_of_in_flight_fetch":0,"task_max_waiting_in_queue_millis":0,"active_shards_percent_as_number":100.0}
```
