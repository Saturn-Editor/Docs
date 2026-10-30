# Configuration

The saturn-extension.json file defines how the Extension behaves.

## Basic Configuration

<table><thead><tr><th width="176.22265625">Key</th><th width="171.0078125">Accepted Values</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>Lowercase <code>a-z</code>, <code>0-9</code>, <code>.</code> and <code>-</code></td><td>The ID of your module, modules in the Saturn Marketplace must have unique IDs. </td></tr><tr><td>name</td><td>Any string</td><td>The name of your module that is shown in Saturn's interface.</td></tr><tr><td>version</td><td>Any version string</td><td>The version of your module.</td></tr><tr><td>description</td><td>Any string</td><td>A short description of what your module does.</td></tr><tr><td>saturnApi</td><td>Any version string</td><td>The Saturn-API version your module supports. For example, ^0.4. See <a href="../../saturn-api.md#compatibility">Compatibility</a>. This is different to the Saturn-Core version.</td></tr><tr><td>main</td><td>Any valid file</td><td>Optional. An ES module that exports <code>activate(saturn)</code> and, optionally, <code>deactivate()</code>.</td></tr><tr><td>badge</td><td>Any short string</td><td>Optional. A short text icon for your extension, for example "CC". If there is an <code>icon.svg</code> or <code>icon.png</code> at the extension's root, Saturn uses that instead.</td></tr><tr><td>activationEvents</td><td>Array</td><td>Optional. When to activate the extension: <code>workspaceContains:&lt;glob&gt;</code>, <code>onLanguage:&lt;id&gt;</code>, <code>onCommand:&lt;id&gt;</code> or <code>*</code>. With <code>*</code>, or with no <code>activationEvents</code> at all, the extension activates at startup. See <a href="../../saturn-api.md#activation">Activation</a>.</td></tr><tr><td>requires</td><td>Array</td><td>Optional. External tools your extension needs: <code>{ name, command, args?, install? }</code>. <code>install</code> is the hint shown when the tool is missing.</td></tr></tbody></table>

The <code>id</code> and <code>version</code> must match your Marketplace entry. The <code>id</code> must be unique across all languages, extensions and themes.

## Contributions

| Key | Description |
| --- | --- |
| contributes.commands | `[{ id, title }]`. These appear in action search. Implement each one with `commands.register`. |
| contributes.buttons | `[{ command, title, icon? }]`. Buttons on the editor's right edge, below Settings. Clicking one runs its command. `icon` must be a `.svg` or `.png` file in your zip. |
| contributes.panels | `[{ id, title, dock, kind?, icon?, showWhen? }]`, where `dock` is `"left"` or `"bottom"`. `kind` is `"tree"` (implement with `panels.registerTree`) or `"webview"` (implement with `webviews.register`, see [Webview panels](webviews.md)). See [showWhen](#showwhen). |
| contributes.settings | `[{ id, label, type, default?, options?, description? }]`, where `type` is `text`, `select`, `checkbox`, `folder` or `file`. For `select`, `options` is `[{ value, label }]`. Settings appear on your extension's tab in Settings → Extensions. |
| contributes.runConfigurations | Set to `true` if your extension calls `run.registerProvider`. |
| contributes.tests | Set to `true` if your module calls `tests.registerController`. See [Tests](tests.md). |

Extensions can't contribute `languages`, `languageServer`, `debugger`, `previews`, `fileTemplates` or `newProject`. Use a [language module](../languages.md) for those.

### showWhen

New in 0.4, so modules that use it need `"saturnApi": "^0.4"`. `showWhen` hides a panel until the project has a matching file. It's a list of file-name patterns, where `*` is a wildcard:

* A plain pattern, such as `package.json`, matches files in the project's root folder.
* A pattern starting with `**/`, such as `**/*.csproj`, matches files up to 4 folders deep. Hidden folders, `node_modules`, `bin` and `obj` are skipped.

Saturn checks again as files change, so the panel appears once a matching file is added.

```json
"panels": [
  { "id": "dotnet.solution", "title": "Solution", "dock": "left", "showWhen": ["**/*.sln", "**/*.slnx", "**/*.csproj"] }
]
```

## Example

An example from our Claude Code extension is shown below:

```
{
  "id": "claudecode",
  "name": "Claude Code",
  "version": "1.0.1",
  "description": "Claude Code integration for Saturn",
  "badge": "CC",
  "saturnApi": "^0.3",
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
    "buttons": [{ "command": "claudecode.open", "title": "Claude Code", "icon": "run-claude.svg" }],
    "panels": [{ "id": "claudecode.panel", "title": "Claude Code", "dock": "left" }],
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
