---

copyright:
  years: 2016,2018
lastupdated: "2017-10-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# A propos de {{site.data.keyword.composeForElasticsearch}}
{: #about-compose-for-elasticsearch}

{{site.data.keyword.composeForElasticsearch_full}} allie la puissance d'un moteur de recherche en texte intégral à la puissance d'indexation d'une base de données de documents JSON. Combinées, ces deux fonctions constituent un outil puissant dédié aux analyses de données enrichies sur d'importantes quantités de données. Elasticsearch vous permet de définir des scores d'exactitude pour vos recherches, et ainsi, de parcourir votre fichier à la recherche de concordances parfaites et de concordances approximatives que vous pourriez manquer.
{:shortdesc}

**Remarque :** les instances de service Compose qui ont été mises à disposition avant le 14 septembre 2016 et qui sont toujours actives peuvent encore être utilisées et sont directement accessibles sur le site [https://www.compose.com/](https://www.compose.com). Les instances de service Compose mises à disposition depuis cette date sont directement accessibles et utilisables dans votre compte {{site.data.keyword.cloud}}.

## Création d'une instance de service {{site.data.keyword.composeForElasticsearch}}

Vous pouvez créer un service {{site.data.keyword.composeForElasticsearch}} à partir de la [page {{site.data.keyword.composeForElasticsearch}}](https://console.{DomainName}/catalog/services/compose-for-elasticsearch/) du catalogue {{site.data.keyword.cloud_notm}}.

Choisissez un nom de service ainsi qu'une région, une organisation et un espace dans lesquels mettre le service à disposition. Vous pouvez utiliser la zone **Select a database version** pour sélectionner parmi les versions de base de données disponibles.

Lorsque vous mettez votre instance {{site.data.keyword.composeForElasticsearch}} à disposition, vous avez le choix entre le plan *Standard* et le plan *Entreprise*. Avec le plan *Entreprise*, vous pouvez mettre votre instance {{site.data.keyword.composeForElasticsearch}} à disposition dans un cluster {{site.data.keyword.composeEnterprise}} disponible. {{site.data.keyword.composeEnterprise}} offre la sécurité et l'isolement requis par les normes de conformité des entreprises et utilise un réseau dédié pour garantir les performances des bases de données déployées. Pour plus d'informations, voir la [documentation Compose - Entreprise](../ComposeEnterprise/index.html).

## Gestion de {{site.data.keyword.composeForElasticsearch}}

Vous pouvez gérer votre service depuis son tableau de bord. Vous y trouverez des informations concernant la base de données Compose d'{{site.data.keyword.cloud}} et la manière de vous y connecter. Vous pouvez également :

- gérer vos sauvegardes ;
- allouer plus de ressources à votre service ; 
- constituer des listes blanches pour limiter l'accès à vos bases de données.

Pour plus d'informations, voir [Paramètres](./dashboard-settings.html).

## Connexion à {{site.data.keyword.composeForElasticsearch}}

Vous pouvez vous connecter à votre service avec les données d'identification créées en même temps que le service ou avec les chaînes de connexion et la ligne de commande fournies dans l'onglet *Vue d'ensemble* du tableau de bord de votre service.

## Connexion d'une application {{site.data.keyword.cloud_notm}} à {{site.data.keyword.composeForElasticsearch}}

Pour connecter une application {{site.data.keyword.cloud_notm}} à votre service, utilisez les données d'identification créées en même temps que le service. Pour plus d'informations sur la connexion d'une application {{site.data.keyword.cloud_notm}} à un service {{site.data.keyword.composeForElasticsearch}}, voir [Connexion d'une application {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Connexion à {{site.data.keyword.composeForElasticsearch}} depuis l'extérieur d'{{site.data.keyword.cloud_notm}}

Pour vous connecter à {{site.data.keyword.composeForElasticsearch}} depuis l'extérieur d'{{site.data.keyword.cloud_notm}}, vous pouvez utiliser les chaînes de connexion ou la ligne de commande fournies. Pour plus d'informations sur ce type de connexion, voir [Connexion d'une application externe](./connecting-external.html).
