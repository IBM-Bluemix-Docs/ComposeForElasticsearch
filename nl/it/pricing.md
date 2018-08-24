---

copyright:
  years: 2016,2018
lastupdated: "2018-05-04"
---

# Prezzi
{: #pricing}

## Configurazione di base

Un servizio {{site.data.keyword.composeForElasticsearch_full}} è configurato con tre nodi di dati identici. Ciascuno ha 2 GB di archiviazione e 204 MB di memoria, che equivale a due unità di risorse. Il servizio _include_ la replica e l'elevata disponibilità, per cui ogni unità e il prezzo per unità _include_ il costo delle risorse in tutti e tre i nodi.

La configurazione di base include anche le due capsule HAProxy con 64 MB ciascuna per supportare l'autenticazione, HTTPS e l'inserimento in whitelist di IP. 

La configurazione del servizio di base a un prezzo fissato. Controlla i tile del catalogo {{site.data.keyword.cloud_notm}} per i prezzi di base nella tua valuta locale. Ad esempio, il prezzo di base in dollari US è $36/mese.

## Aumento delle risorse

Se hai bisogno di ulteriore archiviazione o memoria per il tuo servizio, puoi aumentare le risorse. Le risorse vengono aumentate in un rapporto 10:1 tra archiviazione su disco e unità di memoria e l'aumento della dimensione del disco assegnata alla distribuzione aumenta anche la RAM assegnata. Un'unità {{site.data.keyword.composeForElasticsearch}} consiste in 1 GB di archiviazione e 102 MB di memoria. Ciascuna unità e il prezzo per unità _include_ il costo per aumentare le risorse su tutti e tre i nodi.

## Calcolo del costo della tua distribuzione
{: #tiered-pricing}

Ogni unità aggiuntiva (1 GB di archiviazione/102 MB di memoria) ha un prezzo per unità, che viene elencato nella tua valuta corrente nel tile del catalogo {{site.data.keyword.cloud_notm}} per il servizio. In dollari US ogni unità aggiuntiva costa $18. Quando la dimensione _totale_ di tutti i tuoi servizi {{site.data.keyword.composeForElasticsearch}} aumenta, il prezzo per unità diminuisce, come mostrato nella tabella dei prezzi a livelli.

Numero di unità|Sconto a livelli
----------|-----------
2 - 9 unità|prezzo per unità di base - $18,00 USD/Unità
10 - 24 unità|10% di riduzione - $16,20 USD/Unità
25 - 49 unità|20% di riduzione - $14,40 USD/Unità
50 - 99 unità|30% di riduzione - $12,60 USD/Unità
100 - 499 unità|40% di riduzione - $10,80 USD/Unità
500 - 999 unità|50% di riduzione - $9,00 USD/Unità
1.000 - 4.999 unità|60% di riduzione - $7,20 USD/Unità
5.000+ unità|70% di riduzione - $5,40 USD/Unità
{: caption="Tabella 1. Prezzi a livelli di {{site.data.keyword.composeForElasticsearch}}" caption-side="top"}

