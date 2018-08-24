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

# Architektur 
{: #architecture}


## Replikation
Ein {{site.data.keyword.composeForElasticsearch_full}}-Service besteht aus drei Datenknoten in einem Cluster.  Sie wählen eine Region aus, in der der Service bereitgestellt wird, und die drei Datenknoten werden über die Verfügbarkeitszonen der Region verteilt. Der Replikatzähler wird standardmäßig auf den Wert 1 gesetzt, so dass Sie zwei Kopien Ihrer Daten erhalten. Elasticsearch sorgt automatisch für den Ausgleich Ihrer Replikate im gesamten Cluster. Wenn ein Datenknoten nicht mehr erreichbar ist, kann der Betrieb Ihres Clusters trotzdem normal fortgesetzt werden.
   
## Plattenverschlüsselung

Für alle {{site.data.keyword.composeForElasticsearch}}-Services erfolgt eine Verschlüsselung im Ruhezustand. Ihre Daten befinden sich auf Servern, auf denen die Verschlüsselung auf Datenträgerebene aktiviert ist. 

Da es sich bei {{site.data.keyword.composeForElastcisearch}} um einen verwalteten Service handelt, sind Operatoren von Compose auf {{site.data.keyword.cloud}} in der Lage, Daten zu sehen. Zusätzlich zur Plattenverschlüsselung wird empfohlen, persönliche Informationen zu verschlüsseln, bevor sie in der Datenbank gespeichert werden, oder Erweiterungen bzw. Features zu verwenden, um die Verschlüsselung auf der Datenbank selbst zu ermöglichen. Obwohl diese Methoden den Bedienungskomfort oder die Leistung beeinträchtigen können, hat es sich bewährt, den Schutz persönlicher Informationen mit Verschlüsselung sicherzustellen.

## Automatische Skalierung

Die Ressourcen werden basierend auf der Gesamtplattenspeicherbelegung der Elasticsearch-Datenbanken automatisch skaliert. Die Hauptspeicherbelegung basiert auf einem Faktor des bereitgestellten Speichers im Verhältnis von 1:10. Diese Ressourcen werden als Einheiten gebündelt.

Die automatische Skalierung ist so konzipiert, dass sie als Reaktion auf die kurz-bis mittelfristigen Trends Ihrer Datenbank erfolgt. Ihr Service wird in stündlichen Intervallen überprüft. Wenn der Plattenspeicherplatz für den Service knapp zu drohen wird, werden der Bereitstellung weitere Einheiten zugeordnet. Die Neuzuordnung erfolgt einmal in jedem 24-Stunden-Zeitraum. Falls Sie beabsichtigen, arbeitsspeicherintensive Operationen durchzuführen, die gegebenenfalls eine ungewöhnlich hohe Arbeitsspeichernutzung zur Folge haben können, oder beliebige Datenoperationen auszuführen, durch die ein Überlauf des zugeordneten Speichers verursacht wird, empfiehlt es sich, die Ressourcen für Ihren Service zunächst manuell nach oben zu skalieren, um so zu vermeiden, dass Ressourcengrenzwerte erreicht werden, die sich nachteilig auf Clusteroperationen auswirken würden.

Bei der automatischen Skalierung wird kein Scale-down für Bereitstellungen durchgeführt, bei denen sich die Platten-/Speicherbelegung verringert hat. Die für Ihre Datenbanken bereitgestellten Ressourcen bleiben für zukünftige Anforderungen weiterhin bestehen, sofern Sie Ihre Bereitstellung nicht per Scale-down manuell nach unten skalieren.
{: .tip}
