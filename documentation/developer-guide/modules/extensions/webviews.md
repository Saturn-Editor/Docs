# Webview panels

A webview panel shows your own HTML instead of a tree.

## Declaring the panel

Set `kind` to `"webview"` in `contributes.panels`:

```json
"panels": [{ "id": "myext.view", "title": "My View", "dock": "bottom", "kind": "webview" }]
```

## Filling the panel

```js
export function activate(saturn) {
  saturn.webviews.register('myext.view', {
    html: (ctx) => `
      <button id="ping">Ping</button>
      <script>
        document.getElementById('ping').onclick = () => saturn.postMessage({ type: 'ping' });
        saturn.onMessage((message) => console.log(message));
      </script>`,
    onMessage: (message, ctx) => {
      if (message.type === 'ping') {
        saturn.webviews.postMessage('myext.view', { type: 'pong' }, ctx.projectPath);
      }
    },
  });
}
```

* `html(ctx)` returns the page's HTML, or a promise of it.
* `onMessage(message, ctx)` receives messages that the page sends. It can be async.
* `webviews.postMessage(panelId, message, projectPath?)` sends a message to the page. Pass `projectPath` to send it to that project's window, or leave it out to send it to every window.
* `webviews.refresh(panelId)` calls `html` again and reloads the page.

## Inside the page

The page runs in a sandboxed iframe that only allows scripts. Its Content Security Policy allows inline scripts and styles, and images from `data:` and `https:`. It has no network, Node or Saturn access. It has:

* `saturn.postMessage(message)` to send a message to your module
* `saturn.onMessage(listener)` to receive messages from your module
* CSS variables with the current theme's colours: `--saturn-app`, `--saturn-editor`, `--saturn-panel`, `--saturn-raised`, `--saturn-divider`, `--saturn-border`, `--saturn-text`, `--saturn-strong`, `--saturn-muted`, `--saturn-accent`, `--saturn-on-accent`, `--saturn-selection`, `--saturn-hover`, `--saturn-error` and `--saturn-warning`

The page reloads when the theme changes, so keep any state in your module, not in the page.
