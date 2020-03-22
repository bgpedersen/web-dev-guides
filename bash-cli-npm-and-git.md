# Bash, cli, NPM and Git

## Contents

- [Bash, cli, NPM and Git](#bash-cli-npm-and-git)
  - [Contents](#contents)
  - [NPM](#npm)
    - [NPM: Remove packages not listed in package.json](#npm-remove-packages-not-listed-in-packagejson)
    - [NPM: Go to repository of package](#npm-go-to-repository-of-package)
    - [NPM: Package versions](#npm-package-versions)
  - [Git](#git)
    - [Git: Rebase](#git-rebase)
  - [GitHub](#github)
    - [GitHub: References](#github-references)
    - [GitHub: Mirror GitHub repository](#github-mirror-github-repository)
  - [Bash scripts](#bash-scripts)
    - [Bash scripts: Check code for debugger word](#bash-scripts-check-code-for-debugger-word)
    - [Bash scripts: Auto clean deleted branches](#bash-scripts-auto-clean-deleted-branches)
    - [Bash scripts: Rename folders and files to Kebabcase](#bash-scripts-rename-folders-and-files-to-kebabcase)

## NPM

### NPM: Remove packages not listed in package.json

`npm prune` to auto remove packages not listed in package.json.

### NPM: Go to repository of package

`npm home your-package` to go to specific repo

`npm repo your-package` to go to the overall repo.

### NPM: Package versions

Always check the `CHANGELOG.md` of a repository before upgrading. Using `npm repo your-package` to get to it fast.

Versions:

- A semver / semantic versioning: Given a version number MAJOR.MINOR.PATCH, increment the:
  - MAJOR version when you make incompatible API changes,
  - MINOR version when you add functionality in a backwards compatible manner, and
  - PATCH version when you make backwards compatible bug fixes.
- `~` update all future **patch** versions, without incrementing the minor version. ~1.2.3 will use releases from 1.2.3 to <1.3.0.
- `^` update all future **minor**/**patch** versions, without incrementing the major version. ^2.3.4 will use releases from 2.3.4 to <3.0.0.

`npm outdated` to list current packages and new versions.

`npm update` if you get a lot of red rows, updating your packages to the latest possible version according to your package.json, which is as well stated in the wanted column.

If you are using the caret `^` in front of your versions in package.json, the major versions will not be updated (hence the yellow rows). This is good, cause there might be breaking changes updating to a new major version.

`npm i the-package@latest` to update a dependency to their latest version.

`npx npm-update-all` to blindly auto update all to the latest version. I don't recommend using this, since it can lead to snow ball effect of errors down the line of breaking changes.

## Git

### Git: Rebase

- [Beginner's Guide to Interactive Rebasing](https://dev.to/blakedeboer/beginners-guide-to-interactive-rebasing-1ob)

## GitHub

### GitHub: References

- [Windows: Setup SSH with Github](https://docs.joyent.com/public-cloud/getting-started/ssh-keys/generating-an-ssh-key-manually/manually-generating-your-ssh-key-in-windows)
- [GitHub Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
- [Basic formatting syntax markdown](https://help.github.com/en/github/writing-on-github/basic-writing-and-formatting-syntax)

### GitHub: Mirror GitHub repository

- [Mirror GitHub Repo](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/duplicating-a-repository#mirroring-a-repository-in-another-location)

1. Open Git Bash Terminal
2. Create a bare mirrored clone of the original repository you want to mirror

```bash
git clone --mirror https://github.com/exampleuser/repository-to-mirror.git
```

3. Create new empty repository on GitHub, and set the push location to your mirror location

```bash
cd repository-to-mirror.git
git remote set-url --push origin https://github.com/exampleuser/mirrored
```

4. When you need to update the mirror, run following commands

```bash
git fetch -p origin
git push --mirror
```

## Bash scripts

### Bash scripts: Check code for debugger word

Bash script for checking if 'debugger;' line forgot in code, before commit.

Make file 'check_debugger.sh' (bash script) file in example .ops folder

```bash
#!/usr/bin/env bash

if grep -HiREn "debugger" ./app; then
  echo 'debug lines found in commit.  Aborting' >&2
  exit 1
fi
```

and use for example Husky to run pre-commit in package.json

```javascript
  "husky": {
    "hooks": {
      "pre-commit": "bash ./.ops/check_debugger.sh",
      "pre-push": "npm run test && npm run \"pretty?\""
    }
  }
```

### Bash scripts: Auto clean deleted branches

Bash script for using together with Git to clean already merged branches.

Use CLI/terminal and go into user dir using
`cd`
go into bin
`cd bin`
create file
`touch clean_branches`
open file
`cat > clean_branches`
insert

```bash
#!/bin/bash
REMOVE_BRANCHES=$(git fetch --prune && git branch -vv | grep ": gone]" | awk '{print $1}')
COUNT=$(echo $REMOVE_BRANCHES | wc -w)
if [[ $COUNT -gt 0 ]]; then
    echo "Will be removing branches: " "${REMOVE_BRANCHES}"
    read -rsn1 -p"Press any key to continue with removal of ${COUNT} branches.";
    echo ""
    echo $REMOVE_BRANCHES | xargs git branch -D
else
    echo "no branches to remove"
fi
```

save pressing `ctrl + D`

### Bash scripts: Rename folders and files to Kebabcase

- [Rename folders and files to Kebabcase](https://github.com/kcjmowright/rename-2-kebabcase)
