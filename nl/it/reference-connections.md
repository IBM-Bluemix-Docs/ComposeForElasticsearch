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

# Configurazione della connessione
{: #connection-configuration}

Le connessioni al database {{site.data.keyword.composeForElasticsearch_full}} sono gestite da 2 portali HAProxy. Ciascun portale ha 64 MB di memoria.

I due portali consentono alle applicazioni di mantenere la connettività se uno dei portali diventa irraggiungibile. Il failover al lato client è responsabilità del progettista dell'applicazione. Alcuni driver Elasticsearch gestiscono più stringhe di connessione e il failover normale in modo automatico.

Se non stai lavorando con un driver che gestisce completamente gli errori di failover, attieniti alla seguente procedura per gestire personalmente gli errori.

* Lavorare con un driver che accetta più endpoint nella sua configurazione della connessione.
* Assicurati che la tua applicazione reagisca correttamente agli errori quando legge dal o scrive nel database. Puoi configurare la tua applicazione in modo che reagisca agli errori in modi diversi, inclusi i seguenti:
  + Registra l'errore e ristabilisci la connessione.
  + Ristabilisci la connessione e ritenta l'operazione che ha causato l'errore.
  + Accoda l'operazione non riuscita e pianifica una riconnessione.

## Crittografia in transito

Tutti i portali HAProxy {{site.data.keyword.composeForElasticsearch}} sono abilitati a TLS/SSL e supportano TLS versione 1.2. I certificati per il servizio sono certificati di Let's Encrypt.

## Limiti di connessioni

I due portali HAProxy hanno un limite di connessioni di 2000 connessioni per portale. Se superi il limite di connessioni, il servizio può smettere di rispondere. Puoi gestire le connessioni dalla tua applicazione tramite la funzione di pooling di connessioni del tuo driver.

## Timeout proxy

I portali HAProxy gestiscono il ciclo di vita delle connessioni tra il server e il client. I valori di timeout sono descritti in dettaglio qui come guida di riferimento per gli scrittori di driver e per coloro che hanno un interesse nell'attività di basso livello delle connessioni al database {{site.data.keyword.composeForElasticsearch}}.

Impostazione | Valore | Si applica
----------|-----------|-----------
client | 1m | Quando si prevede che il client riconosca o invii i dati.
connect | 9s | Quando si sta stabilendo una connessione tra il proxy e il server.
server | 2m | Quando si prevede che il server riconosca o invii i dati.
check | 5s | Come un controllo di timeout secondario quando si sta stabilendo una connessione tra il proxy e il server.
http-request | 5s | Il tempo massimo consentito di attesa di una richiesta HTTP completa.
{: caption="Tabella 1. Timeout HAProxy Elasticsearch" caption-side="top"}
