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

# 連線配置
{: #connection-configuration}

{{site.data.keyword.composeForElasticsearch_full}} 資料庫連線由 2 個 HAProxy 入口網站管理。每一個入口網站都有 64 MB 的記憶體。

這兩個入口網站讓應用程式在其中一個入口網站無法聯繫時，仍能維持連線功能。用戶端的失效接手是應用程式設計人員的責任。部分 Elasticsearch 驅動程式會自動處理多個連線字串及循序失效接手。

如果您未使用能完全處理失效接手錯誤的驅動程式，請採取下列步驟自行處理錯誤。

* 使用可在其連線配置中接受多個端點的驅動程式。
* 確定在應用程式讀取資料庫或寫入資料庫時，應用程式能對錯誤有適當的反應。您可以配置應用程式，以不同方式對錯誤做出反應，包括下列各項：
  + 記載錯誤並重新連接。
  + 重新連接並重試已導致錯誤的作業。
  + 將失敗的作業放入佇列並排定重新連線。

## 傳輸中加密

所有 {{site.data.keyword.composeForElasticsearch}} HAProxy 入口網站都已啟用 TLS/SSL，並支援 TLS 1.2 版。服務的憑證為 Let's Encrypt 憑證。

## 連線限制

兩個 HAProxy 入口網站具有每個入口網站各 2000 個連線的連線限制。如果超出連線限制，服務可能會變成無回應。您可以透過驅動程式的連線儲存區特性，管理來自應用程式的連線。

## Proxy 逾時

HAProxy 入口網站會管理伺服器與用戶端之間連線的生命週期。此處會詳細說明逾時值，作為驅動程式撰寫者的參考手冊，以及對 {{site.data.keyword.composeForElasticsearch}} 資料庫連線低階活動有興趣者的參考手冊。

設定 |值|適用時機
----------|-----------|-----------
client | 1m |當用戶端預期要確認或傳送資料時。
connect | 9s |在 Proxy 和伺服器之間建立連線時。
server | 2m |當伺服器預期要確認或傳送資料時。
check | 5s |作為次要的逾時檢查，在 Proxy 和伺服器之間建立連線時。
http-request | 5s |等待完成 HTTP 要求的容許時間上限。
{: caption="表 1. Elasticsearch HAProxy 逾時" caption-side="top"}
