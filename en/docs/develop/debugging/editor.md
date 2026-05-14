---
sidebar_position: 1
title: Editor Debugging
description: Set breakpoints and start a debug session from the WSO2 Integrator editor.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Editor Debugging

Editor debugging lets you pause an integration mid-run and inspect the values flowing through it. This page covers the everyday quick start: set a breakpoint and launch a debug session. For the full set of features available once the session is running — stepping, inspection panels, advanced breakpoints, and test or remote debugging — see [Features](features.md).

## Before you start

- Open the integration project in WSO2 Integrator.
- Make sure the project builds successfully (`bal build`). Debugging a project that does not compile is not supported.
- Confirm the integration has an executable entry point — a service, an automation, or a `main()` function.

## Set a breakpoint

Breakpoints tell the debugger where to pause. Set one on the line or node where you want to start inspecting state.

<Tabs>
<TabItem value="visual" label="Visual Designer" default>

1. Open the integration in the visual designer.
2. Click the three-dot menu on the node where you want to pause.
3. Select **Add Breakpoint**.

A red dot appears on the node to confirm the breakpoint is active.

![Breakpoint set on a node in the visual designer](/img/develop/debugging/editor-debugging/flow-diagram.png)

</TabItem>
<TabItem value="code" label="Code editor">

1. Open the `.bal` file.
2. Click in the gutter to the left of the line number where you want to pause.

A red dot appears next to the line.

![Breakpoint set in the Ballerina code editor](/img/develop/debugging/editor-debugging/bal-code.png)

</TabItem>
</Tabs>

## Start a debug session

You can start a debug session two ways.

- **Quick start** — Click the **Debug** CodeLens that appears above `main()`, a service, or any other entry point. This is the fastest path and works for most integrations.
- **Custom configuration** — Create a `launch.json` file in the project, pick a configuration from the **Run and Debug** dropdown, and click **Start Debugging**. Use this when you need to pass arguments, environment variables, or non-default settings.

When you launch a debug session from the editor, the working directory is the Ballerina package root.

![Debug session paused at a breakpoint](/img/develop/debugging/editor-debugging/debug-session.png)

Execution pauses at the first breakpoint it hits. Output streams to the **Debug Console**.

## Advanced debugging methods

Most debugging happens against a program running locally from the editor. The next two methods cover the cases that do not fit that pattern — debugging an individual test, or attaching to an integration that is already running somewhere else.

### Test debugging

Test debugging steps through a single test function the same way program debugging steps through `main()`. A **Debug** CodeLens appears above every test function. Click it to launch the debugger scoped to that test. Use this when a test fails and you want to inspect the inputs and intermediate values that produced the failure, rather than the full integration.

### Remote debugging

Remote debugging attaches the editor to an integration that is already running on another machine, in a container, or as an executable JAR. Start the integration in debug mode with one of the following commands:

```bash
bal run --debug <port> <path>     # package or file
bal run --debug <port> <jar>      # executable JAR
bal test --debug <port> <path>    # tests
```

Then add a **Ballerina Remote** configuration to `launch.json` with the `debuggeeHost` and `debuggeePort` matching the running process, and click **Start Debugging**. The same breakpoints, stepping, and inspection features work against the remote process.

## Next steps

- [Features](features.md) — stepping, variable inspection, advanced breakpoints, and test or remote debugging.
- [Errors and stack traces](/docs/develop/troubleshooting/errors-and-stack-traces) — read error messages and trace failures back to source.
- [Logging](/docs/develop/troubleshooting/logging) — when adding logs is a better fit than a live debug session.
