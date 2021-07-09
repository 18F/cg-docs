---
title: Custom URLs
permalink: /pages/documentation/custom-urls/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from:
  - /pages/how-federalist-works/custom-urls/
---


## How Federalist custom URLs work

This is technical documentation about how custom domains work on Federalist. It is not required reading to use Federalist and only provided for background; instructions for our partners are [here](/pages/using-federalist/launch-checklist/).

All Federalist deploys are rendered via S3 at a .com URL such as: 

- [http://cg-06ab120d-836f-49a2-bc22-9dfb1585c3c6.s3-website-us-gov-west-1.amazonaws.com/site/18f/federalist-modern-team-template/](http://cg-06ab120d-836f-49a2-bc22-9dfb1585c3c6.s3-website-us-gov-west-1.amazonaws.com/site/18f/federalist-modern-team-template/) 

These web addresses are public and useful for testing, but the URLs are not useful and there's no HTTPS. (You'll also notice that images and CSS are not loading on the page. We'll explain why below.)

Luckily, Federalist also proxies — that is, creates a forwarding address — for those S3 URLs at URLs such as:

- [https://federalist-proxy.app.cloud.gov/site/18f/federalist-modern-team-template/](https://federalist-proxy.app.cloud.gov/site/18f/federalist-modern-team-template/). 

The proxy adds some required headers and is much cleaner for sending preview links around. 

When ready to go live at their own .gov URL, partners point DNS for sampleprogram.sampleagency.gov at a CloudFront CDN service created by a cloud.gov broker. The Federalist team directs the CDN service to load from the proxy URL, and since the proxy is loading from S3, that completes a technical connection from the live URL to the S3 bucket contents.

Here's a full example chain:

 - https://federalist-modern-team-template.18f.gov/ is CNAME'd to https://d2xyasfn4889hb.cloudfront.net/
 - https://d2xyasfn4889hb.cloudfront.net/ is set to load from [https://federalist-proxy.app.cloud.gov/site/18f/federalist-modern-team-template/](https://federalist-proxy.app.cloud.gov/site/18f/federalist-modern-team-template/)
 - [https://federalist-proxy.app.cloud.gov/site/18f/federalist-modern-team-template/](https://federalist-proxy.app.cloud.gov/site/18f/federalist-modern-team-template/) proxies [http://cg-06ab120d-836f-49a2-bc22-9dfb1585c3c6.s3-website-us-gov-west-1.amazonaws.com/site/18f/federalist-modern-team-template/](http://cg-06ab120d-836f-49a2-bc22-9dfb1585c3c6.s3-website-us-gov-west-1.amazonaws.com/site/18f/federalist-modern-team-template/)

The URLs above have broken CSS and assets because published Federalist sites on their own URLs don't publish to a "/site/18f" directory. The above links are showing HTML content that is published at [https://federalist-modern-team-template.18f.gov/](https://federalist-modern-team-template.18f.gov/). The assets for this site are at addresses like [https://federalist-modern-team-template.18f.gov/assets/img/logo-main.gif](https://federalist-modern-team-template.18f.gov/assets/img/logo-main.gif) - whereas to display properly on the proxy or S3 buckets the assets would need to be published at https://federalist-modern-team-template.18f.gov/site/18f/assets/img/logo-main.gif. See the "/site/18f/" in the second link?


### Technical steps to set up a new site

1. The partner confirms the site is ready for an initial scan; the Federalist team scans the site and sends to GSA IT for initial approval.
1. After initial scans, the partner confirms readiness for the site to go-live at a specific permanent URL (this URL process needs to happen within a few hours timespan when started).
1. The Federalist team uses the [cloud.gov CloudFront broker](https://cloud.gov/docs/services/cdn-route/) to begin set up for a distribution for a given URL.
    * We do this by accessing our org in cloud.gov and running the command `cf create-service cdn-route cdn-route YOUR.URL.gov-route -c '{"domain": "YOUR.URL.gov", "origin": "federalist-proxy.app.cloud.gov", "path": "/site/<org>/<repo-name>"}'`. Note that the path argument here does not have a trailing slash.
1. After running the command above, the command `cf service YOUR.URL.gov-route` is used to retrieve the CloudFront URL to be used in DNS changes. We communicate that URL to whoever sets the DNS records.
1. The partner sets DNS records with a CNAME to point the subdomain (e.g. `yourprogram.youragency.gov`) to the CloudFront distribution URL (e.g. `d2oezh1w8w4o1u.cloudfront.net`).
    * It takes roughly 15-30 minutes for the CNAME to a `d2oezh1w8w4o1u.cloudfront.net`-style URL to propagate and the broker to create an HTTPS certificate, which also must propagate. Once `yourprogram.youragency.gov` shows site content (perhaps without images or CSS) under the correct HTTPS certificate, the CloudFront delegation process is complete.
1. The partner then modifies their site's Federalist's configuration with the custom URL to ensure assets and CSS are loaded at the custom URL. This is deliberate so that the site only loads at the correct URL, such as [https://federalist-modern-team-template.18f.gov/](https://federalist-modern-team-template.18f.gov/). **Please note that URLs must start with https://**.
1. The site is then served from the CloudFront broker with automatic HTTPS. Partners retain control of DNS.

Here's a [specific GitHub issue](https://github.com/18F/federalist/issues/551#issuecomment-255841203) with instructions from a successful migration. Note that the HTTPS certifications are managed automatically by the broker using [Let's Encrypt](https://en.wikipedia.org/wiki/Let%27s_Encrypt), which means there is no cost for the HTTPS certificate to partners or 18F.
