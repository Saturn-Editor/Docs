---
icon: book
---

# Notebooks

A notebook is a folder of pages for notes, plans and documentation, with text formatting, lists, tables and images.

## Creating and opening notebooks

In the Welcome window, open the **Notebooks** tab:

* **New:** enter a name to create a notebook.
* **Open:** choose an existing notebook folder.
* Search your notebooks.
* Right-click a notebook to **Open** it, **Remove from list** or **Move to Trash**.

New notebooks go in the folder set in [Settings](settings.md#notebooks) → Notebooks. You can also open recent notebooks from the project name's menu in the [top bar](editor.md#top-bar).

## The notebook window

A notebook opens in Saturn's normal window, with some parts hidden:

* **Kept:** the explorer (titled "Notebook"), the top bar (without Run or Debug), and the Commit and Git panels so you can sync the notebook with [Git](git.md).
* **Hidden:** the tab strip, terminal, debug panels, Notices, Usages, Outline and language modules.

## Pages

Each page is a `<Title>.snp` file. The explorer hides the `.snp` extension and the `notebook.json` file.

* Use **New** → **Page** or **Folder** to add one. New names are made unique, for example "Untitled 2".
* To add a sub-page, right-click a page and choose **New Sub-page**, or type `/` in a page and choose **Page**. Sub-pages are stored in a folder named after the page, and the explorer shows them nested under it.
* Renaming, moving, copying or deleting a page does the same to its sub-pages. **Undo** restores both.

## Writing

Pages save automatically.

* **Title:** edit the title at the top of the page to rename it.
* **Formatting:** select text to show a toolbar with Bold, Italic, Underline, Strikethrough, Code, left/centre/right alignment and Link. A link can go to a URL or to another page in the notebook.
* **Blocks:** type `/` to add Text, Heading 1–3, a bulleted, numbered or to-do list, Quote, Code block, Table, Divider, Image or Page.
* **Images:** paste or drop them into the page. Each image can be up to 5 MB and is stored inside the page.

## Page style

Click **Aa** on a page to set its font, background and text colour. Choose **Use defaults** to go back to your default page style, or **Change theme…** to pick a different theme.

Set the default page style for every page in [Settings](settings.md#notebooks) → Notebooks.

## Known limits

* Links to a page stop working if the page is renamed or moved.
* There's no helper to set up sync yet. Use the Commit and Git panels.
* A notebook's name comes from its folder name.
