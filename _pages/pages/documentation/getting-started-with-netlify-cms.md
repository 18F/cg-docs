---
layout: docs
navigation: pages
sidenav: pages-documentation
title: Getting started with Netlify CMS
permalink: /pages/documentation/getting-started-with-netlify-cms/
redirect_from:
  - /pages/getting-started-with-netlify-cms/
---

Federalist recently integrated support for Netlify CMS, an open source content management system for your Github-based content that provides editors with a friendly UI and workflow.

### Building a new site from template
The Federalist templates include pre-configured Netlify CMS functionality. To begin utilizing: 
1. select the [template]({{site.baseurl}}/documentation/templates/) that you wish to use as your base site
2. navigate to the new repository that Federalist created, and **update** the Netlify CMS configuration to point to the current repository

```
    # Jekyll: /admin/config.yml
    # Gatsby or Hugo: /static/admin/config.yml

    backend:
      ...
      repo: <your org>/<your repo>
      ...
```
3. when you are done, your configuration should look something like the one in [configuration requirements](#configuration-requirements)

4. once your site is rebuilt in Federalist, Netlify CMS is ready to use! Navigate to `https://<your site url>/admin` and edit away.

### Adding Netlify CMS to existing site
If your site is already up and running, please follow the instructions on [Netlify's Add To Your Site](https://www.netlifycms.org/docs/add-to-your-site/) page to add Netlify CMS. Please pay particular attention to the location of the admin folder depending on your static site generator.

- Jekyll: `/admin/config.yml`
- Gatsby or Hugo: `/static/admin/config.yml`

For examples of existing configurations, see the Federalist starter:
- [Federalist USWDS Jekyll](https://github.com/18F/federalist-uswds-jekyll/blob/master/admin/config.yml)
- [Federalist USWDS Gatsby](https://github.com/18F/federalist-uswds-gatsby/blob/master/static/admin/config.yml)

### Configuration Requirements
To use Netlify CMS, you must authenticate with Github, and in order for Federalist to facilitate this, your Netlify CMS configuration should include the following:

```
  
    backend:
      name: github
      repo: <your org>/<your repo>
      base_url: https://federalistapp.18f.gov
      auth_endpoint: external/auth/github
      preview_context: federalist/build
      branch: master
      

      # optional
      use_graphql: true
    ...
```

See [Netlify CMS Backends Overview](https://www.netlifycms.org/docs/backends-overview) for a full description of the configuration options.


### Getting familiar with Netlify CMS
To learn more about Netlify CMS and how it may help you manage content changes on your Federalist site, please visit [netlifycms.org/](https://www.netlifycms.org/)
