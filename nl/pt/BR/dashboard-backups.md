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

# Gerenciando backups
{: #backups}

É possível criar e restaurar backups por meio da guia _Backups_ da página _Gerenciar_ de seu painel de serviço. Backups diários, mensais e on demand estão disponíveis. Eles são retidos de acordo com planejamento a seguir:

Tipo de backup|Planejamento de retenção
----------|-----------
Diárias|Os backups diários são retidos por 7 dias
Semanal|Backups semanais são retidos por 4 semanas
Mensal|Backups mensais são retidos por 3 meses
On demand|Um backup on demand é retido. O backup retido é sempre o backup on demand mais recente.
{: caption="Tabela 1. Planejamento de retenção de backup" caption-side="top"}

## Visualizando backups existentes

Os backups diários de seu banco de dados são planejados automaticamente. É possível visualizar os backups existentes no painel de serviço.

1. Navegue para seu Painel de serviço.
2. Clique em **Backups** nas guias para abrir a página _Backups_. Uma lista de backups disponíveis é mostrada:

  ![Available backups](./images/elastic_search-backups-show.png "A list of available backups.")

Clique na linha correspondente para expandir as opções de qualquer backup disponível.
  ![Opções de backup](./images/elastic_search-backups-options.png "Opções para um backup.") 

### Usando a API para visualizar backups existentes

Uma lista de backups está disponível no terminal `GET /2016-07/deployments/:id/backups`. O Terminal base com o ID da instância de serviço e o ID de implementação são mostrados na _Visão geral_ do serviço. Por exemplo: 
``` 
https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
```  

## Criando um backup manual

Para criar um backup manual, siga as etapas para visualizar os backups existentes, em seguida, clique em **Fazer backup agora** acima da lista de backups disponíveis. É exibida uma mensagem para permitir que você saiba que um backup foi iniciado e que um backup 'pendente' foi incluído na lista de backups disponíveis.

### Usando a API para criar um backup

Envie uma solicitação de POST para o terminal de backups para iniciar um backup manual: `POST /2016-07/deployments/:id/backups`. Ele retorna imediatamente com o ID da orientação e informações sobre o backup conforme ele está em execução. Antes de usar o backup, é necessário verificar o terminal de backups para ver se o backup foi concluído e localizar o valor `backup_id` antes de usá-lo.

```
GET /2016-07/deployments/:id/backups/
```

## Restaurando um backup

1. Siga as etapas para visualizar backups existentes.
2. Clique na linha correspondente para expandir as opções do backup que você deseja restaurar.
3. Clique no botão **Restaurar**. É exibida uma mensagem para permitir que você saiba que uma restauração foi iniciada. A nova instância de serviço aparece no painel quando o fornecimento inicia e tem o nome gerado `elasticsearch-restore-[timestamp]`.

Ao restaurar de um backup, seus dados serão restaurados para a versão secundária mais recente disponível para o {{site.data.keyword.composeForElasticsearch}}. É possível substituir essa configuração restaurando por meio da CLI do {{site.data.keyword.cloud_notm}} e enviando a versão para a qual deseja restaurar.

**Nota:** é possível restaurar somente para uma versão que esteja disponível para fornecimento.

### Restaurando via CLI

Use as etapas a seguir para restaurar um backup de um serviço do Elasticsearch em execução para um novo serviço do Elasticsearch usando a CLI do {{site.data.keyword.cloud_notm}}. 
1. Se for necessário, [faça download e instale a CLI](https://console.{DomainName}/docs/cli/index.html#overview). 
2. Localize o backup que você gostaria de restaurar na página _Backups_ em seu serviço e copie o ID de backup.  
  **Ou**  
  Use o `GET /2016-07/deployments/:id/backups` para localizar um backup e seu ID por meio da API do Compose. O Terminal base e o ID da instância de serviço são ambos mostrados na _Visão geral_ do serviço. Por exemplo: 
  ``` 
  https://composebroker-dashboard-public.mybluemix.net/api/2016-07/instances/$INSTANCE_ID/deployments/$DEPLOYMENT_ID/backups
  ```  
  A resposta inclui uma lista de todos os backups disponíveis para essa instância de serviço. Selecione o backup do qual você deseja restaurar e copie seu ID.

3. Efetue login com a conta e as credenciais apropriadas. `ibmcloud login` (ou `ibmcloud login -help` para ver todas as opções de login).

4. Alterne para a sua Organização e Espaço `ibmcloud target -o "$YOUR_ORG" -s "YOUR_SPACE"`

5. Use o comando `service create` para provisionar um novo serviço e forneça o serviço de origem e o backup específico que você está restaurando em um objeto JSON. Por exemplo:
``` 
ibmcloud service create SERVICE PLAN SERVICE_INSTANCE_NAME -c '{"source_service_instance_id": "$SERVICE_INSTANCE_ID", "backup_id": "$BACKUP_ID" }'
```
  O campo _SERVICE_ deve ser `compose-for-elasticsearch` e o campo _PLAN_ deve ser Padrão ou Corporativo, dependendo de seu ambiente. _SERVICE\_INSTANCE\_NAME_ é onde você colocará o nome para o seu novo serviço. O _source\_service\_instance\_id_ é o ID da instância de serviço da origem do backup; ele pode ser obtido executando `ibmcloud cf service DISPLAY_NAME --guid`, em que _DISPLAY\_NAME_ é o nome do serviço do qual o backup é feito. 

  Há um parâmetro JSON opcional "db_version" caso seja necessário especificar para qual versão do Elasticsearch restaurar. Esse parâmetro também é usado para [fazer upgrade para uma versão principal do Elasticsearch](./upgrading.html).
  
  Os usuários corporativos também precisam especificar qual cluster implementar no objeto JSON com o parâmetro `"cluster_id": "$CLUSTER_ID"`.

