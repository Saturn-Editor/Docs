---
icon: palette
---

# Themes

A theme is a `theme.json` file that sets Saturn's interface colours and syntax highlighting. Themes have no code and no `saturnApi`.

## theme.json

| Key | Required | Description |
| --- | --- | --- |
| id | Yes | Must match the theme's [Marketplace](marketplace.md) entry. |
| version | Yes | Must match the Marketplace version. |
| name | Yes | Display name. |
| description | No | Short description. |
| light | One of `light` or `dark` | The light variant. |
| dark | One of `light` or `dark` | The dark variant. |

Saturn's Light/Dark toggle chooses which variant to use. Each variant has `colors` and `syntax`.

### colors

You must set all 13 tokens. Each value is a Tailwind colour, such as `neutral-900`, `white` or `amber-500/18`.

`app`, `editor`, `panel`, `raised`, `divider`, `border`, `text`, `strong`, `muted`, `accent`, `on-accent`, `selection`, `hover`

### syntax

A list of rules:

| Key | Description |
| --- | --- |
| scope | A TextMate scope. Separate scopes with spaces to match ancestors. |
| color | Optional. A Tailwind colour or a token name such as `text`. |
| fontStyle | Optional. Any mix of `bold`, `italic`, `underline` and `strikethrough`, separated by spaces. Use `""` to reset. |

A rule for a scope such as `keyword` also covers more specific scopes such as `keyword.control`. Rules for more specific scopes override it. Leave out `color` to keep the surrounding colour. Syntax rules only set the text colour and font style. They can't set backgrounds.

The Markdown preview doesn't use syntax rules. It uses the 13 colour tokens, which are available as `--saturn-*` CSS variables.

## Example

A shortened version of the VS theme:

```json
{
  "id": "vs",
  "name": "VS",
  "version": "1.1.2",
  "description": "A theme based on Visual Studio.",
  "light": {
    "colors": {
      "app": "neutral-100", "editor": "white", "panel": "neutral-50", "raised": "white",
      "divider": "neutral-200", "border": "neutral-300", "text": "neutral-800",
      "strong": "neutral-950", "muted": "neutral-500", "accent": "sky-600",
      "on-accent": "white", "selection": "sky-200/70", "hover": "black/5"
    },
    "syntax": [
      { "scope": "comment", "color": "green-700" },
      { "scope": "keyword.control", "color": "purple-700" },
      { "scope": "markup.heading", "color": "blue-800", "fontStyle": "bold" },
      { "scope": "meta.embedded", "color": "text" },
      { "scope": "comment.block.documentation entity.name.tag", "color": "neutral-500" }
    ]
  },
  "dark": { "colors": { }, "syntax": [ ] }
}
```

## Publishing

The zip only needs `theme.json` at its root. Publish it as a GitHub Release, then add it to the Marketplace. See [Marketplace](marketplace.md).

Users install themes from the [Marketplace](../../user-guide/marketplace.md).
