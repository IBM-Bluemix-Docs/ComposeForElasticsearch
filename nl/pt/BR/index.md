---

copyright:
  years: 2016,2017
lastupdated: "2017-10-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Introdução ao {{site.data.keyword.composeForElasticsearch}}
{: #getting-started-with-compose-for-elasticsearch}

O {{site.data.keyword.composeForElasticsearch_full}} combina o poder de um
mecanismo de procura de texto completa com os potenciais de indexação de um banco de
dados de documento JSON. Juntos, eles criam uma ferramenta poderosa para análise de dados
ricos em grandes volumes de dados. Com o Elasticsearch, sua procura pode ser pontuada
para fins de exatidão, permitindo que você pesquise a fundo seu conjunto de dados em busca de
correspondências próximas e quase perdas que você possa estar deixando passar.
{:shortdesc}

**Nota:** todas as instâncias de serviço do Compose que foram
provisionadas antes de 14 de setembro de 2016 que ainda estiverem ativas poderão ser
usadas e acessadas diretamente em
[https://www.compose.com/](https://www.compose.com). Qualquer instância de serviço do Compose fornecida deste ponto em diante é acessada e usada diretamente dentro de sua conta do {{site.data.keyword.cloud}}.

## Criando uma instância de serviço do Compose for Elasticsearch

[Crie uma instância do {{site.data.keyword.composeForElasticsearch}}](https://console.bluemix.net/catalog/services/compose-for-elasticsearch/).

Ao criar uma instância do serviço, assegure-se de escolher um nome para seu
serviço e um nome de credencial. Deixe o serviço desvinculado; é possível conectar um
aplicativo ao seu serviço mais tarde usando as credenciais que são fornecidas quando o
serviço é provisionado. Os diversos valores de credenciais são listados na seção
*Credenciais disponíveis*.

Quando você provisiona sua instância do {{site.data.keyword.composeForElasticsearch}}, é possível escolher os planos *Padrão* ou *Corporativo*. Com o plano *Corporativo*, é possível provisionar sua instância do {{site.data.keyword.composeForElasticsearch}} em um cluster disponível do {{site.data.keyword.composeEnterprise}}. O {{site.data.keyword.composeEnterprise}} fornece a segurança e o isolamento requeridos pela conformidade corporativa e usa rede dedicada para assegurar o desempenho dos bancos de dados implementados. Veja a [Documentação do Compose Enterprise](../ComposeEnterprise/index.html) para obter mais detalhes.

## Gerenciando o Compose for Elasticsearch

É possível gerenciar seu serviço no painel de serviço. Aqui é possível localizar informações sobre o banco de dados do {{site.data.keyword.cloud}} Compose e como conectar-se a ele. Também é possível:

- gerenciar seus backups
- alocar mais recursos para seu serviço 
- use listas de desbloqueio para restringir o acesso a seus bancos de dados.

Para obter mais informações, veja [Configurações](./dashboard-settings.html).

## Conectando-se ao {{site.data.keyword.composeForElasticsearch}}

É possível se conectar a seu serviço usando as credenciais que são criadas junto com o serviço ou com as sequências de conexões e linha de comandos que são fornecidas na guia *Visão geral* de seu painel de serviço.

## Conectando um aplicativo {{site.data.keyword.cloud_notm}} ao {{site.data.keyword.composeForElasticsearch}}

Para conectar um aplicativo {{site.data.keyword.cloud_notm}} a seu serviço, use as credenciais que são criadas com o serviço. É possível localizar informações sobre como conectar um aplicativo {{site.data.keyword.cloud_notm}} a um serviço {{site.data.keyword.composeForElasticsearch}} em [Conectando um aplicativo {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Conectando-se ao {{site.data.keyword.composeForElasticsearch}} de fora do {{site.data.keyword.cloud_notm}}

Se você deseja se conectar ao {{site.data.keyword.composeForElasticsearch}} de fora do {{site.data.keyword.cloud_notm}}, é possível usar as sequências de conexões fornecidas ou a linha de comandos. É possível localizar informações sobre como conectar-se em [Conectando um aplicativo externo](./connecting-external.html).
