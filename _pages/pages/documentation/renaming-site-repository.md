---
title: Renaming a site's repository
permalink: /pages/documentation/renaming-site-repository/
layout: docs
navigation: pages
sidenav: pages-documentation
---

## Updating your Federalist site after renaming your repository

Renaming your site repository will interrupt a site's ability to build.  In order to resume site building, it will require you to add a new site to Federalist with the new repository url.  (Note: Please be aware that if your site is already live and its repository has been renamed, it will be frozen until a new site is added to Federalist at the new repository and a security scan is successfully performed on your new site.)

## Next steps after renaming your repository

1. Add the new site repository as described [here]({{site.baseurl}}/pages/using-federalist/)
2. Build your site and verify that the site is ready for launch using our preview URLs.
3. Contact the Federalist team; we’ll scan your site within a few days as part of our compliance process and let you know of the results.
4. Let us know when you are ready to go live with the new site and we will update your CloudFront URL to point to your new site.
5. Optional: After the new site is live, you can delete the old site within Site Settings.  (Note: Once deleted, the settings and build history for the site will be lost and is not transferable to a different site.)
