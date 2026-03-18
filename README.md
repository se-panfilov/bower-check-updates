# DEPRECATED: 
This project is no longer actively maintained. It may still work, but please use [npm-check-updates](https://github.com/tjunnone/npm-check-updates) instead.

<!-- [![npm stable version](https://img.shields.io/npm/v/bower-check-updates.svg?label=stable)](https://npmjs.org/package/bower-check-updates) -->
[![Dependency Status](https://david-dm.org/se-panfilov/bower-check-updates.svg)](https://david-dm.org/se-panfilov/bower-check-updates)
[![devDependency Status](https://david-dm.org/se-panfilov/bower-check-updates/dev-status.svg)](https://david-dm.org/se-panfilov/bower-check-updates#info=devDependencies)
<!-- [![npm unstable version](https://img.shields.io/github/tag/se-panfilov/bower-check-updates.svg?label=unstable)](https://github.com/se-panfilov/bower-check-updates/tags) -->

`npm-check-updates` **supports bower** as of v2.3.0

```sh
$ npm install -g npm-check-updates
$ npm-check-updates --packageManager bower   # or 'ncu -m bower' for short
```

---

bower-check-updates is a command-line tool that allows you to find and save the latest versions of dependencies, regardless of any version constraints in your bower.json file (unlike npm itself).

bower-check-updates maintains your existing semantic versioning policies, i.e., it will upgrade your `"express": "^4.11.2"` dependency to `"express": "^5.0.0"` when express 5.0.0 is released.

<!-- ![bower-check-updates-screenshot](https://cloud.githubusercontent.com/assets/750276/8864534/0788a4d8-3171-11e5-9881-8f7dcf634d14.png) -->

What?
--------------
`bower-check-updates` - is totally clone of [npm-check-updates][1], but it updates bower.json dependencies (bower-check-updates updates bower.json).

All the code is written by [tjunnone][2]. I have just renamed `package.json` to `bower.json` (and added [closest-bower][3] module instead of [closest-package][4]). So if you want to contribute - better do it to `bower-check-updates`, and I'll merge the changes (notify me if I'm not).

Installation
--------------

```sh
npm install -g bower-check-updates
```

Usage
--------------
Show any new dependencies for the project in the current directory:

```sh
$ bcu

 express           4.12.x  →   4.13.x
 multer            ^0.1.8  →   ^1.0.1
 react-bootstrap  ^0.22.6  →  ^0.24.0
 react-a11y        ^0.1.1  →   ^0.2.6
 webpack          ~1.9.10  →  ~1.10.5

Run with -u to upgrade your bower.json
```

Upgrade a project's bower.json:

> **Make sure your bower.json is in version control and all changes have been committed. This *will* overwrite your bower.json.**

```sh
$ bcu -u

 bootstrap           4.12.x  →   4.13.x

bower.json upgraded
```

Include or exclude specific packages:
```sh
# match mocha and should packages exactly
$ bcu -f mocha,should         

# match packages that start with "gulp-" using regex
$ bcu -f /^gulp-/             

# match packages that do not start with "gulp-". Note: single quotes are required 
# here to avoid inadvertant bash parsing
$ bcu -f '/^(?!gulp-).*$/'    
```

Options
--------------
    -d, --dev                check only devDependencies
    -e, --error-level        set the error-level. 1: exits with error code 0 if no
                             errors occur. 2: exits with error code 0 if no
                             packages need updating (useful for continuous
                             integration)
    -g, --global             check global packages instead of in the current project
    -h, --help               output usage information
    -j, --jsonAll            output new bower.json instead of human-readable
                             message
    --jsonUpgraded           output upgraded dependencies in json
    --packageData            include stringified bower.json (use stdin instead)
    -o, --optional           check only optionalDependencies
    -p, --prod               check only dependencies (not devDependencies)
    -r, --registry           specify third-party NPM registry
    -s, --silent             don't output anything
    -t, --greatest           find the highest versions available instead of the 
                             latest stable versions (alpha release only)
    -u, --upgrade            upgrade bower.json dependencies to match latest 
                             versions (maintaining existing policy)
    -ua, --upgradeAll        upgrade bower.json dependencies even when the latest
                             version satisfies the declared semver dependency
    -V, --version            output the version number

Integration
--------------
The tool allows integration with 3rd party code:

```javascript
var bcu = require('bower-check-updates');

bcu.run({
    packageData: fs.readFileSync('./some/project/bower.json', 'utf-8'),
    // Any command-line option can be specified here.
    // These are set by default:
    // silent: true,
    // jsonUpgraded: true
}).then(function(upgraded) {
    console.log('dependencies to upgrade:', upgraded);
});
```

How dependency updates are determined
--------------

- Direct dependencies will be increased to the latest stable version:
  - 2.0.1 => 2.2.0
  - 1.2 => 1.3
-  Semantic versioning policies for levels are maintained while satisfying the latest version:
  - ^1.2.0 => ^2.0.0
  - 1.x => 2.x
- "Any version" is maintained:
  - \* => \*
- "Greater than" is maintained:
  - \>0.2.0 => \>0.3.0
- Closed ranges are replaced with a wildcard:
  - 1.0.0 \< 2.0.0 => ^3.0.0

Problems?
--------------

Please [file an issue on github](https://github.com/se-panfilov/bower-check-updates/issues).

Always include your bower.json when reporting a bug!

Pull requests are welcome, and will not collect dust :)

## Legal Disclaimer

This project is provided "as is", without warranty of any kind. The authors and contributors disclaim all liability for any damages arising from its use, inability to use, or performance. Users are responsible for ensuring compliance with applicable laws and regulations in their jurisdiction, including but not limited to:

- European Union: Cyber Resilience Act (CRA) and Artificial Intelligence Act (AI Act)
- United States: Uniform Commercial Code (UCC) and Export Administration Regulations (EAR)
- China: Cybersecurity Law
- India: Information Technology Act

For details, see the full [DISCLAIMER.md](DISCLAIMER.md) file.

[1]: https://github.com/tjunnone/npm-check-updates
[2]: https://github.com/tjunnone
[3]: https://github.com/se-panfilov/closest-bower
[4]: https://github.com/hughsk/closest-package
