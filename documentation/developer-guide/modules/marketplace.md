---
icon: basket-shopping
---

# Marketplace

The Saturn Marketplace is a set of JSON files in the [Saturn-Editor/Marketplace](https://github.com/Saturn-Editor/Marketplace) GitHub repository. There is no server: Saturn reads the index straight from GitHub.

{% hint style="info" %}
This page describes index schema 2.
{% endhint %}

## How the index works

Saturn fetches:

```
https://raw.githubusercontent.com/Saturn-Editor/Marketplace/refs/heads/main/index.json
```

```json
{
  "schema": 2,
  "languages": "languages.json",
  "themes": "themes.json",
  "extensions": "extensions.json"
}
```

Each list file sits next to `index.json` and is a JSON array of entries. Saturn only downloads the list for the section you're viewing.

## Entries

| Key | Required | Description |
| --- | --- | --- |
| id | Yes | Lowercase `a-z`, `0-9`, `.` and `-`, starting with a letter or digit. Must be unique across languages, extensions and themes. |
| name | Yes | Display name. |
| description | Yes | Short description. |
| author | Yes | Author name. |
| repo | Yes | The GitHub repository, as `owner/name`. |
| versions | Yes | 1 to 10 versions, newest first. See below. |
| image | No | An `https` URL for the image shown in the marketplace. |
| default | No | Languages only. When `true`, the language is pre-ticked in the "Welcome to Saturn" popup on first launch. |

Each item in `versions`:

| Key | Description |
| --- | --- |
| version | Semver version (`x.y.z`). Must match the `version` in the module's manifest. |
| saturnApi | Caret range such as `^0.2`. Required for languages and extensions. Not allowed for themes. |
| download | A `.zip` GitHub Release asset from the entry's own repo: `https://github.com/<repo>/releases/download/...` |
| sha256 | SHA-256 of the zip, as 64 lowercase hex characters. |

## Packaging

The zip must contain the module folder's **contents** at its root. Don't put them inside a subfolder.

| Module type | Manifest |
| --- | --- |
| Language | `saturn-language.json` |
| Extension | `saturn-extension.json` (see [Configuration](extensions/configuration.md)) |
| Theme | `theme.json` (see [Themes](themes.md)) |

* The manifest's `id` and `version` must match the marketplace entry and version.
* Put `icon.svg` (or `icon.png`) at the zip root to use it as the module's picture.
* Any `icon` paths in the manifest must be relative `.svg` or `.png` files inside the zip. Subfolders are fine. `..` and absolute paths are not.
* Zips can be at most 50 MB.

## Versioning and compatibility

The current module API version is **0.2.1** (`^0.2`).

Saturn installs the newest version whose `saturnApi` matches the running Saturn. If a newer version needs a newer Saturn, the Update button is disabled with "You need to update Saturn to install version X".

When you release an update, **add** a new version to the top of `versions`; don't replace the old one. That way, older Saturn installs can still get a compatible release. Keep at most 10 versions and drop the oldest.

## Publishing

1. Publish a GitHub Release in your module's repository with the `.zip` attached.
2. Get the zip's SHA-256:
   * macOS: `shasum -a 256 module.zip`
   * Linux: `sha256sum module.zip`
3. Open a pull request to [Saturn-Editor/Marketplace](https://github.com/Saturn-Editor/Marketplace) that adds your entry, or a new version, to the right list file.
4. The **Validate index** check runs automatically. It downloads every new or changed version and checks the size, the SHA-256 and the manifest.
5. A maintainer reviews and merges your pull request.

You can run the same checks locally from the Marketplace repository:

```bash
node scripts/validate.mjs --offline   # structure only
node scripts/validate.mjs --all       # structure, plus download and verify every version
```
