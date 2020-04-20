# Git

## Git references

- [CS Visualized: Useful Git Commands](https://dev.to/lydiahallie/cs-visualized-useful-git-commands-37p1)
- [10 Useful Git Commands You Always Need](https://medium.com/android-bits/10-useful-git-commands-you-always-need-b0d0a50b81e6)
- [The most useful git commands that every developer should know](https://dev.to/krantikr/the-most-useful-git-commands-that-every-developer-should-know-49hi)

## Rebase

- [Beginner's Guide to Interactive Rebasing](https://dev.to/blakedeboer/beginners-guide-to-interactive-rebasing-1ob)

## GitHub

### GitHub references

- [Windows: Setup SSH with Github](https://docs.joyent.com/public-cloud/getting-started/ssh-keys/generating-an-ssh-key-manually/manually-generating-your-ssh-key-in-windows)
- [GitHub Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
- [Basic formatting syntax markdown](https://help.github.com/en/github/writing-on-github/basic-writing-and-formatting-syntax)
- [GitHub Corners - adding github svg in the corner](http://tholman.com/github-corners/)

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