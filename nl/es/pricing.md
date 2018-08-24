---

copyright:
  years: 2016,2018
lastupdated: "2018-05-04"
---

# Tarifas
{: #pricing}

## Configuración básica

Un servicio de {{site.data.keyword.composeForElasticsearch_full}} se configura con tres nodos de datos idénticos. Cada uno de ellos tiene 2 GB de almacenamiento y 204 MB de memoria, lo que es igual a dos unidades de recursos. El servicio _incluye_ la réplica y la alta disponibilidad, de modo que cada unidad y el precio por unidad _incluye_ el coste de los recursos en los tres nodos.

La configuración básica también incluye las dos cápsulas HAProxy con 64 MB cada una para dar soporte a la autenticación, a HTTPS y a la lista blanca de IP. 

La configuración de servicio base tiene un precio establecido. Consulte los mosaicos del catálogo en {{site.data.keyword.cloud_notm}} para ver los precios básicos en su moneda local. Por ejemplo, el precio básico en dólares de EE.UU. es de 36 dólares/mes.

## Incremento de recursos

Si necesita más almacenamiento o memoria para el servicio, puede aumentar los recursos. Los recursos se aumentan en una proporción 10:1 del almacenamiento de disco a la unidad de memoria y el aumento del tamaño de disco que se asigna al despliegue también aumenta la RAM que se asigna. Una unidad {{site.data.keyword.composeForElasticsearch}} consta de 1 GB de almacenamiento y de 102 MB de memoria. Cada unidad y el precio por unidad _incluyen_ el coste para aumentar los recursos en los tres nodos.

## Cálculo del coste del despliegue
{: #tiered-pricing}

Cada unidad adicional (1 GB de almacenamiento/102 MB de memoria) tiene un precio por unidad, que aparece en la moneda local en el mosaico del catálogo de {{site.data.keyword.cloud_notm}} para el servicio. En dólares de EE.UU., cada unidad adicional cuesta 18 dólares. A medida que aumenta el tamaño _total_ de todos los servicios de {{site.data.keyword.composeForElasticsearch}}, el precio por unidad disminuye, como se muestra en la tabla de precio por niveles.

Número de unidades|Descuento por nivel
----------|-----------
De 2 a 9 unidades|Precio base por unidad - 18,00 dólares USD/unidad
De 10 a 24 unidades|10 % de reducción - 16,20 dólares USD/unidad
De 25 a 49 unidades|20 % de reducción - 14,40 dólares USD/unidad
De 50 a 99 unidades|30 % de reducción - 12,60 dólares USD/unidad
De 100 a 499 unidades|40 % de reducción - 10,80 dólares USD/unidad
De 500 a 999 unidades|50 % de reducción - 9,00 dólares USD/unidad
De 1.000 a 4.999 unidades|60 % de reducción - 7,20 dólares USD/unidad
Más de 5.000 unidades|70 % de reducción - 5,40 dólares USD/unidad
{: caption="Tabla 1. Precio por niveles de {{site.data.keyword.composeForElasticsearch}}" caption-side="top"}

