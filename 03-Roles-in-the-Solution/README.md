# Section 3 — Roles in the Solution

## Setting the Vocabulary Straight

Before I identified roles, I had to clarify three terms that often get conflated but mean very different things in a properly designed enterprise system. Getting this distinction right early saved me from confusion later.

**Role** — A *function* a person performs **inside the system**. Roles answer: *"Who logs in, what do they do, and what can they see?"* Roles map to security permissions and to the screens, fields, and actions the system exposes to them.

**Persona** — A *type of user* with characteristic needs, behaviours, and constraints. Personas help us design *experiences*. The same person may match multiple personas depending on context.

**Job Title** — What appears on someone's business card. Job titles are organisational; roles are systemic. The same job title might fulfil multiple roles, and the same role might be filled by multiple job titles.

For my design, I focused on **Roles** — the systemic functions — because that's what drives the security model, the app design, and the data access patterns. I noted where roles correspond to ITIL-defined functions, because aligning to ITIL is the whole point of a production-grade design.

---

## The ITIL Foundation for Roles

ITIL 4 defines a small number of generic roles within the **Service Desk practice** and **Incident Management practice**. The most relevant for my scope are:

- **Service Desk Analyst** — the frontline; receives, logs, classifies, and attempts first-resolution of all tickets
- **Incident Manager** — owns the incident lifecycle, ensures process compliance, manages escalations
- **Specialist Technical Support** — Level 2/3 resolvers with deeper expertise in specific domains
- **Service Owner** — accountable for the end-to-end quality of an IT service
- **Process Owner** — accountable for the design and effectiveness of a process (e.g., the Incident Management process itself)

In a mid-to-large enterprise like TechCare, these ITIL roles map onto practical operational roles that real people fill. I needed to translate the framework into the working roles the system must support.

---

## The Six Core Roles I Identified for TechCare

After analysing TechCare's scenario, ITIL alignment, and operational needs, I concluded the system must support **six core roles**. For each, I documented the same five things: who they are, what ITIL function they fulfil, what they do in the system, what they cannot do, and why this distinction matters.

---

### Role 1 — End User (Requester)

**Who they are:** Any TechCare employee who has an IT-related issue or request.

**ITIL alignment:** *Customer / User of the IT service.*

**What they do in the system:**
- Submit new tickets (incidents and service requests)
- View status and history of tickets they have personally submitted
- Add comments or upload supporting attachments to their own tickets
- Confirm resolution or reopen a ticket if the issue persists
- Receive automated notifications about their tickets
- Access a self-service portal with FAQs and standard request forms

**What they cannot do:**
- View other users' tickets
- Self-assign priority (they propose Urgency and Impact; the system calculates Priority)
- Edit ticket fields after submission (they can only add comments)
- Access categories, SLA configuration, or any administrative functions
- See internal agent notes — only customer-facing communication

**Why this matters:** This is by far the largest user population — 500 to 2,000 people. Their experience must be *frictionless*. Heavy permissions or complex screens for end users will tank adoption and drive people back to email. The end-user experience is also the visible face of the IT service — a poor experience here damages trust in IT broadly.

---

### Role 2 — Service Desk Analyst (Level 1 Support)

**Who they are:** Frontline IT staff who receive every incoming ticket, perform initial triage and classification, and resolve the majority of routine issues without escalation.

**ITIL alignment:** *Service Desk Analyst — the operational heart of the Service Desk practice.*

**What they do in the system:**
- View tickets in their assigned queue
- Triage incoming tickets — verify categorisation, urgency, impact
- Apply known fixes from the knowledge base (password resets, account unlocks, basic troubleshooting)
- Add internal notes, customer-facing comments, and resolution notes
- Update ticket status as work progresses
- Escalate to Level 2 when an issue exceeds their capability or scope
- Close tickets they have resolved
- Communicate with end users via the system's notification channels

**What they cannot do:**
- Reassign tickets across teams (only escalate up the support tier)
- Modify SLA timers or override priority calculations
- Delete tickets or comments
- Access system administration functions
- View tickets outside their assigned categories or queues unless explicitly granted

**Why this matters:** Level 1 is where **First Call Resolution (FCR)** happens — the single biggest driver of both efficiency and user satisfaction in any service desk. The system must make L1 work *fast*: minimal clicks to log a ticket, instant access to knowledge articles, clear visual queues, simple status updates. If L1 is slow, the whole operation is slow.

---

### Role 3 — Specialist Technician (Level 2 / Level 3 Support)

**Who they are:** Senior IT staff with domain expertise — network engineers, application specialists, security analysts, infrastructure administrators. They handle escalations from Level 1 that require deeper technical knowledge or elevated system access.

**ITIL alignment:** *Specialist Technical Support — also known as Tier 2/3 in operational vocabulary.*

**What they do in the system:**
- View tickets escalated to their specialist queue
- Take ownership of escalated tickets
- Add detailed technical investigation notes
- Update ticket status and resolution
- Document **Root Cause** for resolved incidents (mandatory for high and critical priority tickets — feeds future Problem Management)
- Liaise with vendors or external parties where required (logged in the system)
- Return tickets to Level 1 with guidance, or close them when resolved
- Contribute to the knowledge base based on novel resolutions (Phase 2 capability)

**What they cannot do:**
- Modify SLA configurations or priority rules
- Reassign tickets to other specialist teams without manager approval
- Access end-user-only screens
- Delete records

**Why this matters:** Specialists are expensive and finite. The system must protect their time by ensuring tickets reach them only when L1 has genuinely exhausted its options — and by giving them the technical context they need (full ticket history, related tickets, attached diagnostics) to resolve quickly without re-interviewing the user.

---

### Role 4 — IT Support Manager / Incident Manager

**Who they are:** The supervisor of the Service Desk and incident operations. Combines two ITIL functions in TechCare's Phase 1 scope: **operational team management** and **Incident Management practice ownership**.

**ITIL alignment:** *Incident Manager + Service Desk Manager — typically merged in mid-sized organisations until scale justifies separation.*

**What they do in the system:**
- View **all** tickets across the entire organisation, regardless of category, requester, or assignee
- Reassign tickets between agents and across teams
- Override priority where operationally justified (with audit trail capturing who, when, why)
- Manage queue distribution and balance workloads
- Monitor SLA compliance in real time and intervene before breaches
- Declare and manage **Major Incidents** (high-impact incidents requiring coordinated response)
- Approve sensitive operations (e.g., access escalations, ticket deletions in extreme cases)
- Generate operational reports and dashboards
- Act as the escalation point for end users and other stakeholders
- Conduct post-incident reviews for major incidents

**What they cannot do:**
- Modify the underlying SLA framework or category structure (that's System Administrator)
- Bypass the audit trail (no one can — but managers especially must be visibly accountable)

**Why this matters:** This role holds the operation together. The manager is the only role with full visibility, so the system's reporting and dashboarding capabilities are largely designed for this audience. They are also the human safety valve when automation produces incorrect outcomes — the manual override capability matters, but every override must leave a trace.

---

### Role 5 — IT Service Owner / Senior Leadership Viewer

**Who they are:** Senior IT leadership — the IT Director, CIO, Head of IT Operations — who need strategic visibility into IT service performance but do not perform operational ticket work themselves.

**ITIL alignment:** *Service Owner — accountable for the IT service to the business, but not operationally hands-on.*

**What they do in the system:**
- Access executive dashboards showing aggregate metrics — ticket volumes, MTTA, MTTR, SLA compliance, top categories, trend lines
- Drill down from aggregate metrics to individual tickets when investigating specific issues
- Review monthly and quarterly performance reports
- Approve major changes to SLA framework, category structure, or service catalog (governance role)

**What they cannot do:**
- Edit any ticket data
- Reassign or modify tickets
- Access detailed ticket content unless explicitly granted (privacy and audit considerations)
- Configure system settings

**Why this matters:** Senior leadership rarely uses the system day-to-day, but their occasional use must produce *insight, not noise*. They need clean dashboards, not raw queues. A poorly designed executive view erodes leadership confidence in IT — and IT leaders need their executives' confidence to secure budget, headcount, and strategic priority.

---

### Role 6 — System Administrator

**Who they are:** The technical custodian of the platform itself. In Phase 1, this is likely a Power Platform developer or IT systems administrator — possibly the same person who builds the solution.

**ITIL alignment:** *Process Owner combined with Tooling Administrator — they own the configuration of the system that operationalises the Incident Management process.*

**What they do in the system:**
- Configure and maintain reference data — categories, sub-categories, SLA policies, choice values
- Manage user accounts and security role assignments
- Configure business rules, workflows, and automation
- Monitor system health, performance, and audit logs
- Implement enhancements, patches, and configuration changes
- Manage integrations with email, Teams, and other systems
- Administer environments (dev, test, production)

**What they cannot do:**
- View or interact with ticket *content* unless explicitly granted (separation of duties — administrators should not casually browse user data, especially in a regulated environment)
- Resolve tickets in the operational sense — they configure the system; they don't run it

**Why this matters:** This role is often underestimated in early-stage implementations. Without a designated System Administrator, configuration changes get made ad-hoc, the system drifts from its intended design, and audit trails get muddied. Even in Phase 1, this role must be *named* and *staffed*.

---

## A Critical ITIL Design Decision: Separation of Duties

I want to highlight something important in my design: the **System Administrator does not get default access to ticket data**. This is **separation of duties** — a fundamental control in regulated environments. The person who *configures* the system must not also have unrestricted access to *operational data*, because doing so would create both a privacy concern and an audit weakness.

In small organisations this principle is often violated for practical reasons. In a mid-to-large regulated enterprise, it should be enforced. I designed for the proper pattern, knowing TechCare may grant supplementary access where genuinely needed — with that access documented and audited.

This was a deliberate design choice, not an oversight.

---

## Roles I Deliberately Left Out of Phase 1

A senior consultant always documents what they *aren't* including, and why. The following ITIL-recognised roles exist in mature ITSM operations but I explicitly left them out of TechCare's Phase 1:

| Role | ITIL Function | Why I Deferred It |
|---|---|---|
| **Problem Manager** | Problem Management practice | Requires a stable Incident dataset to identify recurring problems. Phase 2 candidate. |
| **Change Manager** | Change Enablement practice | Requires Change Advisory Board (CAB), change calendar, RFC workflow. Phase 2/3. |
| **Knowledge Manager** | Knowledge Management practice | Requires KB authoring workflow, review process, retirement process. Phase 2. |
| **Configuration Manager** | Service Configuration Management | Requires CMDB. Phase 3+. |
| **Approver** | Service Request approvals | Required only when introducing service catalog with controlled requests. Phase 2. |

Documenting these explicitly tells reviewers: I considered the full ITIL role set and made deliberate scoping decisions, not omissions of ignorance.

---

## Roles Summary Matrix

| # | Role | ITIL Function | Primary System Activity | Rough Population |
|---|---|---|---|---|
| 1 | End User | User of the service | Submits and tracks own tickets | 500–2,000 |
| 2 | Service Desk Analyst | Service Desk Analyst (L1) | Triage, classification, first-call resolution | 4–8 |
| 3 | Specialist Technician | Specialist Support (L2/L3) | Investigate and resolve escalated tickets | 4–10 |
| 4 | IT Support Manager | Incident Manager + Service Desk Manager | Oversight, assignment, escalation, reporting | 1–2 |
| 5 | IT Service Owner | Service Owner | Strategic visibility and governance | 1–3 |
| 6 | System Administrator | Process Owner + Tooling Admin | Configuration and platform maintenance | 1–2 |

---

## How These Roles Translate into Security

When this design moves to implementation, each of these six roles becomes a **Dataverse Security Role** with carefully scoped permissions following the **Principle of Least Privilege** — each role gets exactly the permissions needed to fulfil its function, nothing more.

The detailed permission model is part of the requirements section, but the design intent is clear:

- End users: read/write their own records only
- Service Desk Analysts: read/write within their assigned queue
- Specialists: read/write within their specialist queue, read access to predecessor history
- Manager: read/write across the entire operation
- Service Owner: read-only with reporting access
- System Administrator: configuration access; ticket data access only by exception

---

## Why This Section Matters

A clear role model is the foundation for everything that follows in my design:

- **Stakeholder analysis** in Section 4 distinguishes roles (system users) from stakeholders (parties with interest)
- **Requirements** in Section 5 are written from the perspective of these roles ("the system shall allow Service Desk Analysts to...")
- **Acceptance criteria** in Section 6 verify behaviour from each role's vantage point
- **Data model** in Section 7 reflects role-based ownership and access patterns

Get the roles right, and the rest follows naturally. Get them wrong — by lumping disparate users together, by skipping ITIL alignment, by missing the System Administrator — and the whole design wobbles.

That's why I took the time to do this properly.

---

➡️ **Next:** [Section 4 — Key Stakeholders](../04-Key-Stakeholders)
⬅️ **Previous:** [Section 2 — Business Scenario](../02-Business-Scenario)
