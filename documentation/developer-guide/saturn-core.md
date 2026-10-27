---
icon: code
---

# Saturn-Core

{% hint style="info" %}
Saturn-Core is the editor's main software. If you are looking to develop modules for Saturn, see [Saturn-API](saturn-api.md).
{% endhint %}

Saturn is a code editor built with Electron, Preact and Tailwind. The source code is at [github.com/Saturn-Editor/Saturn](https://github.com/Saturn-Editor/Saturn).

## Architecture

| Process | Role |
| --- | --- |
| Main | Windows, files, git, language servers, debuggers and updates. |
| Preload | Provides the `window.saturn` API to the interface. |
| Renderer | The interface, including the CodeMirror 6 editor. |
| Extension host | A separate process that runs code from language modules and extensions. |

## Building and running

```bash
npm install
npm run dev        # start Saturn in development mode
npm run typecheck
npm test           # Vitest unit tests (end-to-end tests use Playwright)
npm run dist       # build installers with electron-builder
```

`npm run dist` builds a dmg and zip on macOS, an NSIS installer on Windows and an AppImage on Linux.

## Releases

Pushing a `v*` tag runs a GitHub Actions workflow that publishes a GitHub Release. Saturn updates itself from GitHub Releases using electron-updater. Unsigned macOS builds show a Download link instead of updating automatically.

## Repository layout

| Path | Contents |
| --- | --- |
| `src/main` | The main process: `SaturnApp.ts`, `IpcRouter.ts`, `WindowManager.ts`, `Services/`, `Store/`, `Helpers/` |
| `src/preload/index.ts` | The preload script. |
| `src/host/index.ts` | The extension host and the `saturn` API passed to modules. |
| `src/renderer/src` | The interface: `screens/`, `editor/`, `panels/`, `modules/`, `components/`, `commands/`, `preview/`, `theme/` |
| `src/shared` | Code shared between processes: `api.ts`, `types.ts`, `modules.ts`, `keymaps.ts`, `marketplace.ts` |
| `resources/` | Icons and built-in themes. |
| `docs/superpowers/specs` | The design document. |
