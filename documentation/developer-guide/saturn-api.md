---
icon: rectangle-api
---

# Saturn-API

Saturn-API is the interface between Saturn and its modules. The current version is **0.3.0**. Each module states which version it needs in its manifest's `saturnApi` field, for example `^0.3`.

{% hint style="info" %}
The Saturn-API version is separate from the Saturn-Core version.
{% endhint %}

## Compatibility

Versions are `X.Y.Z`. Saturn runs a module when:

* the module's `X` matches Saturn's exactly,
* the module's `Y` is less than or equal to Saturn's, and
* `Z` is ignored.

So a `^0.2` module runs on Saturn-API 0.3.0, but a `^0.3` module doesn't run on 0.2.x. This is looser than npm's caret rule for `0.x` versions.

## What's new in this release

Nothing is deprecated or removed. The API version number for this release isn't set yet.

* **New API:** `window.showInput`, `workspace.renameFile`, `languages.onDidRenameSymbol` and `languages.registerDocComment`. See [Extensions](modules/extensions/README.md).
* **Language servers:** three more `features`: `inlayHints`, `codeLens` and `semanticTokens`. Quick fixes (code actions) are requested automatically. See [Languages](modules/languages.md#language-server).
* **Doc comments:** `docCommentTriggers` on a language. See [Languages](modules/languages.md#languages).
* **Panels:** `showWhen` hides a panel until the project has a matching file. See [showWhen](modules/extensions/configuration.md#showwhen).
* **File templates** show in New menus before the module loads.
* **Marketplace:** `default` now works for extensions as well as languages. Users can install older versions.

## What's new in 0.3.0

Modules that declare `^0.2` still run. Nothing is deprecated or removed.

* **On-demand activation:** modules that declare `activationEvents` only activate when they're needed. See [Activation](#activation).
* **Tests:** `contributes.tests` and `saturn.tests.registerController` add tests to the Tests panel. See [Tests](modules/extensions/tests.md).
* **Previews** re-render when your module's settings change.
* **Tree panels** no longer flash empty when they refresh.
* **Settings:** each extension's settings are now a tab under Settings → Extensions, instead of a sidebar entry of its own. Opening `module:<id>` still goes to the right tab.

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

* `activate(saturn)`: required. Saturn calls it when the module activates. See [Activation](#activation).
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
* show messages, notifications and input boxes
* rename files
* read settings
* add tests to the Tests panel
* restart the language server
* add doc comments and react to symbol renames

Each module also has its own storage folder (`saturn.storagePath`).

Installing, enabling or removing a module reloads modules straight away. Saturn doesn't need to restart. See [Extensions](modules/extensions/README.md) for the full API.

## Activation

If a module has no `activationEvents`, Saturn activates it at startup.

If it has `activationEvents`, Saturn activates it the first time one of these happens:

| Event | When |
| --- | --- |
| `workspaceContains:<glob>` | The open project has a file matching the glob. |
| `onLanguage:<id>` | A file in that language opens. |
| `onCommand:<id>` | That command runs. |
| `*` | At startup, the same as having no `activationEvents`. |

Using any of the module's contributions also activates it: a command, panel, preview, file template, Run entry, New Project type or tests. Doc comments and rename hooks start a module through its `onLanguage:<id>` event.

{% hint style="info" %}
If a module has `activationEvents` but no `onLanguage:<id>` event for one of its own languages, Saturn currently loads it at startup instead. This rule may change. Add an `onLanguage:<id>` event for each of your languages.
{% endhint %}

A module that activates after projects are already open still gets `projects.onDidOpen` for each of them.

## Common manifest fields

Language and extension manifests share these fields:

| Key | Description |
| --- | --- |
| id | Lowercase `a-z`, `0-9`, `.` and `-`. |
| name, version, description | Shown in Saturn and the Marketplace. |
| badge | A short text icon. An `icon.svg` or `icon.png` at the module's root is used instead when there is one. |
| saturnApi | A range such as `^0.3`. See [Compatibility](#compatibility). |
| main | The ES module entry file. |
| activationEvents | When to activate the module. See [Activation](#activation). |
| requires | External tools: `[{ name, command, args?, install? }]`. |
| contributes | What the module adds. See [Configuration](modules/extensions/configuration.md) and [Languages](modules/languages.md). |

## Publishing

Modules are distributed through the [Marketplace](modules/marketplace.md).
