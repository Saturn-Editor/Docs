---
icon: rectangle-api
---

# Saturn-API

Saturn-API is the interface between Saturn and its modules. The current version is **0.2.1**. Each module states which versions it supports in its manifest's `saturnApi` field, using a caret range such as `^0.2`.

{% hint style="info" %}
The Saturn-API version is separate from the Saturn-Core version.
{% endhint %}

## What's new in 0.2.1

Modules that declare `^0.2` or `^0.2.0` still load. Nothing is deprecated or removed.

* **Language servers:** five more `features`: `symbols`, `references`, `rename`, `formatting` and `signatureHelp`. If you list `features`, add these to turn them on.
* **Debuggers:** `contributes.debugger` for language modules. See [Debuggers](modules/debuggers.md).
* **Previews:** `defaultView` sets how a previewable file opens.
* **New projects:** `newProject.tabs` with templates, and `when` on fields. `projects.onCreate` can be async and gets `progress(message)`. If it throws, Saturn now deletes the new folder.
* **Editor API:** `saturn.editor` reads open files and the selection. See [Editor API](modules/extensions/editor-api.md).
* **Webview panels:** `"kind": "webview"` panels and `saturn.webviews`. See [Webview panels](modules/extensions/webviews.md).
* **Notifications:** actions now show in a pop-up as well as in the bell menu.
* **Language choice:** users can choose which language opens a file, for one file or for an extension, so don't assume an extension always maps to the same language.

## Module types

| Type | Manifest | Code | Guide |
| --- | --- | --- | --- |
| Language | `saturn-language.json` | Optional | [Languages](modules/languages.md) |
| Extension | `saturn-extension.json` | Yes | [Extensions](modules/extensions/README.md) |
| Theme | `theme.json` | No | [Themes](modules/themes.md) |

Languages and extensions can both add panels, commands, buttons, settings and run configurations. Only languages can add language definitions, language servers, debuggers, previews, file templates and new-project templates.

## Lifecycle

A module's `main` file is an ES module that runs in Saturn's extension host. It exports:

* `activate(saturn)`: required. Saturn calls it at startup for every enabled module.
* `deactivate()`: optional. Saturn waits for it briefly when quitting or reloading.

The `saturn` object lets your module:

* register commands
* fill tree panels and webview panels
* render previews and fill file templates
* set up new projects
* add Run menu entries
* run commands in a terminal
* read the active file, open files and the selection
* find and open files, and watch for file changes
* listen for projects opening and closing
* show messages and notifications
* read settings
* restart the language server

Each module also has its own storage folder (`saturn.storagePath`).

Installing, enabling or removing a module reloads modules straight away. Saturn doesn't need to restart. See [Extensions](modules/extensions/README.md) for the full API.

## Common manifest fields

Language and extension manifests share these fields:

| Key | Description |
| --- | --- |
| id | Lowercase `a-z`, `0-9`, `.` and `-`. |
| name, version, description | Shown in Saturn and the Marketplace. |
| badge | A short text icon. An `icon.svg` or `icon.png` at the module's root is used instead when there is one. |
| saturnApi | A caret range such as `^0.2`. While the major version is 0, the minor version is fixed too, so `^0.2` means 0.2.x. |
| main | The ES module entry file. |
| activationEvents | `workspaceContains:<glob>` patterns. |
| requires | External tools: `[{ name, command, args?, install? }]`. |
| contributes | What the module adds. See [Configuration](modules/extensions/configuration.md) and [Languages](modules/languages.md). |

## Publishing

Modules are distributed through the [Marketplace](modules/marketplace.md).
