# NPM

## Cleanup packages

Remove packages not listed in package.json

```bash
npm prune
```

## Go to package repository

Open repository home on github

```bash
npm repo your-package
```

## Updating

Always check the `CHANGELOG.md` of a repository before upgrading. Using `npm repo your-package` to get to it fast.

Versions:

- A semver / semantic versioning: Given a version number MAJOR.MINOR.PATCH, increment the:
  - MAJOR version when you make incompatible API changes,
  - MINOR version when you add functionality in a backwards compatible manner, and
  - PATCH version when you make backwards compatible bug fixes.
- `~` update all future **patch** versions
- `^` update all future **minor** versions. This is normally used.

List current packages and new versions. If `Current` and `Wanted` is different, they can be updated.

```bash
npm outdated
```

![npm update](../_media/npm-update.png)

Update all packages accordingly to the package.json and `^` and `~`

```bash
npm update
```

If you are using the caret `^` in front of your versions in package.json, the major versions will not be updated (hence the yellow rows). This is good, cause there might be breaking changes updating to a new major version.

Update a dependency to their latest version.

```bash
npm i the-package@latest
```

blindly auto update all to the latest version. I don't recommend using this, since it can lead to snow ball effect of errors down the line of breaking changes.

```bash
npx npm-update-all
```

## NPM trends

See [NPM Trends](https://www.npmtrends.com) for npm trends.
