# IT Support Ticketing System — TechCare Solutions

> A Phase 1 design and specification for an ITIL-aligned IT Support Ticketing System, produced as part of the **Platform Explorers Cohort 2 — Power Platform track (Week 2-3)**.

---

## 📌 Project Status

| | |
|---|---|
| **Status** | ✅ Complete — Phase 1 design and specification submitted |
| **Phase** | Phase 1 (Design and Specification) |
| **Build** | Phase 2 — to follow as a separate engagement |
| **Last Updated** | April 2026 |

This repository is the **Phase 1 deliverable**: a complete solution design including business analysis, requirements, acceptance criteria, and data model. The Phase 2 build (Power Platform implementation) is intentionally out of scope and would be delivered as a follow-on engagement once this design is reviewed and signed off.

---

## About This Project

This repository documents my end-to-end design work for an IT Support Ticketing System for a fictional mid-to-large regulated enterprise — **TechCare Solutions** — that currently handles 200+ IT support requests weekly through email and phone, with no centralised system in place.

Rather than treating this as a generic ticketing exercise, I approached it as a **real consulting engagement**. I grounded the design in **ITIL 4** — the global standard for IT Service Management — and treated TechCare as a regulated enterprise where audit, compliance, and operational discipline are non-negotiable. The result is a Phase 1 specification that reflects how production-grade ITSM solutions are actually designed in industry.

---

## The Problem

TechCare Solutions receives **200+ IT support requests weekly via email and phone**. There is no centralised system, which leads to:

- **Missed SLAs** — no formal commitments, no timer, no visibility
- **Duplicated work** — multiple agents unknowingly working the same issue
- **Frustrated staff** — both end users with no visibility, and IT staff with no tooling
- **No audit trail** — a serious gap for a regulated organisation
- **No data for improvement** — the IT function cannot measure or prove its value

Every one of these is a symptom of a missing **Service Desk function** in ITIL terms — the foundational practice that sits beneath any mature IT operation.

---

## My Approach

I worked through the project in **seven structured sections**, each one building on the previous. The sections are:

| # | Section | What It Contains |
|---|---|---|
| 1 | [Use Case and Domain](./01-Use-Case-and-Domain) | What an IT Support Ticketing System is, framed in ITIL 4 terms |
| 2 | [Business Scenario](./02-Business-Scenario) | Analysis of TechCare's pain points through an ITIL service value chain lens |
| 3 | [Roles in the Solution](./03-Roles-in-the-Solution) | The six core roles, each aligned to a recognised ITIL function |
| 4 | [Key Stakeholders](./04-Key-Stakeholders) | Internal, governance, and external stakeholders mapped on a Power/Interest grid |
| 5 | [Business Requirements](./05-Business-Requirements) | 50 functional and 14 non-functional requirements, fully traced to stakeholder needs |
| 6 | [Acceptance Criteria](./06-Acceptance-Criteria) | 199 acceptance criteria covering every requirement comprehensively |
| 7 | [Entity Relationship Diagram](./07-Entity-Relationship-Diagram) | The full data model — 6 custom tables, 2 built-in tables, 15 relationships |

Each section is documented in its own folder with a dedicated `README.md` file.

---

## Key Design Decisions I Made

A few decisions were significant enough that they shaped the whole design:

**ITIL 4 as the governing framework.** I chose to align the entire design to ITIL 4 — the international standard for IT Service Management — rather than treat this as a generic ticketing application. This influenced everything from role definitions to the priority calculation method to the data model.

**Urgency × Impact = Priority (calculated, not selected).** Industry practice doesn't let users choose Priority directly because everyone marks their ticket "Critical." Instead, the user selects Urgency and Impact independently, and the system calculates Priority from an ITIL-aligned matrix. This is a small change with a huge effect on data quality.

**Three-level category hierarchy.** A flat category list is enough for a small business but inadequate for an enterprise. I designed Category as a self-referencing table supporting Level 1 → Level 2 → Level 3, mirroring how production ITSM platforms classify tickets in practice.

**Root Cause capture for high-priority tickets.** For Priority 1 (Critical) and Priority 2 (High) incidents, the system requires Root Cause to be documented before resolution. This builds the dataset that future Problem Management (Phase 2) will rely on.

**Complete audit trail as a first-class requirement.** TechCare is a regulated organisation. The audit trail isn't an afterthought — it's a mandatory, immutable record of every change, with explicit protection against editing even by System Administrators (separation of duties).

**Phase 1 scope discipline.** I was deliberate about what's *out* of scope: formal Problem Management, Change Management, CMDB, Service Catalog, AI triage. These are real ITIL practices that belong in Phase 2 once the foundation is operational. Trying to do everything at once is how ITIL implementations fail.

---

## What I Learned Through This Project

Working through this exercise taught me that **good solution design happens before the build**. The temptation when given a "ticketing system" brief is to jump into Power Apps and start dragging controls onto a canvas. I deliberately resisted that. Every requirement traces to a stakeholder need. Every stakeholder need traces to an organisational pain. Every data structure traces to a requirement. By the time the build phase begins, every decision has already been made and justified.

I also learned the value of **triangulating against real-world references**. Mid-way through the project I cross-referenced the design against a production ITSM tool used in a banking environment, and I caught several gaps in my original thinking — most notably the absence of an Urgency/Impact split for Priority calculation. That cross-check elevated the design from textbook-acceptable to industry-grade.

Finally, I now have working fluency in ITIL 4 vocabulary — Incident vs. Service Request, MTTA, MTTR, FCR, Major Incident, Problem Record, Known Error, the Service Value Chain — which I expect to be useful for the rest of my career in this space.

---

## Repository Structure

```
Platform-Explorers-IT-Support-Ticketing-System/
│
├── README.md                                ← You are here
├── LICENSE                                  ← MIT License
│
├── 01-Use-Case-and-Domain/                  ← What and why
├── 02-Business-Scenario/                    ← TechCare's pain points
├── 03-Roles-in-the-Solution/                ← Six core ITIL-aligned roles
├── 04-Key-Stakeholders/                     ← Power/Interest stakeholder map
├── 05-Business-Requirements/                ← 50 FRs + 14 NFRs
├── 06-Acceptance-Criteria/                  ← 199 acceptance criteria
└── 07-Entity-Relationship-Diagram/          ← The data model
```

---

## Project Context

| | |
|---|---|
| **Programme** | Platform Explorers Cohort 2 |
| **Track** | Power Platform |
| **Use Case** | IT Support Ticket System (Week 2-3) |
| **Author** | [Martins Osahon Osimen](https://github.com/martosah) |
| **Phase** | Phase 1 — Design and Specification |
| **Framework** | ITIL 4 |

---

## Acknowledgements

Thanks to the **Platform Explorers** programme and to the cohort coaches — **Juan Ojochemi Idowu, Mathew Ede, Rachel Irabor, Sarah Anueyiagu, Church Ephraim, Thomas Okuya, and Adewale Yusuf** — for designing and running this learning experience. The cohort framing pushed me to treat this as a real consulting engagement rather than an academic exercise, which made the work substantially more meaningful.

---

## License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE) file for details.
