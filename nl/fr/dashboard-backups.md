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

# Gestion des sauvegardes
{: #backups}

Vous pouvez créer et télécharger des sauvegardes à partir de l'onglet *Backups* du tableau de bord de votre service. Les sauvegardes planifiées comme manuelles sont disponibles.

## Affichage des sauvegardes existantes

Des sauvegardes quotidiennes de votre base de données sont automatiquement planifiées. Pour afficher vos sauvegardes existantes :

1. Accédez au tableau de bord de votre service.
2. Cliquez sur l'onglet **Backups** pour ouvrir la page _Backups_. La liste des sauvegardes disponibles s'affiche :

  ![Sauvegardes disponibles](./images/elastic_search-backups-show.png "Liste des sauvegardes disponibles.")

Cliquez sur la ligne correspondante pour développer les options de chaque sauvegarde disponible.

![Options de sauvegarde](./images/elastic_search-backups-options.png "Options d'une sauvegarde.") 

## Création d'une sauvegarde manuelle

Outre les sauvegardes planifiées, vous pouvez créer une sauvegarde manuelle. Pour créer une sauvegarde manuelle, suivez la procédure d'affichage des sauvegardes existantes, puis cliquez sur **Back up now** au-dessus de la liste des sauvegardes disponibles. Un message indiquant qu'une sauvegarde a été initiée s'affiche et une sauvegarde en attente (pending) est ajoutée à la liste des sauvegardes disponibles.

## Contenu de sauvegarde

Les sauvegardes {{site.data.keyword.composeForElasticsearch}} sont effectuées avec l'utilitaire d'instantané de l'API Elasticsearch. Le processus est exécuté sur l'intégralité du cluster sans blocage, de sorte que toutes les opérations d'indexation et de recherche se poursuivent normalement. Une sauvegarde à un point de cohérence est effectuée au moment de la création de l'instantané.

## Restauration d'une sauvegarde
Pour restaurer une sauvegarde sur une nouvelle instance de service, suivez la procédure d'affichage des sauvegardes, puis cliquez sur la ligne correspondante afin de développer les options de la sauvegarde que vous voulez télécharger. Cliquez sur le bouton **Restore**. Un message vous indiquant qu'une restauration a été initiée s'affiche. La nouvelle instance de service sera automatiquement nommée "elasticsearch-restore-[timestamp]" ; elle s'affiche dans votre tableau de bord au démarrage de la mise à disposition.
