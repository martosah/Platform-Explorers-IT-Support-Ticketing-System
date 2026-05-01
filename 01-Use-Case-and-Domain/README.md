# Section 1 — Use Case and Domain

## What I Set Out to Design

I set out to design an **IT Support Ticketing System** — also known in industry as an **IT Service Management (ITSM) platform** or **Service Desk system** — for a fictional mid-to-large regulated enterprise called TechCare Solutions.

To approach this properly, I started by grounding myself in what an IT Support Ticketing System actually is in a modern enterprise context, because the term means very different things at different scales. A tool that works for a 50-person startup is not the same as one that works for a 1,500-person regulated enterprise. I needed to understand which version I was building before I designed anything.

---

## The ITIL 4 Foundation

Modern IT Support Ticketing Systems are not built in a vacuum. They are built to operationalise a globally recognised framework: **ITIL 4** (the Information Technology Infrastructure Library, version 4) — the de facto standard for IT Service Management used by virtually every Fortune 500 company, government agency, bank, and healthcare provider in the world.

ITIL 4 defines IT Service Management as *"a set of specialized organizational capabilities for enabling value to customers in the form of services."* In simpler terms: ITIL is how mature IT organisations work. It tells us how to handle disruptions, fulfil requests, manage changes, prevent recurring problems, and continuously improve service delivery.

For my design, three ITIL practices directly shape what the system must do:

### 1. Incident Management

The practice of minimising the negative impact of incidents by restoring normal service operation as quickly as possible.

An **incident** is *an unplanned interruption to a service or a reduction in the quality of a service.* Examples: "my laptop won't boot," "the email server is down," "I can't access SAP."

### 2. Service Request Management

The practice of supporting the agreed quality of a service by handling all pre-defined, user-initiated service requests in an effective and user-friendly manner.

A **service request** is *a request from a user for something to be provided.* Examples: "I need access to the finance drive," "please install Visio on my laptop," "set up a new joiner."

### 3. Problem Management

The practice of reducing the likelihood and impact of incidents by identifying actual and potential causes of incidents.

A **problem** is *the underlying cause of one or more incidents.* Example: "the printer driver update on Build 22631 is causing repeated print failures across the organisation."

---

## Why This Distinction Matters

The distinction between Incident, Service Request, and Problem is not academic — it shapes the entire system.

| | Incident | Service Request | Problem |
|---|---|---|---|
| **Says** | "Something is broken — fix it." | "I need something — provide it." | "Why does this keep happening — eliminate it." |
| **Trigger** | Unplanned disruption | Pre-defined request | Pattern across multiple incidents |
| **SLA** | Tight (minimise downtime) | Standard fulfilment time | No SLA — investigative |
| **Workflow** | Diagnose → resolve | Approve (if needed) → fulfil | Investigate → eliminate root cause |

A mature ticketing system handles all three, with different workflows for each. My Phase 1 design covers Incident and Service Request management. Problem Management is documented as a Phase 2 capability — a deliberate scoping decision I'll explain later.

---

## What an IT Support Ticketing System Actually Does

At the operational level, the system performs eight core functions. I structured my design around these:

**1. Intake** — Capture every issue or request, regardless of how it arrives — web form, email, phone call, walk-up. The system serves as a single source of truth for all IT demand.

**2. Classification** — Categorise each ticket properly: by type (Incident vs. Service Request), by category and sub-category, by urgency, by impact, by affected service. This drives everything else: routing, SLA assignment, reporting, and root cause analysis.

**3. Prioritisation** — Determine how quickly the ticket must be addressed, using the **ITIL Priority Matrix** (Urgency × Impact = Priority). I chose this approach deliberately because it's a calculated outcome based on objective criteria, which prevents the well-known anti-pattern of every user marking their ticket "Critical."

**4. Assignment & Routing** — Route the ticket to the right support team or individual based on category, skills, location, and current workload. This includes **support tier escalation** — Level 1 (Service Desk, generalist), Level 2 (specialist technicians), Level 3 (senior engineers, vendors).

**5. Lifecycle Management** — Track the ticket through its full lifecycle — from logged, through assigned, in progress, on hold (with sub-statuses for awaiting user, awaiting vendor, awaiting parts, awaiting approval), resolved, closed, and reopened where applicable. Every status change is timestamped and audited.

**6. Communication** — Notify the right people at the right time: confirmation to the requester, assignment notification to the agent, escalation alerts to managers, SLA breach warnings, resolution confirmations.

**7. SLA Enforcement** — Measure response and resolution times against agreed Service Level Agreements, flag tickets approaching breach, escalate breaches to managers, and report compliance to leadership.

**8. Reporting & Continuous Improvement** — Provide dashboards and reports across multiple dimensions: operational (today's queue), tactical (this month's performance), strategic (year-over-year trends, root cause patterns, service quality). This data feeds the Continual Improvement practice in ITIL — turning operational data into service enhancements.

---

## Why TechCare Needs This — In ITIL Terms

In ITIL 4 vocabulary, TechCare currently has **no operational Service Desk function**. They have IT staff who *do* support work, but they have no system, no process discipline, and no measurable service. This produces several specific failures:

- **No Incident Records** — incidents are tracked in personal inboxes, sticky notes, and memory
- **No SLA framework** — there is no agreement about response or resolution times, and no way to measure compliance even if there were
- **No Problem Management** — recurring incidents are never linked, so the underlying problems are never identified or solved
- **No Service Catalog** — users don't know what they can request or how to request it
- **No Continual Improvement** — there is no operational data, so improvement is anecdotal at best
- **No audit trail** — for a regulated organisation, this is not just an inefficiency; it is a **compliance risk**

The system I am designing addresses all of these. It will be TechCare's **operational backbone for IT service delivery** — the platform that converts informal, ad-hoc support work into a measurable, governable, auditable service.

---

## Scope of My Phase 1 Deliverable

I want to be explicit about what is in and out of scope, because this is one of the most important decisions in any ITSM implementation. Trying to do everything at once is the most common reason ITIL implementations fail.

### In scope for Phase 1

- Incident Management
- Service Request Management
- Basic SLA management with Urgency/Impact priority matrix
- Multi-level service categorisation
- Knowledge capture (resolution notes, root cause)
- Role-based access aligned to ITIL functions
- Reporting and dashboards
- Audit trail for regulatory compliance

### Out of scope for Phase 1 (deferred to Phase 2)

- Formal Problem Management (linking incidents to problem records)
- Change Management (CAB, change calendar, RFC workflow)
- Configuration Management Database (CMDB)
- Service Catalog with approval workflows
- Major Incident Management war rooms and dedicated dashboards
- AI-driven triage and chatbot self-service
- Knowledge Base with publishing workflow

This scoping is deliberate. Mature organisations stage their ITSM rollouts. Phase 1 establishes the foundation; Phase 2 layers on the maturity. I documented this distinction explicitly so reviewers understand my decisions are deliberate scope choices, not omissions.

---

## Key Vocabulary I Use Throughout

These terms appear throughout the rest of the project documentation. They're not jargon — they are the working vocabulary of professional IT service delivery, and I learned to use them fluently as part of this exercise.

| Term | Definition |
|---|---|
| **Incident** | An unplanned service disruption |
| **Service Request** | A pre-approved, standard user request |
| **Problem** | The root cause of one or more incidents |
| **Ticket** | A generic term for any record (incident or service request) in the system |
| **Service Desk** | The function that owns ticket intake and Level 1 resolution |
| **L1 / L2 / L3** | Support tiers — generalist, specialist, expert |
| **SLA** | Service Level Agreement — time-based commitment for response and resolution |
| **MTTA** | Mean Time To Acknowledge — how long until someone responds |
| **MTTR** | Mean Time To Resolve — how long until the issue is fixed |
| **FCR** | First Call Resolution — percentage of tickets resolved on first contact |
| **Urgency** | How time-sensitive the issue is |
| **Impact** | How broad the effect is on the organisation |
| **Priority** | Calculated from Urgency × Impact; drives SLA |
| **Major Incident** | A high-impact incident requiring special handling |

---

## What This Section Establishes

By grounding the use case in ITIL 4 and explicitly framing TechCare's situation as a missing Service Desk function — rather than a generic "ticketing" need — I set up everything that follows. The roles I identify next are ITIL-defined roles. The requirements I write are mapped to ITIL practices. The data model I design supports ITIL workflows. The acceptance criteria I define are testable against ITIL outcomes.

This is the difference between *"I built a ticketing app"* and *"I operationalised the ITIL Incident Management and Service Request Management practices for a regulated mid-to-large enterprise."* Both build the same general thing. Only one of them is consulting-grade work.

That's the standard I aimed for.

---

➡️ **Next:** [Section 2 — Business Scenario](../02-Business-Scenario)
