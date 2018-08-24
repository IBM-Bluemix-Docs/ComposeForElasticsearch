---

copyright:
  years: 2016,2018
lastupdated: "2018-05-04"
---

# Preços
{: #pricing}

## Configuração base

Um serviço {{site.data.keyword.composeForElasticsearch_full}} é configurado com três nós de dados idênticos. Cada um tem 2 GB de armazenamento e 204 MB de memória, o que é igual a duas unidades de recursos. O serviço _inclui_ replicação e alta disponibilidade, de maneira que cada unidade e o preço por unidade _inclua_ o custo dos recursos em todos os três nós.

A configuração base também inclui as duas cápsulas HAProxy com 64 MB cada uma para suportar autenticação, HTTPS e a lista de aplicativos confiáveis de IP. 

A configuração de serviço de base tem um preço definido. Verifique os quadros do catálogo no {{site.data.keyword.cloud_notm}} para precificação base na moeda local. Por exemplo, o preço base em dólares americanos é US$ 36/mês.

## Aumentando recursos

Se for necessário mais armazenamento ou memória para seu serviço, será possível aumentar os recursos. Os recursos são aumentados em uma razão 10:1 de armazenamento em disco para a unidade de memória e aumentar o tamanho do disco alocado para a implementação também aumenta a RAM alocada. Uma unidade do {{site.data.keyword.composeForElasticsearch}} consiste em 1 GB de armazenamento e 102 MB de memória. Cada unidade e o preço por unidade _incluem_ o custo para aumentar os recursos nos três nós.

## Calculando o custo de sua implementação
{: #tiered-pricing}

Cada unidade adicional (1 GB de armazenamento/102 MB de memória) tem um preço por unidade, que é listado em sua moeda local no quadro do catálogo do {{site.data.keyword.cloud_notm}} do serviço. Em dólares americanos cada unidade adicional custa $18. Conforme o tamanho _total_ de todos os serviços do {{site.data.keyword.composeForElasticsearch}} aumenta, o preço por unidade diminui, conforme mostrado na tabela de precificação em camadas.

Número de unidades|Desconto em camadas
----------|-----------
2 - 9 unidades|Preço base por unidade - US$ 18,00 USD/Unidade
10 - 24 unidades|Redução de 10%-$16.20 USD/Unidade
25 - 49 unidades|Redução de 20%-US$ 14,40 USD/Unidade
50 - 99 unidades|Redução de 30%-$12,60 USD/Unidade
100 - 499 unidades|Redução de 40%-US$ 10,80 USD/Unidade
500 - 999 unidades|50%de redução-$9,00 USD/Unidade
1.000 - 4.999 unidades|Redução de 60%-$7,20 USD/Unidade
+ de 5.000 unidades|70% redução-$5,40 USD/Unidade
{: caption="Tabela 1. Precificação em camadas do {{site.data.keyword.composeForElasticsearch}}" caption-side="top"}

