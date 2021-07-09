---
title: Cloud.gov Setup
permalink: /pages/documentation/cloud-gov-setup/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from:
  - /pages/how-federalist-works/cloud-gov-setup/
---

## Configuring Federalist in cloud.gov's GovCloud environment

Federalist is built to be deployed to the [GovCloud](https://cloud.gov/docs/apps/govcloud/) region in [cloud.gov](https://cloud.gov/docs/). This guide is targeted at anyone who wishes to deploy the entire Federalist system to cloud.gov from the ground up, or anyone who wishes to get a better grasp of how the system is architected by looking at how it is deployed.

Before reading this guide, it may be helpful to refer to [How Federalist Works]({{site.baseurl}}/documentation/how-federalist-works/) to understand what components make up the Federalist architecture and how they are interrelated.

A Federalist deploy comprises of the following steps:

- Deploy a private registry
- Pushing a federalist-garden-build image to the registry
- Deploying build containers
- Provisioning AWS resources
- Deploying federalist-builder
- Deploying federalist

## Deploying a private registry

Federalist's private registry stores the image that Federalist uses for its build containers. Under the hood it is a [private Docker registry](https://docs.docker.com/registry/deploying/) using an [S3 storage driver](https://docs.docker.com/registry/storage-drivers/s3/) with [read-only](https://docs.docker.com/registry/configuration/#/read-only-mode) mode enabled.

Push a registry to cloud.gov using the `library/registry:2` image:

```shell
cf push federalist-registry -o library/registry:2
```

The registry needs an S3 bucket in which to store images. To this end a S3 service can be created:

```shell
cf create-service s3 basic federalist-registry-production-s3
```

After the registry and the S3 bucket are both up and running the registry's environment will need to be configured such that the registry uses the S3 bucket as a storage backend and is in read-only mode. This can be done by setting the registry [environment](https://docs.cloudfoundry.org/devguide/deploy-apps/environment-variable.html) as such:

```yaml
REGISTRY_STORAGE: s3
REGISTRY_STORAGE_S3_ACCESSKEY: <ACCESS KEY FOR S3 SERVICE>
REGISTRY_STORAGE_S3_SECRETKEY: <SECRET KEY FOR S# SERVICE>
REGISTRY_STORAGE_S3_BUCKET: <BUCKET NAME FOR S3 SERVICE>
REGISTRY_STORAGE_S3_REGION: us-gov-west-1
REGISTRY_STORAGE_MAINTENANCE_READONLY: enabled: true
```

After setting the environment, the registry will need to be restaged to use the new configs:

```shell
cf restage federalist-registry
```

## Pushing a federalist-garden-build image to the registry

Unfortunately pushing an image to the private registry is not as simple as building the image and pushing it to the registry. To push an image you must follow the following steps:

- Build the federalist-garden-build image
- Scan the federalist-garden-build image
- Push the federalist-garden-build image

### Building the image

The federalist-garden-build image can be built with docker. Clone the repo, cd into it and run `docker build`.

```shell
docker build --no-cache --tag federalist-garden-build .
```

The image should now appear in the list when you run `docker images`.

### Pushing the image

Since the registry in Cloud.gov is in read-only mode, it is not possible to push an image to it directly. In order to push an image to the registry, a local registry with the same S3 storage driver is started and the image is pushed to that. The image is then saved in the S3 bucket where it is available to the remote registry.

We recommend using [Docker Compose](https://docs.docker.com/compose/) to run the local registry. To use Docker Compose you'll need to create a `docker-compose.yml` file. Here is an example of what a `docker-compose.yml` file would look like for a local registry:

```yaml
registry:
  restart: always
  image: registry:2
  ports:
    - 5000:5000
  environment:
    REGISTRY_STORAGE: s3
    REGISTRY_STORAGE_S3_ACCESSKEY: <ACCESS KEY FOR S3 SERVICE>
    REGISTRY_STORAGE_S3_SECRETKEY: <SECRET KEY FOR S# SERVICE>
    REGISTRY_STORAGE_S3_BUCKET: <BUCKET NAME FOR S3 SERVICE>
    REGISTRY_STORAGE_S3_REGION: us-gov-west-1
```

After creating a `docker-compose.yml`, the registry can be started:

```shell
docker-compose up
```

By default the registry runs at `localhost:5000`. To push an image to the registry it will need to be tagged it with the URL for the local registry:

```shell
docker tag federalist-garden-build localhost:5000/federalist-garden-build
```

Finally, the image can be pushed to the local registry:

```
docker push localhost:5000/federalist-garden-build
```

Now the image should be available from the remote registry as `<remote-registry-url>/federalist-garden-build`.

To confirm that this worked you can pull the image from the remote registry and inspect it:

```shell
docker pull federalist-registry.fr.cloud.gov/federalist-garden-build
docker inspect federalist-registry.fr.cloud.gov/federalist-garden-build
```

## Deploying build containers

Federalist's "build containers" are cloud.gov apps running with the `federalist-garden-build` image. The app needs to run without a healthcheck since it is not fitted to respond to HTTP requests. The build containers are set to have 1500 MB of memory and 4 GB of disk space.

To push the app use `cf push`:

```shell
cf push federalist-garden-build-1 --no-route -u none -m 1500M -k 4G -o "federalist-registry.fr.cloud.gov/federalist-garden-build"
```

The first time the app is pushed it may fail to start. That is okay. The app cannot stand on its own without an environment provided by federalist / federalist-builder. It will be restarted with a valid environment when federalist-builder schedules a build on it.

Note that it is possible to push multiple build containers, as long as they have unique names.

## Provisioning AWS resources

The next pieces of Federalist that need to be deployed will depend on certain AWS resources. That makes this a good time to provision the AWS resources that are needed:

- An RDS instance for the database
- An S3 bucket for storing the sites' files
- An SQS queue for build messages

[How Federalist Works]({{site.baseurl}}/documentation/how-federalist-works/) contains the details about how each of these should be configured.

### Provisioning an RDS instance

The RDS instance can be provisioned with cloud.gov's RDS service broker and doesn't need any special configuration:

```shell
cf create-service aws-rds shared-psql federalist-production-rds
```

### Provisioning an S3 Bucket

The S3 bucket can be provisioned with cloud.gov's S3 service broker and requires some additional configuration:

```
cf create-service s3 basic-public federalist-production-s3
```

Configuration of the S3 bucket is done with the [AWS CLI tool](https://aws.amazon.com/cli/).

Once the bucket is created CORS will need to be [enabled and configured to only serve GET or HEAD requests that come from a specific whitelist](https://cloud.gov/docs/services/s3/#allowing-client-side-web-access-from-external-applications).

First save the following snippet as `cors.json`:

```json
{
  "CORSRules": [
    {
      "AllowedOrigins": [
        "https://s.codepen.io"
      ],
      "AllowedHeaders": [
        "*"
      ],
      "AllowedMethods": [
        "HEAD",
        "GET"
      ],
      "ExposeHeaders": [
        "ETag"
      ]
    }
  ]
}
```

Then, run:

```sh
aws s3api put-bucket-cors --bucket $BUCKET_NAME --cors-configuration file://cors.json
```

Next, enable static website hosting in the bucket:

```shell
aws s3 website s3://${BUCKET_NAME} --index-document index.html
```

### Provisioning an SQS queue

At this time cloud.gov does not have an SQS service broker. The SQS queue will need to be configured elsewhere.

## Deploying federalist-builder

Note: `federalist-builder` is built to be [deployed from CircleCI](https://circleci.com/docs/2.0/deployment-integrations/). The instructions for manually deploying federalist-builder are described below and are necessary for the initial deploy, but subsequent deploys should happen on CircleCI.

The first step to deploy federalist-builder is to configure the environment. The app's [manifest.yml](https://github.com/18F/federalist-builder/blob/master/manifest.yml) set's the app's environment. The manifest sets environment variables directly for non-secret configs. For secret configs it binds user-provided services.

federalist-builder has 2 user provided services that must be created before deploying in order for the app to work.

The first is named `federalist-production-sqs-creds` or `federalist-staging-sqs-creds` depending on the environment. This user provided service should be created with the following values:

```yaml
access_key: <AWS ACCESS KEY FOR SQS QUEUE>
secret_key: <AWS SECRET KEY FOR SQS QUEUE>
region: <AWS REGION>
sqs_url: <AWS SQS QUEUE URL>
```

The second is `federalist-deploy-user` which has the following values:

```yaml
DEPLOY_USER_USERNAME: <THE USERNAME OF THE FEDERALIST DEPLOY USER>
DEPLOY_USER_PASSWORD: <THE PASSWORD OF THE FEDERALIST DEPLOY USER>
```

The user provided services can be created with the [Cloud Foundry CLI](https://docs.cloudfoundry.org/devguide/services/user-provided.html#create).

Once the user provided services are created deploying is a simple as running `cf push`.

## Creating a GitHub OAuth Application

Federalist requires a GitHub OAuth application in order to work. The OAuth
application should be setup under the GitHub organization that is deploying Federalist (e.g., the 18F org for any instance of Federalist used by 18F).

Adding an application is done within an organization's settings. Navigate to
`Settings > Developer Settings > OAuth Applications`. Fill out all of the
necessary fields. Make sure that the authorization URL is set to Federalist's
OAuth authorization URL, e.g. `https://federalist.18f.gov/auth`.

When the application is created, save the client and secret keys to add to the
`federalist-<environment>-env` user provided service as described below.

## Deploying federalist

Note: federalist is built to be [deployed from CircleCI](https://circleci.com/docs/2.0/deployment_integrations/). The instructions for manually deploying federalist are described below and are necessary for the initial deploy, but subsequent deploys should happen on CircleCI.

The first step to deploy federalist-builder is to configure the environment. The app's [manifest.yml](https://github.com/18F/federalist-builder/blob/master/manifest.yml) or [staging_manifest.yml](https://github.com/18F/federalist-builder/blob/master/staging_manifest.yml) set's the app's environment. The manifest sets environment variables directly for non-secret configs. For secret configs it binds user-provided services.

The manifest specifies the following services which are provided by cloud.gov by cloud.gov service brokers:

- `federalist-production-rds` / `federalist-staging-rds`
- `federalist-production-s3` / `federalist-staging-s3`

In additional, there's a user provided service named `federalist-production-env` or `federalist-staging-env` depending on the environment. This user provided service should be created with the following values:

```yaml
FEDERALIST_SESSION_SECRET: <SECRET FOR THE SESSION STORE>
GITHUB_CLIENT_ID: <CLIENT ID FOR GITHUB AUTH>
GITHUB_CLIENT_SECRET: <CLIENT SECRET FOR GITHUB AUTH>
GITHUB_CLIENT_CALLBACK_URL: <OAUTH CALLBACK URL FOR GITHUB AUTH>
GITHUB_WEBHOOK_SECRET: <SECRET FOR SIGNING GITHUB WEBHOOKS>
GITHUB_WEBHOOK_URL: <URL FOR GITHUB WEBHOOKS TO CALLBACK TO>
```

The user provided service can be created with the [Cloud Foundry CLI](https://docs.cloudfoundry.org/devguide/services/user-provided.html#create).

Once the user provided services are created deploying is a simple as running `cf push`.

### Federalist CDN Route

Federalist uses a CDN route service provided by cloud.gov to route traffic from the public domain to the application. To create such a service, run the following while targeting the appropriate space in cloud.gov:

```shell
cf create-user-provided-service cdn-route cdn-route federalist-route -c '{
  "domain": "federalist.18f.gov",
  "origin": "<URL WHERE FEDERALIST IS HOSTED>"
}'
```

Once this is done, running `cf service federalist-route` should give you a CloudFront domain name. Next, [open a PR against the DNS repo to add a CNAME record for federalist.18f.gov with the CloudFront URL](https://github.com/18F/dns/commit/403f67db920e629c73bd7e2c43fbb367514af8cf). Once the DNS changes propagate, Federalist should be available at [federalist.18f.gov](https://federalist.18f.gov).

By default, the CDN caches error responses, so you will also need to work with cloud.gov support to [change the mimimum error caching TTL to 0](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HTTPStatusCodes.html).

## Updating CloudFront to serve from S3

**Only to be used in proxy downtime situations (which has only happened once historically).**

1. Review initial “Default” cache behavior and origin.
[![Initial cache settings]({{site.baseurl}}/assets/images/cloudfront-update-1.png)]({{site.baseurl}}/assets/images/cloudfront-update-1.png)

2. Review the “origin path” for existing federalist-proxy.app.cloud.gov origin
[![Current origin settings]({{site.baseurl}}/assets/images/cloudfront-update-2.png)]({{site.baseurl}}/assets/images/cloudfront-update-2.png)

3. Add a new S3 origin pointing directly to the federalist S3 bucket and site’s origin path found in step 2. (Note: you can skip this if the S3 origin already exists)
[![S3 origin settings]({{site.baseurl}}/assets/images/cloudfront-update-3.png)]({{site.baseurl}}/assets/images/cloudfront-update-3.png)

4. Modify “Default” cache behavior to point to the new S3 Origin:
[![Use new origin]({{site.baseurl}}/assets/images/cloudfront-update-4.png)]({{site.baseurl}}/assets/images/cloudfront-update-4.png)

5. After downtime, modify default cache behavior back to the original state.
[![Initial cache settings]({{site.baseurl}}/assets/images/cloudfront-update-1.png)]({{site.baseurl}}/assets/images/cloudfront-update-1.png)


## Rotating credentials

Federalist uses cloud.gov's [space deployer](https://cloud.gov/docs/services/cloud-gov-service-account/#plans) service to commission separate deployer account credentials for CI (for automated deployments) and for Federalist Builder (so that it can deploy garden build containers).

The space deployer services are called `federalist-[SPACE NAME]-deployer-circle` for the credentials used in CircleCI, and `federalist-[SPACE NAME]-deployer-build` for the credentials in the `federalist-deploy-user` user-provided service that is used by Federalist Builder. `[SPACE NAME]` is either `production` or `staging`.

Occasionally, these credentials expire or otherwise need to be changed. When they do, it is necessary to regenerate the space deployer services by following the cloud.gov [space deployer documentation](https://cloud.gov/docs/services/cloud-gov-service-account/#plans). The new credentials then need to be updated in the `federalist-deploy-user` user-provided service and/or in CircleCI.

To update the credentials in CircleCI, go to the settings for [federalist](https://circleci.com/gh/18F/federalist/edit#env-vars) and [federalist-builder](https://circleci.com/gh/18F/federalist-builder/edit#env-vars). There the `CF_USERNAME_STAGING`, `CF_USERNAME_PRODUCTION`, `CF_PASSWORD_STAGING`, and `CF_PASSWORD_PRODUCTION` environment variables can be set to the correct values. This needs to be done for each app.

After the credentials are updated in CI, they need to be updated for the builder in `staging` and in `production`. The credentials live in a user provided service named `federalist-deploy-user`. That needs to be updated with new values for `DEPLOY_USER_USERNAME` and `DEPLOY_USER_PASSWORD`. To do that, see the docs on [updating user provided services](https://docs.cloudfoundry.org/devguide/services/user-provided.html#update).

