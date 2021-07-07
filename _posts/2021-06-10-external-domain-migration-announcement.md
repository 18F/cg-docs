---
layout: post
title: "Migrate to the external-domain service"
date: June 10, 2021
excerpt: Customers need to take action to migrate from the cdn-service and custom-domain service to the external-domain service
---

## Who is this for?

This post is for all users of the cdn-route or custom-domain services.

## Background

The custom-domain and cdn-route services leverage Let's Encrypt to provision certificates on our users' behalf.
Several months ago, Let's Encrypt announced that they're deprecating their v1 API. To work with their new API, we've written a replacement
to the cdn-route and custom-domain services, the external-domain service. We now need to migrate your service instances
to the new service to ensure their certificates can continue renewing without issue. To accomplish this, we've written internal tooling to
migrate instances automatically without service interruption, but it does require some action on your part to initiate the migration.

## What you need to do

The new external-domain service uses a different method of validation with Let's Encrypt. The new method relies on specific DNS records being present for each of your domains. To begin the migration, you need to have configured the following DNS records for each domain on your custom-domain or cdn-route instances.

| Type  | Name                      | Value                                                           |
|-------|---------------------------|-----------------------------------------------------------------|
| CNAME | `_acme-challenge.$DOMAIN` | `_acme-challenge.$DOMAIN.external-domains-production.cloud.gov` |
| CNAME | `$DOMAIN`                 | `$DOMAIN.external-domains-production.cloud.gov`                 |

The `_acme-challenge.$DOMAIN` CNAME record allows us to provision SSL certificates on your behalf.

As an example, if you have a custom-domain service created for `directorate.agency.gov`, you'd want to create a CNAME record `_acme-challenge.directorate.agency.gov` with value `_acme-challenge.directorate.agency.gov.external-domains-production.cloud.gov`.

The `$DOMAIN` CNAME is responsible for routing the user traffic to your site.  Using the `directorate.agency.gov` example above, this would be `directorate.agency.gov.external-domains-production.cloud.gov`.

Note that the second update here changes how users get to your site.
We've made every effort to validate we're prepared for this change, but you should confirm that
`$DOMAIN.external-domains-production.cloud.gov` currently resolves before making this change.

You can do this by directly comparing the outputs of `nslookup $DOMAIN` and
`nslookup $DOMAIN.external-domains-production.cloud.gov`, or by modifying your `hosts` file
to point `$DOMAIN` to one of the IP addresses `$DOMAIN.external-domains-production.cloud.gov`
currently resolves to. **If either of these tests fail, _STOP_ and do not update**
your DNS, and contact cloud.gov support for assistance.


## What to expect

### During the migration

The migration will *not* cause:
- downtime 
- interruption to your services
- change in functionality or configuration to your services

During the migration, you may see a new service instance with a name you do not recognize in your space(s).

### After the migration

After the migration, your service instance(s) will have a new instance ID, so any references to your service
instance ID will need to be updated.
Additionally, your service instance(s) will be of a different service (`external-domain`) and plan (`domain` or `domain-with-cdn`, depending on your current instance type).

Customers that experience issues, or that have questions about this change, can send a request to support@cloud.gov.
