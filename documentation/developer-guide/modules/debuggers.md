---
icon: bug
---

# Debuggers

A [language module](languages.md) can add a debugger with `contributes.debugger`. Saturn is a Debug Adapter Protocol (DAP) client: it starts your adapter and talks to it over stdio.

## contributes.debugger

| Key | Required | Description |
| --- | --- | --- |
| label | Yes | The debugger's name. |
| languages | Yes | The language IDs it debugs. |
| command | Yes | The adapter to start. |
| args | No | Arguments for `command`. |
| launch | Yes | The DAP `launch` arguments for the default configuration. |
| configurations | No | More launch configurations: `[{ id, label, launch }]`. |
| attach | No | DAP `attach` arguments, for attaching to a running process. |

### Variables

These are replaced in `launch` and `attach`:

| Variable | Value |
| --- | --- |
| `${file}` | The active file. |
| `${project}` | The project folder. |
| `${processId}` | The process the user picked to attach to. A value that is exactly `"${processId}"` becomes a number. |
| `${setting:<id>}` | The value of one of your module's settings. |

`configurations` adds a configuration picker to the Debug panel, and `attach` adds an **Attach…** process picker.

## Protocol

1. Saturn sends `initialize` (with `supportsRunInTerminalRequest: true`), then `launch` or `attach`.
2. On the `initialized` event, it sends `setBreakpoints` (with `condition` and `logMessage` when the user set them), then `configurationDone`.
3. While debugging, it uses `threads`, `stackTrace`, `scopes`, `variables`, `evaluate`, `continue`, `next`, `stepIn`, `stepOut`, `pause` and `disconnect`. `evaluate` uses the context `repl` for the debug console and `watch` for watches.
4. A `runInTerminal` request opens a Terminal tab.

## What users get

Saturn supports breakpoints (including conditional breakpoints and logpoints), watch expressions, threads and attaching to a process. If your adapter sends a `runInTerminal` request, the program runs in a Terminal tab so the user can type input.

See [Debugging](../../user-guide/debugging.md) in the User Guide.

## Example

```json
"debugger": {
  "label": "My Debugger",
  "languages": ["mylang"],
  "command": "node",
  "args": ["debug/adapter.js"],
  "launch": { "type": "mydebug", "request": "launch", "program": "${file}", "cwd": "${project}" },
  "configurations": [
    { "id": "tests", "label": "Debug tests", "launch": { "program": "${project}/tests", "cwd": "${project}" } }
  ],
  "attach": { "processId": "${processId}" }
}
```
