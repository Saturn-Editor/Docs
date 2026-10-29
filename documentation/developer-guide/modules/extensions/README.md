---
icon: link
---

# Extensions

Extensions add commands, buttons, panels, settings and Run menu entries to Saturn. Each extension is a folder with a [`saturn-extension.json`](configuration.md) manifest and a JavaScript entry file.

## Entry point

The `main` file runs in Node inside Saturn's extension host, so `node:` modules are available. Saturn loads it with a dynamic `import()`, which means ES modules work (set `"type": "module"` in `package.json`).

```js
export function activate(saturn) {
  saturn.commands.register('myext.hello', (ctx) => {
    saturn.window.showMessage(`Hello from ${ctx.projectPath}`);
  });
}

// Optional. Must finish within 1.5 seconds.
export function deactivate() {}
```

Callbacks receive `ctx = { projectPath }`.

Language modules use the same entry point and API. Some parts of the API, marked **Languages only** below, go with contributions that only language modules can make.

## Saturn API (0.3.0)

### platform and storagePath

* `platform` is the operating system Saturn is running on.
* `storagePath` is a folder where your module can keep files. Its language server gets the same path in the `SATURN_MODULE_STORAGE` environment variable.

### commands

* `commands.register(id, ctx => …)` runs the code for a command listed in `contributes.commands`.

### panels

* `panels.registerTree(id, { getChildren(parentId, ctx), onAction?(actionId, item, ctx) })` fills a tree panel listed in `contributes.panels`. For the root, `parentId` is `null` or `undefined`.
* `panels.refresh(id, itemId?)` reloads the whole panel, or one item.

A tree item looks like this:

```js
{
  id, label, hasChildren,
  open?: { path, line?, column? }, // absolute or project-relative path
  badge?, description?, tooltip?,
  actions?: [{ id, label }]
}
```

### webviews

Fill a panel declared with `"kind": "webview"` with your own HTML. See [Webview panels](webviews.md).

* `webviews.register(panelId, { html(ctx), onMessage?(message, ctx) })`
* `webviews.postMessage(panelId, message, projectPath?)`
* `webviews.refresh(panelId)`

### editor

Read what the user is editing. See [Editor API](editor-api.md).

* `editor.activeFile(projectPath)`
* `editor.openFiles(projectPath)`
* `editor.file(projectPath, path)`
* `editor.onDidChangeActive`, `editor.onDidChangeSelection`, `editor.onDidChangeFile`, `editor.onDidCloseFile`

### run

* `run.registerProvider(ctx => [{ name, command }])` adds entries to the Run menu. Each one runs in a Terminal tab. The provider can be async. Requires `contributes.runConfigurations: true`.

### terminal

* `terminal.run(projectPath, { name, command })` opens a new Terminal tab in that project's window and runs the command in the user's shell.

### workspace

* `workspace.findFiles(projectPath, glob)` finds files in the project that match a glob.
* `workspace.openFile(projectPath, path, line?, column?)`
* `workspace.onDidChangeFiles(e => …)`
* `workspace.renameFile(projectPath, path, newName)` renames a file and returns its new absolute path. If the file is open, its tab moves too. New in this release.

### projects

* `projects.onDidOpen(ctx => …)`
* `projects.onDidClose(ctx => …)`
* `projects.onCreate(ctx => …)`: **Languages only.** Runs when the user creates a project from your `contributes.newProject`. `ctx` is `{ projectPath, fields, progress(message) }`. The handler can be async. If it throws, Saturn shows the error and deletes the new project folder.

### previews

* `previews.registerRenderer(id, (text, ctx) => …)`: **Languages only.** Renders a preview listed in `contributes.previews`.

### files

* `files.registerTemplate(id, { fileName?, content? })`: **Languages only.** Fills a template listed in `contributes.fileTemplates`.

### window

* `window.showMessage(message, { kind?: 'info' | 'warning' | 'error', projectPath? })` shows a message in the status bar.
* `window.notify({ title, message?, kind?, actions? })` shows a notification in a pop-up and in the bell menu. Each action can run a command, open Settings, or open an `https` URL.
* `window.showInput(projectPath, { title, value?, placeholder? })` asks the user to type something. It returns the text, or `null` if the user cancels. New in this release.

### languages

**Languages only.**

* `languages.registerVersions(languageId, { list(ctx), addLabel?, add?(ctx), onChange?(ctx) })` fills the status-bar version picker, for example "Java 17". The language needs `versions: true`. Each method can be async.
  * `list` returns the versions: `[{ id, label, detail? }]`.
  * `add` is offered with a folder picker labelled `addLabel`. `ctx` also has `folder`. It returns the new version.
  * `onChange` runs when the user picks a version. `ctx` also has `version`.
* `languages.getVersion(projectPath, languageId)` returns the chosen version `{ id, label, detail? }`, or `null`.
* `languages.registerDocComment(languageId, provide)` adds doc comments for a language with `docCommentTriggers`. New in this release. `provide` gets `{ projectPath, path, text, line, trigger, indent }`, where `line` is the 0-based line the trigger was typed on. Return a snippet string to replace the trigger, or `null` to do nothing. Use `${1}` or `$1` for placeholders. Don't indent the lines; the editor does that.
* `languages.onDidRenameSymbol(e => …)` runs after a symbol is renamed. `e` is `{ projectPath, path, languageId, oldName, newName, changedFiles }`. New in this release.

```js
saturn.languages.registerDocComment('csharp', ({ trigger }) =>
  trigger === '///' ? '/// <summary>\n/// ${1}\n/// </summary>' : null);
```

### tests

* `tests.registerController(id, { label, discover(ctx), run(ctx) })` adds tests to the Tests panel. Requires `contributes.tests: true`. See [Tests](tests.md).

### languageServer

* `languageServer.restart()` restarts your module's language server, for example after a setting changes.

### settings

* `settings.get(id)` uses the `id` from `contributes.settings`, without any prefix.
* `settings.onChange(values => …)`. Previews also re-render when your settings change.

## Publishing

Zip the extension folder's contents and publish it through the [Marketplace](../marketplace.md).
