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

# Sicherungen verwalten
{: #backups}

Sie können Sicherungen erstellen und über die Seite *Sicherungen* Ihres Service-Dashboards herunterladen. Es sind sowohl geplante als auch manuelle Sicherungen verfügbar.

## Vorhandene Sicherungen anzeigen

Tägliche Sicherungen Ihrer Datenbank werden automatisch geplant. Gehen Sie wie folgt vor, um Ihre vorhandenen Sicherungen anzuzeigen:

1. Navigieren Sie zu Ihrem Service-Dashboard.
2. Klicken Sie auf die Registerkarte **Sicherungen**, um die Seite _Sicherungen_ zu öffnen. Es wird eine Liste der verfügbaren Sicherungen angezeigt:

  ![Verfügbare Sicherungen](./images/elastic_search-backups-show.png "Liste der verfügbaren Sicherungen.")

Klicken Sie in eine Zeile, um die Optionen für die entsprechende verfügbare Sicherung zu erweitern.

![Sicherungsoptionen](./images/elastic_search-backups-options.png "Optionen für eine Sicherung.") 

## Manuelle Sicherung erstellen

Neben geplanten Sicherungen können Sie manuelle Sicherungen erstellen. Führen Sie zum Erstellen einer manuellen Sicherung die Schritte zum Anzeigen der vorhandenen Sicherungen aus. Klicken Sie dann über der Liste der vorhandenen Sicherungen auf **Jetzt sichern**. Es wird eine Nachricht darüber angezeigt, dass eine Sicherung eingeleitet wurde, und zur Liste der verfügbaren Sicherungen wird eine 'anstehende' Sicherung hinzugefügt.

## Inhalt von Sicherungen

{{site.data.keyword.composeForElasticsearch}}-Sicherungen werden mit dem in der Elasticsearch-API verfügbaren Dienstprogramm für Snapshots erstellt. Der Prozess wird in dem gesamten Cluster auf nicht blockierende Weise ausgeführt, sodass während der Ausführung alle Indexierungs- und Suchoperationen unverändert fortgesetzt werden können. Im Moment der Erstellung des Snapshots wird eine Sicherung mit Zeitangabe vorgenommen.

## Sicherung wiederherstellen
Führen Sie zum Wiederherstellen einer Sicherung in eine neue Serviceinstanz die Schritte zum Anzeigen der vorhandenen Sicherungen aus. Klicken Sie dann in die entsprechende Zeile, um die Optionen für die Sicherung zu erweitern, die Sie herunterladen wollen. Klicken Sie auf die Schaltfläche **Wiederherstellen**. Es wird eine Nachricht darüber angezeigt, dass eine Wiederherstellung eingeleitet wurde. Die neue Serviceinstanz erhält automatisch den Namen "elasticsearch-restore-[timestamp]" und wird beim Start der Bereitstellung in Ihrem Dashboard angezeigt.
