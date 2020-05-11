# Code Editors

## Web Editors

-   [Code Sandbox](https://codesandbox.io/)
-   [StackBlitz](https://stackblitz.com/)

## Visual Studio Code

### Setup as GIT editor

-   [Setup VS Code as GIT editor](https://stackoverflow.com/questions/30024353/how-to-use-visual-studio-code-as-default-editor-for-git)

### Extensions

#### Extentions I use (21/4/20)

```text
  Angular-BeastCode v8.1.2
  Angular2 v9.1.2
  AngularMaterial v7.0.1
  bracket-pair-colorizer-2 v0.0.29
  code-settings-sync v3.4.3
  code-spell-checker v1.8.0
  debugger-for-chrome v4.12.6
  gitignore v0.6.0
  gitlens v10.2.1
  json-to-ts v1.7.5
  markdown-all-in-one v2.8.0
  ng-template v0.901.2
  NgRxSnippets v0.4.0
  npm-intellisense v1.3.0
  prettier-vscode v4.4.0
  project-manager v10.11.0
  vscode-icons v10.1.1
  vscode-markdownlint v0.34.0
  vscode-node-readme v3.0.2
  vscode-npm-script v0.3.11
  vscode-readme-pattern v1.2.0
  vscode-sort-json v1.18.0
  vscode-typescript-tslint-plugin v1.2.3
  vscode-yaml v0.7.2
```

#### Markdown All in One

This extension is highly recommended for markup, since it auto updates table of contents, make editing easier and have shortcuts: [vs code markdown all in one](https://github.com/yzhang-gh/vscode-markdown)

#### Markdown

-   Markdown auto linting extention [markdownlint by davidanson](https://github.com/DavidAnson/vscode-markdownlint.git)

#### Prettier

Use [prettier](https://prettier.io/docs/en/install.html) to follow a set of good and consistent styling rules for auto-formatting ([article: Code style matters](https://christianlydemann.com/style-angular-apps-using-prettier-and-tslint/)).

Install

```bash
npm install --save-dev --save-exact prettier
```

`.prettierrc` make this project file in root and put options here

```json
{
    "singleQuote": true
}
```

If your using TSLint and Prettier, then use [tslint-config-prettier](https://github.com/prettier/tslint-config-prettier) to make TSLint only do TS errors/warnings and Prettier only do the formatting corrections. This way TSLint don't conflict with prettier formatting.
Install and add `tslint-config-prettier` to extends in tslint.json:

```bash
npm install --save-dev tslint-config-prettier
```

`tslint.json`

```json
{
    "extends": ["tslint:recommended", "tslint-config-prettier"],
    "//": "..."
}
```

To format whole codebase easily, install prettier to dev-dependencies and add the scripts to `package.json`

```json
"scripts": {
  "//": "...",
  "prettier:test": "prettier \"**/*.{ts,html,md,scss,json}\" --list-different",
  "prettier:write": "prettier \"**/*.{ts,html,md,scss,json}\" --write"
}
```

#### Debugger for Chrome

Install `debugger-for-chrome` extension. Press F5 to start. If no launch.json file is found, it will give you an option to create it. The easiest way is to auto create a `launch` config, and just change the port. Place a breakpoint and press F5 to start the debugger.

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:4200",
            "webRoot": "${workspaceFolder}"
        }
    ]
}
```

If you want to make `attach` to Chrome, you need to add `--remote-debugging-port=9222` to a Chrome shortcut. This might make your chrome login not being able to sync because testing is not allowed, so better to make a copy of your chrome shortcut, and add it here with no profile login.
