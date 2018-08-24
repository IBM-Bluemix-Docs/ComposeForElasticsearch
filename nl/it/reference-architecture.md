---
copyright:
  years: 2016,2018
lastupdated: "2018-06-11"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Architettura 
{: #architecture}


## Replica
Un servizio {{site.data.keyword.composeForElasticsearch_full}} è costituito da tre nodi di dati in un cluster.  Tu selezioni una regione in cui il servizio viene distribuito e i tre nodi di dati vengono distribuiti nelle zone di disponibilità della regione. Il numero di repliche è impostato per impostazione predefinita su 1, fornendoti due copie dei tuoi dati. Elasticsearch bilancia automaticamente le tue repliche nell'ambito del cluster. Se un nodo di dati diventa non raggiungibile, il cluster può continuare a funzionare normalmente.
   
## Crittografia del disco

Tutti i servizi {{site.data.keyword.composeForElasticsearch}} hanno la crittografia dei dati inattivi. I tuoi dati si trovano su server che hanno la crittografia a livello di volume abilitata. 

Poiché {{site.data.keyword.composeForElastcisearch}} è un servizio gestito, gli operatori di {{site.data.keyword.cloud}} Compose possono visualizzare i dati. Oltre alla crittografia del disco, ti consigliamo di crittografare le informazioni personali prima di archiviarle nel database o di usare delle estensioni o delle funzioni per abilitare la crittografia sul database stesso. Anche se questi metodi possono influire sull'usabilità o sulle prestazioni, è buona norma assicurarsi che le informazioni personali siano protette con la crittografia.

## Ridimensionamento automatico

Le risorse vengono ridimensionate automaticamente in base all'utilizzo di archiviazione disco totale dei database Elasticsearch. L'utilizzo della memoria è basato su una proporzione di archiviazione di cui viene eseguito il provisioning a un rapporto di 1:10. Queste risorse sono raccolte in bundle come unità.

Il ridimensionamento automatico è progettato per rispondere alle tendenze a medio e a lungo termine del tuo database. Il tuo servizio viene controllato ogni ora e, se sta esaurendo lo spazio su disco, delle ulteriori unità vengono assegnate alla distribuzione. La riassegnazione si verifica una volta ogni 24 ore. Se intendi eseguire delle operazioni che richiedono una notevole quantità di RAM che potrebbero causare un picco nel normale utilizzo di RAM, oppure delle operazioni di dati che causeranno un riempimento eccessivo della tua archiviazione assegnata, ti consigliamo di procedere prima a ridimensionare in modo manuale le risorse del tuo servizio per evitare di raggiungere qualche limite delle risorse, con conseguenti ripercussioni sulle operazioni del cluster.

Il ridimensionamento automatico non riduce le distribuzioni in cui l'utilizzo di disco/memoria si è ridotto. Le risorse di cui è stato eseguito il provisioning ai tuoi database rimarranno per le tue future esigenze oppure finché non riduci manualmente la tua distribuzione.
{: .tip}
