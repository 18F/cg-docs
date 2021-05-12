---
parent: policies
layout: ops
layout: docs
sidenav: true
title: Service disruption guide
---

This guide outlines how we handle communications for service disruptions during East Coast business hours.

Not a guide for:

* Hours outside of East Coast business hours -- out of scope for this document.
* Security incidents -- switch to [security incident guide]({{ site.baseurl }}{% link _docs/ops/security-ir.md %}).
* Major service disruptions (more than 30 minutes of unexpected downtime or significantly reduced service) -- switch to [contingency plan]({{ site.baseurl }}{% link _docs/ops/contingency-plan.md %}).

## Threshold for "service disruption"

**If there is an unexpected user-impacting problem that we believe is a platform problem.** 

Examples:

* We caused end-user-visible errors in customer applications.
* A platform-owned component such as the dashboard or brokers is giving errors.
* Two or more teams using cloud.gov have reported the same non-trivial problem, which indicates it isn't just a problem with a customer application.
* AWS or another service we depend on is causing problems for our users.
* Something unexpected went wrong during scheduled maintenance, and it impacts users.

## StatusPage

### How soon to post to [Status Page](http://cloudgov.statuspage.io/)?

As soon as possible. Goal: at most 15 minutes after the first cloud.gov team member notices the problem.

### Who is responsible for posting and updating?

* First person on the cloud.gov team who notices a problem is responsible for finding the right people to address it (can include themself), including finding a person with [Status Page](http://cloudgov.statuspage.io/) access to post about it (can include themself).
* Platform squad person on support rotation should be the first person to begin addressing the technical issue (and the first person for non-platform squad people to ping for help with this). See the platform squad channel topic for the current person.
* A person should explicitly say “I'm taking care of Status Page for this” and continue being responsible for updates until they explicitly hand off that responsibility.

### Drafting and approval

Log into the [status page management console](https://manage.statuspage.io/pages/swcbylb1c30f) and create an incident post.

To draft the post: summarize the *observable symptoms* and the state of the incident; don't try to explain the detailed internal state/cause in detail! Use active voice as much as you can (*"we discovered x"* rather than *"x was discovered"*). In a *security incident*, do not reveal any exploits, or information that advantages/encourages malicious actors, without coordinating with compliance and leadership.

If there are user-visible problems, set the status of appropriate components.

[We have some templates and drafting space in this doc.](https://docs.google.com/document/d/1paDOxlB7GFItrEJ9pqPExApiAd4GeB_SpGR6Ronf4Lw/edit)

### Updates

* If you're writing an update to a post about a new related problem, also update the post summary (the subject line) to make it summarize the whole event.

### When to close

We close the event when **we believe the disruption is no longer affecting customers**.

The person with StatusPage responsibility is responsible for asking “any objections to closing it?” and closing it if none.

* It's helpful but not required to write something that’s informative about the resolution as a closing message.
* Ensure that the component status is set appropriately.

A postmortem is not necessary to close, and it does not have to happen immediately after closing.

## FedRAMP

Report to FedRAMP non-security outages that impact agency customers (per their [incident communication guide](https://www.fedramp.gov/assets/resources/documents/CSP_Incident_Communications_Procedures.pdf). Don't notify US-CERT.  Send a summary to info@fedramp.gov upon closing the incident, and file the post-mortem in [18F incidents information & forms](https://community.max.gov/pages/viewpage.action?pageId=1034682617).

## Ensure a postmortem happens

The person who closes the event should then put a card on the component-owning squad’s board in the urgent lane about holding a retrospective discussion and writing the public postmortem.  The card's acceptance criteria should be that the retrospective discussion has happened, and that the public postmortem that resulted has been posted.

For non-security-sensitive work, work to resolve root causes does not have to be completely done before we post the postmortem.

### Retrospective meeting guide

We hold a retrospective discussion as soon as possible after a cloud.gov service disruption or other incident.

We keep our retro notes in [this folder](https://drive.google.com/drive/folders/0B58iDAWKmw_BSEtqcUFFQ041MHc).

Before or during the meeting, we put together a timeline of the incident in the doc, beginning at the time the incident was announced, and ending at the point we declared the incident over. Everybody is welcome to add their observations to the timeline, including which actions were taken when, the effects observed, and their understanding of the events.

<!-- Discussing remediation steps is important for IR-4 and SI-2 -->

1. The facilitator starts by reading the retrospective prime directive: _Regardless of what we discover, we understand and truly believe that everyone did the best job they could, given what they knew at the time, their skills and abilities, the resources available, and the situation at hand._
1. We review the timeline and add anything we have missed.
1. We analyze the factors that contributed to the incident.
1. We propose, discuss, and prioritize remediation steps to reduce the likelihood of future incidents, to improve detection and response times for future incidents, to improve our incident handling processes and training, and to validate and test these remediation steps.

We add the work that comes out of the analysis to our backlog, and we write a public postmortem for our users.

### Checklist for drafting postmortems

In the public post, we exclude any information that may be sensitive, such as information related to exploitable security flaws/vulnerabilities, sensitive infrastructure details, or PII. For details, see our [guidelines for protecting sensitive information](https://github.com/18F/open-source-policy/blob/master/practice.md#protecting-sensitive-information).

We typically use a **What happened** + **What we're doing** structure.

Check your draft against the following questions that customers typically have:

* What part(s) of the system had a problem, in terms that I recognize?
* How long did the problem last, and at what time?
* What was the effect on my applications and my own users? (This is important.)
* How are you going to prevent it from happening again?
* What public issue or ticket can I track to follow your progress?
* What actions should I take, if any?

They also have a baseline expectation that we have good answers to the following questions. The answers to these questions might be implied in your explanation, instead of being explicitly addressed:

* Did you notice the problem quickly after it started happening?
* Did you take appropriate actions after you noticed the problem?
* Do you know why the problem happened?
* If it happens again, how are you going to notice faster and respond better next time?
* Was there anything suspicious from a security perspective, or do you have confidence that all of the cause-and-effect steps were caused by ordinary reasons instead of malicious actors?

---

### Page information

* Last modified on: {% last_modified_at %}
* [Recent document history](https://github.com/cloud-gov/cg-site/commits/master/{{ page.path }}) (since 2020-02-05)
* [Older document history](https://github.com/cloud-gov/cg-site/commits/master/content/docs/ops/{{ page.slug }}.md) (before 2020-02-05)
