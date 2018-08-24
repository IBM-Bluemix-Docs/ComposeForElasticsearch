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

# Upgrade für Elasticsearch durchführen
{: #upgrading}

Damit der Service jederzeit sicher und auf dem neuesten Stand ist, werden neue Versionen der Elasticsearch-Engine, die {{site.data.keyword.composeForElasticsearch_full}} zugrunde liegt, veröffentlicht. Es wird empfohlen, das Upgrade durchzuführen, sobald die neuen Versionen verfügbar sind. Wenn eine neue Version von {{site.data.keyword.composeForElasticsearch}} verfügbar ist, wird eine entsprechende Benachrichtigung auf der Seite _Übersicht_ angezeigt.

## Untergeordnete Versionen
In beinahe allen Fällen stehen Upgrades für untergeordnete Versionen als Inplace-Upgrades zur Verfügung. Sie können das Upgrade für Elasticsearch mit minimalen Ausfallzeiten durchführen, ohne dass Daten migriert werden müssen. Das Upgrade kann über die Registerkarte _Einstellungen_ des Service durchgeführt werden. Wählen Sie die gewünschte Upgradeversion im Dropdown-Menü aus.

## Übergeordnete Versionen
Derzeit stehen drei übergeordnete Elasticsearch-Versionen für {{site.data.keyword.composeForElasticsearch}} zur Verfügung: 2.4.6, 5.x und 6.x. Zum gegenwärtigen Zeitpunkt ist Folgendes möglich:
- Migration von Elasticsearch 5.x auf 6.x
- Migration von Elasticsearch 2.x auf 5.x

Beim Migrationsprozess zwischen allen übergeordneten Versionen wird eine aktuelle oder bedarfsorientierte Sicherung verwendet, um die Daten in einer neuen Bereitstellung wiederherzustellen. Dieser Prozess bietet eine Reihe von Vorteilen:

- Die ursprüngliche Datenbank bleibt aktiv und die Produktionsaktivitäten müssen nicht unterbrochen werden.
- Sie können die neue Datenbank außerhalb der Produktion testen und bei Anwendungsinkompatibilitäten entsprechend reagieren.
- Der gesamte Prozess kann zu jedem beliebigen Zeitpunkt erneut ausgeführt werden.
- Eine aktualisierte Wiederherstellung reduziert die Wahrscheinlichkeit, dass nicht benötigte Artefakte der älteren Datenbankversion in die neue Datenbank übernommen werden.

### Migrationsaspekte

Die meisten übergeordneten Versionen umfassen einige Änderungen, die sich auf die Verwendung von Elasticsearch auswirken können. Dies trifft vor allem für die Migration auf Elasticsearch 6.x zu. Berücksichtigen Sie diese Aspekte vor dem Upgrade, während des Testens der neuen Version und vor dem Zurücknehmen der Bereitstellung der älteren Version.

#### Indizes vor Version 5.x
Elasticsearch unterstützt normalerweise das Lesen von Indizes, die in der vorherigen übergeordneten Version erstellt wurden. Dies bedeutet, dass alle Indizes, die in Elasticsearch 5.x erstellt wurden, in Elasticsearch 6.x lesbar sind.

In Elasticsearch 2.x erstellte Indizes sind in Elasticsearch 6.x **nicht** lesbar. Diese Indizes müssen in Elasticsearch 5.x erneut indexiert werden, bevor die Migration auf 6.x erfolgt. Wenn die Daten in Elasticsearch 2.x erstellte Indizes enthalten und Sie versuchen, auf Elasticsearch 6.x zu migrieren, können die Knoten nicht gestartet werden und die Migration schlägt fehl.

[Die erneute Indexierung kann als Inplace-Prozess](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html) in Elasticsearch 5.x durchgeführt werden, bevor die Migration auf 6.x erfolgt, indem die [API für die erneute Indexierung](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html) verwendet wird.
{: .tip}

#### Indexzuordnungen

Die Möglichkeit, mehrere Zuordnungstypen pro Index zu erstellen, wurde in Elasticsearch 6.x entfernt. Alle in Elasticsearch 5.x erstellten Mehrfachindexzuordnungen können weiterhin verwendet werden, Elasticsearch 6.x unterstützt jedoch nur die Erstellung eines einzelnen Zuordnungstyps pro Index.

In Elasticsearch 7.x und höheren Versionen werden Zuordnungstypen entfernt.

#### Weitere Änderungen

Eine umfassende Dokumentation aktueller, zentraler Änderungen finden Sie in den [Elasticsearch-Dokumenten](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/breaking-changes-6.0.html).

### Migration auf eine neue Version

Für die Migration auf eine neue Elasticsearch-Version benötigen Sie die [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](https://console.{DomainName}/docs/cli/index.html#overview). Der Prozess ähnelt weitgehend der [Wiederherstellung von Sicherungen über die Befehlszeilenschnittstelle](./dashboard-backups.html#restoring-via-cli). Der Unterschied besteht darin, dass Sie im JSON-Feld "db_version" die gewünschte Upgradeversion angeben. Der vollständige Befehl lautet wie folgt:

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

Beispiel: Wiederherstellung eines {{site.data.keyword.composeForElasticsearch}}-Service mit Version 5.x auf einen neuen Service mit Elasticsearch 6.2.2:

```
ibmcloud service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
```
