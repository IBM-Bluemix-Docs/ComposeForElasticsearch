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

# Arquitectura 
{: #architecture}


## Réplica
Un servicio de {{site.data.keyword.composeForElasticsearch_full}} consta de tres nodos de datos en un clúster.  Seleccione una región en la que se ha desplegado el servicio y los tres nodos de datos se distribuirán a través de las zonas de disponibilidad de la región. El recuento de réplicas se establece de forma predeterminada en 1, dándole dos copias de los datos. Elasticsearch equilibra automáticamente las réplicas en todo el clúster. Si un nodo de datos se convierte en inalcanzable, el clúster puede continuar funcionando normalmente.
   
## Cifrado de disco

Todos los servicios de {{site.data.keyword.composeForElasticsearch}} tienen cifrado en reposo. Los datos residen en servidores que tienen el cifrado de nivel de volumen habilitado. 

Puesto que {{site.data.keyword.composeForElastcisearch}} es un servicio gestionado, es posible que los operadores de {{site.data.keyword.cloud}} Compose vean los datos. Además del cifrado de disco, le recomendamos que cifre la información personal antes de almacenarla en la base de datos o que utilice extensiones o funciones para habilitar el cifrado en la propia base de datos. Si bien estos métodos pueden afectar a la usabilidad o al rendimiento, es una buena práctica asegurarse de que la información personal esté protegida con cifrado.

## Escalado automático

Los recursos se escalan automáticamente en función del uso de almacenamiento de disco total de las bases de datos Elasticsearch. El uso de memoria se basa en una proporción de almacenamiento suministrado en una proporción de 1:10. Estos recursos se empaquetan como unidades.

El escalado automático está diseñado para responder a las tendencias a corto y medio plazo de la base de datos. Cada hora, el servicio está marcado y si se está quedando corto en el espacio de disco, se asignan más unidades al despliegue. La reasignación se produce una vez en cada periodo de 24 horas. Si está pensando en ejecutar operaciones pesadas de RAM que pueden poner un pico en el uso habitual de RAM, o cualquier operación de datos que desbordará el almacenamiento asignado, se recomienda escalar manualmente los recursos del servicio en primer lugar para evitar que se alcance ningún límite de recursos que afecte a las operaciones del clúster.

El escalado automático no reduce los despliegues en los que el uso de disco/memoria se ha reducido. Los recursos suministrados a las bases de datos permanecerán para sus necesidades futuras, o hasta que reduzca el despliegue manualmente.
{: .tip}
