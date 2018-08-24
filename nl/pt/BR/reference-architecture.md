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

# Arquitetura 
{: #architecture}


## Replicação
Um serviço {{site.data.keyword.composeForElasticsearch_full}} consiste em três nós de dados em um cluster.  Você seleciona uma região na qual o serviço é implementado, e os três nós de dados são distribuídos pelas zonas de disponibilidade da região. A contagem de réplicas é configurada por padrão como 1, fornecendo a você duas cópias de seus dados. O Elasticsearch equilibra suas réplicas no cluster automaticamente. Se um nó de dados se tornar inacessível, o seu cluster poderá continuar a operar normalmente.
   
## Criptografia de Disco

Todos os serviços do  {{site.data.keyword.composeForElasticsearch}}  têm criptografia em repouso. Seus dados residem em servidores que possuem criptografia em nível de volume ativada. 

Como o {{site.data.keyword.composeForElastcisearch}} é um serviço gerenciado, é possível que os operadores do {{site.data.keyword.cloud}} Compose vejam dados. Além da criptografia de disco, recomendamos criptografar informações pessoais antes de armazená-las no banco de dados ou usar extensões ou recursos para ativar a criptografia no próprio banco de dados. Embora esses métodos possam afetar a usabilidade ou o desempenho, é uma boa prática assegurar-se de que as informações pessoais sejam protegidas com criptografia.

## Auto-scaling

Os recursos são escalados automaticamente com base no uso total de armazenamento em disco dos bancos de dados do Elasticsearch. O uso de memória baseia-se em uma razão de armazenamento provisionado em uma proporção de 1:10. Esses recursos são empacotados como unidades.

O Auto-scaling foi projetado para responder às tendências de curto a médio prazo de seu banco de dados. A cada hora, seu serviço será verificado e, se ele estiver sendo executado com pouco espaço em disco, mais unidades serão alocadas para a implementação. A realocação acontece uma vez em cada período de 24 horas. Se você estiver planejando executar operações pesadas de RAM que possam colocar um pico no uso de RAM usual ou quaisquer operações de dados que sobrecarregarão seu armazenamento atribuído, aumente a capacidade dos recursos de seu serviço manualmente primeiro para evitar atingir quaisquer limites de recurso que possam afetar as operações do cluster.

O Auto-scaling não reduz a capacidade de implementações em que o uso de disco/memória foi reduzido. Os recursos provisionados para seus bancos de dados permanecerão para suas necessidades futuras ou até você reduzir a capacidade de sua implementação manualmente.
{: .tip}
