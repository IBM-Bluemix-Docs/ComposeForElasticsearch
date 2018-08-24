---

copyright:
  years: 2016,2018
lastupdated: "2018-05-04"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 關於 {{site.data.keyword.composeForElasticsearch}}
{: #about-compose-for-elasticsearch}

{{site.data.keyword.composeForElasticsearch_full}} 結合了全文檢索引擎的強大功能與 JSON 文件資料庫的編製索引強項。它們會一起建立強大的工具，以對大量資料進行豐富的資料分析。使用 Elasticsearch，可以針對確切程度進行搜尋的評分，讓您可以探索資料集以尋找最相符的項目以及可能遺漏的接近遺漏項目。
{:shortdesc}

**附註：**在 2016 年 9 月 14 日之前佈建且仍然作用中的任何 Compose 服務實例，仍然可以使用，而且可以在 [https://www.compose.com/](https://www.compose.com) 直接存取。從這個時間點以後佈建的任何 Compose 服務實例，可以在 {{site.data.keyword.cloud}} 帳戶內直接存取及使用。

## 建立 {{site.data.keyword.composeForElasticsearch}} 服務實例

您可以從 {{site.data.keyword.cloud_notm}} 型錄中的 [{{site.data.keyword.composeForElasticsearch}} 頁面](https://console.{DomainName}/catalog/services/compose-for-elasticsearch/)建立 {{site.data.keyword.composeForElasticsearch}} 服務。

選擇服務名稱，以及要在其中佈建服務的地區、組織和空間。您可以使用**選取資料庫版本**欄位，從可用的資料庫版本中進行選擇。

當您佈建 {{site.data.keyword.composeForElasticsearch}} 實例時，可以選擇*標準* 或*企業* 方案。使用*企業* 方案，您可以將 {{site.data.keyword.composeForElasticsearch}} 實例佈建到可用的 {{site.data.keyword.composeEnterprise}} 叢集。{{site.data.keyword.composeEnterprise}} 提供企業法規遵循所需的安全和隔離，並使用專用網路來確保已部署之資料庫的效能。如需詳細資料，請參閱 [{{site.data.keyword.composeEnterprise}} 文件](/docs/services/ComposeEnterprise/index.html)。

## 管理 {{site.data.keyword.composeForElasticsearch}}

您可以從服務儀表板來管理服務。在這裡，您可以找到 {{site.data.keyword.cloud}} Compose 資料庫的相關資訊，以及連接方式。您也可以：

- 管理備份。
- 為您的服務配置更多資源。 
- 使用白名單來限制對資料庫的存取權。

如需相關資訊，請參閱[設定](./dashboard-settings.html)。

{{site.data.keyword.composeForElasticsearch}} 根據 Cloud Foundry 角色來管理對服務的存取權。只有具有「開發人員」角色的使用者才能看到或使用服務儀表板。如需 Cloud Foundry 角色的相關資訊，請參閱 [Cloud Foundry 存取](https://console.{DomainName}/docs/iam/cfaccess.html#cfaccess)及[管理 Cloud Foundry 存取](https://console.{DomainName}/docs/iam/mngcf.html#mngcf)頁面。
{: .tip}

## 連接至 {{site.data.keyword.composeForElasticsearch}}

您可以使用與服務一起建立的認證來連接至服務，或使用服務儀表板的*概觀* 標籤中所提供的連線字串及指令行來連接至服務。

## 將 {{site.data.keyword.cloud_notm}} 應用程式連接至 {{site.data.keyword.composeForElasticsearch}}

若要將 {{site.data.keyword.cloud_notm}} 應用程式連接至服務，請使用與服務一起建立的認證。您可以在[連接 {{site.data.keyword.cloud_notm}} 應用程式](./connecting-bluemix-app.html)中，找到如何將 {{site.data.keyword.cloud_notm}} 應用程式連接至 {{site.data.keyword.composeForElasticsearch}} 服務的資訊。

## 從 {{site.data.keyword.cloud_notm}} 之外連接至 {{site.data.keyword.composeForElasticsearch}}

如果您想要從 {{site.data.keyword.cloud_notm}} 之外連接至 {{site.data.keyword.composeForElasticsearch}}，則可以使用所提供的連線字串或指令行。您可以在[連接外部應用程式](./connecting-external.html)中，找到如何連接的相關資訊。
