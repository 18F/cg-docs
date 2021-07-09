---
title: Monorepos on Federalist
permalink: /pages/documentation/monorepos-on-federalist/
layout: docs
navigation: pages
sidenav: pages-documentation
---


Monorepos on Federalist follow the same rules as any other Federalist site; as long as Federalist knows what command to run to build the site AND the contents are available in the `_site` folder in the project root, Federalist will be happy.

## Using the `script-only` option

The following is an example of using Federalist to build a site that is in a subfolder of a monorepo using the `script-only` option for a `node`-based site.

See [federalist-monorepo-test](https://github.com/18F/federalist-monorepo-test) for the full working example.

1. Like any other `script-only` site, create a `package.json` file in the project root with an entry in the `scripts` section with a key of `federalist`.
```
// package.json
{
  ...
  "scripts": {
       "federalist": "do some stuff here"
  },
  ...
}
```

2. When the `federalist` script runs, change the working directory to the appropriate folder, install any dependencies, run any build steps, and revert the working directory.
```
// package.json
{
  ...
  "scripts": {
       "build:subproject1": "cd subproject1 && npm install && npm run build && cd ..",
       "federalist": "npm run build:subproject1"
  },
  ...
}
```

3. Make the results of the build step available in the `_site` folder for Federalist to deploy, these could be copied or linked to, here we use a symbolic link.
```
// package.json
{
  ...
  "scripts": {
       "build:subproject1": "cd subproject1 && npm install && npm run build && cd ..",
       "link:subproject1": "ln -sf ./subproject1/dist _site",
       "federalist": "npm run build:subproject1 && npm run link:subproject1"
  },
  ...
}
```
