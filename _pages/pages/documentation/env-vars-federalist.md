---
title: Build environment variables
permalink: /pages/documentation/env-vars-on-federalist-builds/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from:
  - /pages/how-federalist-works/env-vars-on-federalist-builds/
---

At the time your site is built with either Jekyll or Hugo, a number of special environment variables are exposed. You can access these environment variables with your build engine to customize your build (for instance, to add some special text to your site to show which branch has been built).

See the [federalist-garden-build README](https://github.com/18f/federalist-garden-build#variables-exposed-during-builds) for further information on the environment variables that your site build can access.

## Default environment variables available when your site builds

When Federalist builds your site it makes available the following environment variables:

Name|Description
---|---
BRANCH|Github branch being built
OWNER|Owner of Github repository
REPOSITORY|Github repository
SITE_PREFIX|Path of s3 bucket in which your site will be deployed
BASEURL|Path for deployed site

## Adding custom environment variables available when your site builds

Additional environment variables can be added to your live and demo site builds by:
1. Visit your site's Site Settings -> Advanced Settings
2. In the Site/Demo Configuration textarea enter your environment variables as yaml formatted text (Note: nested data is supported)
3. Click "Save advanced settings"
