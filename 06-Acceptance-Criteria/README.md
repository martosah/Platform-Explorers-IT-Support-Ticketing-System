# Section 6 — Acceptance Criteria

## What Acceptance Criteria Are

Acceptance criteria are the specific, testable conditions that must be met for a requirement to be considered successfully delivered. They turn requirements into provable outcomes.

A requirement says *"the system shall do X."* Acceptance criteria say *"here is exactly how we will know it's actually doing X correctly."*

Without acceptance criteria, "done" is an opinion. With them, "done" is a fact.

I produced acceptance criteria for **every one of the 64 requirements** in Section 5 — no sampling, no abbreviations. The result is **199 distinct acceptance criteria** that collectively form the test specification for the system.

---

## Two Formats — Used Together

I used two industry-standard formats throughout this section:

**Scenario-Based (Given-When-Then / Gherkin syntax)** — for behaviour, workflows, and user actions:

> *Given* a precondition,
> *When* an action is performed,
> *Then* an expected outcome occurs.

**Rule-Based (Checklist)** — for data rules, constraints, validations, and configuration:

> A list of testable conditions that must all be true.

Most requirements use one or the other. Some use both. The choice depends on what the requirement is asserting.

---

## SMART Criteria

Every acceptance criterion below is:

- **Specific** — no vague language
- **Measurable** — verifiable by direct observation or test
- **Achievable** — realistic given the design scope
- **Relevant** — directly supports the parent requirement
- **Time-bound** — where timing matters, it's explicit

---

## Functional Acceptance Criteria

---

### Group A — Ticket Intake and Capture

---

**AC for FR-01 — Multi-Channel Ticket Submission**

**AC-01.1 (Web Portal Submission)**
> *Given* an authenticated end user is on the self-service portal,
> *When* they click "Submit New Ticket" and complete all mandatory fields,
> *Then* the system shall create a ticket with a unique reference number and display a confirmation screen showing that reference.

**AC-01.2 (Email-to-Ticket Conversion)**
> *Given* an email is sent to the configured IT support mailbox,
> *When* the system polls the mailbox,
> *Then* it shall create a ticket within 5 minutes, with the email subject as the Title, the email body as the Description, the sender mapped to the Requester, and any attachments preserved on the ticket.

**AC-01.3 (Manual Entry by Agent)**
> *Given* a Service Desk Analyst is logged into the operational console,
> *When* they select "Log Ticket on Behalf of User," select the user, and complete the form,
> *Then* the system shall create a ticket with the selected user as the Requester, the agent recorded as the creator, and the source recorded as "Phone" or "Walk-up" as selected.

**AC-01.4 (Source Tracking)**
> Every ticket record shall carry a Source field with one of: Web Portal, Email, Phone, Walk-up, Chatbot (Phase 2). The Source shall be set automatically based on intake method and shall not be user-editable after creation.

---

**AC for FR-02 — Mandatory Ticket Classification at Submission**

**AC-02.1 (Mandatory Field Enforcement — Web Portal)**
- Requester is mandatory and pre-populated from the authenticated user; cannot be blank.
- Ticket Type (Incident or Service Request) is mandatory; default not pre-selected.
- Title is mandatory; minimum 5 characters; maximum 200 characters.
- Description is mandatory; minimum 20 characters; maximum 4,000 characters.
- Category is mandatory; selection from configured Level 1 list.
- Sub-Category becomes mandatory if the selected Category has sub-categories.
- Third-Level Category becomes mandatory if the selected Sub-Category has third-level options.
- Urgency is mandatory; default not pre-selected; selection from {Critical, High, Medium, Low}.
- Impact is mandatory; default not pre-selected; selection from {Whole Organisation, Entire Department/Affiliate, Whole Branch/Office, Only Me}.

**AC-02.2 (Validation Feedback)**
> *Given* a user attempts to submit a ticket with one or more mandatory fields empty,
> *When* they click "Submit,"
> *Then* the system shall prevent submission, highlight every empty mandatory field, and display a validation message next to each, in plain language.

**AC-02.3 (Mandatory Field Enforcement — Email Channel)**
> Where a ticket is created via email, the system shall attempt to derive Category and Type from email subject prefixes (configurable) or assign defaults ("Type: Incident", "Category: Uncategorised"). The ticket shall be flagged for triage by a Service Desk Analyst before SLA timers begin.

---

**AC for FR-03 — Automated Ticket Reference Generation**

**AC-03.1 (Format)**
- Incident reference format: `INC-YYYY-NNNNNN` (e.g., `INC-2026-000142`)
- Service Request format: `SR-YYYY-NNNNNN` (e.g., `SR-2026-000087`)
- The numeric portion is sequential within each calendar year and resets at the start of a new year.

**AC-03.2 (Uniqueness)**
- No two tickets shall ever share the same reference, across the entire history of the system.
- The reference is generated at the moment of ticket creation; ticket creation cannot complete without a reference.

**AC-03.3 (Immutability)**
- Once generated, the reference cannot be edited or deleted by any role, including System Administrators.
- The reference appears in all displays, notifications, exports, and audit records.

---

**AC for FR-04 — Submission Timestamp Capture**

**AC-04.1 (Capture)**
- The Created On timestamp is recorded automatically at the moment the ticket record is committed to storage.
- The timestamp uses UTC internally and is displayed in the viewing user's local time zone.
- The timestamp is precise to the second.

**AC-04.2 (Immutability)**
- No user, regardless of role, shall be able to modify the Created On timestamp through the UI, API, or any administrative function.
- Any attempt to modify the timestamp shall be rejected and logged as a security event.

---

**AC for FR-05 — Attachment Support**

**AC-05.1 (Permitted File Types)**
> The system shall accept the following file types: PNG, JPG, JPEG, GIF, PDF, DOCX, XLSX, PPTX, TXT, LOG, CSV, ZIP, MSG, EML.

**AC-05.2 (Blocked File Types)**
> The system shall reject the following potentially executable file types: EXE, BAT, CMD, COM, SCR, VBS, JS, PS1, JAR, MSI, DLL.

**AC-05.3 (File Size Limit)**
- Individual files larger than 25 MB shall be rejected at upload with a clear error message.
- A single ticket shall accept up to 10 attachments at the time of submission.
- Additional attachments may be added through comments after submission, subject to the same per-file limit.

**AC-05.4 (Attachment Visibility)**
> Attachments shall inherit the visibility of the comment they are attached to: attachments on Customer Updates are visible to the requester; attachments on Internal Notes are not.

---

**AC for FR-06 — Branch / Location Capture**

**AC-06.1 (Default from Profile)**
> *Given* an end user has a Branch/Location set in their profile,
> *When* they submit a new ticket,
> *Then* the Branch/Location field shall be pre-populated with their profile value but remain editable.

**AC-06.2 (Mandatory Field)**
- Branch/Location is mandatory.
- Selection is from the configured list of organisational locations.
- A free-text "Other" option may be selected, which prompts for a brief description.

**AC-06.3 (Reporting Dimension)**
> All operational and strategic reports shall be filterable and groupable by Branch/Location.

---

**AC for FR-07 — Affected Service Capture**

**AC-07.1 (Optional Field, Configurable List)**
- Affected Service is optional at submission and may be added or edited by agents during ticket handling.
- Selection is from the configured Service Catalog list.
- The list is maintainable by the System Administrator.

**AC-07.2 (Reporting Dimension)**
> Strategic reports shall be filterable and groupable by Affected Service.

---

### Group B — Categorisation and Prioritisation

---

**AC for FR-08 — Hierarchical Multi-Level Categorisation**

**AC-08.1 (Three Levels)**
- The system shall support three hierarchy levels: Level 1 (Category), Level 2 (Sub-Category), Level 3 (Third-Level Category).
- A category at any level may have zero or more children at the level below.
- Selection at one level shall filter the available options at the next level.

**AC-08.2 (Cascading Selection — UI Behaviour)**
> *Given* a user is on a ticket form,
> *When* they select a Level 1 Category,
> *Then* the Sub-Category dropdown shall populate with only the children of that Category, and the Third-Level Category dropdown shall remain disabled until a Sub-Category is selected.

**AC-08.3 (Mandatory at Lowest Available Level)**
> If a selected Category has Sub-Categories, the user must select one. If the selected Sub-Category has Third-Level Categories, the user must select one. The system enforces selection at the lowest available level of the chosen branch.

**AC-08.4 (Display in Ticket Header)**
> The full category path (Level 1 → Level 2 → Level 3) shall be displayed on the ticket header in operational views.

---

**AC for FR-09 — Configurable Category Library**

**AC-09.1 (Add)**
> A System Administrator can create a new category at any level by specifying its name, description, parent (where applicable), default priority (optional), and active status. The new category becomes available immediately.

**AC-09.2 (Edit)**
> A System Administrator can edit a category's name, description, default priority, and active status. Edits are versioned in the configuration audit log.

**AC-09.3 (Deactivate)**
> A System Administrator can deactivate a category. Deactivated categories do not appear in selection dropdowns for new tickets but remain associated with historical tickets and remain visible in historical reports.

**AC-09.4 (Restrict Delete)**
> A category cannot be hard-deleted if any ticket references it. The system shall return a clear error and instruct the administrator to deactivate instead.

**AC-09.5 (Reordering)**
> A System Administrator can re-order categories within a level via drag-and-drop or numeric position field. The display order is reflected immediately in user-facing dropdowns.

---

**AC for FR-10 — Urgency Selection**

**AC-10.1 (Controlled List)**
> The Urgency field accepts only one of four values: Critical, High, Medium, Low. No free-text entry is permitted.

**AC-10.2 (Definitions Displayed)**
> The selection control shall display the following definitions visibly to the user (via tooltip or inline help):
- **Critical:** Service is completely unusable; significant business impact requiring immediate attention.
- **High:** Service is severely degraded; user cannot perform key tasks; needs prompt attention.
- **Medium:** Service is functional but with workarounds or minor disruption.
- **Low:** Issue causes inconvenience but not work stoppage; can be resolved within standard timeframes.

**AC-10.3 (No Default Selection)**
> The Urgency field shall not have a pre-selected value; the user must consciously choose.

---

**AC for FR-11 — Impact Selection**

**AC-11.1 (Controlled List)**
> The Impact field accepts only one of four values: Whole Organisation, Entire Department/Affiliate, Whole Branch/Office, Only Me. No free-text entry is permitted.

**AC-11.2 (Definitions Displayed)**
- **Whole Organisation:** All employees across all locations are affected.
- **Entire Department/Affiliate:** All members of a major department or affiliate entity are affected.
- **Whole Branch/Office:** All staff at a single branch or office location are affected.
- **Only Me:** Only the requester is affected.

**AC-11.3 (No Default Selection)**
> The Impact field shall not have a pre-selected value.

---

**AC for FR-12 — Calculated Priority via ITIL Matrix**

**AC-12.1 (Calculation)**
> *Given* a ticket has a selected Urgency and Impact,
> *When* the ticket is created or these fields are updated,
> *Then* the Priority field shall be set automatically using the ITIL Priority Matrix specified in FR-12, with no user-visible Priority dropdown for direct selection.

**AC-12.2 (Display)**
> The calculated Priority shall be displayed prominently on the ticket form, accompanied by a hover or info icon explaining how it was derived from Urgency × Impact.

**AC-12.3 (Recalculation on Change)**
> If Urgency or Impact is edited after submission, the Priority shall be recalculated automatically, the change recorded in the audit trail, and the SLA timers re-evaluated.

**AC-12.4 (Matrix Verification)**
> The matrix shall produce, for every (Urgency, Impact) combination, exactly the Priority value specified in FR-12. Test cases shall verify all 16 cells of the matrix.

---

**AC for FR-13 — Manager Priority Override with Audit**

**AC-13.1 (Permission)**
> *Given* a user is in the IT Support Manager role,
> *When* they view a ticket,
> *Then* they shall see an "Override Priority" action that is not visible to any other role.

**AC-13.2 (Override Process)**
> *Given* the manager invokes the override,
> *When* they select a new Priority,
> *Then* the system shall require a free-text Reason for Override (minimum 20 characters) before saving.

**AC-13.3 (Audit Capture)**
- The audit trail shall record: the original calculated Priority, the new Priority, the timestamp, the manager's identity, and the reason for override.
- The override and its reason shall be visible to the IT Support Manager, Compliance Officer, and Internal Auditor roles on the ticket history.

**AC-13.4 (No Silent Override)**
> The system shall not allow Priority override to be performed via API or direct database access without recording the same audit information.

---

### Group C — Assignment, Routing, and Escalation

---

**AC for FR-14 — Initial Assignment**

**AC-14.1 (Routing Rules)**
> *Given* configurable routing rules exist that map (Category, Sub-Category, Third-Level Category) combinations to specific support queues,
> *When* a new ticket is created with a category selection,
> *Then* the ticket shall be assigned to the matching queue automatically.

**AC-14.2 (Default Queue Fallback)**
> If no routing rule matches, the ticket shall route to the configured default Level 1 queue.

**AC-14.3 (Speed)**
> Initial routing shall complete within 30 seconds of ticket creation under normal load.

---

**AC for FR-15 — Manual Assignment by Manager**

**AC-15.1 (Single Assignment)**
> *Given* an IT Support Manager is viewing a ticket,
> *When* they select an assignee from the team picker,
> *Then* the ticket shall be assigned to that person, the previous assignee (if any) removed, and the change captured in audit.

**AC-15.2 (Bulk Assignment)**
> *Given* an IT Support Manager has selected multiple tickets in a queue view,
> *When* they invoke "Assign Selected,"
> *Then* the system shall allow assignment of all selected tickets to a chosen agent in a single action, with audit entries created for each ticket.

**AC-15.3 (Cross-Queue Reassignment)**
> The manager can reassign a ticket to any queue or any individual agent across the entire support organisation, regardless of the ticket's original routing.

---

**AC for FR-16 — Self-Assignment by Agents**

**AC-16.1 (Claim Action)**
> *Given* a Service Desk Analyst is viewing an unassigned ticket in a queue they have access to,
> *When* they click "Claim,"
> *Then* the ticket shall be assigned to them and their name shall appear as the assignee.

**AC-16.2 (No Stealing of Assigned Tickets)**
> The Claim action shall be available only on unassigned tickets. An agent cannot claim a ticket already assigned to another agent; only the manager can reassign in that case.

---

**AC for FR-17 — Tier Escalation (L1 → L2 → L3)**

**AC-17.1 (Escalation Action)**
> *Given* a Service Desk Analyst is viewing a ticket assigned to them,
> *When* they invoke "Escalate to Level 2,"
> *Then* the system shall present a list of Level 2 queues, require selection of a target queue, and require a free-text escalation reason.

**AC-17.2 (Escalation Audit)**
> The audit trail shall record: the originating tier, the target tier, the escalating agent, the timestamp, and the reason.

**AC-17.3 (Status Behaviour)**
> Upon escalation, the ticket's assignee shall be cleared, the queue updated, and the status shall remain "In Progress" so that SLA timers continue to run.

**AC-17.4 (L2 to L3 Escalation)**
> The same escalation pattern shall be available from Level 2 to Level 3.

**AC-17.5 (No Silent De-Escalation)**
> A higher-tier agent may return a ticket to a lower tier, but this action is treated as a Reassignment (with reason), not a Resolution. The audit trail captures the de-escalation explicitly.

---

**AC for FR-18 — Hierarchical Escalation on SLA Breach**

**AC-18.1 (Manager Notification on Breach)**
> *Given* a ticket's Resolution SLA Due Date has passed without resolution,
> *When* the SLA timer detects breach,
> *Then* the system shall send a notification to the IT Support Manager within 5 minutes, including the ticket reference, requester, priority, and time elapsed since breach.

**AC-18.2 (Escalation to Service Owner)**
> If a ticket remains unresolved at 200% of its Resolution SLA target (configurable), the system shall send an additional notification to the IT Service Owner.

**AC-18.3 (No Duplicate Notifications)**
> The system shall not send repeated breach notifications for the same ticket. One notification at first breach, one at the 200% threshold, no further automatic alerts unless re-armed by manager action.

---

**AC for FR-19 — Restriction of Visibility by Assignment**

**AC-19.1 (Agent Visibility)**
- Service Desk Analysts can see tickets in their assigned L1 queue and tickets directly assigned to them.
- Specialist Technicians can see tickets in their assigned L2/L3 queue and tickets directly assigned to them.
- Neither role can see tickets in other queues or assigned to other agents in different teams.

**AC-19.2 (Manager Visibility)**
> The IT Support Manager can see every ticket in the system, regardless of queue or assignment.

**AC-19.3 (Cross-Queue Search)**
> An agent searching for a ticket reference they do not have access to shall receive a "No matching ticket found or you do not have access" message — the system shall not reveal whether the ticket exists.

---

### Group D — Ticket Lifecycle and Status Management

---

**AC for FR-20 — Defined Status Lifecycle**

**AC-20.1 (Defined Statuses)**
> The system shall maintain only the following statuses, in this order: New, Assigned, In Progress, On Hold, Resolved, Closed, Reopened.
> The On Hold status shall have a mandatory sub-status: Awaiting User, Awaiting Vendor, Awaiting Parts, or Awaiting Approval.

**AC-20.2 (Default Status on Creation)**
> A newly created ticket shall have the status "New" if not yet assigned, transitioning automatically to "Assigned" when an assignee is set.

**AC-20.3 (Sub-Status Required for On Hold)**
> A ticket cannot be set to On Hold without selecting a sub-status. The sub-status drives SLA pause behaviour.

---

**AC for FR-21 — Status Transition Rules**

**AC-21.1 (Permitted Transitions)**
The system shall permit only the following status transitions:
- New → Assigned (automatic when assignee set)
- Assigned → In Progress
- In Progress → On Hold (with sub-status)
- On Hold → In Progress
- In Progress → Resolved
- Resolved → Closed (manual or auto via FR-25)
- Closed → Reopened (within reopen window per FR-26)
- Reopened → In Progress (automatic)

**AC-21.2 (Blocked Transitions)**
> Any other transition shall be rejected by the UI and the API, with a clear error message. Examples of blocked transitions: New → Resolved, Assigned → Closed, Closed → New.

**AC-21.3 (Audit on Block)**
> Attempted invalid transitions via API shall be logged as security events for review.

---

**AC for FR-22 — Status Change Logging**

**AC-22.1 (Capture)**
For every status change, the system shall record:
- Previous status (and previous sub-status if applicable)
- New status (and new sub-status if applicable)
- Timestamp (UTC, second-precision)
- Actor (user ID and name)
- Source (UI / Automation / API)

**AC-22.2 (Immutability)**
> Status change log entries cannot be edited or deleted by any user, including System Administrators.

**AC-22.3 (Visibility)**
> The full status history of a ticket shall be visible to the IT Support Manager, Specialist Technicians, Service Desk Analyst (for tickets in their queue), Compliance Officer, and Internal Auditor.

---

**AC for FR-23 — Resolution Notes Mandatory**

**AC-23.1 (Mandatory Field)**
> *Given* an agent attempts to set a ticket's status to Resolved,
> *When* they save the change,
> *Then* the system shall require a Resolution Notes field of at least 30 characters before allowing the transition.

**AC-23.2 (Visibility)**
> Resolution Notes are visible to the requester in the Customer Update view of the ticket.

**AC-23.3 (Cannot Be Blanked Later)**
> Once Resolution Notes are set, they cannot be deleted, only superseded by additional notes (which append, not overwrite).

---

**AC for FR-24 — Root Cause Capture for Priority 1 and Priority 2**

**AC-24.1 (Mandatory for P1 / P2)**
> *Given* a ticket has Priority P1 (Critical) or P2 (High),
> *When* an agent attempts to set the status to Resolved,
> *Then* the system shall require a Root Cause field (minimum 50 characters) in addition to Resolution Notes.

**AC-24.2 (Optional for P3 / P4)**
> For tickets with Priority P3 or P4, Root Cause is presented as an optional field.

**AC-24.3 (Categorical Root Cause Selection)**
> In addition to free-text Root Cause, the agent shall select a Root Cause Category from a configurable list (e.g., Hardware Failure, Software Defect, Configuration Error, User Error, Process Gap, Third-Party Issue, Pending Investigation).

**AC-24.4 (Reporting Dimension)**
> Root Cause Category shall appear as a reporting dimension in strategic reports, supporting future Problem Management (Phase 2).

---

**AC for FR-25 — Auto-Closure of Resolved Tickets**

**AC-25.1 (Configurable Window)**
> The auto-closure window shall be configurable by the System Administrator (default: 3 business days).

**AC-25.2 (Pre-Closure Warning)**
> 24 hours before auto-closure, the system shall send a notification to the requester stating the ticket will close automatically and inviting them to reopen if the issue persists.

**AC-25.3 (Auto-Closure Process)**
> *Given* a ticket has been in Resolved status for the configured window without requester action,
> *When* the auto-closure scheduled job runs,
> *Then* the system shall set the status to Closed, set the Closed Date to the current timestamp, and record "Auto-closed by system" in the audit trail.

**AC-25.4 (Business Hours Awareness)**
> The auto-closure window shall be measured in business hours (using the support team's configured calendar), not calendar hours.

---

**AC for FR-26 — Ticket Reopen Capability**

**AC-26.1 (Configurable Reopen Window)**
> The reopen window shall be configurable by the System Administrator (default: 7 business days after Closed Date).

**AC-26.2 (Reopen Action — Requester)**
> *Given* a closed ticket within the reopen window,
> *When* the requester clicks "Reopen,"
> *Then* the system shall require a brief explanation (minimum 20 characters), set status to In Progress (Reopened), reactivate SLA Resolution timer with a fresh resolution target based on current Priority, and notify the previous assignee.

**AC-26.3 (Reopen Action — Agent)**
> A Service Desk Analyst, Specialist Technician, or IT Support Manager can also reopen a closed ticket within the window.

**AC-26.4 (Past Reopen Window)**
> Tickets older than the reopen window cannot be reopened. The user shall be advised to submit a new ticket linked to the previous one.

---

### Group E — Communication and Notifications

---

**AC for FR-27 — Requester Notifications on Lifecycle Events**

**AC-27.1 (Confirmation on Submission)**
> Within 1 minute of ticket creation, the requester shall receive an email confirmation containing: ticket reference, title, summary, priority, expected resolution time (per SLA), and a link to view the ticket.

**AC-27.2 (Notification on Assignment)**
> Within 5 minutes of an assignee being set, the requester shall receive a notification stating their ticket has been assigned and providing the assignee's name.

**AC-27.3 (Notification on Status Change to On Hold)**
> When status changes to On Hold (Awaiting User), the requester receives a notification specifying what is needed and how to respond.

**AC-27.4 (Notification on Resolution)**
> Upon transition to Resolved, the requester receives an email containing the Resolution Notes and instructions to reopen if the issue persists.

**AC-27.5 (Notification on Closure)**
> Upon transition to Closed (manual or automatic), the requester receives a closure confirmation.

**AC-27.6 (Notification on Customer Update Comments)**
> When an agent posts a comment of type Customer Update, the requester receives an email containing the comment text.

**AC-27.7 (No Notification for Internal Notes)**
> Internal Notes shall never trigger notifications to the requester.

---

**AC for FR-28 — Agent Notifications on Assignment**

**AC-28.1 (Email Notification)**
> Within 1 minute of being assigned to a ticket, the agent shall receive an email containing the ticket reference, requester, priority, SLA due time, and a direct link to the ticket.

**AC-28.2 (Teams Notification)**
> Where Microsoft Teams integration is configured for the agent, an additional Teams notification shall be sent within the same window.

**AC-28.3 (Reassignment)**
> When a ticket is reassigned away from an agent, that agent shall receive a notification confirming the reassignment; the new assignee shall receive the same notification as for an initial assignment.

---

**AC for FR-29 — Manager SLA Breach Alerts**

**AC-29.1 (75% Warning)**
> When a ticket reaches 75% of its Resolution SLA elapsed time without being resolved, the IT Support Manager shall receive an email and Teams notification listing the ticket reference, requester, assignee, priority, and time remaining.

**AC-29.2 (Breach Alert)**
> When a ticket exceeds its Resolution SLA without being resolved, the IT Support Manager shall receive an immediate breach notification.

**AC-29.3 (Daily Digest of Breaches)**
> At the start of each business day, the manager shall receive a digest summarising all currently SLA-breached tickets.

---

**AC for FR-30 — Major Incident Broadcast**

**AC-30.1 (Manager-Only Action)**
> The "Declare Major Incident" action is visible only to the IT Support Manager and IT Service Owner roles.

**AC-30.2 (Broadcast on Declaration)**
> *Given* a ticket is declared a Major Incident,
> *When* the declaration is saved,
> *Then* the system shall send a broadcast notification to the configured Major Incident distribution list (executive leadership, IT leadership, communications team, affected business units).

**AC-30.3 (Major Incident Flag and Reporting)**
> The Major Incident flag is preserved in the ticket history and is a reporting dimension. Major Incidents appear in a dedicated section of the strategic dashboard.

**AC-30.4 (Status Updates During Major Incident)**
> Status updates on a Major Incident automatically generate broadcast notifications to the same list, ensuring stakeholders remain informed.

---

**AC for FR-31 — Internal vs Customer-Facing Comments**

**AC-31.1 (Comment Type Selection)**
> When an agent adds a comment, they must select Comment Type from: Internal Note, Customer Update, Resolution. The default is Internal Note.

**AC-31.2 (Visibility Enforcement)**
- Internal Notes: visible to all agents and managers; NOT visible to the requester in any view; NOT included in any notification to the requester.
- Customer Updates: visible to agents, managers, and the requester; included in notifications to the requester.
- Resolution: a special type of Customer Update used at the moment of resolution.

**AC-31.3 (Type Cannot Be Changed Retroactively)**
> Once posted, a comment's type cannot be changed. To correct, the comment is superseded by a new one referencing the previous.

---

### Group F — Service Level Agreement Management

---

**AC for FR-32 — Configurable SLA Framework**

**AC-32.1 (SLA Records)**
> The system shall maintain SLA records, each with: SLA Name, Priority Level (P1/P2/P3/P4), Response Time (hours), Resolution Time (hours), Active flag, Effective From date, Effective To date (optional).

**AC-32.2 (Default SLA Targets)**
The Phase 1 default SLAs are:
- P1 — Response 30 min, Resolution 4 hours
- P2 — Response 2 hours, Resolution 8 hours
- P3 — Response 4 hours, Resolution 24 hours
- P4 — Response 8 hours, Resolution 72 hours

**AC-32.3 (Modification by Administrator)**
> The System Administrator can modify SLA records. Changes apply only to tickets created after the change. Existing tickets retain the SLA values that were in effect at their creation.

**AC-32.4 (Versioning)**
> SLA changes are versioned with effective dates so historical reports can correctly evaluate SLA compliance against the SLAs that were in effect at the time.

---

**AC for FR-33 — SLA Calculation at Ticket Creation**

**AC-33.1 (Response SLA Due Date)**
> At ticket creation, the system shall calculate Response SLA Due Date by adding the priority's Response Time to the Created On timestamp, advancing only during business hours of the assigned support team.

**AC-33.2 (Resolution SLA Due Date)**
> Similarly, Resolution SLA Due Date is calculated by adding the priority's Resolution Time to the Created On timestamp, advancing only during business hours.

**AC-33.3 (Display)**
> Both due dates are visibly displayed on the ticket in operational views, with colour coding: green (>50% time remaining), amber (25–50% remaining), red (<25% or breached).

**AC-33.4 (Recalculation on Priority Change)**
> If Priority changes after creation, both SLA due dates shall be recalculated based on time elapsed since creation, with the recalculation logged in audit.

---

**AC for FR-34 — SLA Pause for Customer-Side Wait**

**AC-34.1 (Pause Trigger)**
> *Given* a ticket's status changes to "On Hold — Awaiting User,"
> *When* the change is saved,
> *Then* the Resolution SLA timer shall pause from that timestamp.

**AC-34.2 (Resume Trigger)**
> *Given* an On Hold (Awaiting User) ticket has its status changed to In Progress,
> *When* the change is saved,
> *Then* the Resolution SLA timer shall resume, with the paused duration excluded from total elapsed time.

**AC-34.3 (Other Hold Sub-Statuses)**
> "On Hold — Awaiting Vendor" and "On Hold — Awaiting Parts" shall NOT pause the SLA by default (this is configurable per organisation). "On Hold — Awaiting Approval" shall pause the SLA.

**AC-34.4 (Pause History)**
> Each pause/resume is recorded with timestamps. Total paused time appears in the ticket's audit trail and in SLA reporting.

---

**AC for FR-35 — SLA Breach Flagging**

**AC-35.1 (Automatic Flagging)**
> When a ticket's Response or Resolution SLA Due Date passes without the corresponding event, the system shall set the appropriate Breach flag on the ticket within 5 minutes.

**AC-35.2 (Visual Indication)**
> Breached tickets appear in red in queue views and carry a visible "SLA Breached" indicator.

**AC-35.3 (Persistent Flag)**
> The Breach flag persists on the ticket history even after resolution; reports clearly identify which tickets met SLA and which breached.

---

**AC for FR-36 — SLA Compliance Reporting**

**AC-36.1 (Compliance Calculation)**
> SLA Compliance is calculated as: (Number of tickets resolved within SLA / Total tickets resolved) × 100, expressed as a percentage.

**AC-36.2 (Reporting Dimensions)**
> Reports shall break down compliance by: Team, Agent, Category, Priority, Branch/Location, Time Period (day, week, month, quarter, year).

**AC-36.3 (Historical Accuracy)**
> Reports use the SLA values that were in effect at each ticket's creation, not the current SLA values, ensuring historical accuracy.

---

### Group G — Reporting, Dashboards, and Analytics

---

**AC for FR-37 — Personal Workload Dashboard for Agents**

**AC-37.1 (Default Widgets)**
The agent's personal dashboard shall include:
- Active tickets assigned to me
- Tickets approaching SLA (within 25% of due time)
- Tickets I closed today / this week
- Personal MTTA, MTTR, FCR for the configurable period (default: this month)

**AC-37.2 (Drill-Down)**
> Clicking any widget shall navigate the agent to the underlying filtered ticket list.

**AC-37.3 (Refresh)**
> Dashboard data shall reflect the current state with a refresh latency no greater than 5 minutes.

---

**AC for FR-38 — Operational Dashboard for Manager**

**AC-38.1 (Default Widgets)**
The manager's operational dashboard shall include:
- Total open tickets across the operation
- Tickets by status (donut/pie)
- Tickets by priority (bar)
- Tickets by category (bar, top 10)
- SLA compliance rate (gauge)
- Tickets by agent (bar) — with workload balance indicator
- Tickets approaching/breaching SLA (count and list)
- Major Incidents currently active

**AC-38.2 (Drill-Down)**
> Every aggregate widget shall support drill-down to the underlying ticket list.

**AC-38.3 (Filter Persistence)**
> Filter selections (e.g., a chosen date range or queue) shall persist across the manager's session.

---

**AC for FR-39 — Strategic Dashboard for IT Service Owner / CIO**

**AC-39.1 (Default Widgets)**
The strategic dashboard shall include:
- Monthly ticket volume (12-month trend)
- MTTA trend (12-month)
- MTTR trend (12-month)
- SLA compliance trend (12-month)
- Top 10 categories by volume
- Top 10 affected services
- Root cause distribution for resolved P1/P2 incidents
- Major Incident frequency (12-month)

**AC-39.2 (Comparative Periods)**
> The dashboard shall support comparison with the previous equivalent period (e.g., month-on-month, year-on-year).

**AC-39.3 (Export)**
> The full dashboard shall export to PDF for executive reporting.

---

**AC for FR-40 — End User Self-Service View**

**AC-40.1 (My Tickets List)**
> The end user's portal shall display: their open tickets (top), their recently resolved tickets (last 30 days), their closed history (searchable).

**AC-40.2 (Per-Ticket Detail)**
> For each of their tickets, the user can view: reference, title, status, priority, SLA due time, last update, all Customer Updates and Resolution notes (no Internal Notes), and a comment-add interface.

**AC-40.3 (Notification History)**
> The user can see a chronological list of notifications they have received about their tickets.

---

**AC for FR-41 — Configurable Filtering and Saved Views**

**AC-41.1 (Filter Combinations)**
> Operational users can filter ticket lists by any combination of: Status, Priority, Category, Sub-Category, Branch, Assignee, Requester, Created Date Range, Resolved Date Range, SLA Status (Met/Breached/At Risk).

**AC-41.2 (Save View)**
> A filtered view can be saved with a user-supplied name and is then accessible from the user's saved views list.

**AC-41.3 (Sharing Saved Views)**
> Saved views are private to the user by default. Managers can mark a saved view as Shared, making it available to all members of a team.

---

**AC for FR-42 — Report Export**

**AC-42.1 (CSV and Excel Export)**
> Any list view or report shall support export to CSV and Excel (.xlsx).

**AC-42.2 (Export Captures Filters)**
> The exported file shall contain only the data matching the current filter set, in the same column order as displayed.

**AC-42.3 (Audit-Grade Exports)**
> A specific Audit Export option shall include the audit trail entries for each exported ticket, suitable for evidence submission to auditors.

---

### Group H — Audit, History, and Compliance

---

**AC for FR-43 — Complete Audit Trail**

**AC-43.1 (Captured Fields)**
For every ticket field change, the audit trail records:
- Field changed (technical name and display name)
- Previous value
- New value
- Change timestamp (UTC, second precision)
- User making the change (UserID and full name at time of change)
- Source: UI / Automation / API / System

**AC-43.2 (Coverage)**
> Audit entries are created for: ticket creation, every field update, every status change, every comment added, every attachment added or removed, every priority override, every escalation, every assignment change.

**AC-43.3 (No Silent Changes)**
> No mechanism in the system — UI, API, automation, or administrative tool — shall allow ticket data to change without producing an audit entry.

---

**AC for FR-44 — Audit Trail Visibility**

**AC-44.1 (View Permissions)**
> The full audit trail of any ticket is viewable by: IT Support Manager, IT Service Owner, Compliance Officer, Internal Auditor.

**AC-44.2 (Read-Only)**
> No user can edit or delete audit trail entries through any interface. Attempts to modify shall be rejected and logged as security events.

**AC-44.3 (System Administrator Restriction)**
> System Administrators do not have audit trail edit privileges, even with elevated database access. The audit log table shall be technically protected from administrator modification.

---

**AC for FR-45 — Configuration Change Logging**

**AC-45.1 (Captured Configuration Categories)**
The configuration audit log shall capture changes to: Categories (all 3 levels), SLA records, Routing rules, Security roles and permissions, Notification templates, System settings.

**AC-45.2 (Captured Fields)**
- Configuration object affected
- Specific change (Add / Edit / Deactivate / Delete)
- Previous value (for edits)
- New value
- Timestamp
- Administrator who made the change

**AC-45.3 (Visibility)**
> The configuration audit log is viewable by the IT Support Manager, Compliance Officer, and Internal Auditor.

---

**AC for FR-46 — Data Retention Enforcement**

**AC-46.1 (Retention Period)**
> Ticket records, comments, attachments, and audit trails shall be retained for a minimum of 7 years from ticket Closed Date (or longer if required by applicable regulation).

**AC-46.2 (Configurable per Regulation)**
> The retention period shall be configurable. Where regulation requires longer retention, the configured value shall override the default.

**AC-46.3 (End-of-Retention Process)**
> At end of retention, records shall be archived to a controlled long-term store (rather than deleted) and removed from the active system, unless an explicit retention extension is recorded (e.g., due to active litigation or regulatory hold).

**AC-46.4 (Pre-Disposition Review)**
> A monthly report identifies records reaching end of retention, allowing the Compliance Officer to apply holds before automatic disposition.

---

### Group I — User and Service Management

---

**AC for FR-47 — Integration with Organisational Identity**

**AC-47.1 (Single Sign-On)**
> Users shall authenticate using their existing organisational credentials (Microsoft Entra ID / Active Directory). The system shall not maintain separate passwords.

**AC-47.2 (Automatic Profile Provisioning)**
> A user logging in for the first time shall have a profile auto-created from directory attributes, requiring no manual administrator step.

**AC-47.3 (Conditional Access)**
> The system shall honour the organisation's conditional access policies (e.g., MFA enforcement, geo-restrictions) without separate configuration.

---

**AC for FR-48 — Role-Based Access Control**

**AC-48.1 (Six Defined Roles)**
> The system shall implement exactly the six roles defined in Section 3, as named security profiles in Dataverse.

**AC-48.2 (Permission Matrix Verification)**
> Each role's permissions shall be verifiable against a documented permission matrix for: each table (Create/Read/Update/Delete/Append/Append-To/Assign/Share) and each column (Read / Edit / Hidden).

**AC-48.3 (No Default Privileged Access)**
> No user shall be granted privileged role permissions automatically. Role assignment is a manual administrative action with audit logging.

---

**AC for FR-49 — User Provisioning and Deprovisioning**

**AC-49.1 (Daily Synchronisation)**
> The system shall synchronise user changes from the identity directory at least once every 24 hours; recommended every 4 hours for production.

**AC-49.2 (Immediate Deprovisioning on Departure)**
> When a user is marked as Disabled or Deleted in the identity directory, their access to the system shall be revoked within 1 hour, regardless of synchronisation schedule.

**AC-49.3 (Tickets of Departed Users)**
> When a requester departs, their open tickets remain visible to the assignee and manager but are flagged "Requester Departed." Closed tickets are preserved unchanged for audit retention.

**AC-49.4 (Tickets of Departed Agents)**
> When an agent departs, their assigned open tickets are reassigned to the manager queue automatically with a system-generated note, ensuring no orphaned tickets.

---

**AC for FR-50 — Out-of-Office Coverage**

**AC-50.1 (Setting Cover)**
> An agent can set an "Out of Office" period with a Cover Agent and a date range.

**AC-50.2 (Cover Agent Visibility)**
> During the OoO period, the Cover Agent gains read/write access to the absent agent's assigned tickets.

**AC-50.3 (Notification Routing)**
> Notifications that would have gone to the absent agent are also sent to the Cover Agent.

**AC-50.4 (Auto-Expiry)**
> Cover access expires automatically at the end of the OoO period; the absent agent's permissions on their own tickets are unaffected.

**AC-50.5 (Audit)**
> All cover assignments and the Cover Agent's actions on covered tickets are logged with the cover relationship preserved in audit.

---

## Non-Functional Acceptance Criteria

---

### AC for NFR-01 — Authentication and Authorisation

**AC-NFR-01.1** All access to the system requires successful authentication against the organisational identity provider; no anonymous access is permitted to any production data.

**AC-NFR-01.2** Multi-factor authentication is enforced for users in the IT Support Manager, IT Service Owner, and System Administrator roles, validated by review of identity provider conditional access policies.

**AC-NFR-01.3** Failed authentication attempts are logged. Accounts experiencing repeated failed authentication trigger lockout per organisational policy.

**AC-NFR-01.4** Session timeout enforces re-authentication after a configurable period of inactivity (default: 60 minutes), shorter for elevated-privilege roles.

---

### AC for NFR-02 — Data Protection in Transit and at Rest

**AC-NFR-02.1** All connections to the system, including between user devices and the platform and between platform services, use TLS 1.2 or higher. TLS 1.0 and 1.1 are explicitly disabled.

**AC-NFR-02.2** All stored data — including ticket content, attachments, audit trails, and configuration — is encrypted at rest using AES-256 or platform-equivalent encryption.

**AC-NFR-02.3** Encryption keys are managed through the organisation's key management service, with rotation policy aligned to information security standards.

**AC-NFR-02.4** Backups are encrypted with the same standards and stored in a manner consistent with the organisation's data residency requirements.

---

### AC for NFR-03 — Privacy and Data Protection

**AC-NFR-03.1** A documented Data Protection Impact Assessment is completed and approved before go-live.

**AC-NFR-03.2** The system supports Subject Access Requests by producing, on demand, a complete export of all data relating to a specific user (their tickets, their comments, their audit references) within 30 days of a documented request.

**AC-NFR-03.3** The system supports Right to Erasure within legal limits — personal data can be redacted from records where retention is no longer legally required, while preserving the record's existence for operational integrity.

**AC-NFR-03.4** Lawful basis for processing personal data is documented and visible in the system's privacy notice presented to users at first login.

**AC-NFR-03.5** Personal data appearing in ticket descriptions or attachments (e.g., names, account numbers) is not unnecessarily exposed to roles that do not require it; sensitive ticket categories may be marked Confidential, restricting visibility.

---

### AC for NFR-04 — Availability

**AC-NFR-04.1** During defined business hours (07:00–19:00, Monday–Friday, excluding public holidays), the system maintains 99.5% availability measured monthly.

**AC-NFR-04.2** Overall availability across all hours is 99.0% measured monthly.

**AC-NFR-04.3** Planned maintenance is scheduled outside business hours where possible, communicated at least 5 business days in advance, and limited to 4 hours per maintenance event.

**AC-NFR-04.4** Unplanned outages are reported to the IT Service Owner and Information Security team within 30 minutes of detection, with status updates every 30 minutes until restoration.

---

### AC for NFR-05 — Page Response Performance

**AC-NFR-05.1** Under normal load (100 concurrent users, 500,000 historical tickets), 95% of operational page loads complete within 3 seconds, measured at the user agent.

**AC-NFR-05.2** Under peak load (200 concurrent users), 95% of operational page loads complete within 5 seconds.

**AC-NFR-05.3** Performance is verified through scheduled performance tests and continuously monitored through real user monitoring.

---

### AC for NFR-06 — Search and Filter Performance

**AC-NFR-06.1** Searches and filter operations against the active ticket dataset return results within 5 seconds for 95% of queries under normal load.

**AC-NFR-06.2** Results are paginated for large result sets (default: 50 per page) to maintain responsiveness.

**AC-NFR-06.3** Full-text search across ticket descriptions and comments is available and returns results within the same performance envelope.

---

### AC for NFR-07 — Scalability

**AC-NFR-07.1** The system sustains a steady-state load of 1,000 tickets per week without performance degradation across the metrics in NFR-05 and NFR-06.

**AC-NFR-07.2** Load testing demonstrates the system can scale to 5,000 tickets per week (5x growth) without architectural redesign — capacity adjustments via configuration only.

**AC-NFR-07.3** Storage scaling supports 10 million historical ticket records over the 7-year retention period.

---

### AC for NFR-08 — Usability and Accessibility

**AC-NFR-08.1** The system is fully usable on supported desktop browsers (Chrome, Edge, Firefox, Safari — current and previous major versions) and on mobile browsers on iOS and Android.

**AC-NFR-08.2** End-user pages meet WCAG 2.1 Level AA accessibility standards, verified by automated and manual accessibility testing.

**AC-NFR-08.3** A user can complete the most common task — submitting a new ticket — in fewer than 90 seconds from landing on the portal, validated by usability testing.

**AC-NFR-08.4** The interface follows the organisation's brand guidelines and is consistent in terminology, layout, and behaviour across modules.

---

### AC for NFR-09 — Auditability

**AC-NFR-09.1** Every ticket has a complete audit trail per FR-43, retained for the full retention period per FR-46.

**AC-NFR-09.2** A complete audit report for any ticket can be produced in human-readable form (PDF) within 10 minutes of request, including all field changes, status changes, comments, and attachments.

**AC-NFR-09.3** Audit reports are admissible as evidence in regulatory or legal proceedings, meeting the documented chain-of-custody requirements of the organisation's compliance framework.

---

### AC for NFR-10 — Maintainability

**AC-NFR-10.1** A System Administrator can perform the following without developer involvement and without system downtime: add/edit/deactivate categories at all 3 levels, modify SLA records, modify routing rules, modify notification templates, manage user role assignments.

**AC-NFR-10.2** Developer involvement is required only for: schema changes (new tables, new columns), new automations, integration changes, security model changes.

**AC-NFR-10.3** Configuration changes take effect within 5 minutes of save, without requiring user re-login.

---

### AC for NFR-11 — Localisation and Time Zones

**AC-NFR-11.1** All timestamps are stored in UTC and displayed in the viewing user's local time zone, derived from their profile or browser locale.

**AC-NFR-11.2** Date and time formats follow the user's locale (e.g., DD/MM/YYYY for British English, MM/DD/YYYY for American English).

**AC-NFR-11.3** Multi-language interface is deferred to Phase 2; Phase 1 is English-only.

---

### AC for NFR-12 — Disaster Recovery

**AC-NFR-12.1** Recovery Time Objective (RTO) is 4 hours from declaration of disaster to restored operational service.

**AC-NFR-12.2** Recovery Point Objective (RPO) is 1 hour — the system can restore to a state no more than 1 hour before the failure point.

**AC-NFR-12.3** A documented disaster recovery plan exists, is reviewed annually, and is tested at least once per year via tabletop or partial failover exercise.

**AC-NFR-12.4** Backup and replication arrangements are verified to meet RTO and RPO; verification reports are retained for audit.

---

### AC for NFR-13 — Auditable Configuration

**AC-NFR-13.1** All configuration changes (per FR-45) are versioned with effective timestamps, the administrator who made the change, and the previous value.

**AC-NFR-13.2** A specific configuration change can be rolled back to a prior version through a controlled administrative action that itself is logged.

**AC-NFR-13.3** Configuration history is retained for the same period as ticket data — minimum 7 years.

---

### AC for NFR-14 — Vendor and Platform Independence

**AC-NFR-14.1** Ticket data and configuration can be exported in open, well-documented formats (CSV, JSON, XML) for migration purposes.

**AC-NFR-14.2** No element of the design relies on proprietary APIs that prevent reasonable migration to alternative platforms in future.

**AC-NFR-14.3** A documented data dictionary exists, mapping all fields to their business meaning, supporting future migration efforts.

---

## Acceptance Criteria Summary

| Section | Requirements | Acceptance Criteria |
|---|---|---|
| Group A — Intake | FR-01 to FR-07 | 25 |
| Group B — Categorisation & Prioritisation | FR-08 to FR-13 | 21 |
| Group C — Assignment & Escalation | FR-14 to FR-19 | 14 |
| Group D — Lifecycle Management | FR-20 to FR-26 | 22 |
| Group E — Communication | FR-27 to FR-31 | 16 |
| Group F — SLA Management | FR-32 to FR-36 | 16 |
| Group G — Reporting & Dashboards | FR-37 to FR-42 | 16 |
| Group H — Audit & Compliance | FR-43 to FR-46 | 14 |
| Group I — User & Service Management | FR-47 to FR-50 | 14 |
| Non-Functional | NFR-01 to NFR-14 | 41 |
| **TOTAL** | **64 Requirements** | **199 Acceptance Criteria** |

---

## Why This Section Matters

What this section produces is a **Phase 1 acceptance specification** for a regulated enterprise ITSM solution. Every requirement has its proof condition. Every condition is testable. Every test traces back to a real organisational need.

This level of specification is what:
- Enables a developer to build with no ambiguity
- Enables a tester to verify with confidence
- Enables a manager to sign off with evidence
- Enables an auditor to confirm controls without the consultant present

In a real engagement, this document would be the **acceptance baseline** — the contract by which delivery is judged. Walking through these criteria with the client is called **User Acceptance Testing (UAT)**, and a successful walkthrough is the milestone that moves the project from build to go-live.

I produced acceptance criteria for every requirement deliberately. Sampling would have left gaps; gaps in acceptance criteria are gaps in delivery confidence. The depth here is the point.

---

➡️ **Next:** [Section 7 — Entity Relationship Diagram](../07-Entity-Relationship-Diagram)
⬅️ **Previous:** [Section 5 — Business Requirements](../05-Business-Requirements)
