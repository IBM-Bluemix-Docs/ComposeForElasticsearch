---

copyright:
  years: 2016,2018
lastupdated: "2018-07-25"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Mise à niveau d'Elasticsearch
{: #upgrading}

Pour que votre service soit toujours sécurisé et à jour, de nouvelles versions du moteur Elasticsearch sous-jacent à {{site.data.keyword.composeForElasticsearch_full}} seront publiées. Il est recommandé d'effectuer une mise à niveau à mesure de la disponibilité des nouvelles versions. Lorsqu'une nouvelle version de {{site.data.keyword.composeForElasticsearch}} est disponible, une notification s'affiche sur la page _Vue d'ensemble_.

## Versions secondaires
Dans la plupart des cas, les mises à niveau de version secondaire sont disponibles en interne. Vous pourrez procéder à la mise à niveau d'Elasticsearch sans migrer de données et sans temps d'indisponibilité minimal. Vous pouvez effectuer une mise à niveau depuis l'onglet _Paramètres_ de votre service en sélectionnant dans le menu déroulant la version vers laquelle mettre à niveau.

## Versions principales
Trois versions principales d'Elasticsearch sont actuellement disponibles pour {{site.data.keyword.composeForElasticsearch}}: 2.4.6, 5.x et 6.x. Vous pouvez :
- migrer depuis Elasticsearch 5.x vers la version 6.x
- migrer depuis Elasticsearch 2.x vers la version 5.x.

Le processus de migration entre les différentes versions principales utilise une sauvegarde récente ou à la demande pour restaurer les données dans un nouveau déploiement. Ce processus présente un les avantage suivants :

- La base de données d'origine reste opérationnelle et le travail de production n'est pas interrompu.
- Vous pouvez tester la nouvelle base de données hors production et prendre des mesures concernant les incompatibilités liées aux applications.
- L'intégralité du processus peut être réexécuté à partir de n'importe quel stade.
- Une restauration dynamique évite que des artefacts inutiles de l'ancienne version de la base de données soient transmis à la nouvelle base de données.

### Considérations concernant la migration

La plupart des versions principales incluent des modifications susceptibles d'affecter votre manière d'utiliser Elasticsearch. Cette remarque touche essentiellement la migration vers Elasticsearch 6.x. Gardez ces informations à l'esprit avant d'effectuer une mise à niveau, lorsque vous testez la nouvelle version et avant d'annuler l'accès à l'ancienne version.

#### Index antérieurs à 5.x
Elasticsearch prend généralement en charge la lecture des index créés dans la version principale précédente. Cela signifie que tout index créé dans Elasticsearch 5.x est lisible par Elasticsearch 6.x.

Si vous avez des index créés dans Elasticsearch 2.x, ils ne seront **pas** lus dans Elasticsearch 6.x. Ces index devront être réindexés dans Elasticsearch 5.x avant migration vers 6.x. Si vos données contiennent des index créés dans Elasticsearch 2.x et que vous essayez de migrer vers Elasticsearch 6.x, les noeuds ne démarreront pas et la migration échouera.

[La réindexation peut être effectuée en interne](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html) dans Elasticsearch 5.x avant migration vers 6.x à l'aide de l'[API Reindex](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html).
{: .tip}

#### Mappages d'index

La possibilité de créer plusieurs types de mappage par index a été supprimée dans Elasticsearch 6.x. Les mappages d'index multiples créés dans Elasticsearch 5.x fonctionnent toujours, mais Elasticsearch 6.x prend uniquement en charge la création d'un seul type de mappage par index.

Les types de mappage seront supprimés dans Elasticsearch 7.x et les versions ultérieures.

#### Autres modifications

La documentation complète relative aux toutes dernières modifications est disponible dans [Elasticsearch docs](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/breaking-changes-6.0.html).

### Migration vers une nouvelle version

Pour migrer vers une nouvelle version d'Elasticsearch vous avez besoin de l'interface de ligne de commande [{{site.data.keyword.cloud_notm}}](https://console.{DomainName}/docs/cli/index.html#overview). Le processus est très semblable à une [restauration de sauvegarde à l'aide de l'interface de ligne de commande](./dashboard-backups.html#restoring-via-cli), sauf que vous utilisez la zone "db_version" de JSON pour indiquer vers quelle version vous effectuez une mise à niveau. La commande complète est :

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

Par exemple, la restauration d'un service {{site.data.keyword.composeForElasticsearch}} qui exécute la version 5.x vers un nouveau service exécutant Elasticsearch version 6.2.2 est similaire à l'exemple suivant :

```
ibmcloud service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
```
