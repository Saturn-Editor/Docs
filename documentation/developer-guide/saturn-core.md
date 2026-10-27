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
| Main | Windows, files, git, language servers, debuggers and updates. `SaturnApp.ts` wires everything together, `IpcRouter.ts` routes IPC and `WindowManager.ts` manages windows. |
| Preload | `src/preload/index.ts` provides the `window.saturn` API to the interface. Its type is `SaturnApi` in `src/shared/api.ts`. |
| Renderer | The interface, built with Preact, signals and Tailwind v4, plus the CodeMirror 6 editor. |
| Extension host | A `utilityProcess` (`src/host/index.ts`) that runs code from language modules and extensions. The main process talks to it over RPC (`src/shared/rpc.ts`). |

IPC channels are named `saturn:<area>.<name>`.

### Main process services

| Service | Role |
| --- | --- |
| `CodeModuleService` | Installs, enables and validates modules, and builds the contribution snapshot sent to windows. |
| `LanguageServerService` | Runs language servers over stdio using LSP (`vscode-jsonrpc`). |
| `DebugService` | A Debug Adapter Protocol (DAP) client. |
| `GitService` | Runs the system `git`. Credential prompts go through `AskpassService`. |
| `MarketplaceService` | Reads the Marketplace index and installs modules. |
| `ThemeService` | Loads built-in and installed themes. |
| `UpdateService` | Checks for and installs Saturn updates. |
| `NotificationService` | Shows notifications. |
| `TrashService` | Deletes files with Undo. |

Stores are JSON files written atomically by `FileStore`. There are stores for settings, recent projects, modules, module settings, layouts, run configurations, breakpoints and language choices.

### Startup

1. When the app is ready, Saturn resolves its paths (`AppPaths`) and creates the stores and services.
2. The askpass server starts.
3. The IPC router registers its handlers.
4. Saturn loads modules. The active languages are passed to the language server and debug services, the extension host restarts with the active modules, and every window receives the contribution snapshot.
5. Saturn starts checking for updates to itself and to installed modules.
6. The Welcome window opens. On first launch, the onboarding popup opens too.

Installing, enabling or removing a module reloads modules in the same way, without restarting Saturn.

### Windows

Saturn has Welcome, Editor (one per project), Settings, New Project, Marketplace and Onboarding windows.

## Building and running

```bash
npm install
npm run dev        # start Saturn in development mode
npm run typecheck
npm test           # Vitest unit tests
npm run e2e        # build, then run the Playwright end-to-end tests
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
| `docs/superpowers/specs` | The design document, which is kept up to date. |
