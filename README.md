# api documentation for  [nsp (v2.6.3)](https://github.com/nodesecurity/nsp#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-nsp.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-nsp) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-nsp.svg)](https://travis-ci.org/npmdoc/node-npmdoc-nsp)
#### The Node Security (nodesecurity.io) command line interface

[![NPM](https://nodei.co/npm/nsp.png?downloads=true)](https://www.npmjs.com/package/nsp)

[![apidoc](https://npmdoc.github.io/node-npmdoc-nsp/build/screen-capture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-nsp_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-nsp/build..beta..travis-ci.org/apidoc.html)

![package-listing](https://npmdoc.github.io/node-npmdoc-nsp/build/screen-capture.npmPackageListing.svg)



# package.json

```json

{
    "author": {
        "name": "^lift security"
    },
    "bin": {
        "nsp": "bin/nsp"
    },
    "bugs": {
        "url": "https://github.com/nodesecurity/nsp/issues"
    },
    "dependencies": {
        "chalk": "^1.1.1",
        "cli-table": "^0.3.1",
        "cvss": "^1.0.0",
        "https-proxy-agent": "^1.0.0",
        "joi": "^6.9.1",
        "nodesecurity-npm-utils": "^5.0.0",
        "path-is-absolute": "^1.0.0",
        "rc": "^1.1.2",
        "semver": "^5.0.3",
        "subcommand": "^2.0.3",
        "wreck": "^6.3.0"
    },
    "description": "The Node Security (nodesecurity.io) command line interface",
    "devDependencies": {
        "code": "^1.5.0",
        "eslint": "^2.5.3",
        "eslint-config-nodesecurity": "^1.3.1",
        "eslint-plugin-hapi": "^1.2.2",
        "git-validate": "^2.1.0",
        "lab": "^6.2.0",
        "nock": "^7.7.2",
        "shrinkydink": "^1.0.0"
    },
    "directories": {},
    "dist": {
        "shasum": "db05035953cda2ab3a571ee82fab84f4cb081d17",
        "tarball": "https://registry.npmjs.org/nsp/-/nsp-2.6.3.tgz"
    },
    "gitHead": "ee544f5fac4899cc6b1f81f123c39c293e4bf01b",
    "homepage": "https://github.com/nodesecurity/nsp#readme",
    "keywords": [
        "security",
        "nsp",
        "nodesecurity"
    ],
    "license": "Apache-2.0",
    "main": "lib/index.js",
    "maintainers": [
        {
            "name": "adam_baldwin",
            "email": "baldwin@andyet.net"
        },
        {
            "name": "andyet-ops",
            "email": "ops@andyet.net"
        },
        {
            "name": "gar",
            "email": "gar+npm@danger.computer"
        },
        {
            "name": "nlf",
            "email": "quitlahok@gmail.com"
        }
    ],
    "name": "nsp",
    "optionalDependencies": {},
    "pre-commit": [
        "test"
    ],
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/nodesecurity/nsp.git"
    },
    "scripts": {
        "lint": "eslint . bin/nsp",
        "nsp": "bin/nsp check",
        "setup-offline": "curl -sS https://api.nodesecurity.io/advisories -o advisories.json",
        "shrinkwrap": "npm shrinkwrap && shrinkydink",
        "test": "lab -a code -t 100 -I Reflect"
    },
    "version": "2.6.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module nsp](#apidoc.module.nsp)
1.  [function <span class="apidocSignatureSpan">nsp.</span>check (options, callback)](#apidoc.element.nsp.check)
1.  [function <span class="apidocSignatureSpan">nsp.</span>getFormatter (name)](#apidoc.element.nsp.getFormatter)
1.  object <span class="apidocSignatureSpan">nsp.</span>formatters

#### [module nsp.formatters](#apidoc.module.nsp.formatters)
1.  [function <span class="apidocSignatureSpan">nsp.formatters.</span>codeclimate (err, data, pkgPath)](#apidoc.element.nsp.formatters.codeclimate)
1.  [function <span class="apidocSignatureSpan">nsp.formatters.</span>default (err, data, pkgPath)](#apidoc.element.nsp.formatters.default)
1.  [function <span class="apidocSignatureSpan">nsp.formatters.</span>json (err, data, pkgPath)](#apidoc.element.nsp.formatters.json)
1.  [function <span class="apidocSignatureSpan">nsp.formatters.</span>none (err, data, pkgPath)](#apidoc.element.nsp.formatters.none)
1.  [function <span class="apidocSignatureSpan">nsp.formatters.</span>summary (err, data, pkgPath)](#apidoc.element.nsp.formatters.summary)



# <a name="apidoc.module.nsp"></a>[module nsp](#apidoc.module.nsp)

#### <a name="apidoc.element.nsp.check"></a>[function <span class="apidocSignatureSpan">nsp.</span>check (options, callback)](#apidoc.element.nsp.check)
- description and source-code
```javascript
check = function (options, callback) {

  if (typeof options === 'function') {
    callback = options;
    options = {};
  }

  var proxy = options.proxy || Conf.proxy;
  proxy = proxy || process.env.https_proxy || process.env.HTTPS_PROXY;
  delete options.proxy;

  if (proxy) {
    Conf.api.agent = new ProxyAgent(proxy);
  }

  // Set defaults
  var wreck = Wreck.defaults(Conf.api);

  var shrinkwrap;
  var offline = options.offline;
  delete options.offline;

  var advisoriesPath = options.advisoriesPath || Conf.advisoriesPath;
  delete options.advisoriesPath;

  if (!options.exceptions) {
    options.exceptions = Conf.exceptions;
  }

  // validate if options are correct
  var isValid = Joi.validate(options, internals.optionSchema);

  if (isValid.error) {
    return callback(isValid.error);
  }

  options = isValid.value;

  if (typeof options.package === 'string') {
    try {
      options.package = require(options.package);
    }
    catch (e) {
      return callback(e);
    }
  }

  options.package = SanitizePackage(options.package);

  if (typeof options.shrinkwrap === 'string') {
    try {
      shrinkwrap = Fs.readFileSync(require.resolve(options.shrinkwrap), 'utf8');
      options.shrinkwrap = require(options.shrinkwrap);
    }
    catch (e) {
      delete options.shrinkwrap;
    }
  }
  else {
    shrinkwrap = JSON.stringify(options.shrinkwrap, null, 2);
  }

  if (offline) {
    var advisories;

    if (!options.shrinkwrap) {
      return callback(new Error('npm-shrinkwrap.json is required for offline mode'));
    }
    try {
      if (advisoriesPath) {
        if (!PathIsAbsolute(advisoriesPath)) {
          advisoriesPath = Path.resolve(process.cwd(), advisoriesPath);
        }

        advisories = require(advisoriesPath);
      }
      else {
        advisories = require('../advisories');
      }
    }
    catch (e) {
      return callback(new Error('Offline mode requires a local advisories.json'));
    }


    advisories = advisories.results;
    var exceptions = options.exceptions.map(function (exception) {

      return Number(internals.exceptionRegex.exec(exception)[1]);
    });

    var generateContent = function (advisory) {

      var markdown = [
        '# ' + advisory.title,
        '## Overview:',
        advisory.overview
      ];

      if (advisory.recommendation) {
        markdown.push('');
        markdown.push('## Recommendation:');
        markdown.push(advisory.recommendation);
      }

      if (advisory.references) {
        markdown.push('');
        markdown.push('## References:');
        markdown.push(advisory.references);
      }

      return markdown.join('\n');
    };

    NPMUtils.getShrinkwrapDependencies(options.shrinkwrap, function (err, tree) {

      var keys = Object.keys(tree);
      var vulns = keys.map(function (key) {

        var mod = tree[key];
        var matches = [];
        for (var i = 0, il = advisories.length; i < il; ++i) {
          if (mod.name === advisories[i].module_name &&
              exceptions.indexOf(advisories[i].id) === -1 &&
              Semver.satisfies(mod.version, advisories[i].vulnerable_versions)) {

            matches.push(advisories[i]);
          }
        }

        return {
          module: mod.name,
          version: mod.version,
          vulnerabilities: matches
        };
      }).filter(function (mod) {

        return mod.vulnerabilities.length > 0;
      });

      var results = [];
      for (var i = 0, il = vulns.length; i < il; ++i) {
        var path = tree[vulns[i].module + '@' + vulns[i].version].paths;
        var line = internals.findLines(shrinkwrap, vulns[i].module, vulns[i].version);
        for (var x = 0, xl = vulns[i].vulnerabilities.length; x < xl; ++x) {
          results.push({
            module: vulns[i].module,
            version: vulns[i].version,
            vulnerable_versions: vulns[i].vulnerabilities[x].vulnerable_versions,
            patched_versions: vulns[i].vulnerabilities[x].patched_versions,
            title: vulns[i].vulnerabilities[x].title,
            path: path,
            advisory: 'htt ...
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.nsp.getFormatter"></a>[function <span class="apidocSignatureSpan">nsp.</span>getFormatter (name)](#apidoc.element.nsp.getFormatter)
- description and source-code
```javascript
getFormatter = function (name) {
  if (Object.keys(Formatters).indexOf(name) !== -1) {
    return Formatters[name];
  }

  try {
    return require('nsp-formatter-' + name);
  }
  catch (e) {}

  return Formatters.default;
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.nsp.formatters"></a>[module nsp.formatters](#apidoc.module.nsp.formatters)

#### <a name="apidoc.element.nsp.formatters.codeclimate"></a>[function <span class="apidocSignatureSpan">nsp.formatters.</span>codeclimate (err, data, pkgPath)](#apidoc.element.nsp.formatters.codeclimate)
- description and source-code
```javascript
codeclimate = function (err, data, pkgPath) {

  if (err) {
    return err.stack;
  }

  if (!data.length) {
    return;
  }

  var returnString = '';
  for (var i = 0, il = data.length; i < il; ++i) {
    returnString += JSON.stringify({
      type: 'issue',
      check_name: 'Vulnerable module "' + data[i].module + '" identified',
      description: ''' + data[i].module + '' ' + data[i].title,
      categories: ['Security'],
      remediation_points: 300000,
      content: {
        body: data[i].content
      },
      location: {
        path: 'npm-shrinkwrap.json',
        lines: {
          begin: data[i].line.start,
          end: data[i].line.end
        }
      }
    }) + '\0\n';
  }

  return returnString;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.nsp.formatters.default"></a>[function <span class="apidocSignatureSpan">nsp.formatters.</span>default (err, data, pkgPath)](#apidoc.element.nsp.formatters.default)
- description and source-code
```javascript
default = function (err, data, pkgPath) {

  var returnString = '';

  if (err) {
    if (data) {
      returnString += Chalk.red('(+) ') + 'Debug output: ' + JSON.stringify(Buffer.isBuffer(data) ? data.toString() : data) + '\
n';
    }

    return returnString + Chalk.yellow('(+) ') + err;
  }

  var width = 80;
  if (process.stdout.isTTY) {
    width = process.stdout.getWindowSize()[0] - 10;
  }
  if (data.length === 0) {

    return Chalk.green('(+)') + ' No known vulnerabilities found';
  }

  returnString += Chalk.red('(+) ') + data.length + ' vulnerabilities found\n';

  data.sort(function(a, b) {

    return b.cvss_score - a.cvss_score;
  }).forEach(function (finding) {

    var table = new Table({
      head: ['', finding.title],
      colWidths: [15, width - 15]
    });

    table.push(['Name', finding.module]);
    table.push(['CVSS', finding.cvss_score + ' (' + Cvss.getRating(finding.cvss_score) + ')']);
    table.push(['Installed', finding.version]);
    table.push(['Vulnerable', finding.vulnerable_versions === '<=99.999.99999' ? 'All' : finding.vulnerable_versions]);
    table.push(['Patched', finding.patched_versions === '<0.0.0' ? 'None' : finding.patched_versions]);
    table.push(['Path', finding.path.join(' > ')]);
    table.push(['More Info', finding.advisory]);

    returnString += table.toString() + '\n';
  });

  return returnString;
}
```
- example usage
```shell
...
};

internals.exceptionRegex = /^https\:\/\/nodesecurity\.io\/advisories\/([0-9]+)$/;

internals.optionSchema = Joi.object({
package: Joi.alternatives().try(Joi.string(), Joi.object()),
shrinkwrap: Joi.alternatives().try(Joi.string(), Joi.object()),
exceptions: Joi.array().items(Joi.string().regex(internals.exceptionRegex)).default([]),
advisoriesPath: Joi.string(),
proxy: Joi.string()
}).or(['package', 'shrinkwrap']);

/*
options should be an object that contains one or more of the keys package, shrinkwrap, offline, advisoriesPath
{
...
```

#### <a name="apidoc.element.nsp.formatters.json"></a>[function <span class="apidocSignatureSpan">nsp.formatters.</span>json (err, data, pkgPath)](#apidoc.element.nsp.formatters.json)
- description and source-code
```javascript
json = function (err, data, pkgPath) {

  if (err) {
    return 'Debug output: ' + JSON.stringify(Buffer.isBuffer(data) ? data.toString() : data) + '\n' + JSON.stringify(err);
  }

  return JSON.stringify(data, null, 2);
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.nsp.formatters.none"></a>[function <span class="apidocSignatureSpan">nsp.formatters.</span>none (err, data, pkgPath)](#apidoc.element.nsp.formatters.none)
- description and source-code
```javascript
none = function (err, data, pkgPath) {

  return '';
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.nsp.formatters.summary"></a>[function <span class="apidocSignatureSpan">nsp.formatters.</span>summary (err, data, pkgPath)](#apidoc.element.nsp.formatters.summary)
- description and source-code
```javascript
summary = function (err, data, pkgPath) {

  var returnString = '';

  if (err) {
    if (data) {
      returnString += Chalk.red('(+) ') + 'Debug output: ' + JSON.stringify(Buffer.isBuffer(data) ? data.toString() : data) + '\
n';
    }

    return returnString + Chalk.yellow('(+) ') + err;
  }

  if (data.length === 0) {

    return Chalk.green('(+)') + ' No known vulnerabilities found';
  }

  returnString += Chalk.red('(+) ') + data.length + ' vulnerabilities found\n';

  var table = new Table({
    head: ['Name', 'Installed', 'Patched', 'Path', 'More Info'],
    chars: {
      top: '',
      'top-mid': '',
      'top-left': '',
      'top-right': '',
      'bottom': '',
      'bottom-mid': '',
      'bottom-left': '',
      'bottom-right': '',
      left: '',
      'left-mid': '',
      'mid': '',
      'mid-mid': '',
      right: '',
      'right-mid': '',
      middle: ' '
    }
  });

  data.forEach(function (finding) {

    table.push([finding.module, finding.version, finding.patched_versions === '<0.0.0' ? 'None' : finding.patched_versions, finding
.path.join(' > '), finding.advisory]);
  });

  returnString += table.toString() + '\n';
  return returnString;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
