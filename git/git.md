# Git

## Git references

- [CS Visualized: Useful Git Commands](https://dev.to/lydiahallie/cs-visualized-useful-git-commands-37p1)
- [10 Useful Git Commands You Always Need](https://medium.com/android-bits/10-useful-git-commands-you-always-need-b0d0a50b81e6)
- [The most useful git commands that every developer should know](https://dev.to/krantikr/the-most-useful-git-commands-that-every-developer-should-know-49hi)

## Rebase

- [Beginner's Guide to Interactive Rebasing](https://dev.to/blakedeboer/beginners-guide-to-interactive-rebasing-1ob)

### Undo a rebase

Get a list of previous git actions.

```bash
git reflog
```

Find the line where it says `commit`, that will be your latest normal commit before the rebase happend. If that commit has the name `HEAD@{84}` and you want to go back to that, write:

```bash
git reset --hard HEAD@{84}
```

`git reflog` stores all your actions, even after a `reset --hard`, meaning that if you wrote a wront HEAD, you can still write `git reflog` again after that, and find the correct HEAD to go back to. Remember that HEAD numbers are changed, so you can't use the same as before.

If you pushed to origin, then write `git push -f` to push your local changes to origin.

## Git Stash

Git stash can be used, to stash changes in a branch instead of a commit, and be able to apply thoese changes later, to any branch.

- To stash uncomitted changes: `git stash push -m "my_stash"`
- To see list of stashes: `git stash list`
- To apply a stash `git stash apply stash@{n}` or `git stash apply n`
- To apply a stash and remove it from stash `git stash pop stash@{n}` or `git stash pop n`
- To remove a stash: `git stash drop stash@{n}` or `git stash drop n`

## Delete all local branches already merged to master

Has to be run from Git Bash terminal. Be on Master and run:

```bash
git fetch --prune
git branch -r | awk '{print $1}' | egrep -v -f /dev/fd/0 <(git branch -vv | grep origin) | awk '{print $1}' | xargs git branch -D
```

After that you can write `git branch` to see if some still remains. If some branches are 'not found' then you can delete them manually one by one, surrounding the names with "", example `git branch "some-()-not-found-branch" -D`

## Remove all files and folder in project not staged etc

```bash
git clean -xfd
```

## Create empty commit used to restart CI pipeline

```bash
git commit -m "Empty commit." --allow-empty
```

## GitHub

### GitHub references

- [GitHub docs for setting up SSH with Github](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)
- [Windows: Setup SSH with Github](https://docs.joyent.com/public-cloud/getting-started/ssh-keys/generating-an-ssh-key-manually/manually-generating-your-ssh-key-in-windows)
- [GitHub Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
- [Basic formatting syntax markdown](https://help.github.com/en/github/writing-on-github/basic-writing-and-formatting-syntax)
- [GitHub Corners - adding github svg in the corner](http://tholman.com/github-corners/)

### Manually make GitHub compare multiple commits

- Open PR conversation
- Find the last commit that have already been reviewed and click its hash link, your URL will look like <https://github.com/airbnb/javascript/pull/188/commits/2b6c524e6984f300f6c6e08e57c8dbab9497e798>
- Change commits to files and add ..HEAD to the end of URL such as <https://github.com/airbnb/javascript/pull/188/files/2b6c524e6984f300f6c6e08e57c8dbab9497e798..HEAD>

You'll see commits range you want to review in context of the PR.

P.S. Now GitHub provides functionality to view "Changes since your previous code review" but sometimes it is useful to select changes manually

### Mirror repository

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
