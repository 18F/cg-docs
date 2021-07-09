---
title: Access and Permissions
permalink: /pages/documentation/access-permissions/
layout: docs
navigation: pages
sidenav: pages-documentation
redirect_from: 
  - /pages/using-federalist/access-permissions/

---

## Initial access

### Personal access
Basic access to Federalist is authenticated using GitHub OAuth. Please ask your team's primary contact for Federalist to request access to Federalist by e-mailing federalist-support@gsa.gov with the GitHub usernames of those that should have access. Next, you should receive an invitation to join the federalist-users GitHub organization which you must accept before you can log in to Federalist. Primary contacts should also let us know when to remove access as part of offboarding.

### Organizational access
In order to manage sites where the repository is owned by an organization, Federalist uses GitHub oAuth at the organizational level. After you have logged in to Federalist, as described in the Personal Access section above, you must [request organization approval](https://help.github.com/en/articles/requesting-organization-approval-for-oauth-apps) for the federalist.18f.gov oAuth App. Be sure to follow up with your organization's GitHub admin to confirm approval.

## Site permissions

To take any actions on a site in Federalist, you need permissions set on the corresponding GitHub repository. Write access grants most permissions.

### Collaborator or write access

Collaborators with write access to a site's repository can:
- add a current Federalist site to their sites list
- trigger site rebuilds
- view site build history and build logs
- view site collaborator list
- view and update site settings
- view site branches, branch previews and uploaded files

### Administrator

Administrators of a site's repository can take all actions available to collaborator and can:
- add a new repo to Federalist
- delete a site from Federalist
- add/remove site collaborators

