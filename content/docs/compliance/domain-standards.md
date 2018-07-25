---
menu:
  docs:
    parent: compliance
title: IPv6, HTTPS, and DNSSEC
---

Here's what cloud.gov does to support relevant federal standards and recommendations, for applications on `*.app.cloud.gov` and [custom domains]({{< relref "docs/apps/custom-domains.md" >}}).

## IPv6

cloud.gov ensures all applications are accessible over IPv6. You don't have to take any action.

## HTTPS

cloud.gov ensures all applications are accessible only over HTTPS with [HTTP Strict Transport Security (HSTS) headers](https://https.cio.gov/hsts/) in accordance with the [HTTPS-Only Standard](https://https.cio.gov/). Any HTTP requests are permanantly redirected to HTTPS. You don't have to take any action.

### [HSTS preloading](https://https.cio.gov/guide/#options-for-hsts-compliance)

cloud.gov sets [`Strict-Transport-Security`]({{< relref "docs/apps/headers.md" >}}) headers for all applications by default, and has added the `cloud.gov` domain/subdomains to the HSTS preload list for most major browsers.

You are responsible for setting up HSTS preloading for your [custom domain]({{< relref "docs/apps/custom-domains.md" >}}). cloud.gov doesn't set this up for you. If you need HSTS preloading, follow [the guidance from the maintainers of the HSTS preload list](https://hstspreload.org/#opt-in). The HTTPS-Only Standard encourages HSTS preloading.

*Additional details are available in the [cloud.gov FedRAMP P-ATO documentation package]({{< relref "overview/security/fedramp-tracker.md#how-you-can-use-this-p-ato" >}}), including in System Security Plan controls SC-8, SC-12, and SC-20.*

## DNSSEC

cloud.gov does not currently support DNSSEC on `cloud.gov` domains. For example, an application at `*.app.cloud.gov` would not support DNSSEC.

If you need DNSSEC for your custom domain, you are responsible for configuring DNSSEC in your DNS system. cloud.gov can't configure DNSSEC for you because cloud.gov does not have access to your DNS system. 

cloud.gov supports mapping your DNSSEC-enabled custom domain to your applications hosted on cloud.gov -- see [DNSSEC support for the CDN service]({{< relref "docs/services/cdn-route.md#dnssec-support" >}}) and [DNSSEC support for the custom domain service]({{< relref "docs/services/custom-domains.md#dnssec-support" >}}).

*Additional details are available the cloud.gov System Security Plan, including controls SC-20, SC-21, SC-22, and SC-23.*
