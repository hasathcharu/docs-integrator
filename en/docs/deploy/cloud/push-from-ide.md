---
title: Deploy from the IDE
description: Push your integrations from the WSO2 Integrator IDE directly to WSO2 Cloud, either as a whole project or as a single integration.
keywords: [wso2 integrator, wso2 cloud, deploy, push, cloud deployment]
---

# Deploy to WSO2 Cloud from the IDE

You can deploy your integrations to WSO2 Cloud directly from the WSO2 Integrator IDE. You can deploy the entire project at once or deploy a single integration individually.

:::info Prerequisites
- An integration created in the WSO2 Integrator IDE.
- Your project connected to a remote Git repository. See [Connect a Git repository](../connect-git-repository).
:::

## Deploy the whole project

1. In the WSO2 Integrator IDE, open the project overview canvas.

    ![Project Overview](/img/deploy/cloud/push-from-ide/project-overview.png)

2. Under **Deployment Options** in the right column, locate the **Deploy to WSO2 Cloud** box and click **Deploy**.
3. If you are not already signed in to WSO2 Cloud, the IDE prompts you to sign in. Click **Sign In** and complete the authentication in the browser, then return to the IDE.
4. When prompted, select the organization on WSO2 Cloud. You can select an existing project or click **Create New** to create one.

   A new tab opens showing your project's integrations. By default, all integrations are selected for deployment.

    ![Deploying Integrations to WSO2 Cloud](/img/deploy/cloud/push-from-ide/deploy-tab.png)

5. If WSO2 Cloud does not have access to your remote repository, a warning appears. Click the link to grant access on GitHub, complete the authorization, then return to the IDE and click **Refresh** to validate access.
6. Click **Deploy All**.

   WSO2 Cloud creates the integrations. Once the deployment is complete, click **View in Console**.

A browser opens showing your project on WSO2 Cloud.

![WSO2 Cloud Project Home](/img/deploy/cloud/push-from-ide/project-page-wso2-cloud.png)

## Deploy a single integration

1. In the WSO2 Integrator IDE, open the integration overview canvas for the integration you want to deploy.
2. Under **Deployment Options** in the right column, locate the **Deploy to WSO2 Cloud** box and click **Deploy**.
3. Follow the same steps as the whole-project flow, and click **Deploy**.

   Once the deployment is complete, click **View in Console**.

A browser opens showing the deployed integration directly on WSO2 Cloud.
