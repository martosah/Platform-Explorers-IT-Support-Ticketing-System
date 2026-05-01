# Section 5 — Business Requirements

## What Business Requirement mean

A business requirement is a clear, specific, testable statement of what the system must do (or be) in order to solve a business problem or satisfy a stakeholder need. Requirements describe the **"what"** — never the **"how."** They tell the implementer what the business expects; they do not prescribe technology choices.

I held myself to five professional principles throughout this section:

**1. Use "shall" language.** *"The system shall do X."* Industry standard. *"Shall"* means mandatory. *"Should"* is a recommendation. *"May"* is optional.

**2. One requirement, one statement.** If I found myself writing "and" between two distinct behaviours, I split the requirement in two.

**3. Make it testable.** If I couldn't write an acceptance criterion to verify it, the requirement was too vague and I rewrote it.

**4. Trace to a stakeholder need.** Every requirement names the stakeholder it serves. If it didn't, I asked why it existed and removed it.

**5. Avoid implementation detail.** I did not write "the system shall use a Dataverse table called Tickets." I wrote "the system shall store ticket records." The technology comes later.

---

## Two Categories — Functional and Non-Functional

**Functional requirements** describe what the system *does* — features, behaviours, workflows, business rules. They answer: *"What capability does the system provide?"*

**Non-functional requirements** describe how the system *performs* — security, performance, usability, reliability, compliance, scalability. They answer: *"How well must the system work?"*

Both matter equally. A system that does the right things badly fails as surely as one that does the wrong things well.

---

## Structure of Each Requirement

For traceability, every requirement below carries:

- **ID** — unique identifier (FR-XX or NFR-XX)
- **Statement** — the requirement itself in "shall" form
- **Rationale** — why this requirement exists
- **Stakeholder** — who needs this (from Section 4)
- **ITIL alignment** — which ITIL practice or principle this supports

---

## Functional Requirements

I organised functional requirements into nine groups that reflect the ITIL Incident Management and Service Request Management lifecycles, plus supporting capabilities.

---

### Group A — Ticket Intake and Capture

**FR-01 — Multi-Channel Ticket Submission**
The system shall allow tickets to be submitted through multiple channels: a web-based self-service portal, email-to-ticket conversion, and manual entry by Service Desk Analysts on behalf of users contacting them by phone or in person.
*Rationale:* TechCare's pain point of fragmented intake channels. Centralises all demand into a single queue.
*Stakeholder:* End Users, Service Desk Analysts, IT Support Manager
*ITIL alignment:* Service Desk practice — single point of contact

**FR-02 — Mandatory Ticket Classification at Submission**
The system shall capture the following mandatory information at ticket submission: Requester identity, Ticket Type (Incident or Service Request), Title, Description, Category (with sub-category and third-level category as applicable), Urgency, and Impact.
*Rationale:* Proper classification at intake drives every downstream activity — routing, prioritisation, SLA assignment, reporting.
*Stakeholder:* Service Desk Analysts, IT Support Manager
*ITIL alignment:* Incident Management — proper logging and classification

**FR-03 — Automated Ticket Reference Generation**
The system shall automatically generate a unique, immutable ticket reference number for every submitted ticket, in the format `INC-YYYY-NNNNNN` for Incidents and `SR-YYYY-NNNNNN` for Service Requests.
*Rationale:* Distinct prefixes make ticket type immediately recognisable in communications and reports.
*Stakeholder:* All operational roles, Auditors
*ITIL alignment:* Incident Management — unique incident records

**FR-04 — Submission Timestamp Capture**
The system shall automatically capture the date and time of ticket submission, in the system's configured time zone, with no possibility of user override.
*Rationale:* Foundation for SLA timers and audit trail.
*Stakeholder:* IT Support Manager, Compliance Officer, Auditors
*ITIL alignment:* Incident Management — accurate chronology

**FR-05 — Attachment Support**
The system shall allow requesters and agents to attach supporting files (e.g., screenshots, log files, documents) to tickets, subject to file type and size restrictions defined in the security policy.
*Rationale:* Visual evidence accelerates diagnosis. Common files (Outlook errors, network screenshots) are far easier to share than describe.
*Stakeholder:* End Users, Service Desk Analysts, Specialist Technicians
*ITIL alignment:* Incident Management — efficient information capture

**FR-06 — Branch / Location Capture**
The system shall capture the requester's branch or location at ticket submission, automatically defaulted from the user's profile but editable where the user is reporting an issue from a different location.
*Rationale:* Multi-site enterprises route many tickets by location. Reporting also requires location dimensioning.
*Stakeholder:* Service Desk Analysts, IT Support Manager, Department Heads
*ITIL alignment:* Service Configuration — service location awareness

**FR-07 — Affected Service Capture (Optional Field)**
The system shall capture the IT service affected by the ticket where applicable, selected from a defined list of organisational services (e.g., Email, Core Banking, VPN, File Services).
*Rationale:* Service-level reporting is a Phase 1 capability that supports later Service Catalog work in Phase 2.
*Stakeholder:* IT Service Owner, IT Support Manager
*ITIL alignment:* Service Owner — service-level visibility

---

### Group B — Categorisation and Prioritisation

**FR-08 — Hierarchical Multi-Level Categorisation**
The system shall support a three-level hierarchical category structure (Category → Sub-Category → Third-Level Category), where each level is selectable from values configured by the System Administrator.
*Rationale:* Real enterprise classification requires depth. "Hardware → Printer → Cannot scan" is meaningfully different from "Hardware → Laptop → Won't boot" for routing and reporting.
*Stakeholder:* Service Desk Analysts, IT Support Manager, IT Service Owner
*ITIL alignment:* Incident Management — proper classification

**FR-09 — Configurable Category Library**
The system shall allow the System Administrator to add, edit, deactivate, and re-order categories at all three levels without requiring developer intervention.
*Rationale:* Categories evolve as the business evolves. The system must support this without code changes.
*Stakeholder:* System Administrator, IT Support Manager
*ITIL alignment:* Continual Improvement — adaptable taxonomy

**FR-10 — Urgency Selection**
The system shall require the selection of an Urgency value from a controlled list: Critical, High, Medium, Low. Each value shall carry a clear, displayed definition to guide consistent interpretation.
*Rationale:* Without a defined Urgency taxonomy, prioritisation is subjective and inconsistent.
*Stakeholder:* End Users, Service Desk Analysts
*ITIL alignment:* Incident Management — Urgency in priority calculation

**FR-11 — Impact Selection**
The system shall require the selection of an Impact value from a controlled list: Affects Whole Organisation, Affects Entire Department/Affiliate, Affects Whole Branch/Office, Affects Only Me. Each value shall carry a clear, displayed definition.
*Rationale:* Forces objective thinking about scope. Prevents the universal anti-pattern of every user marking their ticket Critical.
*Stakeholder:* End Users, Service Desk Analysts
*ITIL alignment:* Incident Management — Impact in priority calculation

**FR-12 — Calculated Priority via ITIL Matrix**
The system shall automatically calculate Priority from the combination of Urgency and Impact using the following ITIL-aligned matrix, and shall not permit users to set Priority directly:

| | Affects Only Me | Whole Branch/Office | Entire Department/Affiliate | Whole Organisation |
|---|---|---|---|---|
| **Low Urgency** | P4 — Low | P4 — Low | P3 — Medium | P3 — Medium |
| **Medium Urgency** | P4 — Low | P3 — Medium | P3 — Medium | P2 — High |
| **High Urgency** | P3 — Medium | P3 — Medium | P2 — High | P1 — Critical |
| **Critical Urgency** | P3 — Medium | P2 — High | P1 — Critical | P1 — Critical |

*Rationale:* Priority is a calculated outcome, not an opinion. This matrix is the international standard.
*Stakeholder:* IT Support Manager, IT Service Owner, all operational users
*ITIL alignment:* Incident Management — formal priority calculation

**FR-13 — Manager Priority Override with Audit**
The system shall allow the IT Support Manager to manually override the calculated Priority for operational reasons, capturing the reason for override and recording the override in the ticket's audit trail.
*Rationale:* The matrix is right 95% of the time. The 5% of cases requiring human judgment must be handled, with full traceability.
*Stakeholder:* IT Support Manager, Compliance Officer, Auditors
*ITIL alignment:* Incident Management — controlled exception handling

---

### Group C — Assignment, Routing, and Escalation

**FR-14 — Initial Assignment**
The system shall route every newly submitted ticket to the appropriate Level 1 queue based on its category, where the queue is determined by configurable routing rules maintained by the System Administrator.
*Rationale:* Tickets must reach a person quickly. Manual triage of every ticket by the manager doesn't scale.
*Stakeholder:* IT Support Manager, Service Desk Analysts
*ITIL alignment:* Incident Management — efficient assignment

**FR-15 — Manual Assignment by Manager**
The system shall allow the IT Support Manager to assign or reassign tickets to specific agents, individually or in bulk, across queues and tiers.
*Rationale:* Automated routing covers the common case; the manager handles exceptions, balances workload, and assigns based on agent skill or availability.
*Stakeholder:* IT Support Manager
*ITIL alignment:* Incident Management — manager oversight

**FR-16 — Self-Assignment by Agents**
The system shall allow Service Desk Analysts and Specialist Technicians to claim unassigned tickets from their queue, without manager intervention.
*Rationale:* Empowers agents to manage their own workload and prevents bottlenecks at the manager.
*Stakeholder:* Service Desk Analysts, Specialist Technicians
*ITIL alignment:* Empowerment principle

**FR-17 — Tier Escalation (L1 → L2 → L3)**
The system shall allow Service Desk Analysts to escalate a ticket from Level 1 to a designated Level 2 specialist queue, and shall similarly support escalation from Level 2 to Level 3, capturing the reason for escalation as a mandatory field.
*Rationale:* Escalation is an explicit, traceable action — not a quiet handoff.
*Stakeholder:* Service Desk Analysts, Specialist Technicians, IT Support Manager
*ITIL alignment:* Incident Management — formal escalation paths

**FR-18 — Hierarchical Escalation on SLA Breach**
The system shall automatically notify the IT Support Manager when a ticket breaches its SLA Response or Resolution time, and shall escalate to the IT Service Owner if the breach extends beyond a configurable threshold (e.g., 200% of SLA target).
*Rationale:* SLA breaches must surface to leadership before they become customer-visible disasters.
*Stakeholder:* IT Support Manager, IT Service Owner
*ITIL alignment:* Incident Management — hierarchical escalation

**FR-19 — Restriction of Visibility by Assignment**
The system shall ensure that Service Desk Analysts and Specialist Technicians can view only tickets in their assigned queues or tickets explicitly assigned to them, while preserving the IT Support Manager's full visibility across all queues.
*Rationale:* Operational focus and basic information hygiene.
*Stakeholder:* All operational roles, Compliance Officer, Information Security Team
*ITIL alignment:* Principle of least privilege

---

### Group D — Ticket Lifecycle and Status Management

**FR-20 — Defined Status Lifecycle**
The system shall enforce the following status lifecycle for tickets: New → Assigned → In Progress → On Hold (with sub-states: Awaiting User, Awaiting Vendor, Awaiting Parts, Awaiting Approval) → Resolved → Closed → Reopened (where applicable).
*Rationale:* A defined lifecycle is the backbone of process discipline. The On Hold sub-states are critical for accurate SLA pause logic.
*Stakeholder:* All operational roles
*ITIL alignment:* Incident Management — defined incident lifecycle

**FR-21 — Status Transition Rules**
The system shall enforce permitted status transitions (e.g., a ticket cannot move from New directly to Resolved without passing through Assigned and In Progress) and shall prevent invalid transitions through the user interface and any API.
*Rationale:* Process discipline cannot be optional. Allowing arbitrary status jumps corrupts the operational data.
*Stakeholder:* IT Support Manager, Compliance Officer, Auditors
*ITIL alignment:* Process discipline

**FR-22 — Status Change Logging**
The system shall record every status change with the timestamp, the actor, the previous status, and the new status, in an immutable audit trail.
*Rationale:* Forensic traceability — without this, the audit trail has gaps.
*Stakeholder:* Compliance Officer, Auditors, Information Security Team
*ITIL alignment:* Audit and continuous improvement

**FR-23 — Resolution Notes Mandatory**
The system shall require resolution notes (a description of how the ticket was resolved) to be entered before a ticket's status can change to Resolved.
*Rationale:* Resolution notes are the seed of future knowledge management. Empty resolutions waste future opportunity.
*Stakeholder:* Service Desk Analysts, Specialist Technicians, IT Support Manager
*ITIL alignment:* Knowledge Management — capture at point of resolution

**FR-24 — Root Cause Capture for Priority 1 and Priority 2 Incidents**
The system shall require Root Cause to be captured before resolution for tickets at Priority 1 (Critical) or Priority 2 (High), and shall make Root Cause optional for Priority 3 and Priority 4 tickets.
*Rationale:* High-impact incidents demand root cause analysis. Forcing it for every ticket creates documentation burden; forcing it for none undermines the Problem Management practice (Phase 2).
*Stakeholder:* Specialist Technicians, IT Support Manager, IT Service Owner
*ITIL alignment:* Problem Management — root cause as input to problem records

**FR-25 — Auto-Closure of Resolved Tickets**
The system shall automatically transition tickets from Resolved to Closed after a configurable period (default: 3 business days) if the requester does not respond, and shall notify the requester before this transition occurs.
*Rationale:* Tickets cannot stay Resolved forever waiting for confirmation. Auto-closure with warning is the industry-standard balance.
*Stakeholder:* Service Desk Analysts, IT Support Manager
*ITIL alignment:* Operational efficiency

**FR-26 — Ticket Reopen Capability**
The system shall allow a closed ticket to be reopened within a configurable period after closure (default: 7 business days), restoring the ticket to "In Progress" status and reactivating the SLA timer.
*Rationale:* Premature closure is common; the user must have recourse without creating a duplicate ticket.
*Stakeholder:* End Users, Service Desk Analysts
*ITIL alignment:* Customer focus

---

### Group E — Communication and Notifications

**FR-27 — Requester Notifications on Lifecycle Events**
The system shall send automated notifications to the requester at the following events: ticket submission confirmation, assignment to an agent, status change to On Hold, status change to Resolved, status change to Closed, and any agent-authored Customer Update comment.
*Rationale:* End user visibility is one of TechCare's stated pain points. Proactive notification eliminates "where's my ticket?" enquiries.
*Stakeholder:* End Users
*ITIL alignment:* Customer focus, communication

**FR-28 — Agent Notifications on Assignment**
The system shall send a notification to an agent within 1 minute of a ticket being assigned to them, via email and (where configured) Microsoft Teams.
*Rationale:* Speed of agent acknowledgement is the single biggest driver of MTTA. Agents must know immediately when work lands in their queue.
*Stakeholder:* Service Desk Analysts, Specialist Technicians
*ITIL alignment:* Incident Management — fast acknowledgement

**FR-29 — Manager SLA Breach Alerts**
The system shall notify the IT Support Manager when a ticket exceeds 75% of its SLA Resolution time and again immediately upon SLA breach.
*Rationale:* Two-stage alerts give the manager a chance to intervene before breach, not just react after.
*Stakeholder:* IT Support Manager
*ITIL alignment:* Proactive SLA management

**FR-30 — Major Incident Broadcast**
The system shall allow the IT Support Manager to flag a ticket as a Major Incident, triggering a broadcast notification to a configurable distribution list (executive leadership, IT leadership, affected business units).
*Rationale:* Major incidents (outages affecting many users) require coordinated communication outside normal ticket flow.
*Stakeholder:* IT Support Manager, IT Service Owner, Executive Leadership
*ITIL alignment:* Major Incident Management

**FR-31 — Internal vs Customer-Facing Comments**
The system shall distinguish between Internal Notes (visible only to agents and managers) and Customer Updates (visible to the requester), with the comment author selecting the type at the time of writing.
*Rationale:* Agents must be able to coordinate technically without inadvertently exposing internal discussion to the requester.
*Stakeholder:* Service Desk Analysts, Specialist Technicians, End Users
*ITIL alignment:* Information hygiene

---

### Group F — Service Level Agreement Management

**FR-32 — Configurable SLA Framework**
The system shall maintain a configurable SLA framework that defines Response Time and Resolution Time for each Priority level (P1 through P4), maintainable by the System Administrator.
*Rationale:* SLAs evolve with business agreements; the framework must be adjustable.
*Stakeholder:* IT Service Owner, IT Support Manager, System Administrator
*ITIL alignment:* Service Level Management

**FR-33 — SLA Calculation at Ticket Creation**
The system shall calculate and store the Response SLA Due Date and Resolution SLA Due Date at the moment a ticket is created, based on the calculated Priority and the configured business hours of the assigned support team.
*Rationale:* SLAs must be visible from the start, not back-calculated. Business hours awareness is essential.
*Stakeholder:* All operational roles
*ITIL alignment:* Service Level Management

**FR-34 — SLA Pause for Customer-Side Wait**
The system shall pause the Resolution SLA timer when a ticket's status is "On Hold — Awaiting User," and resume the timer when the status changes to an active state.
*Rationale:* Penalising IT for customer response delays is unfair and corrupts SLA metrics. ITIL standard practice.
*Stakeholder:* IT Support Manager, Service Desk Analysts
*ITIL alignment:* Service Level Management — fair measurement

**FR-35 — SLA Breach Flagging**
The system shall automatically flag a ticket as SLA-breached when its Response or Resolution SLA Due Date passes without the corresponding event occurring, and shall maintain this flag visibly on the ticket and in queues.
*Rationale:* Visibility is the precondition for management action.
*Stakeholder:* IT Support Manager, Service Desk Analysts
*ITIL alignment:* Service Level Management

**FR-36 — SLA Compliance Reporting**
The system shall provide reporting on SLA compliance, broken down by team, agent, category, priority, and time period.
*Rationale:* Without measurement, no improvement. Without breakdowns, the measurement is uninformative.
*Stakeholder:* IT Support Manager, IT Service Owner, CIO
*ITIL alignment:* Continual Improvement

---

### Group G — Reporting, Dashboards, and Analytics

**FR-37 — Personal Workload Dashboard for Agents**
The system shall provide each agent with a personal dashboard showing their currently assigned tickets, tickets approaching SLA, recently closed tickets, and personal performance metrics (MTTA, MTTR, FCR rate for the configurable period).
*Rationale:* Self-management is the foundation of good performance.
*Stakeholder:* Service Desk Analysts, Specialist Technicians
*ITIL alignment:* Continual Improvement at the individual level

**FR-38 — Operational Dashboard for Manager**
The system shall provide the IT Support Manager with an operational dashboard showing total open tickets, tickets by status, tickets by priority, SLA compliance rate, tickets by agent, tickets by category, and tickets approaching or breaching SLA.
*Rationale:* The manager needs real-time situational awareness.
*Stakeholder:* IT Support Manager
*ITIL alignment:* Service Desk practice — operational management

**FR-39 — Strategic Dashboard for IT Service Owner / CIO**
The system shall provide a strategic dashboard showing trends over time: monthly ticket volume, MTTA trend, MTTR trend, SLA compliance trend, top categories, top affected services, and root cause patterns from resolved Priority 1 and 2 tickets.
*Rationale:* Strategic decisions require trend data, not snapshots.
*Stakeholder:* IT Service Owner, CIO, Executive Leadership
*ITIL alignment:* Continual Improvement — strategic level

**FR-40 — End User Self-Service View**
The system shall provide each end user with a view of their own tickets — open, closed, and historical — with status, last update, and resolution notes where applicable.
*Rationale:* Self-service visibility eliminates "what's the status?" queries.
*Stakeholder:* End Users
*ITIL alignment:* Customer focus

**FR-41 — Configurable Filtering and Saved Views**
The system shall allow operational users to filter ticket lists by any combination of status, priority, category, location, agent, requester, and date range, and to save commonly used filter combinations as named views.
*Rationale:* Productivity tooling. Different agents focus on different slices of the workload.
*Stakeholder:* Service Desk Analysts, Specialist Technicians, IT Support Manager
*ITIL alignment:* Operational efficiency

**FR-42 — Report Export**
The system shall allow operational and strategic users to export reports and ticket lists to CSV and Excel formats for offline analysis and audit submission.
*Rationale:* External analysis and audit requests are routine.
*Stakeholder:* IT Support Manager, IT Service Owner, Auditors, Compliance Officer
*ITIL alignment:* Audit and reporting

---

### Group H — Audit, History, and Compliance

**FR-43 — Complete Audit Trail**
The system shall maintain an immutable, comprehensive audit trail of every change to every ticket, capturing: the field changed, previous value, new value, the user who made the change, the date and time of the change, and the source (UI, automation, API).
*Rationale:* Regulatory and audit requirement. Non-negotiable in a regulated environment.
*Stakeholder:* Compliance Officer, Internal Auditor, External Auditors, Regulators, Information Security Team
*ITIL alignment:* Audit and accountability

**FR-44 — Audit Trail Visibility**
The system shall make the audit trail viewable by the IT Support Manager, Compliance Officer, and Internal Auditor roles, and shall not allow any user — including System Administrators — to modify or delete audit records.
*Rationale:* Audit trails that can be edited by privileged users provide no audit value.
*Stakeholder:* Compliance Officer, Auditors, Information Security Team
*ITIL alignment:* Audit integrity

**FR-45 — Configuration Change Logging**
The system shall log all configuration changes — to categories, SLA policies, security roles, and routing rules — including who made the change, when, what was changed, and the previous value.
*Rationale:* Configuration changes can dramatically affect operational outcomes; their history must be auditable.
*Stakeholder:* System Administrator, Auditors, Compliance Officer
*ITIL alignment:* Change accountability

**FR-46 — Data Retention Enforcement**
The system shall retain ticket records and audit trails for a minimum of 7 years (or longer if required by applicable regulation), and shall provide a controlled archival or deletion process at end of retention.
*Rationale:* Regulatory minimum in most financial-services jurisdictions; defensible default elsewhere.
*Stakeholder:* Compliance Officer, Regulators, Auditors
*ITIL alignment:* Records management

---

### Group I — User and Service Management

**FR-47 — Integration with Organisational Identity**
The system shall use TechCare's organisational identity directory (e.g., Microsoft Entra ID / Active Directory) for user authentication and shall not maintain a separate user credential store.
*Rationale:* Single sign-on is both a security and usability requirement. Separate credentials are a known source of compromise.
*Stakeholder:* Information Security Team, End Users, System Administrator
*ITIL alignment:* Access Management

**FR-48 — Role-Based Access Control**
The system shall enforce role-based access control aligned to the six roles defined in Section 3, with each role granted only the permissions necessary to perform its function (Principle of Least Privilege).
*Rationale:* Foundation of security and audit.
*Stakeholder:* Information Security Team, Compliance Officer, all users
*ITIL alignment:* Information Security Management

**FR-49 — User Provisioning and Deprovisioning**
The system shall reflect changes in user status (new hire, role change, departure) within 24 hours of those changes being recorded in the organisational identity directory, and shall immediately revoke access for departed users.
*Rationale:* Stale access is a known security risk. Manual provisioning processes drift.
*Stakeholder:* Information Security Team, Human Resources, Compliance Officer
*ITIL alignment:* Access Management

**FR-50 — Out-of-Office Coverage**
The system shall allow agents to designate a covering colleague when out of office, with their assigned tickets visible to and actionable by the cover agent for the duration.
*Rationale:* Operational continuity. Tickets cannot wait for an agent's return from leave.
*Stakeholder:* Service Desk Analysts, Specialist Technicians, IT Support Manager
*ITIL alignment:* Continuity of service

---

## Non-Functional Requirements

Non-functional requirements describe *qualities* of the system. They are no less important than functional requirements — and are often the requirements that determine project success or failure.

---

**NFR-01 — Security: Authentication and Authorisation**
The system shall enforce multi-factor authentication for all administrative roles (IT Support Manager, System Administrator) and shall integrate with the organisation's central identity provider for all user authentication.
*Stakeholder:* Information Security Team, Compliance Officer

**NFR-02 — Security: Data Protection in Transit and at Rest**
The system shall encrypt all data in transit using TLS 1.2 or higher, and shall ensure all stored data is encrypted at rest using industry-standard encryption.
*Stakeholder:* Information Security Team, Compliance Officer, Regulators

**NFR-03 — Privacy and Data Protection**
The system shall comply with applicable data protection regulations (GDPR, NDPR, or equivalent), including support for data subject access requests, right to erasure within legal limits, and documented lawful basis for processing.
*Stakeholder:* Compliance Officer, Data Protection Officer, Regulators

**NFR-04 — Availability**
The system shall be available 99.5% of the time during defined business hours and 99.0% overall, with planned maintenance windows communicated in advance and scheduled outside business hours where possible.
*Stakeholder:* All operational roles, IT Service Owner

**NFR-05 — Performance: Page Response**
The system shall load any operational page within 3 seconds under normal load conditions (defined as up to 100 concurrent users and up to 500,000 historical ticket records).
*Stakeholder:* All operational roles

**NFR-06 — Performance: Search and Filter**
The system shall return search and filter results within 5 seconds for queries against the active ticket dataset.
*Stakeholder:* Service Desk Analysts, Specialist Technicians, IT Support Manager

**NFR-07 — Scalability**
The system shall accommodate at least 1,000 tickets per week without degradation, with the capacity to scale to 5,000 tickets per week as the organisation grows, without architectural redesign.
*Stakeholder:* IT Service Owner, System Administrator

**NFR-08 — Usability and Accessibility**
The system shall provide a user interface that is usable on desktop and mobile devices, accessible via standard web browsers, and shall meet WCAG 2.1 Level AA accessibility standards.
*Stakeholder:* End Users, Service Desk Analysts, Specialist Technicians

**NFR-09 — Auditability**
The system shall maintain auditable records for a minimum of 7 years and shall allow the production of complete audit reports for any ticket on demand.
*Stakeholder:* Compliance Officer, Auditors, Regulators

**NFR-10 — Maintainability**
The system shall allow the System Administrator to perform configuration changes — categories, SLAs, routing rules, security roles — without requiring developer intervention or system downtime.
*Stakeholder:* System Administrator, IT Support Manager

**NFR-11 — Localisation and Time Zones**
The system shall display all timestamps in the user's local time zone while storing all data in a single canonical time zone (UTC), and shall support multi-language interface strings as a Phase 2 capability if required.
*Stakeholder:* End Users (multi-location), Service Desk Analysts

**NFR-12 — Disaster Recovery**
The system shall meet a Recovery Time Objective (RTO) of 4 hours and a Recovery Point Objective (RPO) of 1 hour in the event of a major disruption.
*Stakeholder:* IT Service Owner, Information Security Team, Regulators

**NFR-13 — Auditable Configuration**
The system shall version-control all configuration changes (categories, SLAs, business rules, routing) and shall support rollback to a prior configuration.
*Stakeholder:* System Administrator, Auditors

**NFR-14 — Vendor and Platform Independence (where reasonable)**
The system shall, where commercially reasonable, store data in formats and structures that support future export and migration, avoiding deep dependencies that would prevent platform change.
*Stakeholder:* IT Service Owner, Finance, CIO

---

## Requirements Summary

| Category | Count |
|---|---|
| Functional Requirements | **50** |
| Non-Functional Requirements | **14** |
| **Total** | **64** |

---

## Why This Section Matters

This requirements set is what production-grade ITIL-aligned thinking produces. Every requirement here is:

- Written in proper "shall" language
- Traceable to a stakeholder need from Section 4
- Aligned to a specific ITIL practice or principle
- Testable through acceptance criteria (which I document in Section 6)
- Defensible in a stakeholder review

The requirements set is the **contract** between the design and the implementation. Everything that gets built must satisfy these requirements. Everything that doesn't get built is explicitly documented as out of scope.

This is how I made sure the design is not just complete, but defensible.

---

➡️ **Next:** [Section 6 — Acceptance Criteria](../06-Acceptance-Criteria)
⬅️ **Previous:** [Section 4 — Key Stakeholders](../04-Key-Stakeholders)
