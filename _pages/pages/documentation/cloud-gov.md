---
title: Federalist and cloud.gov
permalink: /pages/documentation/cloud-gov/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from:
  - /pages/how-federalist-works/cloud-gov/
---

As mentioned elsewhere, Federalist is built on top of the [cloud.gov platform-as-a-service](https://cloud.gov) (PaaS). cloud.gov takes care of the vast majority of the systems functionality that enables Federalist, especially by maintaining a FedRAMP authorization. cloud.gov also provides security, logging, authentication, cryptographic protection, monitoring, and provisioning services that Federalist consumes. Federalist's [compliance memo]({{site.baseurl}}/assets/documents/Federalist-Compliance-Memo.pdf) explains the technical relationship in detail.

## How Federalist and cloud.gov collaborate

cloud.gov provides a general platform for sites with embedded compliance, but because it allows some flexibility, every system on cloud.gov must do some individual work to complete required compliance work. By narrowing focus to a more specific scope of customer needs, Federalist can manage all needed compliance work for partners, enabling sites to launch quickly.

cloud.gov, in turn, can refer potential customers to Federalist and enable them to save time and money when their needs are met by Federalist. Federalist has extended the impact of cloud.gov by launching and managing nearly 100 government sites with great efficiency.

## When you should use cloud.gov

Sometimes, customer needs may justify using both a cloud.gov system and Federalist site, especially when showing data visualizations sourced from an API. Working together, a API server and Federalist site can provide all of the functionality of a web server with a much smaller attack surface. You should consider using cloud.gov with Federalist when you need to receive/store information from site users in a custom system, or when you want your Federalist site to work together with an API.

If your site involves content that shouldn't be public, or primarily involves complex user interactions, it may be better to directly leverage cloud.gov. You can find detailed contact help at [their site](https://cloud.gov/docs/help/).
