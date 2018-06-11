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

# Acerca de {{site.data.keyword.composeForElasticsearch}}
{: #about-compose-for-elasticsearch}

{{site.data.keyword.composeForElasticsearch_full}} combina la potencia de un motor de búsqueda de texto completo con la fuerza de indexación de una base de datos de documentos JSON. Juntos, crean una herramienta potente para el análisis de datos enriquecidos en grandes volúmenes de datos. Con Elasticsearch, su búsqueda se puede puntuar para su exactitud, permitiéndole indagar en el conjunto de datos para las coincidencias casi exactas que no puede conseguir.
{:shortdesc}

**Nota:** Cualquier instancia de servicio de Compose proporcionada antes del 14 de septiembre de 2016 que siga activa se puede seguir utilizando y se puede acceder a ella directamente en [https://www.compose.com/](https://www.compose.com). Se puede acceder directamente a cualquier instancia de servicio de Compose que se proporcione desde este momento en adelante y se utilizará dentro de la cuenta de {{site.data.keyword.cloud}}.

## Creación de una instancia de servicio de {{site.data.keyword.composeForElasticsearch}}

Puede crear un servicio de {{site.data.keyword.composeForElasticsearch}} desde la [página de {{site.data.keyword.composeForElasticsearch}}](https://console.{DomainName}/catalog/services/compose-for-elasticsearch/) en el catálogo de {{site.data.keyword.cloud_notm}}.

Elija un nombre de servicio, y una región, organización y espacio en los que suministrar el servicio. Puede utilizar el campo **Seleccionar una versión de base de datos** para elegir entre las versiones de bases de datos disponibles.

Cuando suministre la instancia de {{site.data.keyword.composeForElasticsearch}}, puede elegir los planes *Estándar* o *Empresa*. Con el plan *Empresa*, puede suministrar la instancia de {{site.data.keyword.composeForElasticsearch}} en un clúster disponible de {{site.data.keyword.composeEnterprise}}. {{site.data.keyword.composeEnterprise}} proporciona la seguridad y nivel de aislamiento necesarios para el cumplimiento de las reglas empresariales y utiliza redes dedicadas para garantizar el rendimiento de las bases de datos desplegadas. Consulte la [documentación de Compose Enterprise](../ComposeEnterprise/index.html) para ver más detalles.

## Gestión de {{site.data.keyword.composeForElasticsearch}}

Puede gestionar el servicio desde el panel de control del servicio. Aquí encontrará información sobre su base de datos de {{site.data.keyword.cloud}} Compose y cómo conectarse a la misma. También puede:

- gestionar copias de seguridad
- asignar más recursos para el servicio 
- utilizar listas blancas para restringir el acceso a las bases de datos.

Para obtener más información, consulte [Valores](./dashboard-settings.html).

{{site.data.keyword.composeForElasticsearch}} utiliza roles de Cloud Foundry para gestionar el acceso al servicio. Solo los usuarios con el rol de desarrollador pueden ver o utilizar el panel de control del servicio. Para obtener más información sobre los roles de Cloud Foundry, consulte las páginas [Acceso de Cloud Foundry](https://console.bluemix.net/docs/iam/cfaccess.html#cfaccess) y [Gestión del acceso de Cloud Foundry](https://console.bluemix.net/docs/iam/mngcf.html#mngcf).
{: .tip}

## Conexión a {{site.data.keyword.composeForElasticsearch}}

Puede conectarse a su servicio utilizando las credenciales que se crean junto con el servicio, o con las series de conexión y la línea de mandatos que se proporcionan en el separador *Visión general* del panel de control del servicio.

## Conexión de una aplicación {{site.data.keyword.composeForElasticsearch}} a {{site.data.keyword.cloud_notm}}

Para conectar una aplicación {{site.data.keyword.cloud_notm}} al servicio, utilice las credenciales que se crean junto con el servicio. Encontrará información sobre cómo conectar una aplicación {{site.data.keyword.cloud_notm}} a un servicio {{site.data.keyword.composeForElasticsearch}} en el apartado [Conexión de una aplicación {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Conexión a {{site.data.keyword.composeForElasticsearch}} desde fuera de {{site.data.keyword.cloud_notm}}

Si desea conectarse a {{site.data.keyword.composeForElasticsearch}} desde fuera de {{site.data.keyword.cloud_notm}}, puede utilizar las series de conexión proporcionadas o la línea de mandatos. Encontrará información sobre cómo conectar en el apartado [Conexión de una aplicación externa](./connecting-external.html).
