---
sidebar_position: 2
title: Integrator app
description: The shared shell that surrounds every view in the WSO2 Integrator IDE.
keywords: [wso2 integrator, ide, integrator app, activity bar, editor toolbar]
---

# Integrator app

The WSO2 Integrator IDE is the application you install to build integrations. Every view you open (Project view, Integrator view, Library view, and so on) renders inside the same window chrome: a top menu bar, an activity bar on the far-left edge, an editor tab area, and the side panels for the project explorer and deployment options. This page describes the parts of that chrome that stay the same across views, so the view-specific pages do not need to repeat them.

## Top menu bar

The top menu bar runs across the top edge of the application window and gives you access to the standard VS Code menus.

![Top menu bar](/img/develop/understand-ide/integrator-app/top-menu-bar.png)

| Menu | Description |
|---|---|
| **WSO2 Integrator** | Application-level actions such as About, Preferences, and Quit. |
| **File** | Open, save, and close projects, files, and windows. |
| **Edit** | Standard editing actions such as undo, redo, cut, copy, paste, and find. |
| **Selection** | Selection and multi-cursor actions for the source editor. |
| **View** | Toggle the activity bar, side bar, panel, terminal, and other layout elements. |
| **Go** | Navigation actions such as Go to File, Go to Definition, and Go to Symbol. |
| **Run** | Run and debug actions for the active integration. |
| **Terminal** | Open and manage integrated terminals. |
| **Window** | Manage open windows. |
| **Help** | Documentation, release notes, and support links. |

The menus follow the standard VS Code layout. The WSO2 Integrator extension does not add menus of its own here. Integrator-specific actions live in the [activity bar](#activity-bar) and the [editor toolbar](#editor-toolbar).

## Activity bar

The activity bar is the narrow vertical strip on the far-left edge of the IDE. Each icon opens a different panel or tool, and the active icon is highlighted.

![Activity bar](/img/develop/understand-ide/integrator-app/activity-bar.png)

| Name | Description |
|---|---|
| **Explorer** | Opens the file explorer for browsing project files on disk. |
| **WSO2 Integrator** | Opens the project explorer for the current view. This is the entry point to the Integrator app. |
| **Source Control** | Opens the Git source control panel for staging, committing, and reviewing changes. |
| **Run and Debug** | Opens the debug panel for setting breakpoints, launching the integration with the debugger, and inspecting variables. |
| **Testing** | Opens the test explorer to view, run, and debug the test cases defined for your integration. |

Select the **WSO2 Integrator** icon at any time to return to the project explorer and the current view.

## Editor tab area

The editor tab area sits above the canvas and shows one tab per open editor. The active tab is highlighted, and you can close a tab with the **×** action on it.

![Editor tab area](/img/develop/understand-ide/integrator-app/editor-toolbar.png)

The **WSO2 Integrator** tab represents the visual designer for the current view. Other tabs (for example, a Ballerina source file opened from the explorer) appear alongside it and behave like standard VS Code editor tabs.

### Editor toolbar

The editor toolbar appears on the right side of the editor tab bar and provides quick actions for the active integration.

| Action | Description |
|---|---|
| **WSO2 Integrator Copilot** | Opens the WSO2 Integrator Copilot chat panel for AI-powered assistance with building and troubleshooting your integration. |
| **Run** | Builds and runs the integration locally, starting all services and streaming output to the terminal. |
| **Debug** | Launches the integration with the debugger attached so you can set breakpoints and inspect variables. |
| **Show Source** | Switches the editor from the visual designer to the Ballerina source for the current artifact. |
| **Split editor** | Opens the active editor in a second column so you can view two surfaces side by side. |
| **More** (**⋯**) | Reveals additional editor actions, such as reopening recently closed editors and pinning the tab. |

The toolbar is the same across the Project view, Integrator view, and Library view, so the view-specific pages refer back to this section instead of repeating it.

## What's next

- [Project view](views/project-view.md): work with multiple integrations and libraries.
- [Integrator view](views/integration-view.md): build and manage a single integration.
- [Library view](views/library-view.md): build reusable libraries.
