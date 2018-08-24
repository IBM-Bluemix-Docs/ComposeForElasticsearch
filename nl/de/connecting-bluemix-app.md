---

copyright:
  years: 2016,2018
lastupdated: "2017-06-16"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.cloud_notm}}-Anwendung verbinden

Verbinden Sie eine {{site.data.keyword.cloud}}-Anwendung mit Ihrem Service mithilfe der Berechtigungsnachweise dieses Service, die bei dessen Bereitstellung erstellt werden. Die Beispielapp veranschaulicht die Verwendung von Node.js zur Herstellung einer Verbindung zu einem {{site.data.keyword.composeForElasticsearch}}-Service mithilfe der bereitgestellten Berechtigungsnachweise und die Vorgehensweise zur Erstellung einer Datenbank sowie für Lese- und Schreibvorgänge in dieser Datenbank.

## Verbindungsherstellung mit der Beispielapp 'Hello World'

Die Beispielapp [compose-elasticsearch-helloworld-nodejs](https://github.com/IBM-Cloud/compose-elasticsearch-helloworld-nodejs) veranschaulicht die Verwendung von Node.js zur Herstellung einer Verbindung zu einem {{site.data.keyword.composeForElasticsearch}}-Service mithilfe der bereitgestellten Berechtigungsnachweise. Die Anwendung erstellt einen Elasticsearch-Index, liest daraus und schreibt darin.

Laden Sie die Beispielapp herunter und befolgen Sie die Anweisungen in der Readme-Datei. Anschließend klicken Sie auf der Detailseite für die Anwendung in {{site.data.keyword.cloud}} auf **App anzeigen**, um den Inhalt des Index *examples* anzuzeigen.

## Berechtigungsnachweise

Feldname|Beschreibung
----------|-----------
`uri`|Der URI, der bei der Verbindungsherstellung zum Service verwendet werden soll. Enthält das Schema (`https:`), Administrator-Benutzernamen und Kennwort, den Hostnamen und die Nummer des Ports, zu dem die Verbindung hergestellt werden soll.
`uri_direct_1`|Ein sekundärer URI, der bei der Verbindungsherstellung zum Service verwendet werden kann. Format wie für `uri`.
`uri_health`|Ein `curl`-Befehl, der vom ersten HAProxy Informationen zum Clusterstatus anfordert.
`uri_health_1`|Ein `curl`-Befehl, der vom zweiten HAProxy Informationen zum Clusterstatus anfordert.
`ca_certificate_base64` `(optional)`|Ein selbst signiertes Zertifikat in Base64-Codierung, mit dem bestätigt wird, dass eine Anwendung eine Verbindung zum entsprechenden Server herstellt. Das Zertifikat ist nicht für Services vorhanden, die ein Zertifikat von Let's Encrypt verwenden. Der Schlüssel muss vor seiner Verwendung decodiert werden, wie in der Beispielanwendung gezeigt. Der Schlüssel muss vor seiner Verwendung decodiert werden, wie in der Beispielanwendung gezeigt.
`deployment_id`|Eine interne ID für den Service entsprechend der Erstellung in Compose.
`db_type`|Der Datenbanktyp, der vom Service angeboten wird, in diesem Fall `elastic_search`.
`name`|Der Name der Datenbankimplementierung.
{: caption="Tabelle 1. Compose for Elasticsearch - Berechtigungsnachweise" caption-side="top"}

**Hinweis:** Zwei `haproxy`-Portale bieten Zugriff auf den Elasticsearch-Cluster. Sowohl `uri` als auch `uri_direct_1` können für die Verbindung zum Cluster verwendet werden. In Ihrer Anwendung wechseln Sie zwischen `uri` und `uri_direct_1`, um die Reaktionen auf Verbindungsfehler zu verwalten.
