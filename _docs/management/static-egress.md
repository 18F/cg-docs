---
parent: management
layout: docs
sidenav: true
redirect_from: 
    - /docs/apps/static-egress/
title: Connect to external services
---

You can set up your application to communicate with any external service that is accessible over the internet. For example, you can integrate your application with services in your agency infrastructure or with APIs from commercial providers.

All traffic from applications on cloud.gov will transit the internet. You can secure the communication with authentication and with encryption in transit, and you can limit access to specific IP ranges.

## Securing communication for a single application and service

To ensure only your application can communicate with the external service, the service should be configured to authenticate each client using client-specific credentials.

Authentication options:

* **cloud.gov client application connecting to external services:** If your application on cloud.gov needs to be a client of an existing server application outside of cloud.gov, your application on cloud.gov can have a TLS certificate that the server application validates, or you can use application-level authentication (such as Authorization headers). In addition, you can configure outside services to accept connections from the [cloud.gov egress address ranges](#cloudgov-egress-ranges). There are no restrictions on outbound UDP or TCP connections, such as DNS (UDP), HTTP or HTTPS (TCP).
* **cloud.gov server application accepting external client connections:**  Your server application on cloud.gov can only serve HTTPS requests, not other TCP or UDP protocols. If you have a server application on cloud.gov that needs to secure connections from a client application outside of cloud.gov, your server application can limit inbound IP ranges using a [user-provided service instance](https://docs.cloudfoundry.org/devguide/services/user-provided.html) of a [route service](https://docs.cloudfoundry.org/devguide/services/route-binding.html), and you can set up application-level client authentication, such as Authorization headers. (*Limitations:* In the current cloud.gov architecture, you cannot use protocol-level authentication, such as a TLS certificate, to authenticate clients connecting to your server application on cloud.gov.)

## Restricting network exposure of an external service 

Some service providers filter clients based on IP ranges to minimize the surface area of their service on the open internet. If your service provider requires this, you can provide the [cloud.gov egress addresses](#cloudgov-egress-ranges) to the people who handle network security for the service. 

**Note:** Filtering by IP address **alone** will only ensure that traffic is originating from an application on cloud.gov. It provides no guarantee that the traffic is from _your_ application. Applications from other agencies and [sandbox accounts]({{ site.baseurl }}{% link _docs/pricing/free-limited-sandbox.md %}) will also originate from the same addresses.


### cloud.gov egress ranges
All traffic from cloud.gov-hosted applications originates from these IP ranges:

* `52.222.122.97/32`
* `52.222.123.172/32`

We publish these egress (outbound) IP ranges because that information is already public if you inspect any application on cloud.gov. Filtering by egress IP range is only suitable in *addition* to authentication and authorization between the client and server applications.

### Managing requests to rate limited external resources

Some external resources that your application can connect to may be rate limited. To ensure responsible use of rate-limited external resources, you should:

* Check to see if rate limits will be applied to the resource you are trying to connect to.
* Make sure you understand the behavior of the external resource in the event that a rate limit has been exceeded (e.g., returning an HTTP 429 response).
* Set any fetch or refresh settings in your application for rate limited resources responsibly.
* Check your application logs frequently for any HTTP responses indicating that a rate limit may have been exceeded, and adjust your application settings accordingly.