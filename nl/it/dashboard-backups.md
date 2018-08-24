---

copyright:
  years: 2016,2018
lastupdated: "2018-04-19"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestione dei backup
{: #backups}

Puoi creare e ripristinare i backup dalla scheda _Backups_ della pagina _Manage_ del tuo dashboard del servizio. Sono disponibili backup giornalieri, settimanali, mensili e su richiesta. Vengono conservati in base alla seguente pianificazione:

Tipo di backup|Pianificazione conservazione
----------|-----------
Giornaliero|I backup giornalieri sono conservati per 7 giorni
Settimanale|I backup settimanali sono conservati per 4 settimane
Mensile|I backup mensili sono conservati per 3 mesi
Su richiesta|Il backup su richiesta viene conservato. Il backup conservato è sempre il più recente backup su richiesta.
{: caption="Tabella 1. Pianificazione conservazione backup" caption-side="top"}

## Visualizzazione dei backup esistenti

I backup giornalieri del tuo database vengono automaticamente pianificati. Puoi visualizzare i tuoi backup esistenti dal tuo dashboard del servizio.

1. Passa al tuo dashboard del servizio.
2. Fai clic su **Backups** nelle schede per aprire la pagina _Backups_. Viene visualizzato un elenco di backup disponibili:

  ![Backup disponibili](./images/elastic_search-backups-show.png "Un elenco di backup disponibili.")

Fai clic sulla riga corrispondente per espandere le opzioni per ogni backup disponibile.
  ![Opzioni di backup](./images/elastic_search-backups-options.png "Opzioni di backup.") 

### Utilizzo dell'API per visualizzare i backup esistenti

Un elenco dei backup è disponibile nell'endpoint `GET /2016-07/deployments/:id/backups`. L'endpoint fondazione con l'ID istanza di servizio e l'ID distribuzione sono visualizzati nella _Panoramica_ del servizio. Ad esempio: 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```  

## Creazione di un backup manuale

Per creare un backup manuale, segui le istruzioni per visualizzare i backup esistenti, quindi fai clic su **Back up now** sopra l'elenco dei backup disponibili. Viene visualizzato un messaggio per farti sapere che è stato avviato un backup ed è stato aggiunto un backup 'in sospeso' all'elenco dei backup disponibili.

### Utilizzo dell'API per creare un backup

Invia una richiesta POST all'endpoint di backup per avviare un backup manuale: `POST /2016-07/deployments/:id/backups`. Restituisce immediatamente l'ID ricetta e le informazioni sul backup mentre è in esecuzione. Prima di utilizzare il backup, devi controllare l'endpoint dei backup per verificare che il backup sia terminato e trovare il valore `backup_id` prima di utilizzarlo.

```
GET /2016-07/deployments/:id/backups/
```

## Ripristino di un backup

1. Attieniti alla procedura per visualizzare i backup esistenti.
2. Fai clic sulla riga corrispondente per espandere le opzioni per il backup che vuoi ripristinare.
3. Fai clic sul pulsante **Restore**. Viene visualizzato un messaggio per farti sapere che è stato avviato un ripristino. La nuova istanza del servizio viene visualizzata nel dashboard al momento dell'avvio del provisioning e ha il nome generato `elasticsearch-restore-[timestamp]`.

Quando esegui il ripristino da un backup, i tuoi dati saranno ripristinati alla versione secondaria più recente disponibile per {{site.data.keyword.composeForElasticsearch}}. Puoi sovrascrivere questa impostazione eseguendo il ripristino tramite la CLI {{site.data.keyword.cloud_notm}} e inviando la versione di cui vuoi eseguire il ripristino.

**Nota:** puoi eseguire il ripristino solo a una versione disponibile per il provisioning.

### Ripristino tramite la CLI 

Utilizza la seguente procedura per ripristinare un backup da un servizio Elasticsearch in esecuzione a un nuovo servizio Elasticsearch utilizzando la CLI {{site.data.keyword.cloud_notm}}. 
1. Se ne hai bisogno, [scarica e installa la CLI](https://console.{DomainName}/docs/cli/index.html#overview). 
2. Trova il backup da cui vuoi eseguire il ripristino nella pagina _Backups_ del tuo servizio e copia l'ID backup.  
  **In alternativa**  
  Usa `GET /2016-07/deployments/:id/backups` per trovare un backup e il suo ID tramite la API Compose. L'endpoint fondazione e l'ID istanza di servizio sono entrambi visualizzati nella _Panoramica_ del servizio. Ad esempio: 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
  La risposta include un elenco di tutti i backup disponibili per tale istanza del servizio. Scegli il backup da cui vuoi eseguire il ripristino e copiane l'ID.

3. Accedi con l'account e le credenziali appropriati. `ibmcloud login` (oppure `ibmcloud login -help` per visualizzare tutte le opzioni di login).

4. Passa alla tua organizzazione e al tuo spazio: `ibmcloud target -o "$YOUR_ORG" -s "YOUR_SPACE"`

5. Usa il comando `service create` per eseguire il provisioning di un nuovo servizio e fornire il servizio di origine e lo specifico backup che stai ripristinando in un oggetto JSON. Ad esempio:
``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
  Il campo _SERVICE_ deve essere `compose-for-elasticsearch` e il campo _PLAN_ deve essere Standard o Enterprise, a seconda del tuo ambiente. _SERVICE\_INSTANCE\_NAME_ è dove inserisci il nome per il tuo nuovo servizio. _source\_service\_instance\_id_ è l'ID dell'istanza di servizio dell'origine del backup; può essere ottenuto eseguendo `ibmcloud cf service DISPLAY_NAME --guid`, dove _DISPLAY\_NAME_ è il nome del servizio da cui proviene il backup. 

  Esiste un parametro JSON facoltativo "db_version" se devi specificare a quale versione di Elasticsearch eseguire il ripristino. Questo parametro viene utilizzato anche per [eseguire l'upgrade a una versione principale di Elasticsearch](./upgrading.html).
  
  Gli utenti Enterprise devono anche specificare nell'oggetto JSON il cluster nel quale eseguire la distribuzione con il parametro `"cluster_id": "$CLUSTER_ID"`.

