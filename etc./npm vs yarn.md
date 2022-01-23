# [npm vs yarn](https://javascript.plainenglish.io/npm-vs-yarn-choosing-the-right-package-manager-a5f04256a93f)

## Why yarn was developed when there was already npm?
- Yarn was built by Facebook to solve major problems they faced with npm. 

### Parallel installation of packages
- npm: Waits for a package to be fully installed before moving to another package.
- yarn: Installs tasks in parallel, thus increasing performance and speed.

### Automatic Lock file generation
- the dependency’s version may start with `^` before the version number: This means that whenever we install all the packages in another machine, or manually run the command to install, the package manager looks for newer versions released. 
- There are two ways to avoid this if you don’t want automatic change in your packages,
  1. One is to generate a lock file, so that only a particular version is installed every single time
  2. and the other is to remove `^` in the package file.
- npm: `npm shrinkwrap` command generates a lock file. Does not create the lock file by default. It only updates if a `npm-shrinkwrap.json` exists. (버전 업데이트가 되며 수정되긴 함)
- yarn: Yarn automatically adds a `yarn.lock` file when dependencies are added. And always creates and updates the `yarn.lock` file.

### Security
- npm: Automatically executes a code which allows the other packages to get included into the fly, thus resulting in several vulnerabilities in the security system. 
- yarn: Installs those files which are only from the `yarn.lock` or `package.json` files. Therefore it is considered more secured than npm packages.
