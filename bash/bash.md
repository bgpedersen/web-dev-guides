# Bash

## Check for 'debugger' word

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

## Delete merged branches

Auto clean deleted branches. Bash script for using together with Git to clean already merged branches.

Use CLI/terminal and go into user dir using. If you open bash it should start in user directory automatically.

create/go into bin

```bash
mkdir bin
cd bin
```

create file
`touch clean_branches`

open file
`cat > clean_branches`

insert following and save pressing `ctrl + D`

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

Now you can write `clean_branches` from your git terminal :)

## Rename all to Kebabcase

- [Rename folders and files to Kebabcase](https://github.com/kcjmowright/rename-2-kebabcase)

## Search command history previously used

This works in both Bash and Powershell terminals

- [Read more](https://serverfault.com/questions/891265/how-to-search-powershell-command-history-from-previous-sessions)

When in terminal pressing `ctrl` + `r` and start typing to search backwards in history interactively. This matches text from anywhere in the command line. Press `ctrl`+ `r` again to find next match. You can use `ctrl`+ `s` to go back.

## Make a localhost certificate

To make a valid localhost certificate to use SSL on `https://localhost` go to your projects root dir and type following:

```bash
mkcert localhost
```
