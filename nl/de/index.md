---

copyright:
  years: 2016,2018
lastupdated: "2018-03-26"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Informationen zu {{site.data.keyword.composeForElasticsearch}}
{: #about-compose-for-elasticsearch}

{{site.data.keyword.composeForElasticsearch_full}} vereint die Leistung einer Engine für die Volltextsuche mit den Indexierungsfähigkeiten einer JSON-Dokument-Datenbank. Gemeinsam bilden sie ein leistungsfähiges Tool zur Analyse komplexer Daten bei großen Datenvolumen. Mit Elasticsearch kann Ihre Suche mit Genauigkeits-Scores bewertet werden, was es Ihnen ermöglicht, Ihren Datensatz nach diesen jeweils größten Übereinstimmungen und Beinahe-Treffern zu durchsuchen, die Sie sonst überspringen würden.
{:shortdesc}

**Hinweis:** Alle Compose-Serviceinstanzen, die vor dem 14. September 2016 bereitgestellt wurden und noch aktiv sind, können weiterhin verwendet werden und sind über [https://www.compose.com/](https://www.compose.com) zugänglich. Jede Compose-Serviceinstanz, die ab diesem Punkt bereitgestellt wird, ist direkt über Ihr {{site.data.keyword.cloud}}-Konto zugänglich und kann dort verwendet werden.

## {{site.data.keyword.composeForElasticsearch}}-Serviceinstanz erstellen

Sie können einen {{site.data.keyword.composeForElasticsearch}}-Service über die [{{site.data.keyword.composeForElasticsearch}}-Seite](https://console.{DomainName}/catalog/services/compose-for-elasticsearch/) im {{site.data.keyword.cloud_notm}}-Katalog erstellen.

Wählen Sie einen Servicenamen und eine Region, eine Organisation und einen Bereich zur Bereitstellung des Service aus. Im Feld **Datenbankversion auswählen** können Sie eine der verfügbaren Datenbankversionen auswählen.

Bei der Bereitstellung Ihrer {{site.data.keyword.composeForElasticsearch}}-Instanz können Sie den Plan *Standard* oder *Enterprise* auswählen. Mit dem Plan *Enterprise* können Sie Ihre {{site.data.keyword.composeForElasticsearch}}-Instanz in einem {{site.data.keyword.composeEnterprise}}-Cluster bereitstellen. {{site.data.keyword.composeEnterprise}} stellt die für die Konformität mit Enterprise erforderliche Sicherheit und Isolation bereit und stellt mithilfe eines dedizierten Netzbetriebs die Leistung der bereitgestellten Datenbanken sicher. Weitere Details finden Sie in der [Dokumentation zu Compose Enterprise](../ComposeEnterprise/index.html).

## {{site.data.keyword.composeForElasticsearch}} verwalten

Sie können Ihren Service über das Service-Dashboard verwalten. Hier finden Sie Informationen zu Ihrer {{site.data.keyword.cloud}} Compose-Datenbank und dazu, wie Sie eine Verbindung zu ihr herstellen. Außerdem können Sie:

- Ihre Sicherungen verwalten
- Ihrem Service mehr Ressourcen zuordnen 
- Whitelists verwenden, um den Zugriff auf Ihre Datenbank zu beschränken.

Weitere Informationen finden Sie im Abschnitt [Einstellungen](./dashboard-settings.html).

{{site.data.keyword.composeForElasticsearch}} nutzt Cloud Foundry-Rollen, um den Zugriff auf den Service zu verwalten. Nur Benutzer mit der Entwicklerrolle können das Service-Dashboard anzeigen oder verwenden. Weitere Informationen zu Cloud Foundry-Rollen finden Sie auf den Seiten [Cloud Foundry-Zugriff](https://console.bluemix.net/docs/iam/cfaccess.html#cfaccess) und [Cloud Foundry-Zugriff](https://console.bluemix.net/docs/iam/mngcf.html#mngcf) verwalten.
{: .tip}

## Verbindung zu {{site.data.keyword.composeForElasticsearch}} herstellen

Sie können mit den Berechtigungsnachweisen, die zusammen mit Ihrem Service erstellt werden, oder mithilfe der Verbindungszeichenfolgen und der Befehlszeile, die auf der Registerkarte *Übersicht* Ihres Service-Dashboards bereitgestellt werden, eine Verbindung zu dem Service herstellen.

## Verbindung von einer {{site.data.keyword.cloud_notm}}-Anwendung zu {{site.data.keyword.composeForElasticsearch}} herstellen

Um eine {{site.data.keyword.cloud_notm}}-Anwendung mit Ihrem Service zu verbinden, verwenden Sie die Berechtigungsnachweise, die zusammen mit dem Service erstellt wurden. Informationen zum Herstellen einer Verbindung von einer {{site.data.keyword.cloud_notm}}-Anwendung zu einem {{site.data.keyword.composeForElasticsearch}}-Service finden Sie im Abschnitt [{{site.data.keyword.cloud_notm}}-Anwendung verbinden](./connecting-bluemix-app.html).

## Verbindung zu {{site.data.keyword.composeForElasticsearch}} außerhalb von {{site.data.keyword.cloud_notm}} herstellen

Wenn Sie außerhalb von {{site.data.keyword.cloud_notm}} eine Verbindung zu {{site.data.keyword.composeForElasticsearch}} herstellen wollen, können Sie dazu die bereitgestellten Verbindungszeichenfolgen oder die Befehlszeile verwenden. Informationen dazu, wie sich eine Verbindung herstellen lässt, finden Sie im Abschnitt [Externe Anwendung verbinden](./connecting-external.html).
