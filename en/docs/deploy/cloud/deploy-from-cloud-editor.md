---
title: Deploy from the cloud editor
description: Deploy a single integration or an entire project to WSO2 Cloud from the WSO2 Integrator IDE.
keywords: [wso2 integrator, wso2 cloud, deploy, cloud editor, integration platform]
---

import ThemedImage from '@theme/ThemedImage';
import useBaseUrl from '@docusaurus/useBaseUrl';

# Deploy from the cloud editor

Once your project is connected to a GitHub repository and the latest changes are pushed, you can deploy to WSO2 Cloud from the **Deploy Integration** tab in the WSO2 Integrator IDE. You can deploy a single integration on its own, or deploy every integration in the project together.

:::info Prerequisites
- The project is connected to a GitHub repository. See [Connect a Git repository](connect-git-repository.md).
- The latest changes are committed and pushed. See [Push from the IDE](push-from-ide.md).
:::

## Deploy a single integration

Use this flow when you only want to push one integration in the project to WSO2 Cloud.

1. Open the integration overview canvas for the integration you want to deploy.
2. In the **Deployment Options** panel, expand **Deploy to WSO2 Cloud** and click **Deploy**. The **Deploy Integration** tab opens.
3. Confirm that the **Source Control** card shows the repository and branch you pushed to.
4. Click **Deploy**.
5. After the deployment is triggered, the **Deploy to WSO2 Cloud** panel changes to **Deployed in WSO2 Cloud**.

   <ThemedImage
       alt="Deployed in WSO2 Cloud panel with View in Console button"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/deployed-in-cloud-view-console-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/deployed-in-cloud-view-console-dark.png'),
       }}
   />

6. Click **View in Console** to open the integration in the WSO2 Cloud console.

   <ThemedImage
       alt="WSO2 Cloud console showing the deployed integration"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/cloud-console-integration-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/cloud-console-integration-dark.png'),
       }}
   />

When the build completes, the integration is deployed to the development environment, where you can try it out.

## Deploy a whole project

Use this flow to deploy every integration in the project in a single step.

1. Open the project overview canvas. The **Integrations & Libraries** panel lists all integrations in the project.
2. In the **Deployment Options** panel, expand **Deploy to WSO2 Cloud** and click **Deploy**.

   <ThemedImage
       alt="Deploy to WSO2 Cloud panel in the project overview"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/deploy-button-project-overview-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/deploy-button-project-overview-dark.png'),
       }}
   />

3. The **Deploy Integration** tab opens with every integration in the project preselected. Clear the checkbox next to any integration you do not want to deploy.

   <ThemedImage
       alt="Deploy Integrations page with project integrations selected"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/deploy-integrations-selection-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/deploy-integrations-selection-dark.png'),
       }}
   />

4. Set up the GitHub repository the same way you would for a single integration. See [Connect a Git repository](connect-git-repository.md) and [Push from the IDE](push-from-ide.md).
5. When the **Source Control** card shows the repository and branch, click **Deploy All**.

   <ThemedImage
       alt="Deploy All button after the repository and branch are configured"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/deploy-all-with-git-config-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/deploy-all-with-git-config-dark.png'),
       }}
   />

Each integration appears as a separate component in the WSO2 Cloud console. When the builds complete, every selected integration is deployed to the development environment.

## What's next

- [Connect a Git repository](connect-git-repository.md) — Set up GitHub for a new project.
- [Push from the IDE](push-from-ide.md) — Commit and push changes before redeploying.
- [Environments](../../deploy/environments.md) — Manage development, staging, and production environments after the first deployment.
