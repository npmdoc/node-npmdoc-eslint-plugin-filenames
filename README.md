# api documentation for  [eslint-plugin-filenames (v1.1.0)](https://github.com/selaux/eslint-plugin-filenames)  [![npm package](https://img.shields.io/npm/v/npmdoc-eslint-plugin-filenames.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-eslint-plugin-filenames) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-eslint-plugin-filenames.svg)](https://travis-ci.org/npmdoc/node-npmdoc-eslint-plugin-filenames)
#### Eslint rule for consistent filenames.

[![NPM](https://nodei.co/npm/eslint-plugin-filenames.png?downloads=true)](https://www.npmjs.com/package/eslint-plugin-filenames)

[![apidoc](https://npmdoc.github.io/node-npmdoc-eslint-plugin-filenames/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-eslint-plugin-filenames_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-eslint-plugin-filenames/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-eslint-plugin-filenames/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-eslint-plugin-filenames/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Stefan Lau",
        "email": "github@stefanlau.com"
    },
    "bugs": {
        "url": "https://github.com/selaux/eslint-plugin-filenames/issues"
    },
    "dependencies": {
        "lodash.camelcase": "4.1.1",
        "lodash.kebabcase": "4.0.1",
        "lodash.snakecase": "4.0.1"
    },
    "description": "Eslint rule for consistent filenames.",
    "devDependencies": {
        "chai": "3.5.0",
        "coveralls": "2.11.7",
        "eslint": "^2.0.0",
        "eslint-plugin-filenames": "^1.0.0",
        "istanbul": "0.4.3",
        "mocha": "2.4.5",
        "sinon": "1.17.4"
    },
    "directories": {},
    "dist": {
        "shasum": "bb925218ab25b1aad1c622cfa9cb8f43cc03a4ff",
        "tarball": "https://registry.npmjs.org/eslint-plugin-filenames/-/eslint-plugin-filenames-1.1.0.tgz"
    },
    "gitHead": "1e6e5a2d2107fde65659882a7c71cdd85c80cc70",
    "homepage": "https://github.com/selaux/eslint-plugin-filenames",
    "keywords": [
        "eslint",
        "eslintplugin",
        "eslint-plugin",
        "file",
        "filename",
        "path"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "selaux",
            "email": "github@stefanlau.com"
        }
    ],
    "name": "eslint-plugin-filenames",
    "optionalDependencies": {},
    "peerDependencies": {
        "eslint": "*"
    },
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/selaux/eslint-plugin-filenames.git"
    },
    "scripts": {
        "check-coverage": "istanbul check-coverage --statement 100 --branch 100 --function 100 --lines 100",
        "coveralls": "cat ./build/coverage/lcov.info | coveralls",
        "lint": "eslint .",
        "report-coverage-html": "istanbul report --dir  build/coverage html",
        "test": "npm run lint && npm run unit-test --coverage && npm run check-coverage",
        "unit-test": "istanbul test --dir build/coverage _mocha test -- --recursive --reporter dot"
    },
    "version": "1.1.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module eslint-plugin-filenames](#apidoc.module.eslint-plugin-filenames)
1.  object <span class="apidocSignatureSpan">eslint-plugin-filenames.</span>rules

#### [module eslint-plugin-filenames.rules](#apidoc.module.eslint-plugin-filenames.rules)
1.  [function <span class="apidocSignatureSpan">eslint-plugin-filenames.rules.</span>match-exported (context)](#apidoc.element.eslint-plugin-filenames.rules.match-exported)
1.  [function <span class="apidocSignatureSpan">eslint-plugin-filenames.rules.</span>match-regex (context)](#apidoc.element.eslint-plugin-filenames.rules.match-regex)
1.  [function <span class="apidocSignatureSpan">eslint-plugin-filenames.rules.</span>no-index (context)](#apidoc.element.eslint-plugin-filenames.rules.no-index)



# <a name="apidoc.module.eslint-plugin-filenames"></a>[module eslint-plugin-filenames](#apidoc.module.eslint-plugin-filenames)



# <a name="apidoc.module.eslint-plugin-filenames.rules"></a>[module eslint-plugin-filenames.rules](#apidoc.module.eslint-plugin-filenames.rules)

#### <a name="apidoc.element.eslint-plugin-filenames.rules.match-exported"></a>[function <span class="apidocSignatureSpan">eslint-plugin-filenames.rules.</span>match-exported (context)](#apidoc.element.eslint-plugin-filenames.rules.match-exported)
- description and source-code
```javascript
match-exported = function (context) {
    return {
        "Program": function (node) {
            var transformName = context.options[0],
                filename = context.getFilename(),
                absoluteFilename = path.resolve(filename),
                parsed = parseFilename(absoluteFilename),
                shouldIgnore = isIgnoredFilename(filename),
                exportedName = getExportedName(node),
                isExporting = Boolean(exportedName),
                expectedExport = getStringToCheckAgainstExport(parsed),
                transformed = transform(exportedName, transformName),
                everythingIsIndex = exportedName === 'index' && parsed.name === 'index',
                matchesExported = transformed === expectedExport || everythingIsIndex,
                reportIf = function (condition, messageForNormalFile, messageForIndexFile) {
                    var message = (!messageForIndexFile || !isIndexFile(parsed)) ? messageForNormalFile : messageForIndexFile;

                    if (condition) {
                        context.report(node, message, {
                            name: parsed.base,
                            expectedExport: expectedExport,
                            exportName: transformed,
                            extension: parsed.ext
                        });
                    }
                };

            if (shouldIgnore) return;
            reportIf(
                isExporting && !matchesExported,
                "Filename '{{expectedExport}}' must match the exported name '{{exportName}}'.",
                "The directory '{{expectedExport}}' must be named '{{exportName}}', after the exported value of its index file."
            );
        }
    }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.eslint-plugin-filenames.rules.match-regex"></a>[function <span class="apidocSignatureSpan">eslint-plugin-filenames.rules.</span>match-regex (context)](#apidoc.element.eslint-plugin-filenames.rules.match-regex)
- description and source-code
```javascript
match-regex = function (context) {
    var defaultRegexp = /^([a-z0-9]+)([A-Z][a-z0-9]+)*$/g,
        conventionRegexp = context.options[0] ? new RegExp(context.options[0]) : defaultRegexp,
        ignoreExporting = context.options[1] ? context.options[1] : false;

    return {
        "Program": function(node) {
            var filename = context.getFilename(),
                absoluteFilename = path.resolve(filename),
                parsed = parseFilename(absoluteFilename),
                shouldIgnore = isIgnoredFilename(filename),
                isExporting = Boolean(getExportedName(node)),
                matchesRegex = conventionRegexp.test(parsed.name);

            if (shouldIgnore) return;
            if (ignoreExporting && isExporting) return;
            if (!matchesRegex) {
                context.report(node, "Filename 'function <span class="apidocSignatureSpan">eslint-plugin-filenames.rules.</span>match-regex' does not match the naming convention.", {
                    name: parsed.base
                });
            }
        }
    };
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.eslint-plugin-filenames.rules.no-index"></a>[function <span class="apidocSignatureSpan">eslint-plugin-filenames.rules.</span>no-index (context)](#apidoc.element.eslint-plugin-filenames.rules.no-index)
- description and source-code
```javascript
no-index = function (context) {
    return {
        "Program": function(node) {
            var filename = context.getFilename(),
                absoluteFilename = path.resolve(filename),
                parsed = parseFilename(absoluteFilename),
                shouldIgnore = isIgnoredFilename(filename),
                isIndex = isIndexFile(parsed);


            if (shouldIgnore) return;
            if (isIndex) {
                context.report(node, "'index.js' files are not allowed.");
            }
        }
    };

}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
