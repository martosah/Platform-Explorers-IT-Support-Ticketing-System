# Section 4 — Key Stakeholders

## Setting the Distinction Clearly

A common error in junior consulting work is treating *roles* and *stakeholders* as synonyms. They are not, and getting this wrong leads to predictable project failures.

**Roles** describe **who uses the system** — defined by the functions they perform inside it. I covered roles in Section 3.

**Stakeholders** describe **who has an interest in the project's success or outcome** — whether they touch the system or not.

The IT Support Manager is both a role *and* a stakeholder. The Chief Financial Officer is a stakeholder but probably never logs into the system. The internal auditor is a stakeholder who logs in only quarterly. A vendor providing third-party support may interact with tickets indirectly without being a system user at all.

I separated these because conflating them leads to two predictable failures: either I'd miss critical stakeholders (only thinking about who clicks buttons), or I'd over-engineer the system trying to give every stakeholder a login when they really just need a monthly report.

---

## The ITIL 4 View on Stakeholders

ITIL 4 treats stakeholder management as a **governance discipline**, not just a project activity. The framework distinguishes:

- **Customers** — those who define the requirements for a service and take responsibility for the outcomes
- **Users** — those who use the service operationally
- **Sponsors** — those who authorise budget and resources
- **Service Providers** — internal or external parties delivering elements of the service
- **Governing bodies** — those exercising oversight (audit, compliance, executive)

A mature stakeholder analysis maps **all** of these — not just the operational users.

---

## My Engagement Framework: The Power/Interest Grid

I classified each stakeholder using the **Power/Interest Grid** — a proven tool from project management practice that drives **engagement strategy**:

|  | **Low Interest** | **High Interest** |
|---|---|---|
| **High Power** | *Keep Satisfied* — periodic updates, prevent surprises | *Manage Closely* — deep engagement, decision authority |
| **Low Power** | *Monitor* — light touch, respond when raised | *Keep Informed* — regular communication, change management |

Each stakeholder gets a quadrant assignment, which tells me *how* to engage them — not just *that they exist*. This is the difference between knowing your stakeholders and managing them effectively.

---

## TechCare Stakeholder Inventory

I classified TechCare's stakeholders into three categories: **Internal Operational**, **Internal Governance**, and **External**. I'll cover each group in turn.

---

## Internal Operational Stakeholders

These are TechCare employees directly involved in or affected by IT service delivery operations.

---

### Stakeholder 1 — Chief Information Officer (CIO) / Head of IT

**Role in the project:** Project Sponsor and ultimate accountability for IT service delivery.

**What they care about:**
- Strategic alignment of the IT service with business objectives
- Return on investment for the platform
- Reduction in operational risk and audit exposure
- Improved IT reputation across the organisation
- Headline metrics: SLA compliance, ticket volume trends, incident reduction

**Their concerns and questions:**
- *"How does this position us relative to industry benchmarks?"*
- *"What's our exposure if we don't do this?"*
- *"Will this scale with company growth?"*
- *"How does this support our digital transformation roadmap?"*

**Power/Interest classification:** **High Power, High Interest — Manage Closely.**

**Engagement strategy:** Bi-weekly steering touchpoints during the project. Monthly executive dashboards once live. The CIO must feel personally invested in the project's success, because their authority unblocks every other dependency.

---

### Stakeholder 2 — IT Support Manager

**Role in the project:** Primary business owner of the operational solution. Also a system role (Role #4 from Section 3).

**What they care about:**
- Their team's day-to-day productivity and morale
- Their personal KPIs — SLA compliance, queue health, team retention
- The system's ability to make them look good (or expose them, if poorly designed)
- Workload visibility — being able to demonstrate effort to their leadership

**Their concerns and questions:**
- *"Will this make my team more productive or just add overhead?"*
- *"Can I see at a glance who's drowning and who's idle?"*
- *"Does this give me the data I need to defend my budget?"*

**Power/Interest classification:** **High Power, High Interest — Manage Closely.**

**Engagement strategy:** Weekly working sessions during design and build. They are the *de facto* product owner for the operational features. Their daily reality should drive most of the design decisions.

---

### Stakeholder 3 — Service Desk Analysts and Specialist Technicians (the IT Support Team)

**Role in the project:** End users of the operational system. Also system roles (#2 and #3 from Section 3).

**What they care about:**
- A system that helps them, not one that surveils them
- Speed of common operations (logging a ticket, updating status, finding a resolution)
- Fairness in workload distribution
- Recognition for difficult work (not just ticket counts)
- Knowledge capture so they don't keep solving the same problem

**Their concerns and questions:**
- *"Is this another tool that tracks me without helping me?"*
- *"How many extra clicks does this add to my day?"*
- *"Can I keep using my favourite shortcuts and tools?"*

**Power/Interest classification:** **Low Power individually, High Interest collectively — Keep Informed.**

**Engagement strategy:** This is the most underrated stakeholder group. Their *adoption* makes or breaks the project. Involve them early through user research sessions. Run pilot groups before full rollout. Solicit feedback continuously. Systems imposed on frontline teams without their voice get abandoned in months.

---

### Stakeholder 4 — End Users (TechCare Employees)

**Role in the project:** Submitters and beneficiaries of IT services. Also system role #1.

**What they care about:**
- Fast resolution of their issues
- Knowing where their ticket stands
- Not having to chase IT
- Easy submission — preferably from any device, with minimal friction

**Their concerns and questions:**
- *"Why do I have to fill in all these fields?"*
- *"Can I just send an email like before?"*
- *"How do I know someone's actually working on my issue?"*

**Power/Interest classification:** **Low Power individually, High Interest collectively — Keep Informed.**

**Engagement strategy:** Communicate the change broadly before go-live. Provide quick-reference guides and short training videos. Capture early feedback in the first weeks of go-live and iterate visibly. Their cumulative voice is loud — if 1,500 employees hate the new system, the project is in trouble regardless of how many SLAs improve.

---

### Stakeholder 5 — Department Heads / Business Unit Leaders

**Role in the project:** Indirect stakeholders whose teams' productivity is affected by IT support quality.

**What they care about:**
- Their team's uninterrupted ability to work
- Predictable IT support so they can plan
- Visibility into IT issues affecting their unit specifically
- A clear escalation path when something significant breaks

**Power/Interest classification:** **Medium Power, Medium Interest — Keep Informed (with quarterly engagement).**

**Engagement strategy:** Quarterly business reviews showing IT performance per business unit. Direct line to IT Support Manager for escalations affecting their teams. They are also useful allies — a department head who sees clear improvement becomes a vocal advocate for IT.

---

## Internal Governance Stakeholders

These are TechCare functions whose interest is *oversight* rather than operational use. Their needs are often regulatory or fiduciary, and missing them at design time is a common cause of late-stage project failure.

---

### Stakeholder 6 — Compliance Officer / Data Protection Officer

**Role in the project:** Ensures the solution meets regulatory and data-protection obligations.

**What they care about:**
- Audit trail completeness — every action traceable to a person and timestamp
- Data retention policy compliance — records held for the legally required period, no longer
- Access controls — appropriate separation of duties, principle of least privilege
- Personal data handling — what's captured, who sees it, how long it's kept, when it's purged
- Compliance with applicable frameworks: NDPR (Nigeria), GDPR (EU), HIPAA (US healthcare), SOX (US financial reporting), ISO 27001, PCI-DSS, or industry-specific equivalents

**Their concerns and questions:**
- *"Can I produce a complete history of any ticket on demand for an audit?"*
- *"What personal data does the system store, and is the legal basis documented?"*
- *"How do we handle data subject access requests through this system?"*
- *"What happens to the data when an employee leaves?"*

**Power/Interest classification:** **High Power, Medium Interest — Keep Satisfied.**

**Engagement strategy:** Formal compliance review before go-live. Sign-off on data classification, retention schedule, and access controls. **Their objection at go-live can stop the entire project** — engage them early, not at the end.

---

### Stakeholder 7 — Internal Auditor

**Role in the project:** Periodic verification that controls are designed and operating effectively.

**What they care about:**
- Demonstrable controls — documented, evidenced, tested
- Audit log integrity — logs cannot be tampered with by users (including administrators)
- Segregation of duties enforced through role-based access
- Traceable change history for both data and configuration

**Their concerns and questions:**
- *"Show me an audit trail for ticket #4521."*
- *"Who approved the SLA configuration change last March?"*
- *"How are privileged actions monitored?"*

**Power/Interest classification:** **High Power, Low Interest day-to-day — Keep Satisfied.**

**Engagement strategy:** Pre-implementation control review. Annual or quarterly access reviews post-go-live. Quick response when audit requests come in.

---

### Stakeholder 8 — Information Security Team

**Role in the project:** Ensures the solution doesn't introduce security vulnerabilities or compromise existing controls.

**What they care about:**
- Authentication and authorisation model
- Sensitive data handling (especially in ticket descriptions and attachments)
- Integration security — how the system connects to email, Teams, and other systems
- Incident response process for the system itself if compromised
- Regular security review and patching cadence

**Their concerns and questions:**
- *"How do users authenticate? Is MFA enforced?"*
- *"What happens if a user pastes a password into a ticket description?"*
- *"How are admin actions logged and reviewed?"*

**Power/Interest classification:** **High Power, Medium Interest — Keep Satisfied.**

**Engagement strategy:** Security architecture review before build. Penetration testing or security assessment before go-live. Inclusion in incident response plans.

---

### Stakeholder 9 — Finance Department

**Role in the project:** Approves and tracks costs.

**What they care about:**
- Total cost of ownership — licences, storage, ongoing support
- Cost predictability and scalability
- Justification of investment against measurable benefits
- Budget cycle alignment for any required spending

**Power/Interest classification:** **High Power, Low Interest — Keep Satisfied.**

**Engagement strategy:** Business case sign-off at project initiation. Quarterly review of platform costs versus projections.

---

### Stakeholder 10 — Human Resources

**Role in the project:** Indirect stakeholder; intersection points exist around onboarding, offboarding, and employee experience.

**What they care about:**
- Smooth IT setup for new hires
- Reliable IT access removal for leavers (a security and compliance concern)
- IT issues affecting employee satisfaction
- Their own ability to raise tickets for HR-specific systems

**Power/Interest classification:** **Medium Power, Medium Interest — Keep Informed.**

**Engagement strategy:** Process integration discussions during design. They are a useful collaborator on standardising onboarding/offboarding requests.

---

### Stakeholder 11 — Executive Leadership (CEO, COO, Other C-suite)

**Role in the project:** Strategic oversight; rarely involved in operational details.

**What they care about:**
- The IT function not embarrassing them in board meetings or to customers
- Operational efficiency and cost discipline
- Risk reduction
- Employee experience (a major retention factor)

**Power/Interest classification:** **High Power, Low Interest day-to-day — Keep Satisfied.**

**Engagement strategy:** Quarterly executive dashboard distributed via the CIO. Direct engagement only for major incidents or strategic decisions.

---

## External Stakeholders

These are parties outside TechCare who nevertheless have legitimate interest in or interaction with the IT support service.

---

### Stakeholder 12 — Microsoft (Platform Provider)

**Role in the project:** Vendor of the underlying technology platform.

**What they care about:** Continued use of their products; success stories they can reference.

**Power/Interest classification:** **Low Power, Low Interest — Monitor.**

**Engagement strategy:** Standard vendor relationship. Engage their support channels when issues arise. Stay informed on roadmap changes that could affect TechCare.

---

### Stakeholder 13 — Third-Party Vendors and Service Providers

**Role in the project:** External parties who deliver elements of TechCare's IT services — software vendors, hardware suppliers, network carriers, managed service providers.

**What they care about:**
- Clear, well-formed escalations from TechCare
- Defined responsibilities and handoff points
- Their own SLAs being measurable

**Their interaction with the system:** Typically indirect — TechCare staff log vendor cases on the vendor's portal, then attach the vendor reference to the TechCare ticket. In rare cases (large managed service contracts), vendors may be granted limited system access.

**Power/Interest classification:** **Low Power, Medium Interest — Monitor.**

**Engagement strategy:** Document handoff procedures. Maintain a vendor contact list within the system. Periodic vendor performance reviews.

---

### Stakeholder 14 — External Auditors

**Role in the project:** Periodic independent review of TechCare's controls — typically annual financial audit, plus any sector-specific examinations.

**What they care about:**
- The same things internal auditors care about, but with more skepticism and less context
- Evidence-based answers to control questions

**Power/Interest classification:** **High Power, Low Interest day-to-day — Keep Satisfied (when they're around).**

**Engagement strategy:** Pre-empt audit requests by maintaining clean audit trails and documented controls. The system should be able to produce evidence packages on demand.

---

### Stakeholder 15 — Regulators

**Role in the project:** Sector-specific oversight bodies (Central Bank, NDPC, equivalent regulators in other jurisdictions).

**What they care about:**
- Operational resilience
- Data protection compliance
- Incident reporting (some jurisdictions require notification of significant operational incidents)

**Power/Interest classification:** **Very High Power, Low Interest day-to-day — Keep Satisfied.**

**Engagement strategy:** No direct engagement at the project level. The system must be designed to support compliance with regulatory requirements relevant to TechCare's sector — which is why my requirements include retention, audit trail, and access control specifications that meet or exceed regulatory baselines.

---

## Stakeholder Map at a Glance

| # | Stakeholder | Category | Power/Interest | Engagement |
|---|---|---|---|---|
| 1 | CIO / Head of IT | Internal Operational | High / High | Manage Closely |
| 2 | IT Support Manager | Internal Operational | High / High | Manage Closely |
| 3 | Support Team (Analysts + Specialists) | Internal Operational | Low / High (collective) | Keep Informed |
| 4 | End Users | Internal Operational | Low / High (collective) | Keep Informed |
| 5 | Department Heads | Internal Operational | Medium / Medium | Keep Informed |
| 6 | Compliance Officer | Internal Governance | High / Medium | Keep Satisfied |
| 7 | Internal Auditor | Internal Governance | High / Low | Keep Satisfied |
| 8 | Information Security Team | Internal Governance | High / Medium | Keep Satisfied |
| 9 | Finance | Internal Governance | High / Low | Keep Satisfied |
| 10 | Human Resources | Internal Governance | Medium / Medium | Keep Informed |
| 11 | Executive Leadership | Internal Governance | High / Low | Keep Satisfied |
| 12 | Microsoft (Platform Provider) | External | Low / Low | Monitor |
| 13 | Third-Party Vendors | External | Low / Medium | Monitor |
| 14 | External Auditors | External | High / Low | Keep Satisfied |
| 15 | Regulators | External | Very High / Low | Keep Satisfied |

---

## Stakeholder Traceability

Every requirement I write in Section 5 traces back to **at least one stakeholder need**. Here is the mapping logic that drove that work:

| Stakeholder Need | Requirement Implication |
|---|---|
| CIO needs strategic dashboards | Reporting and analytics requirements |
| Support Manager needs queue oversight | Operational dashboard requirements |
| Support Team needs efficient tooling | Performance and usability requirements |
| End Users need self-service and visibility | Self-service portal requirements |
| Compliance Officer needs audit trail | Audit logging, retention, access control requirements |
| Information Security needs secure design | Authentication, authorisation, data handling requirements |
| Auditors need evidence on demand | Reporting export, immutable history requirements |
| Regulators set the baseline | Data protection, retention, incident handling requirements |

If a requirement could not be traced to a stakeholder, it had no business being in scope. This is how I kept the requirements grounded and defensible.

---

## Why This Section Matters

Stakeholder analysis is the most underrated step in enterprise project work. Junior teams skip it or do it superficially. I treated it as foundational because:

- **Missing a stakeholder at design time is expensive at delivery time.** A compliance objection three days before go-live can delay a project by months.
- **Misjudging power and interest leads to engagement failures.** Over-engaging low-power stakeholders wastes time; under-engaging high-power stakeholders creates project risk.
- **Stakeholder needs *generate* requirements.** I could not write good requirements without first knowing who they served.
- **Adoption depends on stakeholder buy-in.** A technically perfect system that the support team resents will fail. A modest system the team loves will succeed.

The stakeholder map above gave me the foundation. Section 5 builds on it — every requirement I wrote names the stakeholder it serves.

---

➡️ **Next:** [Section 5 — Business Requirements](../05-Business-Requirements)
⬅️ **Previous:** [Section 3 — Roles in the Solution](../03-Roles-in-the-Solution)
