# Visual Studio Code

## Contents

- [Visual Studio Code](#visual-studio-code)
  - [Contents](#contents)
  - [Web guides](#web-guides)
  - [Extensions](#extensions)
    - [Extensions: Extentions I use](#extensions-extentions-i-use)
    - [Extensions: Markdown All in One](#extensions-markdown-all-in-one)
    - [Extensions: Markdown](#extensions-markdown)
    - [Extensions: Prettier](#extensions-prettier)

## Web guides

- [Setup VS Code as GIT editor](https://stackoverflow.com/questions/30024353/how-to-use-visual-studio-code-as-default-editor-for-git)

## Extensions

### Extensions: Extentions I use

- angular-essentials v9.0.1
- Angular2 v9.1.2
- bracket-pair-colorizer-2 v0.0.29
- code-settings-sync v3.4.3
- debugger-for-chrome v4.12.6
- gitignore v0.6.0
- gitlens v10.2.1
- json-to-ts v1.7.4
- markdown-all-in-one v2.7.0
- ng-template v0.900.18
- npm-intellisense v1.3.0
- prettier-vscode v3.20.0
- project-manager v10.11.0
- typescript-hero v3.0.0
- vscode-icons v10.0.0
- vscode-markdownlint v0.34.0
- vscode-npm-script v0.3.11
- vscode-sort-json v1.18.0
- vscode-typescript-tslint-plugin v1.2.3
- vscode-yaml v0.7.2

### Extensions: Markdown All in One

This extension is highly recommended for markup, since it auto updates table of contents, make editing easier and have shortcuts: [vs code markdown all in one](https://github.com/yzhang-gh/vscode-markdown)

### Extensions: Markdown

- Markdown auto linting extention [markdownlint by davidanson](https://github.com/DavidAnson/vscode-markdownlint.git)

### Extensions: Prettier

Use prettier to follow a set of styling rules for auto-formatting ([article: Code style matters](https://christianlydemann.com/style-angular-apps-using-prettier-and-tslint/)). VS Code settings for prettier might sometimes not work, instead make project file in root `.prettierrc` and put options here

```json
{
  "printWidth": 120,
  "singleQuote": true
}
```

If your using TSLint and Prettier, then use [tslint-config-prettier](https://github.com/prettier/tslint-config-prettier) to make TSLint only do TS errors/warnings and Prettier only do the formatting corrections. This way TSLint don't conflict with prettier formatting.
Install `npm install --save-dev tslint-config-prettier` and add "tslint-config-prettier" to extends in tslint.json:

```json
{
  "extends": ["tslint:recommended", "tslint-config-prettier"],
  "rules": {
    "align": false,
    "array-type": false,
    "arrow-parens": false,
    "deprecation": {
      "severity": "warning"
    },
    "component-class-suffix": true,
    "contextual-lifecycle": true,
    "directive-class-suffix": true,
    "directive-selector": [true, "attribute", "app", "camelCase"],
    "component-selector": [true, "element", "app", "kebab-case"],
    "import-blacklist": [true, "rxjs/Rx"],
    "interface-name": false,
    "max-classes-per-file": false,
    "max-line-length": [true, 140],
    "member-access": false,
    "member-ordering": [
      true,
      {
        "order": ["static-field", "instance-field", "static-method", "instance-method"]
      }
    ],
    "no-consecutive-blank-lines": false,
    "no-console": [true, "debug", "info", "time", "timeEnd", "trace"],
    "no-empty": false,
    "no-inferrable-types": [true, "ignore-params"],
    "no-non-null-assertion": true,
    "no-redundant-jsdoc": true,
    "no-switch-case-fall-through": true,
    "no-var-requires": false,
    "object-literal-key-quotes": [true, "as-needed"],
    "object-literal-sort-keys": false,
    "ordered-imports": false,
    "quotemark": [true, "single"],
    "trailing-comma": false,
    "no-conflicting-lifecycle": true,
    "no-host-metadata-property": true,
    "no-input-rename": true,
    "no-inputs-metadata-property": true,
    "no-output-native": true,
    "no-output-on-prefix": true,
    "no-output-rename": true,
    "no-outputs-metadata-property": true,
    "template-banana-in-box": true,
    "template-no-negated-async": true,
    "use-lifecycle-interface": true,
    "use-pipe-transform-interface": true
  },
  "rulesDirectory": ["codelyzer"]
}
```