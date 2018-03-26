---

Copyright:
  years: 2017,2018
lastupdated: "2017-12-05"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Panoramica sul dashboard

Puoi gestire il tuo servizio {{site.data.keyword.composeForElasticsearch_full}} dal dashboard del servizio.

La pagina _Overview_ ti mostra le informazioni sul tuo database {{site.data.keyword.cloud}} Compose. La panoramica include le informazioni di identificazione essenziali e l'utilizzo della risorsa corrente. Troverai inoltre una sezione per le stringhe di connessione che puoi utilizzare con gli strumenti o utilizzare gli strumenti per il collegamento al tuo database.

## Dettagli della distribuzione

Il pannello _Deployment Details_ mostra i dettagli del tuo servizio.

![Dettagli della distribuzione](./images/elastic_search-deployment-details.png "Una vista del pannello dei dettagli della distribuzione")

### Tipo

Il tipo di database offerto dal servizio e la versione che il servizio utilizza. Se è disponibile una versione più recente del database, viene visualizzata una notifica, insieme a un link alla sezione [Aggiorna versione](/docs/services/ComposeForElasticsearch/dashboard-settings.html#upgrade-version) del tuo dashboard del servizio.

### Nome

Un identificativo interno per il servizio.

### Utilizzo

La dimensione del tuo database e la quantità di memoria fornita dal tuo piano di servizio.


## Stringhe di connessione

Le stringhe di connessione possono essere utilizzate da alcune librerie client e contenere tutte le informazioni necessarie al collegamento di altre librerie. Puoi trovare come utilizzare una stringa di connessione al tuo servizio in [Connessione a un'applicazione esterna](/docs/services/ComposeForElasticsearch/connecting-external.html).

Troverai ogni stringa di connessione per il tuo servizio in una scheda differente nel pannello _Connection Strings_.

### HTTPS

Una stringa di connessione formattata URI che può essere utilizzata da alcune librerie client e che contiene tutte le informazioni necessarie al collegamento di altre librerie. Puoi trovare come utilizzare la stringa di connessione per il collegamento in [Connessione a un'applicazione esterna](/docs/services/ComposeForElasticsearch/connecting-external.html).

### Integrità

Una chiamata di esempio che puoi utilizzare per trovare l'integrità del cluster Elasticsearch.

## API di gestione dell'istanza

Puoi gestire il tuo servizio {{site.data.keyword.composeForElasticsearch}} tramite l'API {{site.data.keyword.cloud_notm}} Compose.

### Endpoint fondazione

L'endpoint fondazione è formato dalla regione in cui risiede il servizio e dall'ID dell'istanza del servizio. Sarà all'inizio di ogni endpoint.

### ID distribuzione

L'ID della distribuzione è necessario per la maggior parte delle chiamate e identifica l'istanza di distribuzione specifica. 

### Riferimenti

Per ulteriore documentazione e i riferimenti sull'utilizzo dell'API {{site.data.keyword.cloud_notm}} Compose, in tutti i servizi {{site.data.keyword.cloud_notm}} Compose, leggi [The {{site.data.keyword.cloud_notm}} Compose API](https://www.compose.com/articles/the-ibm-cloud-compose-api/).
