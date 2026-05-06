---
title: Push from the IDE
description: Commit your project changes and publish the branch to GitHub from the WSO2 Integrator IDE.
keywords: [wso2 integrator, wso2 cloud, github, commit, push, source control]
---

import ThemedImage from '@theme/ThemedImage';
import useBaseUrl from '@docusaurus/useBaseUrl';

# Push from the IDE

After you connect a Git repository, push your project files to GitHub so WSO2 Cloud can read them. You commit the staged changes and publish the branch directly from the **Source Control** side bar in the WSO2 Integrator IDE.

:::info Prerequisites
- The project is published to GitHub. See [Connect a Git repository](connect-git-repository.md).
- The **Deploy Integration** tab is open.
:::

## Commit the changes

1. In the **Source Control** side bar, type a commit message in the message field. For the first push, use a message such as `Initial commit`.
2. Click **Commit**.

   <ThemedImage
       alt="Source Control side bar with staged files and the Commit button"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/commit-changes-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/commit-changes-dark.png'),
       }}
   />

3. If no files are staged yet, the IDE asks whether you want to stage all your changes and commit them directly. Click **Yes**.

   <ThemedImage
       alt="Prompt to stage all changes and commit directly"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/stage-changes-prompt-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/stage-changes-prompt-dark.png'),
       }}
   />

## Publish the branch

1. In the **Source Control** side bar, click **Publish Branch**. The IDE pushes the branch to the GitHub repository you selected when you connected the project.

   <ThemedImage
       alt="Publish Branch button in the Source Control side bar"
       sources={{
           light: useBaseUrl('/img/deploy/push-to-cloud/publish-branch-light.png'),
           dark: useBaseUrl('/img/deploy/push-to-cloud/publish-branch-dark.png'),
       }}
   />

2. Switch back to the **Deploy Integration** tab and click the refresh icon on the **Source Control** card to pick up the new repository state.

## Grant WSO2 Cloud access to the repository

WSO2 Cloud reads your code through a GitHub app. If you previously installed the app with access to selected repositories only, the new repository is not yet visible.

If the **Source Control** card shows a **Grant Access** message after the refresh, click **Grant Access** and approve the repository in the GitHub authorization page.

When the **Source Control** card turns green and shows the repository and branch, the project is ready to deploy.

## What's next

- [Deploy from the cloud editor](deploy-from-cloud-editor.md) — Trigger the build and deploy your integration to WSO2 Cloud.
- [Connect a Git repository](connect-git-repository.md) — Set up the GitHub repository if you have not done so already.
