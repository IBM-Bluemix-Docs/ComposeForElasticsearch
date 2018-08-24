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

# Versões

Versões Implementáveis| Versão Preferencial
----------|-----------
2.4.6, 5.6.9, 6.2.2 | 6.2.2
{: caption="Tabela 1. Versões de Elasticsearch" caption-side="top"}

## Versão Preferencial

A versão preferencial é geralmente a mais recente do banco de dados Redis que está disponível para o {{site.data.keyword.composeForElasticsearch}}. A versão preferencial é o padrão no seletor de versão na página de catálogo e também será a versão provisionada automaticamente pela API se nenhuma versão for especificada na chamada.

### Novo Protocolo de Versão Preferencial

Quando uma nova versão é disponibilizada, sua liberação é anunciada e a nova versão é disponibilizada para implementação. Após a data de liberação, há uma janela de aproximadamente 7 dias na qual a versão mais recente fica disponível, mas não é a versão preferencial. Essa janela permite que nossos usuários implementem e testem a nova versão, enquanto ainda têm a versão atual disponível para eles. Também permite que os engenheiros do Compose localizem e corrijam quaisquer problemas que surjam na nova versão. No final da janela de 7 dias, a nova versão é configurada como a versão preferencial ou uma nova data para essa mudança é anunciada.

É possível localizar a lista de versões disponíveis para fornecimento na [página de catálogo](https://console.{DomainName}/catalog/services/compose-for-mongodb) do {{site.data.keyword.composeForMongoDB}}.

Para obter uma lista atual de versões disponíveis para o serviço {{site.data.keyword.composeForElasticsearch}}, é possível usar o terminal [GET /2016-07/deployments/:id/versions](https://apidocs.compose.com/v1.0/reference#2016-07-get-deployments-versions).

