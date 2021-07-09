---
title: Launch Checklist
permalink: /pages/documentation/launch-checklist/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from: 
  - /pages/using-federalist/launch-checklist/

---

We've worked to make launching a live site on Federalist as simple as possible. You will need to complete these steps:

1. Sign an IAA with 18F to use Federalist.
2. Verify that your agency accepts the risk of launching a site on Federalist.
3. Build your site and verify that the site is ready for launch using our URL previews.
4. Contact the Federalist team; as part of our compliance process, GSA will complete a security scan of your site and let you know of the results within a week.
5. When you are ready to launch your site with a production-ready URL, the Federalist team will provide you with a CloudFront URL (For example: https://d2xyasfn4889hb.cloudfront.net/) and TXT record (needed to issue your auto-renewed TLS certificate). You'll need to configure your DNS records with the CNAME and TXT values within 24 hrs. (Note: Please be aware that you cannot set a CNAME for a root domain.  If you are looking to set a CNAME for a root domain like example.gov, a common solution is to create the domain www.example.gov, set the CNAME for www.example.gov, and setup a redirect from example.gov to www.example.gov)
6. When you set the CNAME for your domain, a SSL certificate should automatically be created and associted with your domain within 1 hour. (Note: If you do not set the Cloudfront CNAME and TXT record within 24 hrs, your certificate will fail to be issued and we will need to re-initiate the certificate issuance process.)
7. Once the domain's CNAME and TXT record is set, your site content will be available at your custom URL. If your assets (Javascript, CSS, Images, etc ) do not load properly, you will need to make sure your custom URL is properly set in Federalist web interface. Technical background on this process is [here]({{site.baseurl}}/documentation/custom-urls/).
8. After your customURL is set in the Federalist web interface, Federalist will rebuild your site, which will set the proper URLs in the site.  Once the site is rebuilt, you will be fully live.
