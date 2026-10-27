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

## Saturn API (0.2.0)

### commands

* `commands.register(id, ctx => …)` runs the code for a command listed in `contributes.commands`.

### panels

* `panels.registerTree(id, { getChildren(parentId, ctx), onAction?(actionId, item, ctx) })` fills a panel listed in `contributes.panels`. For the root, `parentId` is `null` or `undefined`.
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

### run

* `run.registerProvider(ctx => [{ name, command }])` adds entries to the Run menu. Each one runs in a Terminal tab. The provider can be async. Requires `contributes.runConfigurations: true`.

### terminal

* `terminal.run(projectPath, { name, command })` opens a new Terminal tab in that project's window and runs the command in the user's shell.

### workspace

* `workspace.findFiles(…)`
* `workspace.openFile(projectPath, path, line?, column?)`
* `workspace.onDidChangeFiles(e => …)`

### projects

* `projects.onDidClose(ctx => …)`

### window

* `window.showMessage(message, { kind?: 'info' | 'warning' | 'error', projectPath? })` shows a message in the status bar.

### settings

* `settings.get(id)` uses the `id` from `contributes.settings`, without any prefix.
* `settings.onChange(values => …)`

### platform

* `platform`

### Not available yet

* Webview or HTML panels
* Stopping a terminal from code
* Reading the active file or selection

## Publishing

Zip the extension folder's contents and publish it through the [Marketplace](../marketplace.md).
