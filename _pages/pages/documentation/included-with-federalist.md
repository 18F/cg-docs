---
title: Included with Federalist
permalink: /pages/documentation/included-with-federalist/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from: 
  - /pages/using-federalist/included-with-federalist/

---

* An automatic deployment mechanism for certain static site assets and [supported site engines]({{site.baseurl}}/documentation/supported-site-engines).
* Web-based configuration and access to preview builds.
* 18F's work to complete and maintain an agency Authority to Operate (ATO) for Federalist's hosting, leveraging the cloud.gov System Security Plan (SSP), with a short process to add new sites to the ATO documentation.
* Premade templates for your sites, in addition to the many sites you can copy in creating your site. All templates now have Netlify CMS support built in, allowing content creators to make site changes without requiring extensive html, CSS, or YAML experience.
* Easy on-boarding or off-boarding: Federalist can deploy content from existing GitHub repositories used with other tools. Your Federalist site will also be easy to port to another deployment mechanism if that's ever needed.
* Content delivery network support for your desired URL, with custom 404 page support via a support ticket to the Federalist team.
* User research with people across government that helped us plan the architecture and core features of Federalist.
* A user community across agencies and vendors using Federalist.
* A permanent team to maintain and patch Federalist as needed.
* GSA's previous work to procure cloud Infrastructure as a Service with extraordinary uptime so your static sites don't go down once deployed.
* GSA's previous work to build a [robust and secure platform as a service](https://cloud.gov), recognized with a FedRAMP Moderate (DISA Level 2) PATO, that ensures Federalist's underlying technology is secure.

## Not included with Federalist

* A large file hosting system. GitHub isn't generally set up to host more than a gigabyte of file content and Federalist's build container can't accommodate extremely large repositories in the build process. Instead, you should store very large files separately and link to them from your Federalist site.
