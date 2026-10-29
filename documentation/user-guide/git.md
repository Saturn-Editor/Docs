---
icon: code-branch
---

# Git

Saturn uses the `git` installed on your computer.

## Committing

Use the **Commit** panel to see your changes and commit them. Open a file's diff to stage individual hunks rather than the whole file.

## Branches and tags

The **Git** panel lists branches and tags. You can rename and delete them from there.

When you switch branches with uncommitted changes, Saturn asks what to do with them:

* **Bring them along** to the new branch.
* **Shelve for that branch**, so they come back when you return to it.

To stop being asked, choose a default in [Settings](settings.md) → Advanced.

## History

The **Git** panel has two tabs:

* **Log:** the project's commits.
* **File History:** the commits that changed the file open in the editor. Click a commit to see that file's diff.

To see who last changed each line, use [blame](editor.md#blame).

## Interactive rebase

In the Log, right-click a commit and choose **Interactive rebase from here…**. The dialog lists the later commits, oldest first.

* Drag commits to reorder them.
* Set each one to **Pick**, **Squash**, **Fixup**, **Edit** or **Drop**.

Click **Start rebase**. The rebase stops at **Edit** commits and at conflicts; use the **Continue** and **Abort** bar to carry on. Rewording commit messages isn't supported yet.

## Resolving conflicts

In the **Commit** panel, hover over a conflicted file and click **Merge…**. The merge editor has three panes:

| Current branch | Result | Incoming |
| --- | --- | --- |
| Your version. | The merged file. You can edit it, and each conflict has buttons to accept either side. | The other branch's version. |

When no conflict markers are left, click **Save and mark resolved**. This saves the file and stages it.

## Other actions

* **Stash** changes and apply them later.
* **Merge**, **rebase**, **cherry-pick** and **revert**.
* **Remotes:** add and manage remotes.

## Signing in

If a remote needs a username and password, Saturn asks for them when you push, pull or fetch.

## Notebooks

You can sync a [notebook](notebooks.md) with the same Commit and Git panels.
