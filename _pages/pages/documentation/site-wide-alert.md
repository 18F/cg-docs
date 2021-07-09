---
title: Site Wide Alert
permalink: /pages/documentation/site-wide-alert/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from:
  - /pages/for-federalist-developers/site-wide-alert/
---

Members of the Federalist dev team can update Federalist to display a banner explaining any maintenance, bugs, or degradation that users should be aware of. This is done by setting values in a user provided service.

## Setting A Site Wide Alert

To set a site wide alert, target the Federalist production space and run the following on the command line:

```shell
cf uups federalist-site-wide-error -p '{
  "HEADING": "There is an error",
  "BODY": "We are working on it"
}'
```

Once the user provided service is updated, the app will need to be restaged. Restaging the app will result in downtime, so it is recommended that instead a zero-downtime-deploy is run. The easiest way to do this is by re-building the master branch in CI.

Also, note the the `BODY` parameter in the user provided service may contain HTML tags to allow for the inclusion of links, bulleted lists, etc.

## Removing A Site Wide Alert

To remove a site wide alert, first update the user provided service to contain no header or body:

```shell
cf uups federalist-site-wide-error -p '{
  "HEADING": "",
  "BODY": ""
}'
```

After updating the user provided service, restage or rebuild the application as described in the instructions for setting a site wide alert above.
