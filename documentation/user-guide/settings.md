---
icon: gear
---

# Settings

Settings has these sections: Appearance, Editor, Languages, Extensions, Notebooks, Keymap, Marketplace and Advanced.

In the Settings window, the sections are in a sidebar. In the Welcome window, they show as tabs.

## Appearance

* **Theme:** choose a theme. Saturn includes **High Contrast** and **Dyslexia Friendly** themes, each with light and dark variants. Dyslexia Friendly uses the OpenDyslexic font for interface text, keeps a monospaced font for code, and has a cream editor background in light mode. Install more themes from the [Marketplace](marketplace.md).
* **Light / Dark:** choose light or dark mode, or turn on **Sync with OS**.
* **Text size:** 80%, 90%, 100%, 110%, 125% or 150%. This changes the text size in every Saturn window.

## Editor

### Behaviour

* **Indentation:** spaces or tabs, with a size of 2, 4 or 8.
* **Auto-indent**
* **Auto-close** brackets and quotes: `(` `[` `{` `<` `'` `"`
* **Format on save**
* **Auto-save:** While typing, When I stop or close, or Off.
* **Encoding:** UTF-8, UTF-16, US-ASCII or ISO-8859-1.

### SmartCode

Turn code completion and error detection on or off.

## Languages

Each installed [language](languages.md) has its own tab, with:

* the language's settings
* **Default view:** for each file type that has a preview, open it as Code, Split or Preview
* **Language features:** a switch for each feature, or "Not supported" if the language doesn't provide it

## Extensions

Each installed extension with settings has its own tab.

## Notebooks

Settings for [notebooks](notebooks.md).

* **Storage:** the folder new notebooks go in. Click **Change…** to pick another folder, or **Use default**. When you change it, Saturn offers to move the notebooks in the previous folder too. Notebooks that are open aren't moved, and Saturn tells you which ones.
* **Default page style:**
  * **Font:** Use theme default, Sans-serif, Serif, Monospaced or OpenDyslexic.
  * **Background** and **Text:** Use theme default, one of five swatches, or a colour you pick.
  * **Reset to theme defaults**

There's also a link to the themes in Appearance.

## Keymap

Choose the keyboard shortcuts for your operating system (Current OS), or for IntelliJ, Visual Studio or VS Code.

To make your own keymap, click **Duplicate**. Then click a shortcut and press the keys you want to record it. You can also clear a shortcut, or reset it to the original.

## Marketplace

The URL of the Marketplace index.

## Advanced

* **Check for updates automatically:** covers Saturn and your installed languages, extensions and themes.
* **Check for updates:** check now.
* **Git: when switching branches with changes:** Ask each time, Bring them along, or Shelve for that branch. See [Git](git.md).
* **Projects:** the default folder for New Project and Clone. Click **Change…** to pick another folder, or **Use default**. The default is `~/SaturnProjects` if it exists, otherwise your home folder.
* **Debugging: Show the debug panels:** In the project window, or In a separate window. See [Debugging](debugging.md#separate-window).
* **Reset Saturn**
