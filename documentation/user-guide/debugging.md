---
icon: bug
---

# Debugging

You can debug a file if its [language](languages.md) module includes a debugger.

## Starting

Click the bug button or press F5. If the language offers more than one configuration, choose the one you want. You can also attach the debugger to a process that is already running.

If the program asks for input, type it in the Terminal tab.

## Breakpoints

To add a breakpoint, click in the column to the left of the line numbers, or press F9.

* **Conditional breakpoints** only stop when a condition is true.
* **Logpoints** write a message instead of stopping.

## While paused

* **Watch:** add expressions to see their values as you step through code.
* **Threads:** see each thread and switch between them.
