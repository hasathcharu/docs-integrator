---
sidebar_position: 3
title: Library View
description: Build and manage reusable libraries in WSO2 Integrator.
---

# Library View

The Library View is a dedicated view in WSO2 Integrator for creating utilities and shared resources that you can use across multiple integrations. Rather than building executable integrations, you use the Library View to bundle shared type definitions, utility functions, custom connections, and data mapper configurations into a centralized module that other integrations can depend on.

![Library View overview](/img/develop/project-views/library-view/overview.png)

## General navigation

The general navigation elements in the Library View work the same as in the Project View:

- **Activity bar** — Access the file explorer, global search, source control, and extension marketplace.
- **Project explorer** — View and manage all artifacts (Connections, Types, Functions, Data Mappers) organized by category.

For more details on these elements, see the [Project View](project-view.md) documentation.

## Library overview canvas

The main canvas provides a central dashboard for your library. It displays:

- **Artifacts summary** — Cards showing the total number of defined Types, Functions, Data Mappers, and Connections in your library.
- **README** — A section at the bottom for documenting the library's purpose, setup instructions, and usage notes to help users understand how to use it.

## Add reusable artifacts

Click the **+ Add Artifacts** button at the top right of the canvas to add a new component to your library. This opens a menu with all available artifact types that can be created in a library:

- **Function**
- **Data Mapper**
- **Type**
- **Connection**
- **Configuration**

![Add artifacts menu](/img/develop/project-views/library-view/add-artifacts.png)

For detailed information on configuring each specific artifact type, see the [Integration Artifacts](../integration-artifacts/integration-artifacts.md) documentation.

## Artifact management

Clicking any of the artifact category cards on the main canvas (such as **Functions** or **Types**) navigates to a specific list view for those artifacts.

![Artifact list view](/img/develop/project-views/library-view/artifact-list.png)

From this view, you can:

- View all defined artifacts of that specific type.
- Search for a specific artifact using the search bar.
- Click the **+ Add [Artifact]** button (e.g., **+ Add Function**) to create a new artifact of that type directly.

## Toolbar

The toolbar sits at the top of the Library View and provides quick access to actions for configuring and publishing your library.

![Toolbar](/img/develop/project-views/library-view/toolbar.png)

| Action | Description |
|---|---|
| **Undo** / **Redo** | Reverses or reapplies recent changes to your library artifacts. |
| **Configure** | Opens the project-level configuration panel for editing package metadata (name, version, organization), build options, and dependencies. |
| **Publish** | Builds the library and pushes it to a central repository (such as Ballerina Central), making the module available for other integrations to import. Libraries are not executable, so they are published rather than run. |

## What's next

- [Packages & Modules](/docs/develop/organize-code/packages-modules) -- Understand package structure
- [Publish to Ballerina Central](/docs/connectors/publish-to-central) -- Share your libraries
