---
parent: policies
layout: ops
layout: docs
sidenav: true
title: Contingency plan
---

## Cloud.gov

This Contingency Plan provides guidance for our team in the case of trouble delivering our essential mission and business functions because of disruption, compromise, or failure of any component of cloud.gov. As a general guideline, we consider "disruption" to mean more than 30 minutes of unexpected downtime or significantly reduced service for customer applications.

Scenarios where that could happen include unexpected downtime of key external services, data loss, or [high-severity security incidents]({{ site.baseurl }}{% link _docs/ops/security-ir.md %}#1---high-severity). In the case of a security incident, the team uses the [Security Incident Response Guide]({{ site.baseurl }}{% link _docs/ops/security-ir.md %}) as well.

## Recovery objective

Short-term disruptions lasting less than 30 minutes are outside the scope of this plan. (See [Service Disruption Guide]({{ site.baseurl }}/docs/ops/service-disruption-guide/) instead.)

More than 3 hours of cloud.gov being offline would be unacceptable. Our objective is to recover from any significant problem (disruption, compromise, or failure) within that span of time.

## Contingency plan outline

### Activation

When any cloud.gov team member identifies a potential contingency-plan-level problem, they should begin following this plan. That first person becomes the **Incident Commander** (communications coordinator) until recovery efforts are complete or they explicitly reassign the Incident Commander role to another team member.

* Security note: If the problem is identified as part of a [security incident response situation]({{ site.baseurl }}{% link _docs/ops/security-ir.md %}) (or becomes a security incident response situation), the same Incident Commander (IC) should handle the overall situation, since these response processes must be coordinated.

The IC's first priority is to notify the following people. They are each authorized to decide that cloud.gov needs to activate the contingency plan. To contact them, use Slack chat, a Slack call, or call and text the phone numbers in their Slack profiles.

* cloud.gov Director (System Owner)
* cloud.gov Deputy Director
* cloud.gov Program Manager
* Cloud Operations team (@cg-operators) -- any two members can together authorize the activation of the contingency plan.

If the IC receives confirmation that cloud.gov is in a contingency plan situation, the plan is activated, and the IC should continue following the rest of this plan.

The IC should:

* Start a team videocall for coordination.
* Start a notes doc with timestamps [in Google Drive](https://drive.google.com/drive/folders/0B58iDAWKmw_BSEtqcUFFQ041MHc).

If this is also a security incident: also follow the [security incident communications process]({{ site.baseurl }}{% link _docs/ops/security-ir.md %}#initiate). As necessary, delegate assistant ICs.

### Notification

The IC should notify the following people within 15 minutes of identifying the service disruption:

* cloud.gov users ([through StatusPage](https://manage.statuspage.io/pages/swcbylb1c30f)) following [our StatusPage guidance]({{site.baseurl}}{% link _docs/ops/service-disruption-guide.md %}#statuspage).

The IC must notify the following people within 1 hour of activating the plan:

* TTS and 18F leadership -- give a brief description of the situation in Slack or email to the TTS Director (or TTS Assistant Commissioner for Operations) and the 18F Director.
* GSA Information Security -- email a link to the StatusPage and a technical description of the situation (including whether there are any known security aspects) to itservicedesk@gsa.gov, gsa-ir@gsa.gov, devops@gsa.gov (even if there is no security impact).
* FedRAMP JAB representatives -- email a link to the StatusPage and a technical description of the situation (including whether there are any known security aspects) to our JAB representatives (contact information [at the top of this doc](https://docs.google.com/document/d/1jGddQkjkQ6e9B0UTq9hfQqHe0btAbTeBGL_DxkozAcg/edit#)).

The IC should also:

* Monitor #cg-support and cloud-gov-support email for questions from customers, and respond as needed.
* Coordinate with Director or Deputy Director to identify any key customer System Owners who we should proactively email.
* Coordinate with Director or Deputy Director to identify any additional people to notify, such as TTS leadership, depending on the situation.

### Recovery

The Cloud Operations team assesses the situation and works to recover the system. See the list of [external dependencies](#external-dependencies) for procedures for recovery from problems with external services.

If this is also a security incident, the IC and Cloud Operations also follow the security incident [assessment]({{ site.baseurl }}{% link _docs/ops/security-ir.md %}#initiate) and [remediation]({{ site.baseurl }}{% link _docs/ops/security-ir.md %}#remediate) processes.

If the IC assesses that the overall response process is likely to last longer than 3 hours, the IC should organize shifts so that each responder works on response for no longer than 3 hours at a time, including handing off their own responsibility to a new IC after 3 hours.

IC communications:

* At least once an hour:
 * Post a brief update to StatusPage for cloud.gov users.
 * Send email updates to GSA Information Security and FedRAMP JAB representatives.
* Monitor #cg-support and cloud-gov-support email.

### Reconstitution

The Cloud Operations team tests and validates the system as operational.

The Incident Commander declares that recovery efforts are complete and notifies all relevant people, which typically includes:

* Post to StatusPage for cloud.gov users.
* Email GSA Information Security and FedRAMP JAB representatives.
* Notify TTS and 18F leadership.
* As needed, email customers who contacted support with concerns.

Next, the team schedules an internal postmortem to discuss the event. This is the same as the [security incident retrospective process]({{ site.baseurl }}{% link _docs/ops/security-ir.md %}#initiate). Then we write a public postmortem and post it on StatusPage for users. We should also discuss with our JAB representatives whether they need additional information.

## Internal dependencies

### Jumpbox

Internal [BOSH troubleshooting]({{ site.baseurl }}{% link _docs/ops/runbook/troubleshooting-bosh.md %}) relies on ephemeral Concourse jumpboxes. If concourse itself is unavailable, we need to create [emergency jumpboxes](https://github.com/cloud-gov/internal-docs/blob/master/runbooks/contingency.md#jumpboxes) (internal documentation)

## External dependencies

cloud.gov depends on several external services.  In the event one or more of these services has a long-term disruption, the team will mitigate impact by following this plan.

### GitHub

If GitHub becomes unavailable, the live cloud.gov will continue to operate in its current state. The disruption would only impact the team's ability to update cloud.gov.

Note: The cloud.gov team members have local copies of the repositories `internal-docs`, `cg-site`, and many others for reference during a major outage.

#### Disruption lasting less than 7 days

Cloud Operations will postpone any non-critical updates to the platform until the disruption is resolved.  If a critical update **must** be deployed, Cloud Operations will:

* Locate a copy of the current version of the required [git](https://git-scm.com/) repository by comparing last commit times of all checked out versions on Cloud Operations local systems and any copies held by cloud.gov systems (Concourse, BOSH director, etc.)
* Pair with another member of Cloud Operations to:
  * Perform the change on the local copy of the repository (if the update requires a change to git-managed source code), or use local copies of the repository instead of remote GitHub repository references (if the update depends on remote repositories but implies no change to those repositories)
  * Manually deploy the change by provisioning a Concourse jumpbox container, copying in the repository/repositories, and executing any required steps by hand based on the Concourse pipeline
    * For example, in the case of a stemcell update, a Cloud Operations member would obtain the latest stemcell object, extract and build the light stemcell using the latest available checkouts of the associated GitHub repositories, then manually upload the build artifact to the S3 bucket specified in the pipeline.

When the disruption is resolved, Cloud Operations will push any changes to the appropriate repositories in GitHub to restore them to the current known-good state. Cloud Operations will monitor Concourse to ensure it redeploys any changes pushed to GitHub. Then, Cloud Operations will verify the system is in the expected state after all automated deployments complete.

#### Disruption lasting more than 7 days

Cloud Operations will:

* Deploy and configure [GitLab Community Edition](https://about.gitlab.com/) to newly-provisioned instances
* Migrate repositories from local backups to GitLab
* Update all Concourse jobs to retrieve resources from the newly-provisioned Gitlab instance

After these steps are complete, updates will be deployed per usual policy using GitLab in place of GitHub.

### cloud-gov-notifications
All alerts automatically get delivered to the Cloud Operations team via GSA email.  If GSA email is unavailable, the [Prometheus Alert Manager](https://alertmanager.fr.cloud.gov) provides current alerts.

### GSA SecureAuth
Cloud Operations will update the `opslogin` UAA instance to allow temporary access via password authentication for any accounts that require access during a disruption in service.

When the disruption in service is resolved, Cloud Operations will disable password authentication for all accounts.

### AWS
In case of a **significant** disruption, after receiving approval from our Authorizing Official, Cloud Operations will deploy a new instance of the entire system to a different region using the instructions stored in the [cg-provision repository](https://github.com/18f/cg-provision).

If all AWS regions are disrupted, Cloud Operations will deploy the system to another BOSH-supported IaaS provider (such as Microsoft Azure).

#### AWS RDS
Major services (Cloud Foundry, BOSH, Concourse, and UAA) rely on databases provided by RDS. To restore a database from backup, see [restoring RDS]({{ site.baseurl }}{% link _docs/ops/runbook/restoring-rds.md %}).

#### Brokered AWS services

cloud.gov is not dependent on the following services, but we do broker customer provisioning and access to these services, so our customers could be impacted if they were unavailable.

If a GovCloud service were wholly unavailable, Cloud Operations would, after receiving approval from our Authorizing Official, broker access to the AWS commercial cloud equivalent, as shown below:

Brokered service | Fallback Services | FedRAMP Fallback Status |
-- | -- | -- |
AWS GovCloud Elasticache for Redis | AWS commercial Elasticache for Redis | Moderate |
AWS GovCloud Elastisearch | AWS commercial Elastisearch | Moderate |



## Continuity of Operations and Disaster Recovery Plan

The full Continuity of Operations Plan (COOP) and Disaster Recovery Plan (DRP) are out of scope for this Information System Contingency Plan (ICSP), (per [NIST Special Publication 800-34](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-34r1.pdf), p. 11). In the event of a major disaster beyond the scope of this plan we will reference the GSA/TTS COOP plan including the following _internal_ resources:

* [TTS Devolution of Operations Playbook (Jan 2020)](https://drive.google.com/file/d/1sfSVhVPBteobeqhqnyb152z1PqboOKJI/view)
* [TTS Google COOP Site](https://sites.google.com/a/gsa.gov/continuity/home/coming-soon---customized-hsso-pages/ocsit)
* [TTS-wide Emergency Contact Spreadsheet](https://docs.google.com/spreadsheets/d/1-Hhv5S0M03W4JY1k-CM5vxrZildHUdMaxAoqLTWF2Ts/edit#gid=1295161336)
* [cloud.gov COOP/DRP resources](https://github.com/cloud-gov/internal-docs/blob/master/runbooks/contingency.md#coop)
* [(deprecated) TTS Emergency Personnel Roster](https://docs.google.com/spreadsheets/d/1ITXXOu2IAntV8r6snIYHyXlRt1MgIlxpr-GudDP8tqQ/edit#gid=0)



## How this document works

This plan is most effective if all core cloud.gov team members know about it, remember that it exists, have the ongoing opportunity to give input based on their expertise, and keep it up to date. Here’s how we make that happen.

* Initial review and approval:
  * cloud.gov team leads propose this draft to the team, inviting all team members to review and comment on it. The team leads approve an initial version to be published on cloud.gov as our working document. Team leads coordinate with 18F Director of Infrastructure to make sure our plan complies with all necessary requirements.
* Distribution:
  * We publish this plan on [{{ site.baseurl }}/docs/]({{ site.baseurl }}/docs/) (which is sourced from [https://github.com/18F/cg-site](https://github.com/18F/cg-site)) in our Operations section, alongside our Security Incident Response Guide and other key team policies. Our [Onboarding Checklist](https://github.com/18F/cg-product/blob/master/OnboardingChecklist.md) includes an item for all new team members to read this plan as well as other key team policies when they join.
* How we review and update this plan and communicate those updates (to address changes to our team or systems, along with any problems we find while testing or using this plan):
  * The cloud.gov team is responsible for maintaining this document and updating it as needed. Any change to it must be approved and peer reviewed by at least another member of the team.
  * All changes to the plan should be communicated to the rest of the team.
  * At least once a year, and after major changes to our systems, we review and update the plan.
* How we protect this plan from unauthorized modification:
  * This plan is stored in a public GitHub repository ([https://github.com/18F/cg-site](https://github.com/18F/cg-site)) with authorization to modify it limited to 18F staff by GitHub access controls. cloud.gov team policy is that 18F staff may not directly make changes to this document; they must propose a change by making a pull request and ask another team member to review and merge the pull request.

## Federalist

[This Contingency Plan doc](https://docs.google.com/document/d/1YG6oucagNO_ZmmlsLPdQeFABCiVcY1BYuhDEEvDwe4Q/edit?usp=sharing) provides guidance for our team in the case of trouble delivering our essential mission and business functions due to disruption, compromise, or failure of any system components or services.

---

### Page information

* Last modified on: {% last_modified_at %}
* [Recent document history](https://github.com/cloud-gov/cg-site/commits/master/{{ page.path }}) (since 2020-02-05)
* [Older document history](https://github.com/cloud-gov/cg-site/commits/master/content/docs/ops/{{ page.slug }}.md) (before 2020-02-05)
