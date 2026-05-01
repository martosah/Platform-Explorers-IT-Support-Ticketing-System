# Section 2 — Business Scenario

## The Stated Scenario

The cohort brief tells us:

> *"TechCare Solutions receives 200+ IT support requests weekly via email and phone. No centralised system exists, leading to missed SLAs, duplicated work and frustrated staff."*

That sentence is short, but it carries a lot of weight. A junior approach would read this and immediately start designing a form. I chose the senior approach: I asked *what is actually broken, why is it broken, and what does ITIL maturity tell us about the path to fix it?*

This section is the result of that analysis.

---

## TechCare Solutions — Organisational Profile

Based on the brief and the operational pattern that emerges from a 200+ tickets-per-week volume, I established the following profile for TechCare Solutions:

- **Workforce:** 500–2,000 employees
- **Industry:** Regulated (financial services, healthcare, professional services, or similar)
- **Geographic footprint:** Multiple offices, branches, or sites — implied by the volume and variety of requests
- **IT function maturity:** Operational but informal — IT staff exist and do real work, but governance and discipline are absent
- **Compliance posture:** Subject to regulatory oversight (data protection, audit, possibly industry-specific frameworks like ISO 27001, SOX, NDPR, GDPR, HIPAA depending on jurisdiction and sector)

**Why this profile matters:** The size and regulated nature of the organisation disqualify simple solutions. A 50-person startup can run IT support out of a Slack channel. A 1,500-person regulated enterprise cannot. Auditors will eventually ask: *"Show me the audit trail for incident #4521 from March 2026 — when was it raised, who handled it, when was it resolved, and what was the root cause?"* If the answer is *"check Funmi's inbox,"* the organisation fails the audit.

---

## The Volume Reality

200+ tickets per week is a deceptively important data point. Here's how I translated it:

| Metric | Calculation | Implication |
|---|---|---|
| Tickets per week | 200+ | Sustained operational load |
| Tickets per business day | ~40+ | High intake rate |
| Tickets per business hour (10hr day) | ~4 | A new ticket every 15 minutes |
| Tickets per IT agent (assume 6 agents) | ~33/week per agent | Each agent juggles ~7 active tickets/day |
| Annual ticket volume | ~10,400 | Significant institutional memory at risk |

This is not a "small system" workload. At this volume, the **cost of every inefficiency compounds**. A 10% duplication rate is over 1,000 wasted resolutions per year. A 15% SLA breach rate is over 1,500 frustrated employees per year. A two-day resolution drift across the team is tens of thousands of staff-hours lost annually.

The numbers also tell me this organisation is past the point where **informal processes can scale**. They are operating a Service Desk function whether they admit it or not — they're just operating it badly.

---

## Decomposing the Pain Points — ITIL Service Value Chain Mapping

The brief identifies three pain points: missed SLAs, duplicated work, frustrated staff. I refused to accept these at face value — they are *symptoms*. I had to find the root failures. I mapped each pain point to the ITIL framework to understand it properly.

---

### Pain Point 1 — Missed SLAs

**Surface symptom:** Tickets are not resolved within agreed timeframes.

**Underlying ITIL failures I identified:**

- **No formal SLA framework.** TechCare likely has no documented commitments — only informal expectations. You cannot "miss" an SLA that doesn't exist on paper.
- **No prioritisation discipline.** Without an Urgency × Impact matrix, all tickets compete equally for attention. Critical incidents drown in routine requests.
- **No SLA timer.** Without a system to start a clock at ticket creation, SLAs cannot be measured even if they exist.
- **No proactive alerting.** Tickets approaching breach are not flagged. By the time anyone notices, the breach has already occurred.
- **No escalation path.** When an agent is overloaded, there is no automatic mechanism to surface the workload to a manager for redistribution.

**Business impact:** Erosion of trust between the IT function and the business. End users develop workarounds (calling individual agents directly, escalating to executives, shadow IT). Once users lose faith in the service desk, recovering that trust takes years.

---

### Pain Point 2 — Duplicated Work

**Surface symptom:** Multiple agents work on the same issue without coordination.

**Underlying ITIL failures I identified:**

- **No single source of truth.** Tickets live in personal inboxes. Agent A and Agent B both see the same issue come in via different channels (email + phone) and neither knows the other is working it.
- **No incident matching.** When a network outage affects 35 people on the 4th floor, 35 separate tickets get created. None are linked. 35 agents do the same diagnostic work.
- **No major incident handling.** Widespread issues that should trigger a single, coordinated response get treated as 35 individual problems.
- **No knowledge reuse.** A solution discovered for ticket #100 is not visible when ticket #245 — the same issue — is logged a week later.

**Business impact:** Direct labour cost — every duplicate ticket consumes agent time that could be spent on novel problems. Indirect frustration — agents feel busy but unproductive, which corrodes morale and retention.

---

### Pain Point 3 — Frustrated Staff

**Surface symptom:** IT agents and end users are unhappy.

I split this pain point into two distinct populations because they have very different complaints.

**End-user frustration drivers:**
- No visibility into ticket status — they don't know if anyone is working on their issue
- No expected resolution time — they cannot plan their day around the disruption
- They have to chase individual IT staff to get updates
- Same issue gets reported repeatedly because no one tracks resolutions
- Different IT staff give different answers to the same question

**IT agent frustration drivers:**
- Workload is unevenly distributed — some agents drown while others coast
- No clear ownership — every ticket feels like a hot potato
- Cannot prove their workload to management — invisible labour
- Constant interruptions break focus on complex tickets
- No way to capture or share institutional knowledge — every junior agent reinvents every solution

**Underlying ITIL failures:** Absence of the **Service Desk practice** itself, absence of **role definition**, absence of **knowledge management**, absence of **performance measurement**.

**Business impact:** This is the most expensive pain point because it manifests in *retention*. Frustrated agents leave. Their departure takes years of institutional knowledge with them. Frustrated end users escalate to executives, drawing senior leadership into operational issues that should never reach their desk.

---

## Hidden Pain Points the Brief Doesn't State

This is where I went beyond what was given to me. The brief mentions three pain points, but at this organisational scale and maturity level, these others are almost certainly also present. I designed for them even though they weren't in the brief.

### Hidden pain 1 — No regulatory audit readiness
A regulated enterprise without an auditable IT support trail is one regulator letter away from a major incident. I will design for this even though it isn't explicitly in the brief.

### Hidden pain 2 — No data for capacity planning
Without ticket volume data, IT leadership cannot justify hiring decisions, tooling investments, or process changes. They are flying blind on resource planning.

### Hidden pain 3 — No service catalog
End users don't know what they're entitled to ask for. Some employees never ask for software they need; others demand things that aren't supported. This creates inconsistency and political friction.

### Hidden pain 4 — Knowledge concentration risk
Critical institutional knowledge sits in 6 agents' heads. If the senior agent leaves, mean-time-to-resolve doubles overnight.

### Hidden pain 5 — No continuous improvement loop
Without ticket data, there is no way to identify recurring issues, build a problem record, and engineer the fix once. The same incidents keep happening.

I kept these hidden pains in mind throughout the rest of the design — they shaped several non-functional requirements and influenced the data model.

---

## The Four Pillars of My Solution

From this analysis, I distilled four foundational capabilities the solution must deliver. These became my **design pillars** — the lens through which I evaluated every subsequent requirement.

### Pillar 1 — Centralisation
One system, one source of truth. Every ticket lives in one place, regardless of intake channel.

### Pillar 2 — Standardisation
Consistent process, classification, prioritisation, and lifecycle for every ticket. Predictable handling regardless of who logs it or who works it.

### Pillar 3 — Visibility
Real-time and historical insight for every audience: agents see their queue, managers see their team, leadership sees the operation, auditors see the trail.

### Pillar 4 — Accountability
Clear ownership, traceable actions, measurable outcomes. Every ticket has an owner; every change has an actor; every commitment has a measurement.

I made these pillars an explicit test for every requirement: *"Does this requirement serve at least one pillar? If not, why is it in scope?"*

---

## What "Success" Looks Like

In ITIL terms, this Phase 1 implementation succeeds when TechCare can demonstrate, six months post-go-live:

- A documented, measurable SLA framework with reported compliance rates
- Mean Time To Acknowledge (MTTA) reduced by at least 50% versus baseline
- Mean Time To Resolve (MTTR) reduced by at least 30% versus baseline
- Zero "lost" tickets — every reported incident has a record and a resolution
- Audit-ready ticket history with full trail of every action
- A working categorisation framework producing useful trend data
- Reduced ticket duplication through visibility and major incident handling
- Improved end-user satisfaction (measurable via post-resolution surveys, recommended for Phase 2)
- An IT team that can articulate its workload and value with data, not anecdotes

These outcomes informed the success criteria I built into the acceptance criteria in Section 6.

---

## Connecting Pain to Practice — My Implementation Logic

To close out my analysis, here is the explicit logical chain that drives the rest of my design:

> **TechCare's pain → ITIL practices that address it → Capabilities the system must deliver → Requirements I documented → Acceptance criteria proving they work → Data model that stores them.**

This traceability is the consulting backbone. Anyone reviewing my work — my tutor, a hiring manager, a senior architect, a future auditor — should be able to ask *"why does this requirement exist?"* and find a clean answer that traces back to a real organisational pain rooted in a recognised framework.

That is the standard I held myself to.

---

➡️ **Next:** [Section 3 — Roles in the Solution](../03-Roles-in-the-Solution)
⬅️ **Previous:** [Section 1 — Use Case and Domain](../01-Use-Case-and-Domain)
