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

# 架構 
{: #architecture}


## 抄寫
{{site.data.keyword.composeForElasticsearch_full}} 服務包含位於叢集中的三個資料節點。您選取部署服務的地區，而這三個資料節點會分散在地區的可用性區域上。依預設，抄本計數會設為 1，為您提供兩個資料副本。Elasticsearch 會自動在叢集之間平衡您的抄本。如果資料節點變成無法聯繫，您的叢集可以繼續正常運作。
   
## 磁碟加密

所有 {{site.data.keyword.composeForElasticsearch}} 服務都有靜態加密。您的資料位於已啟用磁區層次加密的伺服器上。 

由於 {{site.data.keyword.composeForElastcisearch}} 是一項受管理的服務，因此 {{site.data.keyword.cloud}} Compose 操作員能夠看到資料。除了磁碟加密外，我們建議您也先將個人資訊加密，再將它儲存在資料庫中，或使用延伸或特性對資料庫本身啟用加密。這些方法固然可能影響可用性或效能，確定個人資訊受到加密保護仍是個很好的作法。

## 自動調整

資源會根據 Elasticsearch 資料庫的磁碟儲存空間總用量而自動調整。記憶體用量是以 1:10 的佈建儲存空間比例為基礎。這些資源組合成為單位。

自動調整的設計是要回應資料庫的中短期趨勢。每小時都會檢查您的服務，如果它的磁碟空間不足，便會將更多的單位配置給該部署。每 24 小時的期間內會重新配置一次。如果您計劃執行使用大量 RAM 的作業，可能會造成一般 RAM 用量激增，或是任何會使分配之儲存空間溢位的資料作業，建議您先手動調整服務的資源，以避免達到任何資源限制而影響叢集作業。

自動調整對於磁碟/記憶體用量收縮的部署，不會將其縮減。針對您的資料庫佈建的資源將會保留以供您未來需要，或是直到您手動縮減部署為止。
{: .tip}
