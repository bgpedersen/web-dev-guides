# Tools and externals

## Contents

- [Tools and externals](#tools-and-externals)
  - [Contents](#contents)
  - [JavaScript](#javascript)
    - [JavaScript: REST placeholders](#javascript-rest-placeholders)
  - [Images](#images)
    - [Images: Placeholders](#images-placeholders)
    - [Images: Free quality images](#images-free-quality-images)
    - [Images: Online photo editors](#images-online-photo-editors)
  - [Web inspiration](#web-inspiration)
    - [Web inspiration: Full website inspirations](#web-inspiration-full-website-inspirations)
    - [Web inspiration: Colors](#web-inspiration-colors)
  - [Code Editors](#code-editors)
    - [Code Editors: Web Editors](#code-editors-web-editors)
    - [Code Editors: Visual Studio Code](#code-editors-visual-studio-code)
      - [Code Editors: Visual Studio Code: Web guides](#code-editors-visual-studio-code-web-guides)
      - [Code Editors: Visual Studio Code: Extensions](#code-editors-visual-studio-code-extensions)
        - [Code Editors: Visual Studio Code: Extensions: Extentions I use](#code-editors-visual-studio-code-extensions-extentions-i-use)
        - [Code Editors: Visual Studio Code: Extensions: Markdown All in One](#code-editors-visual-studio-code-extensions-markdown-all-in-one)
        - [Code Editors: Visual Studio Code: Extensions: Markdown](#code-editors-visual-studio-code-extensions-markdown)
        - [Code Editors: Visual Studio Code: Extensions: Prettier](#code-editors-visual-studio-code-extensions-prettier)

## JavaScript

### JavaScript: REST placeholders

- [JSONPlaceholder - Fake Online REST API for Testing and Prototyping](https://jsonplaceholder.typicode.com/)

## Images

### Images: Placeholders

- [Generative Placeholders](https://generative-placeholders.glitch.me/)

### Images: Free quality images

- [Pexels](https://www.pexels.com/)
- [Pixabay](https://pixabay.com/)
- [Unsplash](https://unsplash.com/)

### Images: Online photo editors

- [Fotor](https://www.fotor.com/)
- [Image Compressor](Imagecompressor.com)

## Web inspiration

### Web inspiration: Full website inspirations

- [SiteInspire](https://www.siteinspire.com/)
- [Webdesign Inspiration](https://www.siteinspire.com/)
- [Awwwards](https://www.awwwards.com/)
- [Template monster](https://www.templatemonster.com/)

### Web inspiration: Colors

- [Flat Colors UI](http://www.flatcolorsui.com/)

## Code Editors

### Code Editors: Web Editors

- [Code Sandbox](https://codesandbox.io/)
- [StackBlitz](https://stackblitz.com/)

### Code Editors: Visual Studio Code

#### Code Editors: Visual Studio Code: Web guides

- [Setup VS Code as GIT editor](https://stackoverflow.com/questions/30024353/how-to-use-visual-studio-code-as-default-editor-for-git)

#### Code Editors: Visual Studio Code: Extensions

##### Code Editors: Visual Studio Code: Extensions: Extentions I use

- Angular2 v9.1.2
- AngularMaterial v7.0.1
- bracket-pair-colorizer-2 v0.0.29
- code-settings-sync v3.4.3
- debugger-for-chrome v4.12.6
- gitignore v0.6.0
- gitlens v10.2.1
- json-to-ts v1.7.4
- markdown-all-in-one v2.8.0
- ng-template v0.901.0
- npm-intellisense v1.3.0
- prettier-vscode v4.2.0
- project-manager v10.11.0
- vscode-icons v10.1.0
- vscode-markdownlint v0.34.0
- vscode-npm-script v0.3.11
- vscode-sort-json v1.18.0
- vscode-typescript-tslint-plugin v1.2.3
- vscode-yaml v0.7.2

##### Code Editors: Visual Studio Code: Extensions: Markdown All in One

This extension is highly recommended for markup, since it auto updates table of contents, make editing easier and have shortcuts: [vs code markdown all in one](https://github.com/yzhang-gh/vscode-markdown)

##### Code Editors: Visual Studio Code: Extensions: Markdown

- Markdown auto linting extention [markdownlint by davidanson](https://github.com/DavidAnson/vscode-markdownlint.git)

##### Code Editors: Visual Studio Code: Extensions: Prettier

Use prettier to follow a set of good and consistent styling rules for auto-formatting ([article: Code style matters](https://christianlydemann.com/style-angular-apps-using-prettier-and-tslint/)). VS Code settings for prettier might sometimes not work, instead make project file in root `.prettierrc` and put options here

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
  "//": "..."
}
```

To format whole codebase easily, install prettier `npm i prettier -D` and add the scripts to `package.json`:

```json
"scripts": {
  "//": "...",
  "prettier:test": "prettier \"**/*.{ts,html,md,scss,json}\" --list-different",
  "prettier:write": "prettier \"**/*.{ts,html,md,scss,json}\" --write"
}
```
