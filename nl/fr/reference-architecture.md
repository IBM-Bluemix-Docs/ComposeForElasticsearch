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

# Architecture 
{: #architecture}


## Réplication
Un service {{site.data.keyword.composeForElasticsearch_full}} se compose de trois noeuds de données dans un cluster.  Vous sélectionnez la région où le service est déployé et les trois noeuds de données sont propagés dans les zones de disponibilité de la région. Le nombre de répliques est défini par défaut sur 1, ce qui vous donne deux copies de vos données. Elasticsearch équilibre automatiquement vos répliques dans le cluster. Lorsqu'un noeud de données n'est plus accessible, votre cluster continue à fonctionner normalement.
   
## Chiffrement de disque

Tous les services {{site.data.keyword.composeForElasticsearch}} disposent du chiffrement au repos. Vos données résident sur des serveurs où le chiffrement au niveau volume est activé. 

Etant donné que {{site.data.keyword.composeForElastcisearch}} est un service géré, les opérateurs {{site.data.keyword.cloud}} Compose peuvent voir les données. Outre le chiffrement du disque, il est recommandé de chiffrer vos informations personnelles avant de les stocker dans la base de données ou d'utiliser des extensions ou des fonctions permettant d'activer le chiffrement sur la base de données elle-même. Même si ces méthodes risquent d'avoir un impact sur la facilité d'utilisation ou les performances, il est conseillé de s'assurer que les informations personnelles sont protégées par un chiffrement.

## Mise à l'échelle automatique

Les ressources sont automatiquement mises à l'échelle en fonction de l'utilisation totale du stockage sur disque des bases de données Elasticsearch. L'utilisation de la mémoire est basée sur un rapport de stockage mis à disposition de 1 sur 10. Ces ressources sont intégrées en tant qu'unités.

La mise à l'échelle automatique est conçue pour répondre aux tendances à court et moyen termes de votre base de données. Votre service est contrôlé toutes les heures et, s'il est à court d'espace disque, des unités supplémentaires sont attribuées au déploiement. La réaffectation des ressources s'effectue une fois par 24 heures. Si vous prévoyez d'exécuter des opérations gourmandes en mémoire RAM qui risquent de générer un pic d'utilisation de la RAM, ou des opérations de données qui dépasseront la capacité de stockage alloué, nous vous recommandons de procéder d'abord à une mise à l'échelle manuelle des ressources de votre service afin d'éviter tout dépassement des limites de ressource qui pourrait affecter le fonctionnement du cluster.

Une mise à l'échelle automatique ne réduit pas la taille des déploiements où l'utilisation du disque/de la mémoire à diminué. Les ressources mises à disposition de vos bases de données sont conservées pour les besoins ultérieurs ou jusqu'à ce que vous réduisiez manuellement votre déploiement.
{: .tip}
