---
title: Supported Site Engines
permalink: /pages/documentation/supported-site-engines/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from:
  - /pages/using-federalist/supported-site-engines/
---

## Current Support
- [Jekyll](http://jekyllrb.com)
- [Hugo](https://gohugo.io/)
- Node.js - Runs a [Node](https://nodejs.org) script
- Static - Pushes the files in your repository to S3 without modifying them, useful for exports from content management systems or web development tools.

## Requirements
For all engines **except Static** Federalist will serve whatever is in the `_site` folder so you should either:
- configure the generator to produce output in the `_site` folder OR
- create a symlink from `_site` to whatever folder will contain the generator's output

### Node
A Node script can be run to generate a site OR as a preliminary step to generating one with Jekyll or Hugo. To use a Node script as the generator, choose **Node.js** as the **Site Engine** in the Advanced Settings configuration for your site.

For more details on using Node see [node on federalist]({{site.baseurl}}/documentation/node-on-federalist).

## Jekyll Resources
Federalist can generate any Jekyll website, which lets you build custom websites hosted on Federalist. For documentation on getting started with Jekyll, see [jekyllrb.com](http://jekyllrb.com/).

For an example of a Jekyll site optimized for Federalist, see our [templates]({{site.baseurl}}/documentation/templates/).

### Jekyll build features

Federalist provides features beyond just generating Jekyll sites. The steps below outline how to set up custom websites that best take advantage of these features.

### Configuration

Federalist adds a `site.branch` attribute to your global site object with the value of the current branch name. You can access this value in your templates and content and use it to style builds based on the working branch.

### Metadata defaults

If you [specify front-matter defaults](http://jekyllrb.com/docs/configuration/#front-matter-defaults) in your Jekyll site configuration, Federalist will pre-fill the front-matter of a new post with these defaults.

### Base URLs

To handle routing sites for previews, Federalist automatically sets a `baseurl` path for your site. This essentially nests your site in a subdirectory under the `federalist.18f.gov` domain, such as `federalist.18f.gov/preview/18f/hub/new-draft`, where `/preview/18f/hub` is the `baseurl`.

All links to other pages or resources on the site require a `baseurl` prefix. If you're designing a custom template to work with Federalist, make sure all references to relative links include `site.baseurl` prefixes, including trailing slashes, as follows:

Link: `{% raw %}[About Us]({{site.baseurl}}/about-us){% endraw %}`

Image: `{% raw %}![18F]({{site.baseurl}}/uploads/18f-logo.png){% endraw %}`

### Conditionally set robots

To instruct search engines not to index the `preview` builds of your site, try adding the following code within your site's `<head>` tags which are most likely found in your template's head.html or meta.html file:
{% raw %}
```markdown
{% unless site.branch == "master" %}
  <meta name="robots" content="noindex, nofollow">
{% endunless %}
```
{% endraw %}
***Note:*** This code sample assumes the live version of your site's code is maintained in the `master` branch of your site's code repository.

For all versions of your site that aren't built from `master`, the source code of the site will contain the code above. For an example, see [here](https://federalist-proxy.app.cloud.gov/preview/18f/federalist-docs/wslack-patch-1/), view source, and jump to line 57.

### Jekyll Plugins

Jekyll has a plugin system for adding custom features to the build process of your website. Use-cases for plugins include automatically generating new pages or templates, fetching data or content from external resources, and CCS or JavaScript compilation. [Learn more about Jekyll plugins](http://jekyllrb.com/docs/plugins/).

Federalist supports Jekyll plugins, enabling any plugins in a site's `_plugins` directory. If the site includes a `Gemfile`, Federalist will also run `bundle install && bundle exec jekyll build` to install required Ruby gems and generate the site with those libraries available for use in plugins. You can also use a `Gemfile` to change the version of Jekyll used to build the site.

Several dependencies are already available for use in the building environment. These include `ruby`, `python`, and `node.js`. You can write plugins that take advantage of these without needing a `Gemfile`.

To see the exact configuration of the build environment, see the [build environment `Dockerfile`](https://github.com/18F/federalist-garden-build/blob/master/Dockerfile).

**Note:** using `Gemfile` may considerably slow down the generation of your website, depending on how long the `bundle install` step takes to complete.

## Hugo Resources (experimental)

Federalist can also generate websites with [Hugo](http://gohugo.io/), the Go-based site generator. See the [Hugo Docs](https://gohugo.io/documentation/) for getting started with Hugo.

### Hugo version

When building a Federalist site using Hugo, the desired version of Hugo for building your site must be specified in a `.hugo-version` file located in your repository's root directory.
>Sample .hugo-version file contents:
```markdown
0.48.0
```
>For an extended Hugo version:
```markdown
extended_0.48.0
```
