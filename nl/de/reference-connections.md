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

# Verbindungskonfiguration
{: #connection-configuration}

{{site.data.keyword.composeForElasticsearch_full}}-Datenbankverbindungen werden von 2 HAProxy-Portalen verwaltet. Jedes Portal verfügt über 64 MB Hauptspeicher.

Durch die zwei Portale bleibt für Anwendungen die Konnektivität auch dann erhalten, wenn eines der Portale nicht erreichbar ist. Eine Funktionsübernahme auf der Clientseite fällt in die Verantwortung der Anwendungsentwickler. Einige Elasticsearch-Treiber bearbeiten Zeichenfolgen für Mehrfachverbindungen und kontrollierte Funktionsübernahme (Failover) automatisch.

Wenn Sie nicht mit einem Treiber arbeiten, der Funktionsübernahmefehler vollständig behandelt, ergreifen Sie die folgenden Maßnahmen, um Fehler selbst zu bearbeiten.

* Arbeiten Sie mit einem Treiber, der mehrere Endpunkte in seiner Verbindungskonfiguration akzeptiert.
* Stellen Sie sicher, dass Ihre Anwendung angemessen auf Fehler reagiert, wenn sie Daten aus der Datenbank liest oder in die Datenbank schreibt. Sie können Ihre Anwendung so konfigurieren, dass sie auf unterschiedliche Arten auf Fehler reagiert, unter anderem auf die Folgenden:
  + Protokollieren des Fehlers und Wiederherstellen der Verbindung
  + Wiederherstellen der Verbindung und Wiederholen der Operation, die den Fehler verursacht hat
  + Einreihen der fehlgeschlagenen Operation in die Warteschlange und Terminieren der Verbindungswiederherstellung

## Verschlüsselung in Transit

Alle HAProxy-Portale von {{site.data.keyword.composeForElasticsearch}} sind für TLS/SSL aktiviert und unterstützen TLS Version 1.2. Die Zertifikate für den Service sind Zertifikate von Let's Encrypt.

## Grenzwerte für die Anzahl von Verbindungen

Für die beiden HAProxy-Portale gilt ein Grenzwert von 2.000 Verbindungen pro Portal. Bei Überschreiten dieses Verbindungsgrenzwerts besteht die Gefahr, dass Ihr Service unter Umständen nicht mehr reagiert. Über die Funktion Ihres Treibers für Verbindungspooling können Sie Verbindungen von ihrer Anwendung aus verwalten.

## Proxy-Zeitlimits

Die HAProxy-Portale verwalten den Lebenszyklus von Verbindungen zwischen dem Server und dem Client. Die Zeitlimitwerte werden hier als Leitfaden für Treiberautoren und all diejenigen aufgeführt, die sich für die maschinennahe Aktivität von {{site.data.keyword.composeForElasticsearch}}-Datenbankverbindungen interessieren.

Einstellung | Wert | Gültigkeit
----------|-----------|-----------
client | 1 m | Wenn vom Client erwartet wird, Daten zu bestätigen oder zu senden.
connect | 9 s | Wenn eine Verbindung zwischen dem Proxy und dem Server hergestellt wird.
server | 2 m | Wenn vom Server erwartet wird, Daten zu bestätigen oder zu senden.
check | 5 s | Als sekundäre Zeitlimitüberprüfung, wenn eine Verbindung zwischen dem Proxy und dem Server hergestellt wird.
http-request | 5 s | Die maximal zulässige Wartezeit für eine vollständige HTTP-Anforderung.
{: caption="Tabelle 1. HAProxy-Zeitlimitwerte für Elasticsearch" caption-side="top"}
