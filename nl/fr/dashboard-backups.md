---

copyright:
  years: 2016,2018
lastupdated: "2018-04-19"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestion des sauvegardes
{: #backups}

Vous pouvez créer et restaurer des sauvegardes à partir de l'onglet _Backups_ de la page de gestion du tableau de bord de votre service. Vous avez le choix entre les sauvegardes quotidiennes, hebdomadaires, mensuelles et à la demande. Elles sont conservées selon la planification suivante :

Type de sauvegarde|Planification de conservation
----------|-----------
Quotidienne|Les sauvegardes quotidiennes sont conservées pendant 7 jours
Hebdomadaire|Les sauvegardes hebdomadaires sont conservées pendant 4 semaines
Mensuelle|Les sauvegardes mensuelles sont conservées pendant 3 mois
A la demande|Une seule sauvegarde à la demande est conservée. Il s'agit toujours de la dernière sauvegarde à la demande effectuée.
{: caption="Tableau 1. Planification de conservation des sauvegardes" caption-side="top"}

## Affichage des sauvegardes existantes

Des sauvegardes quotidiennes de votre base de données sont automatiquement planifiées. Vous pouvez afficher les sauvegardes existantes à partir du tableau de bord de votre service.

1. Accédez au tableau de bord de votre service.
2. Cliquez sur l'onglet **Backups** pour ouvrir la page _Backups_. La liste des sauvegardes disponibles s'affiche :

  ![Sauvegardes disponibles](./images/elastic_search-backups-show.png "Liste des sauvegardes disponibles.")

Cliquez sur la ligne correspondante pour développer les options de chaque sauvegarde disponible.
  ![Options de sauvegarde](./images/elastic_search-backups-options.png "Options d'une sauvegarde.") 

### Utilisation de l'API pour afficher les sauvegardes existantes

Une liste des sauvegardes est disponible sur le noeud final  `GET /2016-07/deployments/:id/backups`. L'ID d'instance de service et l'ID de déploiement du noeud final Foundation s'affichent dans la page _Vue d'ensemble_ du service. Par exemple : 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```  

## Création d'une sauvegarde manuelle

Pour créer une sauvegarde manuelle, suivez la procédure d'affichage des sauvegardes existantes, puis cliquez sur **Back up now** au-dessus de la liste des sauvegardes disponibles. Un message indiquant qu'une sauvegarde a démarré s'affiche et une sauvegarde en attente (pending) est ajoutée à la liste des sauvegardes disponibles.

### Utilisation de l'API pour la création d'une sauvegarde

Envoyez une demande POST au noeud final des sauvegardes pour initier une sauvegarde manuelle : `POST /2016-07/deployments/:id/backups`. Elle renvoie immédiatement l'ID de la recette et des informations sur la sauvegarde lors de son exécution. Avant d'utiliser la sauvegarde, vous devez devez contrôler le noeud final des sauvegardes afin de vérifier que la sauvegarde est terminée et rechercher la valeur `backup_id` avant de l'utiliser.

```
GET /2016-07/deployments/:id/backups/
```

## Restauration d'une sauvegarde

1. Pour afficher les sauvegardes existantes :
2. Cliquez sur la ligne correspondante pour développer les options de la sauvegarde à restaurer.
3. Cliquez sur le bouton **Restore**. Un message vous indiquant qu'une restauration a démarré s'affiche. La nouvelle instance de service s'affiche dans le tableau de bord au démarrage de la mise à disposition sous le nom `elasticsearch-restore-[timestamp]`.

Lors d'une restauration à partir d'une sauvegarde, vos données sont restaurées dans la version secondaire la plus récente disponible pour {{site.data.keyword.composeForElasticsearch}}. Vous pouvez redéfinir ce paramétrage en effectuant la restauration à l'aide de l'interface de ligne de commande {{site.data.keyword.cloud_notm}} qui permet d'indiquer dans quelle version restaurer.

**Remarque :** vous ne pouvez restaurer que dans une version disponible pour mise à disposition.

### Restauration à l'aide de l'interface de ligne de commande

Procédez comme suit pour restaurer une sauvegarde d'un service Elasticsearch en cours d'exécution vers un nouveau service Elasticsearch à l'aide de l'interface de ligne de commande {{site.data.keyword.cloud_notm}}. 
1. Au besoin, [téléchargez et installez l'interface de ligne de commande](https://console.{DomainName}/docs/cli/index.html#overview). 
2. Recherchez la sauvegarde à restaurer dans la page _Sauvegardes_ de votre service et copiez son ID.  
  **Ou**  
  Utilisez `GET /2016-07/deployments/:id/backups` pour rechercher une sauvegarde et son ID via l'API Compose. Le noeud final Foundation et l'ID d'instance de service s'affichent tous les deux dans la page de vue d'ensemble du service. Par exemple : 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
  La réponse inclut la liste de toutes les sauvegardes disponibles pour cette instance de service. Choisissez la sauvegarde à restaurer et copiez son ID.

3. Connectez-vous avec le compte et les données d'identification appropriés. `ibmcloud login` (ou `ibmcloud login -help` pour afficher toutes les options de connexion).

4. Accédez à votre organisation et votre espace : `ibmcloud target -o "$YOUR_ORG" -s "YOUR_SPACE"`

5. Utilisez la commande `service create` pour mettre à disposition un nouveau service et indiquez le source source et la sauvegarde spécifique que vous restaurez dans un objet JSON. Par exemple :
``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
  _SERVICE_ doit avoir pour valeur `compose-for-elasticsearch` et _PLAN_ doit avoir pour valeur Standard ou Enterprise, en fonction de votre environnement. Pour _SERVICE\_INSTANCE\_NAME_, indiquez le nom de votre nouveau service. _source\_service\_instance\_id_ correspond à l'ID d'instance de service de la source de la sauvegarde ; pour l'obtenir, exécutez `ibmcloud cf service DISPLAY_NAME --guid` où _DISPLAY\_NAME_ est le nom du service à l'origine de la sauvegarde. 

  Un paramètre JSON facultatif, "db_version", permet d'indiquer dans quelle version d'Elasticsearch effectuer la restauration. Ce paramètre est également utilisé pour [effectuer une mise à niveau vers une version principale d'Elasticsearch](./upgrading.html).
  
  Les utilisateurs Enterprise doivent également indiquer le cluster dans lequel effectuer le déploiement dans l'objet JSON grâce au paramètre `"cluster_id": "$CLUSTER_ID"`.

