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

# Fazendo upgrade do Elasticsearch
{: #upgrading}

Para manter seu serviço seguro e atualizado, novas versões do mecanismo Elasticsearch subjacentes a seu {{site.data.keyword.composeForElasticsearch_full}} serão liberadas. Recomenda-se fazer upgrade conforme as novas versões se tornam disponíveis. Quando uma nova versão do {{site.data.keyword.composeForElasticsearch}} estiver disponível, uma notificação aparecerá na página _Visão geral_.

## Versões secundárias
Em quase todos os casos, os upgrades de versões secundárias estarão disponíveis no local. Será possível fazer upgrade do Elasticsearch sem migrar nenhum dado e com tempo de inatividade mínimo. É possível fazer upgrade na guia _Configurações_ de seu serviço selecionando a versão para a qual você gostaria de fazer upgrade no menu suspenso.

## Principais versões
Atualmente, há três versões principais do Elasticsearch disponíveis para o {{site.data.keyword.composeForElasticsearch}}: 2.4.6, 5.x e 6.x. Atualmente, é possível:
- migrar do Elasticsearch 5.x para o 6.x
- migrar do Elasticsearch 2.x para o 5.x.

O processo de migração entre todas as versões principais usa um backup recente ou on demand para restaurar os dados em uma nova implementação. Esse processo fornece uma série de vantagens:

- O banco de dados original permanece em execução e o trabalho de produção pode ficar sem interrupção.
- É possível testar o novo banco de dados fora de produção e tomar ações com relação a quaisquer incompatibilidades do aplicativo.
- O processo inteiro pode ser executado novamente a qualquer momento.
- Uma restauração nova reduz a probabilidade de artefatos desnecessários da versão mais antiga do banco de dados serem transportados para o novo banco de dados.

### Considerações de migração

A maioria das versões principais inclui algumas mudanças que podem afetar como você usa o Elasticsearch. Isso é especialmente verdadeiro ao migrar para o Elasticsearch 6.x. Mantenha esses fatos em mente antes de fazer upgrade, ao testar a nova versão e antes de desprovisionar sua versão antiga.

#### Índices pré-5.x
O Elasticsearch geralmente suporta a leitura de índices que foram criados na versão principal anterior. Isso significa que quaisquer índices criados no Elasticsearch 5.x podem ser lidos pelo Elasticsearch 6.x.

Se você tiver índices que foram criados no Elasticsearch 2.x, eles **não** serão legíveis no Elasticsearch 6.x. Esses índices precisarão ser reindexados no Elasticsearch 5.x antes da migração para o 6.x. Se os seus dados contiverem índices criados no Elasticsearch 2.x e você tentar migrar para o Elasticsearch 6.x, os nós falharão ao iniciar e a migração falhará.

[A reindexação pode ser feita no local](https://www.elastic.co/guide/en/elasticsearch/reference/current/reindex-upgrade-inplace.html) no Elasticsearch 5.x antes da migração para o 6.x por meio da [API Reindex](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-reindex.html).
{: .tip}

#### Mapeamentos de índice

A capacidade de criar múltiplos tipos de mapeamento por índice foi removida no Elasticsearch 6.x. Quaisquer mapeamentos múltiplos de índice criados no Elasticsearch 5.x continuarão a funcionar, mas o Elasticsearch 6.x suporta somente a criação de um único tipo de mapeamento por índice.

Os tipos de mapeamento serão removidos no Elasticsearch 7.x e acima.

#### Outras Alterações

A documentação completa de alterações que afetam o processamento da mensagem pode ser localizada nos [Docs do Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/breaking-changes-6.0.html).

### Migrando para uma nova versão

Para migrar para uma nova versão do Elasticsearch, será necessária a [CLI do {{site.data.keyword.cloud_notm}}](https://console.{DomainName}/docs/cli/index.html#overview). O processo é muito semelhante à [restauração de backup por meio da CLI](./dashboard-backups.html#restoring-via-cli), exceto que o campo "db_version" será usado no JSON para especificar para qual versão você está fazendo upgrade. O comando completo é:

``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": ""$BACKUP_ID", "db_version":"$VERSION_NUMBER" }'
```

Por exemplo, restaurar um serviço {{site.data.keyword.composeForElasticsearch}} que executa o 5.x para um novo serviço que executa o Elasticsearch 6.2.2 é semelhante a:

```
ibmcloud service create compose-for-elasticsearch Standard migrated_elastic -c '{ "source_service_instance_id": "0269e284-dcac-4618-89a7-f79e3f1cea6a", "backup_id":"5a96d8a7e16c090018884566", "db_version":"6.2.2"  }'
```
