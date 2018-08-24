---

Copyright:
  years: 2016,2018
lastupdated: "2018-05-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Présentation du tableau de bord

Vous pouvez gérer votre service {{site.data.keyword.composeForElasticsearch_full}} depuis son tableau de bord.

La page _Vue d'ensemble_ affiche des informations concernant la base de données Compose d'{{site.data.keyword.cloud}}. La vue d'ensemble contient des informations d'identification essentielles et indique l'utilisation actuelle des ressources. Vous y trouverez également une section qui contient les chaînes de connexion que vous pouvez utiliser pour la connexion à votre base de données.

## Deployment Details

Le panneau _Deployment Details_ affiche des détails concernant votre déploiement.

![Deployment Details](./images/elastic_search-deployment-details.png "Vue du panneau Deployment Details")

### Type

Type de base de données fourni par le service et version de base de données qu'utilise votre service. Lorsqu'une version plus récente de la base de données est disponible, une notification s'affiche, accompagnée d'un lien vers la section [Mise à niveau de la version](/docs/services/ComposeForElasticsearch/dashboard-settings.html#upgrade-version) du tableau de bord de votre service.

### ID

Identificateur interne du service.

### Utilisation

Taille de votre base de données et quantité de stockage fournie par le plan de service.


## Chaînes de connexion

Chaînes de connexion qui peuvent être utilisées par certaines bibliothèques client. Elles contiennent toutes les informations nécessaires pour la connexion des autres bibliothèques. Pour savoir comment utiliser une chaîne de connexion pour vous connecter à votre service, voir [Connexion d'une application externe](/docs/services/ComposeForElasticsearch/connecting-external.html).

Chaque chaîne de connexion pour votre service se trouve dans un onglet distinct du panneau _Chaînes de connexion_ panel.

### HTTPS

Chaîne de connexion au format URI qui peut être utilisée par certaines bibliothèques client. Elle contient toutes les informations requises pour la connexion d'autres bibliothèques. Pour savoir comment utiliser une chaîne de connexion pour vous connecter, voir [Connexion d'une application externe](/docs/services/ComposeForElasticsearch/connecting-external.html).

### Santé

Exemple d'appel que vous pouvez utiliser pour connaître l'état de santé du cluster Elasticsearch.

## API d'administration d'instance

Vous pouvez gérer votre service {{site.data.keyword.composeForElasticsearch}} via l'API {{site.data.keyword.cloud_notm}} Compose.

### Noeud final Foundation

Le noeud final Foundation est composé de la région où réside le service et de l'ID d'instance du service. Il se trouve au début de chaque noeud final.

### ID de déploiement

L'ID de déploiement est nécessaire pour la plupart des appels ; il identifie l'instance de déploiement spécifique.

### Référence

Pour plus de documentation et de référence sur l'utilisation de l'API {{site.data.keyword.cloud_notm}} Compose au sein de tous les services {{site.data.keyword.cloud_notm}} Compose, voir [API {{site.data.keyword.cloud_notm}} Compose](https://www.compose.com/articles/the-ibm-cloud-compose-api/).
