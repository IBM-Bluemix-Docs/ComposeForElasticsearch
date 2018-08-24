---

copyright:
  years: 2016,2018
lastupdated: "2018-07-25"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Upgrade di Elasticsearch
{: #upgrading}

Per poter mantenere il tuo servizio sicuro e aggiornato, saranno rilasciate nuove versioni del motore Elasticsearch che stanno alla base del tuo {{site.data.keyword.composeForElasticsearch_full}}. Si consiglia di eseguire l'upgrade quando le nuove versioni diventano disponibili. Quando è disponibile una nuova versione di {{site.data.keyword.composeForElasticsearch}}, viene visualizzata una notifica nella pagina _Overview_.

## Versioni secondarie
In quasi tutti i casi, gli upgrade delle versioni secondarie saranno disponibili sul posto. Potrai eseguire l'upgrade di Elasticsearch senza migrare alcun dato e con un tempo di interruzione minimo. Puoi eseguire l'upgrade dalla scheda _Settings_ del tuo servizio selezionando la versione di cui vuoi eseguire l'upgrade dal menu a discesa.

## Versioni principali
Al momento esistono tre versioni principali di Elasticsearch disponibili per {{site.data.keyword.composeForElasticsearch}}: 2.4.6, 5.x e 6.x. Al momento è possibile:
- migrare da Elasticsearch 5.x a 6.x
- migrare da Elasticsearch 2.x a 5.x.

Il processo di migrazione tra tutte le versioni principali utilizza un backup recente o su richiesta per ripristinare i dati in una nuova distribuzione. Questo processo fornisce diversi vantaggi:

- Il database originale rimane in esecuzione e il lavoro di produzione può non essere interrotto.
- Puoi verificare il nuovo database all'esterno della produzione e agire su ogni incompatibilità dell'applicazione.
- Il processo completo può essere rieseguito in qualsiasi punto.
- Un ripristino da zero riduce la probabilità che vengono riportate risorse non necessarie della versione precedente del database nel nuovo database.

### Considerazioni sulla migrazione

La maggior parte delle versioni principali includono alcune modifiche che possono influenzare il tuo utilizzo di Elasticsearch. Questo è particolarmente vero per la migrazione a Elasticsearch 6.x. Tieni presenti queste cose prima di eseguire l'upgrade, mentre verifichi la nuova versione e prima di eseguire il deprovisioning della tua versione precedente.

#### Indici pre-5.x
Elasticsearch normalmente supporta la lettura degli indici creati nella precedente versione principale. Questo significa che tutti gli indici creati Elasticsearch 5.x sono leggibili da Elasticsearch 6.x.

Se hai degli indici creati in Elasticsearch 2.x, **non** sono leggibili in Elasticsearch 6.x. Questi indici dovranno essere reindicizzati in Elasticsearch 5.x prima della migrazione a 6.x. Se i tuoi dati contengono degli indici creati in Elasticsearch 2.x e tenti la migrazione a Elasticsearch 6.x, i nodi non si avvieranno e la migrazione avrà esito negativo.

[La reindicizzazione può essere eseguita sul posto](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html) in Elasticsearch 5.x prima della migrazione a 6.x tramite [Reindex API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html).
{: .tip}

#### Associazioni dell'indice

La capacità di creare più tipi di associazione per indice è stata rimossa in Elasticsearch 6.x. Tutte le associazioni dell'indice multiple create in Elasticsearch 5.x continueranno a funzionare ma Elasticsearch 6.x supporta la creazione di un solo tipo di associazione per indice.

I tipi di associazione saranno rimossi in Elasticsearch 7.x e successive.

#### Altre modifiche

La documentazione completa delle modifiche può essere trovata nella [documentazione Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/breaking-changes-6.0.html).

### Migrazione a una nuova versione

Per eseguire la migrazione a una nuova versione di Elasticsearch avrai bisogno della [CLI {{site.data.keyword.cloud_notm}}](https://console.{DomainName}/docs/cli/index.html#overview). Il processo è molto simile al [ripristino del backup tramite la CLI](./dashboard-backups.html#restoring-via-cli), con l'eccezione che utilizzerai il campo "db_version" nel JSON per specificare la versione di cui stai eseguendo l'upgrade. Il comando completo è:

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

Ad esempio, il ripristino di un servizio {{site.data.keyword.composeForElasticsearch}} che esegue 5.x a un nuovo servizio che esegue Elasticsearch 6.2.2 è simile a:

```
ibmcloud service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
```
