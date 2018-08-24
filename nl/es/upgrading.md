---

copyright:
  years: 2016,2018
lastupdated: "2018-07-25"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Actualización de Elasticsearch
{: #upgrading}

Con el objeto de mantener el servicio seguro y actualizado, se lanzarán versiones nuevas del motor de Elasticsearch por debajo de {{site.data.keyword.composeForElasticsearch_full}}. Se recomienda actualizar en el momento en que estén disponibles las versiones nuevas. Cuando esté disponible una versión nueva de {{site.data.keyword.composeForElasticsearch}}, aparecerá una notificación en la página _Visión general_.

## Versiones anteriores
En casi todos los casos, las actualizaciones de versiones menores estarán disponibles en la ubicación original. Podrá actualizar Elasticsearch sin migrar ningún dato y con un tiempo de inactividad mínimo. Puede actualizar desde el separador _Configuración_ del servicio seleccionando la versión a la que desea actualizar desde el menú desplegable.

## Versiones principales
Actualmente hay tres versiones principales de Elasticsearch disponibles para {{site.data.keyword.composeForElasticsearch}}: 2.4.6, 5.x y 6.x. En este momento, es posible:
- migrar desde Elasticsearch 5.x a 6.x
- migrar desde Elasticsearch 2.x a 5.x.

El proceso de migración entre todas las versiones principales utiliza una copia de seguridad reciente o bajo demanda para restaurar los datos en un nuevo despliegue. Este proceso proporciona varias ventajas:

- La base de datos original se mantiene en ejecución y el trabajo de producción puede ser ininterrumpido.
- Puede probar la base de datos nueva fuera de producción y actuar en cualquier incompatibilidad de aplicación.
- Todo el proceso se puede volver a ejecutar en cualquier momento.
- Una nueva restauración reduce la probabilidad de que se envíen artefactos innecesarios de la versión más antigua de la base de datos a la nueva base de datos.

### Consideraciones sobre la migración

La mayoría de las versiones principales incluyen algunos cambios que pueden afectar a cómo se utiliza Elasticsearch. Esto es especialmente cierto para migrar a Elasticsearch 6.x. Tenga en cuenta estas cosas antes de actualizar, a la vez que prueba la versión nueva, y antes de dejar de suministrar la versión anterior.

#### Índices anteriores a 5.x
Elasticsearch normalmente da soporte a la lectura de índices creados en la versión principal anterior. Esto significa que cualquier índice creado en Elasticsearch 5.x es legible por Elasticsearch 6.x.

Si tiene índices creados en Elasticsearch 2.x, **no** serán legibles en Elasticsearch 6.x. Estos índices necesitarán volver a indexarse en Elasticsearch 5.x antes de migrar a 6.x. Si los datos contienen índices realizados en Elasticsearch 2.x e intenta migrar a Elasticsearch 6.x, los nodos no podrán iniciarse y la migración fallará.

[La reindexación se puede realizar en la ubicación original](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html) en Elasticsearch 5.x antes de migrar a 6.x mediante la [API de reindexación](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html).
{: .tip}

#### Correlaciones de índice

Se ha eliminado la posibilidad de crear varios tipos de correlación por índice en Elasticsearch 6.x. Cualquier correlación de índices múltiple creada en Elasticsearch 5.x seguirá funcionando, pero Elasticsearch 6.x solo da soporte a la creación de un único tipo de correlación por índice.

Los tipos de correlación se eliminarán en Elasticsearch 7.x y posterior.

#### Otros cambios

Se puede encontrar la documentación completa de los cambios de última hora en la [Documentación de Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/breaking-changes-6.0.html).

### Migración a una nueva versión

Para migrar a una nueva versión de Elasticsearch, necesitará la [CLI de {{site.data.keyword.cloud_notm}}](https://console.{DomainName}/docs/cli/index.html#overview). El proceso es muy similar a la [restauración de la copia de seguridad mediante la CLI](./dashboard-backups.html#restoring-via-cli), excepto que utilizará el campo "db_version" del JSON para especificar a qué versión está actualizando. El mandato completo es:

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

Por ejemplo, la restauración de un servicio de {{site.data.keyword.composeForElasticsearch}} que ejecuta 5.x a un servicio nuevo que ejecuta Elasticsearch 6.2.2 tiene este aspecto:

```
ibmcloud service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
```
