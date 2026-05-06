---
title: Connect a Git repository
description: Publish your WSO2 Integrator project to a Git hosting provider so it can be deployed outside your local machine.
keywords: [wso2 integrator, github, gitlab, azure devops, git, source control, deploy]
---

# Connect a Git repository

Deploying your integration to any environment outside your local machine requires the project to be backed by a Git repository. The project folder open in the WSO2 Integrator IDE must be connected to a remote Git repository, such as one hosted on GitHub, GitLab, or Azure DevOps before you can deploy. This page walks through publishing your project to a remote repository from inside the IDE. The steps below use GitHub as an example.

:::info Prerequisites
- An integration created in the WSO2 Integrator IDE.
- An account with your Git hosting provider (this tutorial uses GitHub).
:::

## Initialize a local repository

1. In the WSO2 Integrator IDE, click **Source Control** in the left sidebar.

![Source Control](/img/deploy/connect-git-repository/source-control.png)

2. Click **Initialize Repository**. This creates a local Git repository inside your project folder.

## Commit your changes

1. In the **Source Control** sidebar, type a commit message in the message field.
2. Click **Commit**.

   The IDE prompts you to confirm whether to stage all current changes and commit.

3. Click **Yes** to stage and commit all files.

:::tip
If you want to commit only specific files, stage them manually in the **Source Control** sidebar before clicking **Commit**.
:::

## Publish to a remote repository

1. Click **Publish**.

   If you are not signed in to GitHub, the IDE prompts you to authorize access. Complete the sign-in in the browser and return to the IDE.

2. When prompted, select a repository name and visibility (**Public** or **Private**).

The IDE creates the repository on GitHub and pushes your code to it.

## What's next

- [Deploy to WSO2 Cloud](/docs/deploy/cloud/push-from-ide.md) — Trigger a deployment to WSO2 Cloud from the IDE.
