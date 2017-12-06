---

copyright:
  years: 2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestione dei backup
{: #backups}

Puoi creare e scaricare i backup dalla pagina *Backups* del tuo dashboard del servizio. Sono disponibili sia i backup manuali che pianificati.

## Visualizzazione dei backup esistenti

I backup giornalieri del tuo database vengono automaticamente pianificati. Per visualizzare i tuoi backup esistenti:

1. Passa al tuo dashboard del servizio.
2. Fai clic su **Backups** nelle schede per aprire la pagina _Backups_. Viene visualizzato un elenco di backup disponibili:

  ![Backup disponibili](./images/elastic_search-backups-show.png "Un elenco di backup disponibili.")

Fai clic sulla riga corrispondente per espandere le opzioni per ogni backup disponibile.

![Opzioni backup](./images/elastic_search-backups-options.png "Opzioni per il backup.") 

## Creazione di un backup manuale 

Come per i backup pianificati puoi creare un backup manualmente. Per creare un backup manuale, segui le istruzioni per visualizzare i backup esistenti, quindi fai clic su **Back up now** sopra l'elenco dei backup disponibili. Viene visualizzato un messaggio per farti sapere che è stato avviato un backup ed è stato aggiunto un backup 'in sospeso' all'elenco dei backup disponibili.

## Contenuti del backup

I backup {{site.data.keyword.composeForElasticsearch}} sono presi dal programma di utilità dell'istantanea nell'API Elasticsearch. Il processo viene eseguito nel cluster completo in modalità non bloccante in modo che tutte le operazioni di ricerca e indicizzazione possono continuare normalmente mentre è in esecuzione. Viene eseguito un backup temporizzato nel momento in cui viene creata l'istantanea.

## Ripristino di un backup
Per ripristinare un backup in una nuova istanza, segui le istruzioni per visualizzare i backup esistenti, quindi fai clic sulla riga corrispondente per espandere le opzioni del backup che desideri scaricare. Fai clic sul pulsante **Restore**. Viene visualizzato un messaggio per farti sapere che è stato avviato un ripristino. La nuova istanza del servizio sarà automaticamente denominata "elasticsearch-restore-[timestamp]" e visualizzata nel tuo dashboard dopo l'avvio del provisioning.
