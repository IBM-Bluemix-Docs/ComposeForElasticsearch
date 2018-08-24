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

# Versioni

Versioni distribuibili| Versione preferita
----------|-----------
2.4.6, 5.6.9, 6.2.2 | 6.2.2
{: caption="Tabella 1. Versioni di Elasticsearch" caption-side="top"}

## Versione preferita

La versione preferita è di norma la versione più recente del database Redis disponibile per {{site.data.keyword.composeForElasticsearch}}. La versione preferita è quella predefinita nel selettore di versioni nella pagina del catalogo ed è anche la versione di cui viene eseguito automaticamente il provisioning dall'API se non viene specificata alcuna versione nella chiamata.

### Nuovo protocollo di versione preferito

Quando viene resa disponibile una nuova versione, il suo rilascio viene annunciato e la nuova versione viene resa disponibile per la distribuzione. Dopo la data di rilascio, c'è una finestra di circa 7 giorni in cui la versione più recente è disponibile ma non è la versione preferita. Questa finestra consente ai nostri utenti di distribuire e verificare la nuova versione continuando però a disporre della versione corrente. Inoltre, consente agli ingegneri di Compose di individuare e risolvere gli eventuali problemi che si presentano nella nuova versione. Alla fine della finestra di 7 giorni, la nuova versione viene impostata come versione preferita oppure viene annunciata una nuova data per questa modifica.

Puoi trovare l'elenco di versioni disponibili per il provisioning nella {{site.data.keyword.composeForMongoDB}} [pagina del catalogo](https://console.{DomainName}/catalog/services/compose-for-mongodb).

Per ottenere un elenco corrente di versioni disponibili per il tuo servizio {{site.data.keyword.composeForElasticsearch}}, puoi utilizzare l'endpoint [GET /2016-07/deployments/:id/versions](https://apidocs.compose.com/v1.0/reference#2016-07-get-deployments-versions).

