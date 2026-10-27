# Configuration

The saturn-extension.json file defines how the Extension behaves.

## Basic Configuration

<table><thead><tr><th width="176.22265625">Key</th><th width="171.0078125">Accepted Values</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>Any string</td><td>The ID of your module, modules in the Saturn Marketplace must have unique IDs. </td></tr><tr><td>name</td><td>Any string</td><td>The name of your module that is shown in Saturn's interface.</td></tr><tr><td>version</td><td>Any version string</td><td>The version of your module.</td></tr><tr><td>description</td><td>Any string</td><td>A short description of what your module does.</td></tr><tr><td>saturnApi</td><td>Any version string</td><td>The Saturn-API version your module supports. For example, 0.2.0, or ^0.2, which also supports any 0.2.x versions. This is different to the Saturn-Core version.</td></tr><tr><td>main</td><td>Any valid file</td><td>The main file that should be loaded when your module is called by Saturn-Core</td></tr></tbody></table>

## Example

An example from our Claude Code extension is shown below:

```
{
  "id": "claudecode",
  "name": "Claude Code",
  "version": "1.0.0",
  "description": "Claude Code integration for Saturn",
  "badge": "CC",
  "saturnApi": "^0.2",
  "main": "extension.js",
  "requires": [
    {
      "name": "Claude Code CLI",
      "command": "claude",
      "args": ["--version"],
      "install": "Install with npm i -g @anthropic-ai/claude-code"
    }
  ],
  "contributes": {
    "commands": [
      { "id": "claudecode.open", "title": "Claude Code: Open" },
      { "id": "claudecode.continue", "title": "Claude Code: Continue Last Session" },
      { "id": "claudecode.resume", "title": "Claude Code: Resume Session…" },
      { "id": "claudecode.usage", "title": "Claude Code: Usage" },
      { "id": "claudecode.openMemory", "title": "Claude Code: Open CLAUDE.md" },
      { "id": "claudecode.openSettings", "title": "Claude Code: Open Project Settings" },
      { "id": "claudecode.checkInstall", "title": "Claude Code: Check Installation" },
      { "id": "claudecode.refresh", "title": "Claude Code: Refresh Panel" }
    ],
    "buttons": [{ "command": "claudecode.open", "title": "Claude Code", "badge": "CC", "icon": "build.svg" }],
    "panels": [{ "id": "claudecode.panel", "title": "Claude Code", "dock": "left", "badge": "CC" }],
    "settings": [
      {
        "id": "claudePath",
        "label": "Claude executable",
        "type": "file",
        "default": "claude",
        "description": "Path to the claude CLI. Leave as 'claude' to use the one on PATH."
      },
      {
        "id": "model",
        "label": "Model",
        "type": "select",
        "default": "",
        "options": [
          { "value": "", "label": "CLI default" },
          { "value": "opus", "label": "Opus" },
          { "value": "sonnet", "label": "Sonnet" },
          { "value": "haiku", "label": "Haiku" }
        ]
      },
      {
        "id": "permissionMode",
        "label": "Permission mode",
        "type": "select",
        "default": "",
        "options": [
          { "value": "", "label": "CLI default" },
          { "value": "acceptEdits", "label": "Accept edits" },
          { "value": "plan", "label": "Plan" },
          { "value": "auto", "label": "Auto" }
        ]
      },
      {
        "id": "extraArgs",
        "label": "Extra arguments",
        "type": "text",
        "default": "",
        "description": "Appended to every claude command started from the Run menu."
      },
      {
        "id": "recentSessions",
        "label": "Recent sessions to show",
        "type": "select",
        "default": "5",
        "options": [
          { "value": "0", "label": "None" },
          { "value": "5", "label": "5" },
          { "value": "10", "label": "10" },
          { "value": "20", "label": "20" }
        ]
      }
    ],
    "runConfigurations": true
  }
}
```
