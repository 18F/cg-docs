---
parent: policies
layout: ops
layout: docs
sidenav: true
title: Security Incident Response Guide
linktitle: Security IR Guide
---

## Cloud.gov

This document outlines cloud.gov's internal process for responding to security incidents. It outlines roles and responsibilities during and after incidents, and it lays out the steps we'll take to resolve them.

*If you're responding to an incident, [here's our IR checklist]({{ site.baseurl }}{% link _docs/ops/security-ir-checklist.md %}) as a short, actionable companion to this guide.*

(If you're a member of the public who has noticed something in cloud.gov that may be a security problem, please see [our vulnerability disclosure policy and reporting process](https://18f.gsa.gov/vulnerability-disclosure-policy/). As it explains, we want security researchers to feel comfortable reporting vulnerabilities they’ve discovered, as set out in that policy, so that we can fix them and keep our information safe.)

## Overview

At a high level, incident response follows this process:

[Initiate](#initiate):

- An 18F staff member inside or outside the cloud.gov team (the *reporter*) notices and reports a cloud.gov-related incident, using the [18F incident response process](https://handbook.18f.gov/security-incidents/) and then notifying the cloud.gov team in [`#cloud-gov`](https://gsa-tts.slack.com/messages/cloud-gov/) using `@cg-team`.
- The first responder on the cloud.gov team (which could be the reporter if the reporter is on the team) becomes the initial *Incident Commander* (IC).
- The IC follows the [18F incident response process](https://handbook.18f.gov/security-incidents/) (or supports the reporter if the reporter already started it).

[Assess](#assess):

- The IC forms a team (*responders*) to determine if the event is actually a confirmed incident, and if so [assesses the severity](#incident-severities).
- The IC assesses whether to also activate the [contingency plan]({{ site.baseurl }}{% link _docs/ops/contingency-plan.md %}).
- The IC sends out an initial situation report (sitrep), or a false-alarm notification.

[Remediate](#remediate):

- The responders work to contain and remediate the issue; timelines vary based on the assessed severity.
- The IC coordinates, communicates, and tracks the investigation and remediation.
- If appropriate to the situation, GSA, TTS, and cloud.gov coordinate on notifying affected customers.

[Retrospective](#retrospective):

- The responding team holds a retrospective to analyze the incident, capture follow-ups and lessons-learned, and write a formal report.

During this process, the team communicates in the following places:

- Situation updates, investigation notes, and other relevant information gets captured in the GitHub issue created to track this event.
- Real-time communication happens in Slack, in the [`#incident-response`](https://gsa-tts.slack.com/messages/incident-response/) channel.
- If needed, the team can use a Google Hangout and/or Google Docs to share information that's not appropriate for Slack or GitHub (PII, etc.).

For full details, read on.

## Response process

### Initiate

An incident begins when someone becomes aware of a potential incident. We define "incident" broadly, following [NIST SP 800-61](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf), as "a violation or imminent threat of violation of computer security policies, acceptable use policies, or standard security practices" (6). This is a deliberately broad definition, designed to encompass any scenario that might threaten the security of cloud.gov.

When a person outside the cloud.gov team (the *reporter*) notices a cloud.gov-related incident, they should begin reporting it by using the [18F incident response process](https://handbook.18f.gov/security-incidents/), and then post about it in [`#cloud-gov`](https://gsa-tts.slack.com/messages/cloud-gov/) using `@cg-team`. If they don't get acknowledgment from the cloud.gov team right away, they should escalate by contacting the cloud.gov leads directly until they receive acknowledgment of their report.

When a cloud.gov team member is the first person to notice an incident, they should also begin reporting it by using the [18F incident response process](https://handbook.18f.gov/security-incidents/) and posting about it in [`#cloud-gov`](https://gsa-tts.slack.com/messages/cloud-gov/) using `@cg-team` (including notifying the cloud.gov leads).

In either case, the first participant on the cloud.gov team becomes the initial *Incident Commander* (IC) and carries out the next steps in the response. The IC's responsibility is coordination, not necessarily investigation. The IC's primary role is to guide the process. The first responder may remain IC throughout the process, or they may hand off IC duties later in the process.

The IC makes sure that the [18F incident response process](https://handbook.18f.gov/security-incidents/) is followed, including supporting the reporter if the reporter already started it, or starting it if nobody has started it yet.

#### Comms at the Initiate phase

Note that at this point the issue's status is "investigating" — we haven't confirmed that it's really an issue yet. So, we should actually refer to this as just an "event" at this point; it doesn't become an "incident" until we've confirmed it.

At this phase, communications should follow the steps in the [18F incident response process](https://handbook.18f.gov/security-incidents/) including creating an issue in the [`security-incidents`](https://github.com/18f/security-incidents) GitHub repository. Copy the following template to create the issue:

```
Short description of what's going on

- **Status**: investigating
- **Severity**: unknown
- **Reporter**: who initially reported the issue
- **IC**: Your Name
- **Responders**: Any other responders

Any extra details about the issue can go here.
```

The IC is responsible for keeping this issue up-to-date as investigation and remediation progresses. Everyone involved in the issue (*responders*) should leave notes as comments on the issue.

- The IC may start a Google Hangout and/or create Google Docs so that responders can share sensitive information not suitable for sharing in GitHub or Slack.


### Assess

The next step is to assess the issue. We need to answer two questions:

- Is this an incident (i.e., did the thing we suspect happen actually happen?)
- If so, how severe is it? (This will determine how our response proceeds.)

To answer these questions, the IC should form a response team by DM'ing people in Slack. The response team should work to confirm the issue and assess its impact.

If the issue turns out to be a false alarm, the IC should update the ticket, setting the status to "false alarm", and closing the issue.

If the issue is valid, the team should assess its impact and determine an initial severity following the incident severity guide below. (Note that the severity can change over the lifespan of the incident, so it's OK to determine the initial severity fairly quickly.)

Once this is done, the IC should update the ticket, noting:

- Status: "confirmed"
- Severity: High/Med/Low
- Any new/changed responders

The IC should assess whether to also activate the [contingency plan]({{ site.baseurl }}{% link _docs/ops/contingency-plan.md %}).

At this point, the IC should write an initial situation report ("sitrep") confirming the incident, summarizing what's going on, identifying the IC, and linking to the issue. Here's an example sitrep:

```
Subject: [sitrep] The chickens have escaped

https://github.com/18F/security-incidents/issues/12345

Severity: high
IC: Farmer Jane
Responders: Spot the Dog, Farmer Dave

We've confirmed reports of escaped chickens.
Looks like a fox may have tunneled into the run.
Dave is working to fix the fence, Spot is tracking the fox.
```

This sitrep should be:

- Posted in [`#incident-response`](https://gsa-tts.slack.com/messages/incident-response/)
- Emailed to `gsa-ir@gsa.gov` and `devops@gsa.gov`
- Emailed to FedRAMP ISSO (JAB TR reps), cc: info@fedramp.gov and cloud-gov-compliance@gsa.gov. They require daily updates on open incidents.
- Sent (email or Slack) to external stakeholders, if applicable and relevant

#### Comms at the Assess phase

Updates and real-time chat should continue as above (updates on the GitHub issue, chat in Slack or Google Hangouts).

### Remediate

At this point, we're trying to fix the issue! Remediation will be very situation-specific, so specific steps are hard to suggest. However, a few guidelines to follow during this process:

- The IC's responsibility is coordination, communication, and information-collection. The remediation team will be focused on resolving the issue, so it's up to the IC to make sure that we properly track what happened, how we're fixing it, who's doing what, etc. Ideally, the notes kept by the IC should be sufficient for an outside investigator to independently follow the work of the response team and validate the team's work.

- The team will develop a list of **leads** — actionable information about breaches, stolen data, etc. The IC should track these leads, maintain information about which are being investigated (and by whom), and what information that investigation leads. These can be tracked as checklists in the GitHub issue.

- Similarly, the team will develop a list of **remediation steps**. The IC is likewise responsible for tracking those, making sure they're assigned and followed-up, and verifying them as they're completed. These may be tracked in the central GitHub issue as well. The IC should distinguish between immediate concerns which should be completed before the incident is considered resolved and long-term improvements/hardening which can be deferred to the Retrospective.
	- **If the incident involved exposure of information to an unauthorized party,** the remediation steps must include working with TTS Infrastructure and GSA Information Security to notify the owner of the information, coordinate with the owner of the information, and notify the recipient of their obligations for appropriate handling of the information in the context of applicable federal laws, directives, policies, and/or regulations. The specific appropriate point of contact for customers depends on the situation.

- The response team should aim to adopt a *containment* strategy: if machines are compromised, they should avoid destroying or shutting them down if possible (this can hamper forensics). For AWS instances, you can leave the instance running and instead reconfigure the Security Group for the instance to drop all ingress and egress traffic until forensics can be performed.

- Remediation may require service disruption. If it does, the team should proceed in a different way depending on the severity:

    - For High-severity incidents, the team should take action immediately, even if this causes disruption. A notification about the disruption should be sent out as soon as possible, but the team needs no permission to take action at this level.

    - For Medium-severity incidents, the team should notify the cloud.gov leads of the planned action, and help them assess the relative risk of disruption vs. security. If the leads are unavailable via Slack, they should be contacted using the phone numbers in their Slack profiles. The team should reach a collaborative decision on action, with a bias towards disruption. If they can't be reached within 1 hour, the team may take action without them.

    - For Low-severity issues, the team should notify as above, and not take action until a mutually-agreed-on course of action has been determined.

- Remediation can sometimes take a long time. If the issue progresses for more than 3 hours without being resolved, the IC should plan for a long remediation. This means:

    - Determine if remediation efforts should be "business hours" or "24/7". This will depend on the severity of the issue, and whether breaches are ongoing.

    - For 24/7 responses, the IC should begin shift-planning. Generally, responders should only work 3 hour shifts, so the IC should begin scheduling shifts and sending people "home" to preserve their ability to function.

    - The IC also needs to get into rotation — again, 3 hour shifts are about the maximum suggested. So, the IC should likely hand off duties at this point.

Once the incident is no longer active — i.e. the breach has been contained, the issue has been fixed, etc. — the IC should spin down the incident. There may still be longer-term remediation needed, and possibly more investigation, but as long as the incident is no longer active these activities can proceed at the regular pace of business. To close out an incident, the IC should:

- Set the status of the incident to "resolved".
- Send a final sitrep to stakeholders.
- Email after-action to FedRAMP ISSO (JAB TR reps), cc: info@fedramp.gov and cloud-gov-compliance@gsa.gov
- Thank everyone involved for their service!

#### Comms at the Remediate phase

- Updates and real-time chat should continue as above (updates on the GitHub issue, chat in Slack or Google Hangouts).

- The IC should continue to post updated sitreps on a regular cadence (the section on severities, below, suggests cadences for each level). These sitreps should be sent to Slack, to GSA-IT, US-CERT, and [FedRAMP ISSO (JAB Technical Reviewers)](https://docs.google.com/document/d/1jGddQkjkQ6e9B0UTq9hfQqHe0btAbTeBGL_DxkozAcg/edit) via email, and to any other stakeholders identified throughout the process (e.g. clients).

### Retrospective

The final step in handling a security incident is figuring out what we learned. The IC (or one of the ICs if there were multiple, or a designated other party) should lead a retrospective and develop an incident report.

We follow [the postmortem steps in the service disruption guide]({{ site.baseurl }}{% link _docs/ops/service-disruption-guide.md %}#ensure-a-postmortem-happens), including writing an internal report. If appropriate, we also write a public postmortem to post on [Updates]({{ site.baseurl }}/updates/) or [StatusPage](https://cloudgov.statuspage.io/) (depending on the type of problem).

The internal report should contain a timeline of the incident, details about how the incident progressed, and information about the vulnerabilities that led to the incident. A cause analysis is an important part of this report; the team should use tools such as [Infinite Hows](http://www.kitchensoap.com/2014/11/14/the-infinite-hows-or-the-dangers-of-the-five-whys/) or [Five Whys](https://en.wikipedia.org/wiki/5_Whys) to try to dig into causes, how future incidents could be prevented, how responses could be better in the future, etc.

The internal report should also contain some basic response metrics:

- Discovery method (how did we become aware of the issue?)
- Time to discovery (how long did it take from when the incident started until we became aware of it?)
- Time to containment (how long did it take from when we became aware until the issue was contained?)
- Threat actions (which specific actions -- e.g. phishing, password attacks, etc) -- were taken by the actor)?

## Incident Severities

Severity ratings drive the actions of the response team. Below are the severities ratings we use, some examples of incidents that might fall into that bucket, and some guidelines for ICs and response teams about how to treat each class of incident.

Note the severities may (and often will) change during the lifecycle of the incident. That's normal.

### 1 - High Severity

High-sev incidents successfully compromise the confidentiality/integrity of Personally Identifiable Information (PII), impact the availability of services for a large number of customers, or have significant financial impact. Examples include:

- Confirmed breach of PII
- Successful root-level compromise of production systems
- Financial malware (ie. CryptoLocker) targeting 18F staff
- Denial of Service attacks resulting in severe outages
- Unauthorized access or traffic across network interconnect

Guidelines for addressing High-sev issues:

- Work will likely be 24/7 (e.g. work until the issue is contained).
- Responders are empowered to take any step needed to contain the issue, up to and including complete service degradation.
- Sitreps should be sent every hour, or more.

### 2 - Medium Severity

Medium-sev incidents represent attempts (possibly un- or not-yet-successful) at breaching PII, or those with limited availability/financial impact. Examples include:

- Suspected PII breach
- Targeted (but as-of-yet) attempts to compromise production systems
- Spam/phishing attacks targeting 18F staff
- DoS attacks resulting in limited service degradation

Guidelines for addressing Medium-sev issues:

- Response should be business-hours.
- Responders should attempt to consult stakeholders before causing downtime, but may proceed without them if they can't be contacted in a reasonable time-frame.
- Sitreps should be sent approximately twice a day.

### 3 - Low Severity

Low-sev incidents don't affect PII, and have no availability or financial impact. Examples include:

- Attempted compromise of non-important systems (staging/testing instances, etc.)
- Incidents involving specific employees
- DoS attacks with no noticeable customer impact

Guidelines for addressing Low-sev issues:

- Response should be business-hours.
- Responders should avoid service degradation unless stakeholders agree.
- Sitreps should be sent approximately daily.

## Federalist

The Federalist platform service adheres to the [TTS Handbook's security incidents guide](https://handbook.tts.gsa.gov/security-incidents/).

---

### Page information

* Last modified on: {% last_modified_at %}
* [Recent document history](https://github.com/cloud-gov/cg-site/commits/master/{{ page.path }}) (since 2020-02-05)
* [Older document history](https://github.com/cloud-gov/cg-site/commits/master/content/docs/ops/{{ page.slug }}.md) (before 2020-02-05)
