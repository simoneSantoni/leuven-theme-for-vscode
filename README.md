# Leuven for VS Code

A port of Fabrice Niessen's Emacs themes **leuven** and **leuven-dark** to
Visual Studio Code.

The extension ships two color themes:

| Theme         | Type  | Background | Foreground |
| ------------- | ----- | ---------- | ---------- |
| `Leuven`      | light | `#FFFFFF`  | `#333333`  |
| `Leuven Dark` | dark  | `#25202a`  | `#cfccd2`  |

## What the port covers

- **Editor syntax colors.** Each TextMate scope maps to the Emacs face that
  colors the same construct, such as `font-lock-keyword-face`,
  `font-lock-string-face` or `font-lock-function-name-face`. The rule names in
  the theme files say which face each rule comes from.
- **Emacs-style restraint.** As in Emacs, only function *definitions* get
  color. Function calls and operators stay in the plain foreground color.
- **Semantic highlighting** is on. Language servers that send semantic tokens
  (TypeScript, Python/Pylance, Rust and others) get the same mapping: blue
  builtins and `self`, red constants and enum members, purple types.
- **Markup.** Markdown headings, links, inline code, quotes and diff
  insertions and deletions use the colors of the matching `org-*` and
  `markdown-*` faces.
- **Org mode (Leuven Dark).** With an Org mode extension that provides a
  TextMate grammar, headings 1–6 get their own `org-level-N` colors. `TODO`,
  `NEXT`/`WAIT`/`HOLD` and `DONE` keywords get the colors of their Emacs chips,
  with an underline in place of the chip background.
- **Workbench.** The activity bar, side bar, tabs, status bar, panels, widgets,
  diff editor, git decorations and the integrated terminal's ANSI palette are
  all colored to match.

### Changes from the Emacs originals

Leuven Dark makes a few deliberate changes to stock `leuven-dark`:

- Comments are a lighter grey (`#9a969e`) so they are easier to read.
  Comment delimiters keep that grey but are not italic.
- Org heading levels 1–3 use cyan, keyword yellow and teal.
- Invalid code is shown in red. Stock leuven-dark rotates the hue of the light
  theme's red, which turns it cyan.

## Installation

The extension is not on the Visual Studio Marketplace or Open VSX yet. Install
it from this repository in one of the two ways below. Both need VS Code 1.70
or later.

### Option 1: build and install a VSIX (recommended)

You need [Node.js](https://nodejs.org/) for `npx`.

```sh
git clone https://github.com/simoneSantoni/leuven-theme-for-vscode.git
cd leuven-theme-for-vscode
npx @vscode/vsce package
code --install-extension leuven-for-vscode-0.1.0.vsix
```

You can also install the `.vsix` from the UI. In the Extensions view
(`Ctrl+Shift+X`), open the `…` menu and choose **Install from VSIX…**.

To uninstall, run:

```sh
code --uninstall-extension simonesantoni.leuven-for-vscode
```

### Option 2: link the source folder (for hacking on the theme)

VS Code loads any extension folder it finds in its extensions directory. If
you link the clone there, edits to the theme files show up after you reload
the window.

```sh
# Linux / macOS
ln -s "$PWD" ~/.vscode/extensions/simonesantoni.leuven-for-vscode-0.1.0
```

```powershell
# Windows (PowerShell)
New-Item -ItemType Junction `
  -Path "$env:USERPROFILE\.vscode\extensions\simonesantoni.leuven-for-vscode-0.1.0" `
  -Target (Get-Location)
```

Restart VS Code once so it picks up the new extension. After that,
**Developer: Reload Window** applies further changes. If you use VSCodium,
replace `~/.vscode/extensions` with `~/.vscode-oss/extensions`.

## Selecting the theme

Open **Preferences: Color Theme** (`Ctrl+K Ctrl+T`, or `Cmd+K Cmd+T` on
macOS) and pick **Leuven** or **Leuven Dark**. You can also set it in
`settings.json`:

```jsonc
{
  "workbench.colorTheme": "Leuven"
}
```

To switch between the two themes automatically when your OS switches between
light and dark mode:

```jsonc
{
  "window.autoDetectColorScheme": true,
  "workbench.preferredLightColorTheme": "Leuven",
  "workbench.preferredDarkColorTheme": "Leuven Dark"
}
```

## Customizing

You can override any color for one theme without editing the extension. Put
the overrides in your user `settings.json`, under the theme name in square
brackets:

```jsonc
{
  "workbench.colorCustomizations": {
    "[Leuven Dark]": {
      "editor.lineHighlightBackground": "#2a2433"
    }
  },
  "editor.tokenColorCustomizations": {
    "[Leuven]": {
      "comments": "#7a7a72"
    }
  },
  "editor.semanticTokenColorCustomizations": {
    "[Leuven]": {
      "rules": { "function": "#006699" }
    }
  }
}
```

To see which scope or semantic token is coloring a piece of code, place the
cursor on it and run **Developer: Inspect Editor Tokens and Scopes**.

## Repository layout

```
package.json                         extension manifest; registers both themes
themes/leuven-color-theme.json       Leuven (light)
themes/leuven-dark-color-theme.json  Leuven Dark
```

Each theme file has three sections. `colors` holds the workbench colors.
`tokenColors` holds the TextMate rules, and each rule is named after its Emacs
face. `semanticTokenColors` holds the semantic token rules.

## Credits

The color palettes come from the [leuven and leuven-dark themes][leuven] by
Fabrice Niessen. Both themes are distributed with GNU Emacs.

[leuven]: https://github.com/fniessen/emacs-leuven-theme

## License

Copyright (C) 2026 Simone Santoni

This theme is a derivative of leuven and leuven-dark, which are licensed under
the GNU General Public License. It is free software: you can redistribute it
and/or modify it under the terms of the GNU General Public License as published
by the Free Software Foundation, either version 3 of the License, or (at your
option) any later version. See [LICENSE](LICENSE) for the full text.
