---
title: Migration Guide
permalink: /pages/documentation/migration-guide/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from: 
  - /pages/using-federalist/migration-guide/

---

Welcome to your migrating journey onto Federalist! This process may be difficult. 18F generally cannot provide support for migration work beyond general guidance under a standard Federalist agreement, but if you are in a situation where you need a lot of help, please [contact us](mailto:inquiries18F@gsa.gov) for a path analysis from our Custom Partner Solutions team.

## Before you start

Get a sense of how big your migration is before you start. How many pages does your site have? This will help you estimate how long it will reasonably take to migrate your content.

- To find this out, try searching with the query “site:[your domain]” and seeing how many pages are found
- Example: “site:18f.gsa.gov” shows that there are about 722 pages on 18F’s website
  - Note that blog tag pages or others may show up as individual pages, skewing the count

## Federalist’s role

Federalist is a backend hosting service that enables lots of flexibility with the look and feel of a hosted website. Federalist is opinionated about the back end hosting of a site, so your code needs to be in a form that Federalist can host, this can be static HTML/CSS/JS files or [supported site engines](/documentation/supported-site-engines).

In general, we expect getting your site up as static files to be the quickest option, but static html can be very difficult to edit and maintain. Even though it takes more effort to initially convert your site content to a new format, using a site generator, such as Jekyll or Hugo, can ease the long-term maintenance burden of your site. For example, Jekyll and Hugo use templates to manage common content, so adding an item to a global menu can be done in one file instead of many.

## Planning the move

You should plan to move everything from your existing site into a format that Federalist can publish, though. For larger sites, getting an export of the source of your old site might be essential.

Working with your current hosting provider, list out your options for exporting, which could include:
- Access to the source code
- Access to an export of the site
- Scraping your current site. (most difficult)

Ideally, to align with how most site generators work, the export will have your content in one set of files and the frontend html/css look/feel in another set of files.

## Building a Timeline

You should create a timeline for the site migration. This may be an opportunity to restructure how your content is organized or redesign the look and feel of the site. Think about how much time/work these additional tasks require.

You should plan a stopgap measure into your migration timeline. This could be a basic Federalist setup with a landing page that provides information to key stakeholders or notifies visitors that work is in progress.

For these pages you can simply fork them on Github and reword the content in order to have it based on your website, or create your own static page to note the work in process. 18F can help you with your stopgap measure if you have a signed IAA.

## Testing the plan

Test your migration concept by first making one page and learning how to work with Federalist. Try using setting up layouts and content separately. Once the first page is live on a prototype site, you can migrate other pages, but focus on completing one first so that you have a good sense of the effort required.

You should create a staging branch in your Github repo and create a pull request from that branch to your master branch. Then, migrate to the staging branch and push to master once you’ve confirmed everything is working correctly in staging. This will help you keep track of changes and provide and easy way to identify what is causing issues. You should push to master everyday.
- You may want to consider creating separate staging branches for content and for settings

Reserve time for the go live process. Plan few days before live launch to do a soft launch for stakeholders at a different URL.

## The migration team

Who you need in your migration team is based on the work involved in your specific migration. Generally, a migration PM is probably not necessary unless you are managing against a timeline.

- Basic migrations will probably only need front end developers if you can pull a good export of content. If you can't, you may need staff to help convert content.
- If you are also doing a site redesign and/or restricting content then you may need to bring on user experience designers, visual designers, content designers or more.
- For high complexity migrations, 18F offers a path analysis to provide you with different technical options, which is not exclusive to a migration to federalist
- If you are planning on bringing on extra developers for the migration, talk about the technologies you plan to use before bringing on contractors.

Your team will need the following access to get started:
- Access to Federalist for each GitHub account involved in the migration
- The ability to create and/or edit the GitHub repo that Federalist will be building

You are able to start working while IAA is being signed.

## Additional info

If your site has over 1 GB of images to serve you should look into another way to store those files. Github is not intended to store large amounts of files.
- Github has no problem storing less than 1GB of different files types. For hosting over 1GB of files, we suggest getting access to a public database service that will load the files. One provider of this type of service is [cloud.gov](https://cloud.gov/). More information on repository size limitations is available [here](https://help.github.com/articles/what-is-my-disk-quota/).
