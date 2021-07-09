---
title: Customer Responsibilities
permalink: /pages/documentation/customer-responsibilities/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from:
  - /pages/using-federalist/customer-responsibilities/
---


## Your Responsibilities

Using Federalist places certain responsibilities on you as the government user of Federalist. GSA has many internal sites running on Federalist, but GSA cannot guarantee that Federalist is compatable with your agency's specific policies.

Federalist retains other responsibilities, clarified below. The intent of these policies is to empower you to use Federalist to its full potential with awareness of your responsibilities when using advanced features.

#### You must use a GitHub account to log onto Federalist.

Federalist is a service that leverages GitHub repositories for publishing. Federalist eliminates redundancy by allowing GitHub users with editing access to a repository to configure the site on Federalist. This requires Federalist users to authorize the Federalist application on their GitHub account **and** for Federalist to be approved by the parent organizations of repos that are hosted with Federalist. Federalist users must also have two factor authentication enabled for their GitHub accounts in order to be given access.

GitHub is used across the government (see [this dashboard](https://gsa.github.io/github-federal-stats/) from our partners in GSA's Office of Government-wide Policy), and a majority of cabinet agencies have a GitHub presence. However, GSA does not endorse Github and there are other ways to launch sites if your agency is not prepared to use GitHub. At this time, no prospective Federalist customer has been deterred by integration with GitHub.

#### You own your content

Federalist provides templates for you to start with in configuring your sites, but is not responsible for editing or updating the content or local configuration of your site. The Federalist team ensures that the publishing mechanism remains available to you so that your content edits can be published nearly immediately. Your content must be low impact according to [FIPS 199](http://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.199.pdf) and each branch on GitHub is published publicly by Federalist. Federalist is not for content that requires authorization to view.

Federalist suggests that you add your 18F point of contact to your repo with editing rights for troubleshooting purposes, but this is not required.

#### You own your URL

To put a site live on Federalist, you must direct DNS for your URL to a custom address provided by the Federalist team (meaning, that visitors to that address will be pointed at Federalist so they can load your site from us). Setting DNS for a new or existing URL may involve working with other offices at your agency; these processes typically vary.

If your site is at a top level .gov domain, the process may be more challenging than for a subdomain as some DNS providers are less flexible for top level domain settings. We'll need to plan a solution for you before signing an agreement.

#### You own any custom code on your webpage

You may wish to use custom Jekyll plugins, Google Analytics from GSA's [Digital Analytics Program](https://www.digitalgov.gov/services/dap/) using embedded Javascript, or other code on your site. Federalist makes this possible - we use analytics extensively on our sites - but you are responsible for the security and stability of any custom code.

Federalist's site management processes prevent any mistakes by one Federalist customer from interfering with the build process of another Federalist customer. An individual site's build process is constrained to 1GB of memory. If your site's build attempts to use more than that limit (such as from a complex npm install), the build will not succeed.

## Federalist's Responsibilities

#### We control access to the core Federalist codebase.

Access to Federalist's configuration tools for your specific content does not grant you access to Federalist's "backend." Federalist's code is open source, but 18F's copy can only be edited by approved members of the 18F team. No one can access Federalist's management tools in [cloud.gov](https://cloud.gov) without FedRAMP-approved two factor authentication.

#### We control access to the hosting service that serves your webpages

Federalist moves content from your GitHub repositories into a secure build process and then into a file storage system that holds your site files. The credentials for the file storage system (Amazon S3 for those familiar) are secured within cloud.gov.
