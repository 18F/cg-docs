---
title: How Builds Work
permalink: /pages/documentation/how-builds-work/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from: 
  - /pages/how-federalist-works/how-builds-work/
---


Federalist is a continuous deployment-like build environment for sites. It works by setting a webhook on your site's GitHub repository and generates your site on each `push` event to that repository, then uploads your site files to S3. Changes made to the site's content and files in its repository through the GitHub web editor or otherwise launch rebuild tasks of the site in a build environment container.

If you don't want to use the Federalist templates, you can add your own GitHub repository to build a completely custom site. When you add an existing repository, you specify a default branch of your repository to serve as the "production" version of the site and choose your build engine.

Federalist is designed to be a modular service so HOW you generate your site is up to you. Some people customize their sites by creating new templates. Others use a default template content, editing with GitHub. When used this way Federalist acts a no-configuration, production-ready hosting solution for GitHub-based static websites, hosted using cloud.gov tooling, with a custom domain.

When building our your sites, please remember that all government websites must meet section 508 accessibility standards. 18F provides [guidance for building accessible websites](https://accessibility.18f.gov/).

## Scheduling nightly builds

Sites with public repositories can schedule builds to occur nightly.  If you wish to schedule automatic builds of your live or demo site, add the following custom environment variable to your [live/demo branch's build configuration]({{site.baseurl}}/documentation/env-vars-on-federalist-builds/):
```
schedule: nightly
```
