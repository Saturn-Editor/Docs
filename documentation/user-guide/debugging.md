---
icon: bug
---

# Debugging

You can debug a file if its [language](languages.md) module includes a debugger.

## Starting

Click the bug button or press F5. If the language offers more than one configuration, choose the one you want in the Debug panel. To debug a program that's already running, click **Attach…**.

If the program asks for input, type it in the Terminal tab.

## Panels

| Panel | Shows |
| --- | --- |
| **Debug** | The controls, launch configuration, **Attach…**, threads and call stack. |
| **Variables** | Watch expressions and scopes. |
| **Debug Console** | Program output. Type expressions to evaluate them. |
| **Breakpoints** | Every breakpoint in the project. |

### Debug layout

When debugging starts, the window switches to a separate layout. By default, Variables is on the left, and Debug, Debug Console and Breakpoints are at the bottom. When debugging ends, the window switches back.

Saturn remembers any changes you make to the debug layout for next time, for each project.

### Separate window

To show the debug panels in their own window, go to [Settings](settings.md) → Advanced → Debugging and set **Show the debug panels** to **In a separate window**. The window shows the four panels in a grid. It opens when debugging starts and closes when it ends. Clicking a stack frame or breakpoint shows the code in the project window.

## Breakpoints

To add a breakpoint, click in the column to the left of the line numbers, or press F9.

* **Conditional breakpoints** only stop when a condition is true.
* **Logpoints** write a message instead of stopping.

The **Breakpoints** panel lists every breakpoint in the project with its condition or log message. Click a breakpoint to go to it. Use its checkbox to turn it on or off, or hover over it to remove it.

A breakpoint that's turned off shows as a hollow ring in the margin, and isn't sent to the debugger.

### Exceptions

The **Exceptions** section of the Breakpoints panel lists the debugger's exception options, such as stopping on all exceptions or only uncaught ones. Saturn remembers your choices for each project until it quits.

## While paused

* **Watch:** add expressions in the Variables panel to see their values as you step through code.
* **Threads:** see each thread in the Debug panel and switch between them.
