---
title: Builds and Previews
permalink: /pages/documentation/previews/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from: 
  - /pages/using-federalist/previews/

---

## Site previews 

_Every_ branch in a Federalist-enabled GitHub repo is built by Federalist with
a site preview.

Site preview links will auto-generate within pull requests. To see them, scroll to
the bottom of your pull request and click "show all checks", then click on
"details" to the right of the Federalist check. The site preview is only
available after a build reports completion, usually within a few minutes.

![Federalist preview screenshot]({{site.baseurl}}/assets/images/federalist-preview.png)

You can also find links to site previews in the "GitHub Branches" page after
logging into Federalist.

{% capture content %}
For security reasons, site previews are unavailable with pull requests from
forked repositories.
{% endcapture %}
{% include pages/components/alert--note.html content=content %}

There can be two "special" branches, which are specified in the web UI's Basic
Settings page as the Live Site Branch and/or the Demo Site Branch. Those
branches can be built with custom domains (that's really the only way they're
special).

Here's how the URLs work:
- for the Live site: Live Site Domain, if set, OR `https://<site-cdn-id>.app.cloud.gov/site/<OWNER>/<REPOSITORY>`.
- for the Demo site:  Demo Site Domain, if set, OR `https://<site-cdn-id>.app.cloud.gov/demo/<OWNER>/<REPOSITORY>`.
- for branch previews:  `https://<site-cdn-id>.app.cloud.gov/preview/<OWNER>/<REPOSITORY>/<BRANCH>`.
