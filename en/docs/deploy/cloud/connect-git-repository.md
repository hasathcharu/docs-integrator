---
title: Connect a Git repository
description: Publish your WSO2 Integrator project to GitHub so it can be pushed to WSO2 Cloud.
keywords: [wso2 integrator, wso2 cloud, github, git, source control, deploy]
---

import ThemedImage from '@theme/ThemedImage';
import useBaseUrl from '@docusaurus/useBaseUrl';

# Connect a Git repository

WSO2 Cloud builds and runs your integration from a connected Git repository. Before you can push to cloud, the project folder open in the WSO2 Integrator IDE must be backed by a GitHub repository. This page walks through publishing your project to GitHub from inside the IDE.

:::info Prerequisites
- An integration created in the WSO2 Integrator IDE.
- A GitHub account.
:::

## Open the deploy view

1. In the WSO2 Integrator IDE, open the integration overview canvas.
2. In the **Deployment Options** panel, expand **Deploy to WSO2 Cloud** and click **Deploy**.

   <ThemedImage
       alt="Deploy to WSO2 Cloud panel in the integration overview"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/deploy-button-integration-overview-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/deploy-button-integration-overview-dark.png'),
       }}
   />

   A new **Deploy Integration** tab opens. If the project folder is not yet a Git repository, the tab shows a notice asking you to initialize one before you can continue.

## Publish the project to GitHub

1. In the **Deploy Integration** tab, click **Source Control** to open the **Source Control** side bar.
2. In the side bar, click **Publish to GitHub**.

   <ThemedImage
       alt="Source Control side bar with Publish to GitHub"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/source-control-publish-github-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/source-control-publish-github-dark.png'),
       }}
   />

3. If you are not already signed in to GitHub, the IDE prompts you to authorize access. Complete the sign-in in the browser and return to the IDE.
4. When prompted, select the repository name and visibility. The repository name defaults to the project name. Choose **Publish to GitHub private repository** or **Publish to GitHub public repository**.

   <ThemedImage
       alt="Repository name and visibility selection"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/repository-name-visibility-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/repository-name-visibility-dark.png'),
       }}
   />

5. When asked which files to include, select all files and click **OK**.

The IDE creates the repository on GitHub and links it to the project folder. The **Source Control** side bar now shows the project files staged as the first set of changes, ready for an initial commit.

## What's next

- [Push from the IDE](push-from-ide.md) — Commit the staged files and publish the branch to GitHub.
- [Deploy from the cloud editor](deploy-from-cloud-editor.md) — Trigger the build and deploy your integration to WSO2 Cloud.
