---

copyright:
  years: 2016,2018
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Conectando um aplicativo {{site.data.keyword.cloud_notm}}

Para conectar um aplicativo {{site.data.keyword.cloud}} a seu serviço, use as credenciais de serviço que são criadas quando o serviço é provisionado. O app de amostra demonstra como usar o Node.js para se conectar a um serviço {{site.data.keyword.composeForElasticsearch}} usando as credenciais fornecidas e como criar um banco de dados, além de ler e gravar nele.

## Conectando ao app de amostra 'Hello World'

O app de amostra [compose-elasticsearch-helloworld-nodejs](https://github.com/IBM-Cloud/compose-elasticsearch-helloworld-nodejs) demonstra como usar o Node.js para se conectar a um serviço {{site.data.keyword.composeForElasticsearch}} usando as credenciais fornecidas. O aplicativo cria, lê e grava em um índice Elasticsearch.

Faça download do aplicativo de amostra e siga as instruções no arquivo leia-me. Em seguida, em sua página de detalhes do aplicativo no {{site.data.keyword.cloud}}, clique em **Visualizar APP** para visualizar os conteúdos do índice *exemplos*.

## Credenciais

Campo de nome|Descrição
----------|-----------
`uri`|O URI a ser usado na conexão com o serviço. Inclui o esquema (`https:`), o nome do usuário administrativo e a senha, o nome do host do servidor e o número da porta à qual se conectar.
`uri_direct_1`|Um URI secundário que pode ser usado ao se conectar ao serviço. Formatado como para `uri`.
`uri_health`|Um comando `curl` que solicita o funcionamento do cluster do primeiro haproxy.
`uri_health_1`|Um comando `curl` que solicita o funcionamento do cluster do segundo haproxy.
` ca_certificate_base64 `  ` (opcional) `|Um certificado autoassinado codificado em base64 usado para confirmar se um aplicativo está se conectando ao servidor apropriado. O certificado não está presente em serviços que têm o certificado Let's Encrypt. É necessário decodificar a chave antes de poder usá-la, conforme mostrado no aplicativo de amostra. É necessário decodificar a chave antes de poder usá-la, conforme mostrado no aplicativo de amostra.
`deployment_id`|Um identificador interno para o serviço conforme criado no Compose.
`db_type`|O tipo de banco de dados que é oferecido pelo serviço; nesse caso, `elastic_search`.
`name`|O nome da implementação do banco de dados.
{: caption="Tabela 1. Credenciais do Compose for Elasticsearch" caption-side="top"}

**Nota:** dois portais `haproxy` fornecem acesso ao cluster do Elasticsearch. `uri` e `uri_direct_1` podem ser usados para conexão com o cluster. Em seus aplicativos, alterne entre `uri` e `uri_direct_1` para gerenciar respostas às falhas de conexão.
