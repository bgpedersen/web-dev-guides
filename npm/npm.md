# NPM

## Node versions and their bundled NPM versions

- [node and npm verions bundles](https://nodejs.org/en/download/releases/)
- [released and life spans](https://nodejs.org/en/about/releases/)

## Setting variables in npm scripts for different environments (mac, windows etc.)

- cross-env [link](https://www.npmjs.com/package/cross-env)

cross-env makes it so you can have a single command without worrying about setting or using the environment variable properly for the platform. Just set it like you would if it's running on a POSIX system, and cross-env will take care of setting it properly.

It's not just for less code, it's also to protect against weird errors, such as since set variable will create a space in the end on mac, therefor make wrong url if using the manual seperate way.

Real world example:

```json
    # old seperate
    "build:windows": "set DEPLOY_URL=%npm_config_deploy_url% && ng build --configuration=production --no-progress --deploy-url %npm_config_deploy_url% && ng build elements --configuration=production --no-progress",
    "build:mac": "DEPLOY_URL=$npm_config_deploy_url ng build --configuration=production --no-progress --deploy-url $npm_config_deploy_url && ng build elements --configuration=production --no-progress",

    # new combined
    "build": "cross-env DEPLOY_URL=%npm_config_deploy_url% ng build --configuration=production --no-progress --deploy-url %npm_config_deploy_url% && ng build elements --configuration=production --no-progress",
```

## Node_module Clean Install (normally used by devops pipeline, but can be used locally)

It is different from npm install in the following ways —

- It installs the exact version of the package that is mentioned in the package-lock.json file.
  Removes the existing node_modules and runs a fresh installation.
- It won’t write to package.json or lock file.
- It doesn’t install individual packages similar to npm install.

```bash
npm ci
```

## Check dependencies of a certain package

Example check all packages dependent on Webpack

```bash
npm list webpack
```

## Installed packages

List all the installed packages

```bash
npm list --depth=1
```

To see a list of globally installed packages use

```bash
npm list -g --depth 0
```

To remove a global package use

```bash
npm uninstall -g moment
```

## Remove not listed packages

Remove packages not listed in package.json

```bash
npm prune
```

## Go to package repository

Open repository home on github

```bash
npm repo your-package
```

## Versions and Updating

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

#or to get more info

npm outdated --long
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

## CHANGELOG.MD generator

- [Standard Version](https://github.com/conventional-changelog/standard-version)

## NPM trends

See [NPM Trends](https://www.npmtrends.com) for npm trends.

## Other useful npm commands

### Quickly generate package.json

```bash
npm init -y
```

### Install multiple packages having the same prefix

```bash
npm i eslint-{plugin-import,plugin-react,loader} express
```

### NPM custom scripts

`npm run` command shows all the scripts that we have defined in our package.json

- We can run multiple scripts using `&&`. Both the scripts run in series i.e one after the other.
- We can also run multiple scripts in parallel using `&`.

### Lists all the npm environment variables present in our package

We can also access the env variables in our code by process.env.npm_package_name and similarly other variables.

```bash
  npm run env
```

We can pass our own variables as npm environment variables with the npm*package_config* prefix by defining them in package.json file under config object. Let’s define the variable myvariable in our package.json file.

```json
"config": {
    "myvariable": "Hello World"
},
```

Find it with `npm run env | grep npm_package_config_` and see `npm_package_config_myvariable=Hello World`

Use the variable in a script: `echo-myvariable echo $npm_package_config_myvariable`

### Check our environment

We can use npm doctor command to run multiple checks on our environment like, whether our npm CLI has sufficient permissions to install the javascript packages and it is able to connect to the npm registry. It also checks for node and npm versions, validates cache for any corrupt packages.

```bash
npm doctor
```

### Install a package from other sources

```bash
# Install a component from Bit (set Bit as a scoped registry)
npm config set @bit:registry https://node.bit.dev
npm i @bit/username.collection.component

# Install from tarball stored locally
npm i ./local-tarball-package.tgz

# Install tarball package from internet
npm i https://abc/xyz/package.tar.gz

# Install from github repo
npm i githubuser/reponame

# Install from bitbucket repo
npm i bitbucket:bitbucketuser/reponame

# Install from gist
npm i gist:gistID
```
