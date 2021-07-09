---
title: RVM on Federalist
permalink: /pages/documentation/rvm-on-federalist/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from:
  - /pages/how-federalist-works/rvm-on-federalist/
---

Federalist uses [RVM](https://rvm.io/) to select which ruby version to use to build a Federalist site.

## Default Ruby version
By default, the build will use Ruby version 2.6.6.

## Specifying a Ruby version

Prior to building a site, the build will check for a file named `.ruby-version`. If one is found, it will use RVM to install and use the version specified there.

For example, if you wish to use Ruby version 2.4.0 to build a site, add a new file named `.ruby-version` to your repository. The `.ruby-version` file should be at the top-level directory. The contents of that file should be "`2.4.0`".
