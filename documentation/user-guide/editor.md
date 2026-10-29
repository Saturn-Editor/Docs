---
icon: file-code
---

# Editor

## Top bar

Click the project name to open its menu:

* **New Project…**, **Open Project…** and **Close Project**
* **Switch to:** your recent projects
* **Notebooks:** your recent [notebooks](notebooks.md)

When you pick a project or notebook, choose whether to open it in **This window** or a **New window**.

The search box gets smaller in narrow windows.

## Split editor

You can split the editor into up to four panes, to the right or down. Choose **Split Right** from the tab bar's "…" menu, or press Cmd/Ctrl+\\. Drag tabs between panes to move files.

## Navigating code

When the file's [language](languages.md) supports them, you can use:

* **Outline:** a panel listing the symbols in the current file.
* **Find Usages:** find everywhere a symbol is used.
* **Rename:** rename a symbol everywhere it's used.
* **Format:** format the document.

These are also in the editor's right-click menu.

## Blame

Right-click in the editor and choose **Show Blame** to add a column beside the line numbers with each line's date and author. Hover over it to see the commit message, or click it to open that commit's diff for the file. Blame is based on the saved file. Choose **Hide Blame** to remove it.

## Syntax highlighting

Highlighting updates as you type. Files over 5000 lines aren't coloured.

## Files

Manage files in the Project panel. Right-click a file or folder for **Cut**, **Copy**, **Paste**, **Duplicate**, **Rename…** and **Delete**. Each menu item shows its shortcut.

While the Project panel has focus:

| Action | macOS | Windows / Linux |
| --- | --- | --- |
| Cut / Copy / Paste | Cmd+X / C / V | Ctrl+X / C / V |
| Duplicate | Cmd+D | Ctrl+D |
| Rename | F2 | F2 |
| Delete | Delete or Cmd+Backspace | Delete or Backspace |

Saturn doesn't ask before deleting. Instead, it shows an **Undo** option for 30 seconds, then moves the file or folder to the system Trash.

### Moving and copying

Drag a file or folder onto a folder to move it there. Hold Option (macOS) or Ctrl while dropping to copy it instead.

Moving never overwrites a file. If you copy into a folder that already has a file with that name, the copy is named "name copy.ext", then "name copy 2.ext" and so on.

Open tabs follow files when you move them.

### Changes outside Saturn

When a file is changed by another app, a terminal or a tool such as Claude Code, its tab reloads, as long as you haven't made unsaved edits in it. Previews reload too.

## New files

The **New** menu has the file types your languages provide. In a project, it also has **Notebook Page**.

## Terminal

Shift+Enter inserts a new line in programs that support it, such as Claude Code. (It sends ESC followed by Enter.)

## Closing a project

On macOS and Linux, if a terminal is still running a program when you close the project's window, Saturn asks: "_program_ is still running in a terminal." Choose **Close Anyway** or **Cancel**.
