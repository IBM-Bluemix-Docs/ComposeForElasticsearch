---

copyright:
  years: 2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestión de copias de seguridad
{: #backups}

Puede crear y descargar copias de seguridad desde la página *Copias de seguridad* del panel de control del servicio. Dispone de copias de seguridad planificadas y manuales.

## Visualización de las copias de seguridad existentes

Se planifican automáticamente copias de seguridad diarias de la base de datos. Para ver las copias de seguridad existentes:

1. Vaya al panel de control del servicio.
2. Pulse **Copias de seguridad** en los separadores para abrir la página _Copias de seguridad_. Aparecerá una lista de las copias de seguridad disponibles:

  ![Copias de seguridad disponibles](./images/elastic_search-backups-show.png "Una lista de copias de seguridad disponibles.")

Pulse en la fila correspondiente para ampliar las opciones para cualquier copia de seguridad disponible.

![Opciones de copia de seguridad](./images/elastic_search-backups-options.png "Opciones de una copia de seguridad.") 

## Creación de una copia de seguridad manual

Además de copias de seguridad planificadas, puede crear una copia de seguridad manualmente. Para crear una copia de seguridad manual, siga los pasos para ver las copias de seguridad existentes y luego pulse **Copia de seguridad ahora** sobre la lista de copias de seguridad disponibles. Se mostrará un mensaje que le indicará que la copia de seguridad se ha iniciado y se añade una copia de seguridad 'pendiente' a la lista de copias de seguridad disponibles.

## Contenido de una copia de seguridad

Las copias de seguridad de {{site.data.keyword.composeForElasticsearch}} se toman con el programa de utilidad de instantáneas de la API de Elasticsearch. El proceso se ejecuta en todo clúster de forma que no bloquea, para que las operaciones de indexación y de búsqueda puedan continuar normalmente mientras está en ejecución. Se realiza una copia de seguridad en un punto en el tiempo en el momento en que se crea la instantánea.

## Restauración de una copia de seguridad
Para restaurar una copia de seguridad en una nueva instancia de servicio, siga los pasos para ver las copias de seguridad existentes y luego pulse en la fila correspondiente para ampliar las opciones para la copia de seguridad que desea descargar. Pulse el botón **Restaurar**. Se mostrará un mensaje que le indicará que se ha iniciado una restauración. A la nueva instancia del servicio se le asignará automáticamente el nombre "elasticsearch-restore-[timestamp]" y aparecerá en el panel de control cuando comience el suministro.
