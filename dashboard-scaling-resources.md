---

Copyright:
  Years: 2017
lastupdated: "2017-08-30"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Scaling Resources

If your service needs additional storage, or you want to reduce the amount of storage allocated to your service, you can do this by scaling resources.

1. Navigate to your service's dashboard overview page.
2. In the _Deployment Details_ pane, click **Scale Resources**. The Scale Resouces page opens.
    ![The Scale Resources page](./images/scaling_resources.png "The Scale Resources page")
3. Adjust the slider to raise or lower the storage allocated to the {{site.data.keyword.composeForElasticsearch}} service. Move the slider to the left to reduce the amount of storage, or move it to the right to increase the storage.
4. Click **Scale Deployment** to trigger the rescaling and return to the dashboard overview. The _Recipes_ pane contains details of the rescaling job that is now in progress. When the scaling is complete the _Deployment Details_ pane updates to show the current usage and the new value for the available storage.