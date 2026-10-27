---
icon: spell-check
---

# Languages

A language module adds support for a programming language to Saturn. It can add syntax highlighting, a language server, previews, run configurations, file templates and new-project templates. Each language module is a folder with a `saturn-language.json` manifest.

## saturn-language.json

| Key | Required | Description |
| --- | --- | --- |
| id | Yes | Must match the module's [Marketplace](marketplace.md) entry. Must be unique across all languages, extensions and themes. |
| name | Yes | Display name. |
| version | Yes | Must match the Marketplace version. |
| saturnApi | Yes | The Saturn-API version range you support, for example `^0.2`. |
| description | No | Short description. |
| main | No | A JavaScript entry file that exports `activate(saturn)` and `deactivate()`. It works the same way as an [extension](extensions/README.md) entry file. |
| requires | No | External tools the module needs: `[{ name, command, args?, install? }]`. Saturn runs each `command` before starting the language server. If one fails, Saturn shows a notification. |
| activationEvents | No | `workspaceContains:<glob>` patterns. These only decide which language icon a project shows. They don't control when the module loads. Saturn activates every enabled module at startup. |
| contributes | No | Everything the module adds to Saturn. See below. |

### contributes

| Key | Description |
| --- | --- |
| languages | `[{ id, name, extensions, grammar?, scopeName? }]`. `grammar` is a TextMate grammar file. |
| languageServer | `{ languages, command, args, features }`. See [Language server](#language-server). |
| debugger | `{ label, languages, command, args?, launch, configurations?, attach? }`. A Debug Adapter Protocol (DAP) adapter. See [Debuggers](debuggers.md). |
| previews | `[{ id, label, extensions, defaultView? }]`. A rendered view of files with these extensions. `defaultView` is `code` (the default), `split` or `preview`. Users can change it in Settings → Languages. Render it with `previews.registerRenderer(id, (text, ctx) => …)`. |
| fileTemplates | `[{ id, label, kind, extension?, placeholder? }]`, where `kind` is `file` or `dir`. Fill each one with `files.registerTemplate(id, { fileName?, content? })`. |
| newProject | `{ language, tabs?, fields }`. Lets users create a new project for this language. See [New projects](#new-projects). |
| commands, buttons, panels, settings, runConfigurations | The same as for extensions. See [Configuration](extensions/configuration.md). |

## Language server

Saturn talks to the language server using the Language Server Protocol (LSP) over stdio.

* If `command` is `"node"`, Saturn uses its bundled copy of Node.
* `${setting:<id>}` in `command` or `args` is replaced with the value of that setting.
* The process runs in the project folder. The `SATURN_MODULE_STORAGE` environment variable points to a folder where the module can store files, for example a downloaded server.
* `features` lists the LSP features to turn on. If you leave out `features`, all of them are turned on. Users can switch each one on or off in Settings → Languages; features you don't list show as "Not supported".

| Feature | Powers |
| --- | --- |
| `completion` | Code completion |
| `diagnostics` | Error detection |
| `hover` | Hover information |
| `definition` | Go to definition |
| `symbols` | The Outline panel |
| `references` | Find Usages |
| `rename` | Rename Symbol |
| `formatting` | Format Document and format on save |
| `signatureHelp` | Parameter hints |

{% hint style="warning" %}
`symbols`, `references`, `rename`, `formatting` and `signatureHelp` are new in API 0.2.1. If your manifest lists `features`, add them to turn them on.
{% endhint %}

## New projects

`contributes.newProject` adds your language to Saturn's New Project window.

* `fields` are settings-style fields (`text`, `select`, `checkbox`, `folder` or `file`) that the user fills in.
* `tabs` is optional: `[{ id, label, templates: [{ id, label, description? }] }]`. Each tab shows a Template dropdown, with the chosen template's description underneath.
* A field can have `when: { template?: [...], tab?: [...] }`. It only shows, and is only sent, when the chosen tab or template matches.
* `fields.tab` and `fields.template` tell your handler which tab and template were chosen.

When the user clicks Create, Saturn calls your `projects.onCreate` handler:

```js
saturn.projects.onCreate(async ({ projectPath, fields, progress }) => {
  progress('Creating project…');
  // write files into projectPath
});
```

Saturn waits for the handler with no time limit, showing "Creating…" or your latest progress message. If the handler throws, Saturn deletes the new folder and shows the error on the New Project page.

## Example

A shortened version of the C# .NET module's manifest:

```json
{
  "id": "csharp",
  "name": "C# .NET",
  "version": "2.0.0",
  "description": "Language module for C#/.NET projects",
  "saturnApi": "^0.2",
  "main": "module.js",
  "requires": [
    {
      "name": ".NET SDK",
      "command": "dotnet",
      "args": ["--version"],
      "install": "Install the .NET SDK from https://dotnet.microsoft.com/download"
    }
  ],
  "activationEvents": ["workspaceContains:**/*.csproj", "workspaceContains:**/*.cs"],
  "contributes": {
    "languages": [
      {
        "id": "csharp",
        "name": "C#",
        "extensions": [".cs", ".csx"],
        "grammar": "grammars/csharp.tmLanguage.json",
        "scopeName": "source.cs"
      }
    ],
    "languageServer": {
      "languages": ["csharp"],
      "command": "node",
      "args": ["server/launch.js", "${setting:csharpLsPath}"],
      "features": ["completion", "diagnostics", "hover", "definition", "symbols", "references", "rename", "formatting", "signatureHelp"]
    },
    "runConfigurations": true
  }
}
```

## Publishing

Zip the module folder's contents and publish it through the [Marketplace](marketplace.md).
