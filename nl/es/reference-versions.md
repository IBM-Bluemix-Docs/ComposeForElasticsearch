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

# Versiones

Versiones desplegables| Versión preferida
----------|-----------
2.4.6, 5.6.9, 6.2.2 | 6.2.2
{: caption="Tabla 1. Versiones de Elasticsearch" caption-side="top"}

## Versión preferida

La versión preferida es normalmente la versión más reciente de la base de datos Redis que está disponible para {{site.data.keyword.composeForElasticsearch}}. La versión preferida es el valor predeterminado en el selector de versión de la página de catálogo, y también es la versión que la API suministra automáticamente si no se especifica ninguna versión en la llamada.

### Nuevo protocolo de versión preferido

Cuando una nueva versión está disponible, su release se anuncia y la nueva versión está disponible para el despliegue. Después de la fecha de release, hay una ventana de 7 días aproximadamente en la que está disponible la versión más reciente, pero no es la versión preferida. Esta ventana permite a nuestros usuarios desplegar y probar la nueva versión, mientras aún tiene la versión actual disponible para ellos. También permite a los ingenieros de Compose detectar y arreglar cualquier problema que surja en la nueva versión. Al final de la ventana de 7 días, la nueva versión se establece como la versión preferida, o se anuncia una nueva fecha para este cambio.

Puede encontrar la lista de versiones disponibles para el suministro en la [página de catálogo](https://console.{DomainName}/catalog/services/compose-for-mongodb) de {{site.data.keyword.composeForMongoDB}}.

Para obtener una lista actual de las versiones disponibles para el servicio de {{site.data.keyword.composeForElasticsearch}}, puede utilizar el punto final
[GET /2016-07/deployments/:id/versions](https://apidocs.compose.com/v1.0/reference#2016-07-get-deployments-versions).

