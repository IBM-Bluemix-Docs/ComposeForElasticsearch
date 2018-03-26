---

copyright:
  years: 2016,2018
lastupdated: "2017-10-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Informazioni su {{site.data.keyword.composeForElasticsearch}}
{: #about-compose-for-elasticsearch}

{{site.data.keyword.composeForElasticsearch_full}} unisce la potenza di un motore di ricerca di testo completo con la forza dell'indicizzazione di un database del documento JSON. Insieme creano uno strumento potente per l'analisi dei dati su grandi volumi di dati. Con Elasticsearch, la tua ricerca può essere effettuata con esattezza, lasciandoti cercare approfonditamente nei tuoi dataset le strette corrispondenze e le quasi corrispondenze che potrebbero essere mancanti.
{:shortdesc}

**Nota:** tutte le istanze del servizio Compose di cui è stato eseguito il provisioning prima del 14 settembre 2016 che sono ancora attive e che possono ancora essere utilizzate a cui si può fare direttamente accesso da [https://www.compose.com/](https://www.compose.com). È possibile accedere direttamente a tutte le istanze del servizio Compose di cui è stato eseguito il provisioning da questo punto in avanti ed è possibile utilizzarle nel tuo account {{site.data.keyword.cloud}}.

## Creazione di un'istanza del servizio {{site.data.keyword.composeForElasticsearch}} 

Puoi creare un servizio {{site.data.keyword.composeForElasticsearch}} dalla pagina [{{site.data.keyword.composeForElasticsearch}}](https://console.{DomainName}/catalog/services/compose-for-elasticsearch/) nel catalogo {{site.data.keyword.cloud_notm}}.

Scegli un nome del servizio, una regione, un'organizzazione e uno spazio in cui eseguire il provisioning del servizio. Puoi utilizzare il campo **Select a database version** per scegliere tra le versioni del database disponibili.

Quando esegui il provisioning della tua istanza {{site.data.keyword.composeForElasticsearch}} puoi scegliere i piani *Standard* o *Enterprise*. Con il piano *Enterprise*, puoi eseguire il provisioning della tua istanza {{site.data.keyword.composeForElasticsearch}} in un cluster {{site.data.keyword.composeEnterprise}} disponibile. {{site.data.keyword.composeEnterprise}} fornisce la sicurezza e l'isolamento necessari per la conformità aziendale e utilizza la rete dedicata per garantire le prestazioni dei database distribuiti. Per ulteriori dettagli, consulta la [Documentazione aziendale Compose](../ComposeEnterprise/index.html).

## Gestione di {{site.data.keyword.composeForElasticsearch}}

Puoi gestire il tuo servizio dal dashboard del servizio. Qui puoi trovare le informazioni sul tuo database {{site.data.keyword.cloud}} Compose e su come collegarti ad esso. Puoi anche:

- gestire i tuoi backup
- assegnare ulteriori risorse al tuo servizio 
- utilizzare le whitelist per limitare l'accesso ai tuoi database

Per ulteriori informazioni, consulta [Impostazioni](./dashboard-settings.html).

## Connessione a {{site.data.keyword.composeForElasticsearch}}

Puoi collegarti al tuo servizio utilizzando le credenziali create insieme al servizio o con le stringhe di connessione e la riga di comando forniti nella scheda *Overview* nel tuo dashboard del servizio.

## Connessione di un'applicazione {{site.data.keyword.cloud_notm}} a {{site.data.keyword.composeForElasticsearch}}

Per collegare un'applicazione {{site.data.keyword.cloud_notm}} al tuo servizio, utilizza le credenziali create insieme al servizio. Puoi trovare le informazioni su come collegare un'applicazione {{site.data.keyword.cloud_notm}} a un servizio {{site.data.keyword.composeForElasticsearch}} in [Connessione a un'applicazione {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Connessione a {{site.data.keyword.composeForElasticsearch}} dall'esterno di {{site.data.keyword.cloud_notm}}

Se desideri collegarti a {{site.data.keyword.composeForElasticsearch}} dall'esterno di {{site.data.keyword.cloud_notm}}, puoi utilizzare la riga di comando o le stringhe di connessione fornite. Puoi trovare le informazioni su come collegarti in [Connessione a un'applicazione esterna](./connecting-external.html).
