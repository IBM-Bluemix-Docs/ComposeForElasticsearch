---

copyright:
  years: 2016,2018
lastupdated: "2018-05-04"
---

# Preisstruktur
{: #pricing}

## Basiskonfiguration

Ein {{site.data.keyword.composeForElasticsearch_full}}-Service wird mit drei identischen Datenknoten konfiguriert. Jeder Knoten hat 2 GB Speicher und 204 MB Hauptspeicher. Dies entspricht zwei Ressourceneinheiten. Zum Service _gehören_ Replikation und Hochverfügbarkeit. In jedem Einzelpreis sind jeweils die Ressourcenkosten für alle drei Knoten _enthalten_.

Zur Basiskonfiguration gehören zudem die beiden HAProxy-Kapseln mit jeweils 64 MB zur Unterstützung von Authentifizierung, HTTPS und IP-Whitelisting. 

Die Basisservicekonfiguration hat einen Festpreis. Den Grundpreis in Ihrer Landeswährung finden Sie über die entsprechenden Katalogkacheln auf {{site.data.keyword.cloud_notm}}. Der Grundpreis in US-Dollar beträgt zum Beispiel $36/Monat.

## Ressourcen erhöhen

Wenn Sie zusätzlichen Speicher oder Hauptspeicher für Ihren Service benötigen, können Sie die Ressourcen erhöhen. Ressourcen werden im Verhältnis von 10:1 von Plattenspeicher zu Speichereinheit erhöht. Durch Erhöhen der Plattengröße, die der Bereitstellung zugeordnet ist, erhöht sich auch der zugeordnete Arbeitsspeicher. Eine {{site.data.keyword.composeForElasticsearch}}-Einheit besteht aus 1 GB Speicher und 102 MB Hauptspeicher. In jeder Einheit und dem Einzelpreis pro Einheit sind die Kosten für die Erweiterung der Ressourcen auf allen drei Knoten _enthalten_.

## Berechnung der Kosten für Ihre Bereitstellung
{: #tiered-pricing}

Für jede zusätzliche Einheit (1 GB Speicher/102 MB Hauptspeicher) gilt ein Einzelpreis, der auf der {{site.data.keyword.cloud_notm}}-Katalogkachel für den Service in Ihrer Landeswährung aufgelistet ist. In US-Dollar beträgt der Preis für jede zusätzliche Einheit $18. Mit zunehmender _Gesamtgröße_ Ihrer {{site.data.keyword.composeForElasticsearch}}-Services verringert sich der Preis pro Einheit, wie aus der folgenden Tabelle zur gestaffelten Preisstruktur hervorgeht.

Anzahl der Einheiten|Gestaffelter Rabatt
----------|-----------
2–9 Einheiten|Basiseinzelpreis - 18,00 USD/Einheit
10-24 Einheiten|10% Ermäßigung - 16,20 USD/Einheit
25-49 Einheiten|20% Ermäßigung - 14,40 USD/Einheit
50-99 Einheiten|30% Ermäßigung - 12,60 USD/Einheit
100-499 Einheiten|40% Ermäßigung - 10,80 USD/Einheit
500-999 Einheiten|50% Ermäßigung - 9,00 USD/Einheit
1.000-4.999 Einheiten|60% Ermäßigung - 7,20 USD/Einheit
5.000 Einheiten und mehr|70% Ermäßigung - 5,40 USD/Einheit
{: caption="Tabelle 1. Gestaffelte Preisstruktur von {{site.data.keyword.composeForElasticsearch}}" caption-side="top"}

