---
icon: rectangle-api
---

# Saturn-API

Saturn-API is the interface between Saturn and its modules. The current version is **0.2.0**. Each module states which versions it supports in its manifest's `saturnApi` field, using a caret range such as `^0.2`.

{% hint style="info" %}
The Saturn-API version is separate from the Saturn-Core version.
{% endhint %}

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
* fill tree panels
* add Run menu entries
* run commands in a terminal
* open files and watch for file changes
* listen for projects opening and closing
* show messages
* read settings

Each module also has its own storage folder. See [Extensions](modules/extensions/README.md) for the full API.

## Publishing

Modules are distributed through the [Marketplace](modules/marketplace.md).
