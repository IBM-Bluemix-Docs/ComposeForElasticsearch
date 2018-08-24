---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuração da Conexão
{: #connection-configuration}

As conexões com o banco de dados do {{site.data.keyword.composeForElasticsearch_full}} são gerenciadas por 2 portais HAProxy. Cada portal tem 64 MB de memória.

Os dois portais permitirão que os aplicativos mantenham a conectividade se um deles se tornar inacessível. Failover no lado do cliente é responsabilidade do designer de aplicativo. Alguns drivers Elasticsearch manipulam múltiplas sequências de conexões e failover normal automaticamente.

Se você não estiver trabalhando com um driver que manipule completamente os erros de failover, execute as etapas a seguir para manipular erros você mesmo.

* Trabalhar com um driver que aceite múltiplos terminais em sua configuração de conexão.
* Assegure-se de que seu aplicativo reaja a erros apropriadamente ao ler ou gravar no banco de dados. É possível configurar seu aplicativo para reagir a erros de diferentes maneiras, incluindo o seguinte:
  + Registre o erro e reconecte-se.
  + Reconecte e tente novamente a operação que causou o erro.
  + Enfileire a operação com falha e planeje uma reconexão.

## Criptografia em Trânsito

Todos os portais HAProxy do {{site.data.keyword.composeForElasticsearch}} têm o TLS/SSL ativado e suportam o TLS versão 1.2. Os certificados para o serviço são certificados Let's Encrypt.

## Limites de Conexão

Os dois portais HAProxy têm um limite de conexão de 2.000 conexões por portal. Se você exceder o limite de conexão, seu serviço poderá se tornar não responsivo. É possível gerenciar conexões de seu aplicativo por meio do recurso de definição do conjunto de conexões do driver.

## Tempos limite de proxy

Os portais HAProxy gerenciam o ciclo de vida de conexões entre o servidor e o cliente. Os valores de tempo limite são detalhados aqui como um guia de referência para gravadores de driver e aqueles que têm interesse na atividade de baixo nível de conexões com o banco de dados do {{site.data.keyword.composeForElasticsearch}}.

Configurando | Value | Aplica
----------|-----------|-----------
cliente | 1m | Quando se espera que o cliente reconheça ou envie dados.
conectar | 9s | Quando uma conexão está sendo feita entre o proxy e o servidor.
server | 2m | Quando se espera que o servidor reconheça ou envie dados.
check | 5s | Como uma verificação de tempo limite secundário quando uma conexão está sendo feita entre o proxy e o servidor.
http-request | 5s | O tempo máximo permitido que se deve esperar por uma solicitação de HTTP completa.
{: caption="Tabela 1. Tempos limite de HAProxy de elasticsearch" caption-side="top"}
