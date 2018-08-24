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

# Configuración de conexión
{: #connection-configuration}

Las conexiones de base de datos {{site.data.keyword.composeForElasticsearch_full}} están gestionadas por 2 portales HAProxy. Cada portal tiene 64 MB de memoria.

Los dos portales permiten a las aplicaciones mantener la conectividad si uno de los portales pasa a ser no accesible. La migración tras error en el lado del cliente es responsabilidad del diseñador de aplicaciones. Algunos controladores de Elasticsearch manejan varias series de conexión y una migración tras error ordenada automáticamente.

Si no está trabajando con un controlador que maneja completamente los errores de migración tras error, realice los pasos siguientes para manejar los errores.

* Utilizar un controlador que acepte varios puntos finales en su configuración de conexión.
* Asegúrese de que la aplicación reacciona a los errores de forma adecuada cuando se lee desde o escribe en la base de datos. Puede configurar la aplicación para que reaccione a los errores de distintas formas, incluido lo siguiente:
  + Registre el error y vuelva a conectarse.
  + Reconectar y reintentar la operación que ha provocado el error.
  + Poner en cola la operación que ha fallado y planificar la reconexión.

## Cifrado en tránsito

Todos los portales HAProxy de {{site.data.keyword.composeForElasticsearch}} están habilitados para TLS/SSL y dan soporte a TLS versión 1.2. Los certificados para el servicio son certificados de Let's Encrypt.

## Límites de conexión

Los dos portales HAProxy tiene un límite de conexión de 2000 conexiones por portal. Si supera el límite de conexión, el servicio puede volverse insensible. Puede gestionar las conexiones desde la aplicación a través de la característica de agrupación de conexiones del controlador.

## Tiempos de espera del proxy

Los portales HAProxy gestionan el ciclo de vida de las conexiones entre el servidor y el cliente. Los valores de tiempo de espera se detallan aquí como guía de referencia para los escritores de controladores y aquellos que tienen un interés en la actividad de bajo nivel de las conexiones de base de datos de {{site.data.keyword.composeForElasticsearch}}.

Valor | Valor | Se aplica
----------|-----------|-----------
client | 1m | Cuando se espera que el cliente reconozca o envíe datos.
connect | 9s | Cuando se realiza una conexión entre el proxy y el servidor.
server | 2m | Cuando se espera que el servidor reconozca o envíe datos.
check | 5s | Como una comprobación de tiempo de espera secundaria, cuando se realiza una conexión entre el proxy y el servidor.
http-request | 5s | El tiempo máximo permitido para esperar a que se realice una solicitud HTTP completa.
{: caption="Tabla 1. Tiempos de espera de HAProxy de Elasticsearch" caption-side="top"}
