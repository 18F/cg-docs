---
title: Why Use Federalist?
permalink: /pages/documentation/why-use-federalist/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from:
  - /pages/about-federalist/why-use-federalist/
---

If you’re a program manager or a federal web developer you’ve probably been given a seemingly simple task: create a website as part of an initiative at your agency. The hardest part is often not crafting the content or designing the website, but getting the security and privacy compliance in order to launch and maintain the actual website. For that work, you might have to hire a contractor or put extra strain on your agency’s web team.

It shouldn’t be that way.

## Why Federalist?

Federalist serves our fellow federal partners by expertly managing the backend and compliance work to launch and manage a website, allowing you to focus your expertise on your content.

- **FedRAMP Certified Platform** Federalist is hosted on cloud.gov, a platform with security and compliance verified by the FedRAMP Joint Authorization Board, which means you can trust that your site will be fast, reliable, and ready for unexpected surges in traffic.

- **Compliance out of the box** Federalist’s specific implementation of cloud.gov’s services to host static sites carries a [GSA Agency ATO]({{site.baseurl}}/assets/documents/Federalist-Compliance-Memo.pdf). If your agency accepts this <span data-term="ATO" class="term">ATO</span>, you can host Federalist sites without running your own compliance process. We understand those processes typically cost around $95,000 per site, and $135,000 when including a pentest. Using Federalist allows you to leverage our compliance work, saving you time and money.

- **Easy previews and content approvals** Federalist builds out draft updates to your site at a special preview URL. This enables your team to know their changes will show up correctly when pushed to the main site, and makes it easy to get signoff on content updates from stakeholders.

[![Diagram of Federalist architecture]({{site.baseurl}}/assets/images/how-federalist-works-diagram.png)]({{site.baseurl}}/assets/documents/how-federalist-works-diagram.pdf)

## Why static websites?

Federalist generates static websites instead of using a more complex content management system (CMS) like Drupal or WordPress. There are several advantages to static websites:

- **Less complexity and vulnerability:** Avoiding the use of a traditional CMS means avoiding problems like maintaining  server applications (no PHP or Apache / IIS) in addition to database scaling and redundancy. Production-level static sites generally require one simple static file server or service as opposed to dozens. This means that the whole website is easier to maintain and has fewer vulnerabilities.

- **Update content without writing code:** Static websites often store content in simple text files that anyone on the team can edit. The GitHub editor enables any team member to complete basic edits to their text files regardless of their experience writing code.

- **Easy to host and maintain:** It’s very easy to host static website files on Federalist. You can easily take advantage of scaling services like Amazon S3 through Federalist where you pay only for what you use.

- **Fully customizable:** Designers and developers can customize static websites into whatever form they want without having to strip away CMS-provided features and defaults. Static sites can be used to build many different types of sites, from organization homepages or intranets to pre-generated web application program interfaces that display data from a separate server.

## What can I build with Federalist?

Federalist supports plain files and certain [site engines]({{site.baseurl}}/documentation/supported-site-engines), and provides [ready-to-use Jekyll templates]({{site.baseurl}}/documentation/templates/) for several common website types.

Possible sites include:

- A simple homepage for your government organization with information about personnel, meetings, and contact information.

- Data visualization sites that pull data from a separate API server (Federalist does not provide API servers).

- A single page with information about a recently released report that may receive a lot of traffic.

For real world examples, see this list of [existing sites]({{site.baseurl}}/success-stories) out of the 134 sites currently live on Federalist.
