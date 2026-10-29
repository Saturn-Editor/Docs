# Tests

A module can add tests to Saturn's Tests panel. Set `contributes.tests` to `true` in your manifest, then register a controller.

```json
"contributes": { "tests": true }
```

## registerController

```js
saturn.tests.registerController(id, {
  label,              // shown as the group for your tests
  discover(ctx),      // returns TestItem[], or a promise of them
  run(ctx),           // can be async
});
```

`discover` gets `{ projectPath }`.

`run` gets:

| Key | Description |
| --- | --- |
| projectPath | The project folder. |
| ids | The ids of the tests to run, or `null` to run all of them. Groups are expanded to their leaf ids. |
| report(result) | Report a test's state. Call it with `running` when a test starts, then again when it finishes. |
| output(text) | Add text to the run's log. |

## TestItem

| Key | Required | Description |
| --- | --- | --- |
| id | Yes | Unique within your controller. |
| label | Yes | Shown in the Tests panel. |
| path | No | The absolute path of the file the test is in. |
| line | No | The test's line, starting at 1. With `path`, this adds a run button in the editor margin. |
| children | No | More `TestItem`s, to make this a group. |

## TestResult

| Key | Required | Description |
| --- | --- | --- |
| id | Yes | The test's id. |
| state | Yes | `running`, `passed`, `failed` or `skipped`. |
| message | No | The failure message, shown when the user selects the test. |
| durationMs | No | How long the test took. |

## Example

```js
export function activate(saturn) {
  saturn.tests.registerController('mylang.tests', {
    label: 'MyLang',
    discover: async ({ projectPath }) => [
      {
        id: 'math',
        label: 'math.test',
        children: [{ id: 'math/adds', label: 'adds numbers', path: `${projectPath}/math.test`, line: 3 }],
      },
    ],
    run: async ({ ids, report, output }) => {
      for (const id of ids ?? ['math/adds']) {
        report({ id, state: 'running' });
        output(`Running ${id}\n`);
        report({ id, state: 'passed', durationMs: 4 });
      }
    },
  });
}
```

Using the Tests panel activates your module if it declares `activationEvents`. See [Activation](../../saturn-api.md#activation).
