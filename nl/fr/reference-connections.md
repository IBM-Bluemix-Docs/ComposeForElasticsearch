---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuration de connexion
{: #connection-configuration}

Les connexions à la base de données {{site.data.keyword.composeForElasticsearch_full}} sont gérées par 2 portails HAProxy. Chaque portail dispose de 64 Mo de mémoire.

Disposer de deux portails permet aux applications de conserver une connectivité lorsque l'un des portails n'est plus accessible. Le basculement côté client relève de la responsabilité du concepteur d'applications. Certains pilotes Elasticsearch gèrent automatiquement plusieurs chaînes de connexion et le basculement sans perte de données.

Si vous ne travaillez pas avec un pilote qui gère totalement les erreurs de basculement, procédez comme suit pour gérer vous-même les erreurs.

* Utilisez un pilote qui accepte plusieurs noeuds finaux dans sa configuration de connexion.
* Vérifiez que votre application réagit aux erreurs de façon appropriée lors de la lecture ou de l'écriture dans la base de données. Vous pouvez configurer votre application pour réagir aux erreurs de différentes manières, notamment :
  + Consignation de l'erreur et reconnexion.
  + Reconnexion et nouvel essai de l'opération à l'origine de l'erreur.
  + Mise en file d'attente de l'opération ayant échoué et planification d'une reconnexion.

## Chiffrement du transport

Tous les portails {{site.data.keyword.composeForElasticsearch}} HAProxy disposent de TLS/SSL et prennent en charge TLS version 1.2. Les certificats du service sont des certificats Let's Encrypt.

## Limites de connexion

Les deux portails HAProxy sont limités à 2000 connexions par portail. Si vous dépassez la limite de connexion, votre service peut ne plus répondre. Vous pouvez gérer les connexions à partir de votre application à l'aide de la fonction de regroupement de connexions de votre pilote.

## Délais d'attente de proxy

Les portails HAProxy gèrent le cycle de vie des connexions entre le serveur et le client. Les valeurs de délai d'attente sont détaillées à titre de référence pour les éditeurs de pilote et les personnes intéressées par les activités de bas niveau des connexions de base de données {{site.data.keyword.composeForElasticsearch}}.

Paramètre | Valeur | S'applique
----------|-----------|-----------
client | 1m | Lorsque le client doit accuser réception de données ou en envoyer.
connect | 9s | Lorsqu'une connexion est établie entre le proxy et le serveur.
server | 2m | Lorsque le serveur doit accuser réception de données ou en envoyer.
check | 5s | En tant que vérification secondaire du délai d'attente lorsqu'une connexion est établie entre le proxy et le serveur.
http-request | 5s | Délai maximum accordé pour l'achèvement d'une requête HTTP.
{: caption="Tableau 1. Délais d'attente des portails HAProxy Elasticsearch" caption-side="top"}
