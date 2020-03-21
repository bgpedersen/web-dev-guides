# Bash and Git

## Contents

- [Bash and Git](#bash-and-git)
  - [Contents](#contents)
  - [Git](#git)
    - [Git: Rebase](#git-rebase)
  - [GitHub](#github)
    - [GitHub: References](#github-references)
    - [GitHub: Mirror GitHub repository](#github-mirror-github-repository)
  - [Bash scripts](#bash-scripts)
    - [Bash scripts: Check code for debugger word](#bash-scripts-check-code-for-debugger-word)
    - [Bash scripts: Auto clean deleted branches](#bash-scripts-auto-clean-deleted-branches)
    - [Bash scripts: Rename folders and files to Kebabcase](#bash-scripts-rename-folders-and-files-to-kebabcase)

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
