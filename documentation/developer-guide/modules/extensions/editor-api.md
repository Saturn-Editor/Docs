# Editor API

The `saturn.editor` API lets a module read the files the user has open in a project's window.

## Reading files

These calls return straight away.

* `editor.activeFile(projectPath)` returns the file in the focused editor with its `selection`, or `null` if there isn't one.
* `editor.openFiles(projectPath)` returns every open file.
* `editor.file(projectPath, path)` returns one open file, or `null`.

An open file looks like this:

```js
{
  path,
  languageId,  // or null
  text,        // the current text, including unsaved changes
  dirty,       // true if there are unsaved changes
  selection    // activeFile only: { text, start: { line, character }, end: { line, character } } or null
}
```

Lines and characters are 0-based.

## Events

| Event | Listener receives | When |
| --- | --- | --- |
| `onDidChangeActive` | `{ projectPath, file }` | The user switched to another file. |
| `onDidChangeSelection` | `{ projectPath, path, selection }` | About 150 ms after the selection changes. |
| `onDidChangeFile` | `{ projectPath, file }` | About 300 ms after the user stops typing. |
| `onDidCloseFile` | `{ projectPath, path }` | A file was closed. |

## Example

```js
export function activate(saturn) {
  saturn.commands.register('myext.countSelection', (ctx) => {
    const file = saturn.editor.activeFile(ctx.projectPath);
    if (!file?.selection) return;
    saturn.window.showMessage(`${file.selection.text.length} characters selected`, {
      projectPath: ctx.projectPath,
    });
  });
}
```
