---

copyright:
  years: 2016,2020
lastupdated: "2020-04-13"

subcollection: ComposeForElasticsearch

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}


# Getting started tutorial
{: #getting-started}

{{site.data.keyword.composeForElasticsearch_full}} is no longer the latest service offering. The next generation is {{site.data.keyword.databases-for-elasticsearch_full}}. If you were considering a {{site.data.keyword.composeForElasticsearch}} deployment, please look into provisioning [{{site.data.keyword.databases-for-elasticsearch}}](/docs/databases-for-elasticsearch?topic=databases-for-elasticsearch-getting-started) instead.
{: .important}

The getting started tutorial uses a [sample app](https://github.com/IBM-Cloud/compose-elasticsearch-helloworld-nodejs) to demonstrate how to use Node.js to connect to an {{site.data.keyword.composeForElasticsearch_full}} service by using the provided credentials. The application uses a web interface to create, read from, and write data to a database.
{: shortdesc}

## Before you begin

Make sure that you have an [{{site.data.keyword.cloud_notm}} account][ibm_cloud_signup_url]{:new_window}.

You also need to install [Node.js](https://nodejs.org/) and [Git](https://git-scm.com/downloads).

## Step 1. Create a {{site.data.keyword.composeForElasticsearch}} service instance
{: #create-service}

You can create a {{site.data.keyword.composeForElasticsearch}} service from the [{{site.data.keyword.composeForElasticsearch}} page](https://{DomainName}/catalogcompose-for-elasticsearch/) in the {{site.data.keyword.cloud_notm}} catalog.

Choose a service name, region, organization and space to provision the service in, and for the **Select a database version** field, choose _Latest Preferred Version_.

Next, choose a pricing plan for your service. You can choose the *Standard* or *Enterprise* plans. With the *Enterprise* plan, you can provision your {{site.data.keyword.composeForElasticsearch}} instance into an available {{site.data.keyword.composeEnterprise}} cluster. {{site.data.keyword.composeEnterprise}} provides the security and isolation that is required by enterprise compliance and uses dedicated networking to ensure the performance of the deployed databases. See the [{{site.data.keyword.composeEnterprise}}](/docs/ComposeEnterprise?topic=compose-enterprise-about) documentation for more details.

## Step 2. Clone the Hello World sample app from GitHub

Clone the Hello World app to your local environment from your terminal by using the following command:

```
git clone https://github.com/IBM-Cloud/compose-elasticsearch-helloworld-nodejs.git
```

## Step 3. Install the app dependencies

Use npm to install dependencies.

1. From your terminal, change the directory to where the sample app is located.
  
  ```
  cd compose-elasticsearch-helloworld-nodejs
  ```

2. Install the dependencies listed in the `package.json` file.
  
  ```
  npm install
  ```

## Step 4. Download and install the {{site.data.keyword.cloud_notm}} CLI tool

The {{site.data.keyword.cloud_notm}} CLI tool is what you use to communicate with {{site.data.keyword.cloud_notm}} from your terminal or command line. For more information, see [Download and install {{site.data.keyword.cloud_notm}} CLI](https://{DomainName}/docs/cli/reference/bluemix_cli/download_cli.html).

## Step 5. Connect to {{site.data.keyword.cloud_notm}}

1. Connect to {{site.data.keyword.cloud_notm}} in the command line tool and follow the prompts to log in.

  ```
  ibmcloud login
  ```

  If you have a federated user ID, use the `ibmcloud login --sso` command to log in with your single sign-on ID. See [Logging in with a federated ID](https://{DomainName}/docs/cli/login_federated_id.html#federated_id) to learn more.
  {: tip}

2. Make sure you're targetting the correct {{site.data.keyword.cloud_notm}} org atargeting
  ```
  ibmcloud target --cf
  ```

  Choose from the options provided, by using the same values that you used when you created the service.

## Step 6. Update the app's manifest file
{: #update-manifest}

{{site.data.keyword.cloud_notm}} uses a manifest file - `manifest.yml` to associate an application with a service. Follow these steps to create your file.

1. In an editor, open a new file and add the following text:
```
  ---
  applications:
  - name: compose-elasticsearch-helloworld-nodejs
    routes:
    - route: compose-elasticsearch-helloworld-nodejs.us-south.cf.appdomain.cloud
    memory:  128M
    services:
      - example-compose-for-elasticsearch
```

2. Change the `route` value to something unique. The route that you choose determines the subdomain of your application's URL: `<route>.{region}.cf.appdomain.cloud`. Be sure the `{region}` matches where your application is deployed.
3. Change the `name` value. The value that you choose is displayed in your {{site.data.keyword.cloud_notm}} dashboard.
4. Update the `services` value to match the name of the service you created in [Create a {{site.data.keyword.composeForElasticsearch}} service instance](#create-service). 

## Step 7. Push the app to {{site.data.keyword.cloud_notm}}.

This step fails if the service provisioning from [Step 1](#create-service) is not complete. You can check its progress by going to the _Manage_ view for the service.
{: tip}

When you push the app, it is automatically bound to the service specified in the manifest file.

```
ibmcloud cf push
```

## Step 8. Check that the app is connected to your {{site.data.keyword.composeForElasticsearch}} service

1. Navigate to your {{site.data.keyword.composeForElasticsearch}} service dashboard
2. Select _Connections_ from the dashboard menu. Your application should be listed under _Connected Applications_.

If your application is not listed, repeat Steps 7 and 8, making sure that you enter the correct details in [manifest.yml](#update-manifest).

## Step 9. Use the app

When you visit `<route>.{region}.cf.appdomain.cloud/`, you can view the contents of your {{site.data.keyword.composeForElasticsearch}} collection. As you add words and their definitions, they are added to the database and displayed. If you stop and restart the app, you'll see any words and definitions you've already added are now listed.

## Running the app locally

Instead of pushing the app into {{site.data.keyword.cloud_notm}} you can run it locally to test the connection to your {{site.data.keyword.composeForElasticsearch}} service instance. To connect to the service, you need to create a set of service credentials.

1. From your {{site.data.keyword.cloud_notm}} dashboard, open your {{site.data.keyword.composeForElasticsearch}} service instance.
2. Select _Service Credentials_ from the main menu to open the Service Credentials view.
3. Click **New Credential**.
4. Choose a name for your credentials and click **Add**.
5. Your new credentials are now listed. Click **View credentials** in the corresponding row of the table to view the credentials, and click the **Copy** icon to copy your credentials.
6. In your editor of choice, create a new file with the following, inserting your credentials.

```
{
  "services": {
    "compose-for-elasticsearch": [
      {
        "credentials": INSERT YOUR CREDENTIALS HERE
      }
    ]
  }
}
```
6. Save the file as `vcap-local.json` in the directory where the sample app is located.

To avoid accidentally exposing your credentials when you push an application to GitHub or {{site.data.keyword.cloud_notm}}, you should make sure that the file that contains your credentials is listed in the relevant ignore file. If you open `.cfignore` and `.gitignore` in your application directory you'll see that `vcap-local.json` is listed in both, so it won't be included in the files that are uploaded when you push the app to either GitHub or {{site.data.keyword.cloud_notm}}.
{: tip}

Now start the local server.

```
npm start
```

The app is now running at [http://localhost:8080](http://localhost:8080). You can add words and definitions to your {{site.data.keyword.composeForElasticsearch}} database. When you stop and restart the app, any words you have already added are displayed when you refresh the page.

## Next steps

To understand more about how the [compose-elasticsearch-helloworld-nodejs](https://github.com/IBM-Cloud/compose-elasticsearch-helloworld-nodejs) sample app works, you can read the application's readme file, or the code comments in `server.js`, which give some information about the app's functions.

To start exploring your {{site.data.keyword.composeForElasticsearch}} service, see the following topics about the service dashboard:

- [Dashboard Overview](/docs/ComposeForElasticsearch?topic=ComposeForElasticsearch-dashboard-overview)
- [Backups](/docs/ComposeForElasticsearch?topic=ComposeForElasticsearch-dashboard-backups)
- [Settings](/docs/ComposeForElasticsearch?topic=ComposeForElasticsearch-dashboard-settings)

For more information about the credentials you created for the application to connect to your service, see a list of the available [credentials](/docs/ComposeForElasticsearch?topic=ComposeForElasticsearch-ibmcloud-cf-app#credentials).

[ibm_cloud_signup_url]: https://ibm.biz/compose-for-elasticsearch-signup

