---
icon: flask
---

# Tests

The **Tests** panel, in the bottom dock, runs your project's tests. Tests come from [language](languages.md) modules that support them. If none of your installed modules do, the panel says so.

## The Tests panel

Tests are shown in a tree, grouped by test runner. Each test is marked as passed, failed, skipped or running.

* **Run all** runs every test.
* **Run failed** runs the tests that failed last time.
* **Find tests again** reloads the list.
* Hover over a test or group and click its run button to run just that one.
* Type in the filter box to narrow the list.

## Test sets

A test set is a saved group of tests that you can run together.

* **Save a set:** Cmd-click (macOS) or Ctrl-click to select several tests, then click **Save as set…**. Enter a **Test set name** and click **Save**.
* **Use a set:** choose it from the dropdown, which shows **All tests** by default. The tree then shows only that set's tests, and the run button becomes **Run the _name_ set**.
* **Delete a set:** choose it, then click the ✕ button (**Delete the _name_ set**).

Test sets are saved in the project's `.saturn` folder, so you can commit them and share them with your team.

## Results

A summary shows how many tests passed and failed. Select a test to see its failure message in the output pane, or see the log of the whole run.

## In the editor

When a module knows where a test is, a run button appears in the margin beside it. After a run, it changes to ✓ or ✕.
