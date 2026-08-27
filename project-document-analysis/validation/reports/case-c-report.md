# Project Understanding Report — Calder Logistics, FleetCheck driver inspection system

Analyzer: project-document-analyzer.md, version 1.1

## 1. Sources and Review Coverage

| Source ID | File / title | Stated version / date | Reviewed content | Limitations |
| --- | --- | --- | --- | --- |
| D1 | case-c-doc1-requirements-summary.md — Requirements Summary, "FleetCheck" Driver Inspection System | Undated; circulated by email on 6 May 2026 | Full document | No version number; author not identified beyond the client organisation |
| D2 | case-c-doc2-technical-addendum.md — Technical Addendum, FleetCheck | Version 0.3, dated 19 May 2026, prepared by Calder IT | Full document | Version is stated as 0.3; whether that denotes draft, interim, or approved status is not stated. Status relative to D1 is not stated |

- Review coverage: Complete for supplied sources.
- Source precedence: Not established. D2 carries a later date and describes itself as an addendum, but neither document states that it supersedes the other, and no user instruction establishes precedence. D1 assigns functional sign-off to the Compliance Director and technical review to IT, which does not resolve the disagreements below, because some of them are functional requirements stated by IT.
- Referenced but unavailable material: None identified. Neither document references attachments, diagrams, or appendices.

## 2. Project at a Glance

Calder Logistics operates 260 vehicles across four depots. Drivers complete a daily walkaround inspection before leaving the depot, currently on carbon-copy paper books. FleetCheck is a new system to capture those inspections digitally, block vehicles with safety-critical defects, notify depot managers, and give the compliance team a searchable, exportable record for traffic commissioner audits. The two supplied documents agree on purpose, users, and core workflow, but contradict each other on four material points: whether the first delivery is a full production rollout or a single-depot MVP, whether hosting is AWS or the on-premises VMware cluster, whether offline capture is required, and whether inspection records are retained for 15 months or 7 years. No precedence between the documents is established.

| Item | Finding | Evidence / checklist reference |
| --- | --- | --- |
| Project type | New | D1, "Purpose": "There is no existing digital system for inspections." See Q3 |
| Classification basis | Both sources describe a first-time digital system replacing paper books; no existing system is being changed or replaced | D1, D2; Q3 |
| Business objective | Replace paper walkaround inspections and provide a searchable compliance record | D1, "Purpose"; Q1 |
| Target users | Approximately 300 drivers including agency drivers, depot managers, and the compliance team; internal users only | D1, "Users"; D2, "Platform notes"; Q2, Q22 |
| Current system status | Not applicable — paper carbon-copy books, no existing digital system | D1, "Purpose"; Q3, Q11 |
| First-release scope | Conflicting: full production across all four depots at go-live (D1) versus an MVP for the Warrington depot only, with remaining depots and compliance reporting in a second phase (D2) | D1, "Delivery expectation"; D2, "Delivery approach"; Q6, Q7, Q8; G02 |
| Current technology stack | Not applicable | Q14 |
| Target technology stack | Preferred, not mandated: .NET and SQL Server (D2). D1 states no stack | D2, "Platform notes"; Q27 |
| Hosting expectations | Conflicting: Amazon Web Services as a group IT standard (D1) versus the on-premises VMware cluster at Warrington, with public cloud not approved for operational data (D2) | D1, "Hosting"; D2, "Deployment target"; Q31, Q32; G03 |
| Timeline / milestones | Go-live required by 1 September 2026 (D1). No milestones stated. D2 states no dates, and its phased approach is not reconciled with this date | D1, "Timeline and budget"; Q49 |
| Budget / currency | Not confirmed; to be discussed once scope is agreed | D1, "Timeline and budget"; Q49 |

## 3. Key Clarifications

Ten most consequential unresolved items, most consequential first. Selection and ordering are analyst judgments about consequence; classifications and timing are reused unchanged from sections 7 and 8.

| Ref | Unresolved item | Question IDs | Decision or activity affected | Classification | Needed by |
| --- | --- | --- | --- | --- | --- |
| G01 | No precedence is established between D1 and D2, and D2's approval status is unstated (labelled v0.3; uses "recommends"), so four contradictions stand unresolved | Q26, Q31, Q37, Q40, Q46, Q6 | Any commitment to scope, architecture, or cost | Blocking | Before estimation |
| G02 | Conflict: full production across four depots at go-live versus an MVP at Warrington only with a second phase | Q6, Q7, Q8, Q19, Q25 | Estimation, release planning, and definition of the first delivery | Blocking | Before estimation |
| G03 | Conflict: hosting on AWS as group standard versus the on-premises VMware cluster, with public cloud not approved for operational data | Q31, Q32 | Architecture, infrastructure design, and cost model | Blocking | Before estimation |
| G04 | Conflict: offline inspection capture with sync on reconnect versus an online-only web application | Q37, Q40, Q28 | Architecture and the client application model | Blocking | Before estimation |
| G05 | Conflict: inspection record retention of 15 months versus 7 years | Q46 | Retention and storage design, and the compliance position | Blocking | Before design or implementation |
| G06 | Driver authentication is stated to be still under discussion, and drivers are the primary user group | Q44, Q22 | Implementation of driver access | Blocking | Before design or implementation |
| G07 | The source and maintenance of vehicle-specific checklists, and of the vehicle and driver data behind them, are not described | Q19, Q23, Q33 | Implementation of the inspection capture feature | Blocking | Before design or implementation |
| G08 | No acceptance criteria are defined, and budget is deferred until scope is agreed | Q51, Q49 | Commitment to scope and cost | Blocking | Before estimation |
| G09 | Exception handling is not covered, including incomplete inspections, duplicate inspections for the same vehicle and day, and failed submissions | Q24 | Implementation of the inspection workflow | Non-blocking | Before design or implementation |
| G10 | Concurrency at the morning departure peak is not quantified, although roughly 300 drivers inspect before departure | Q41, Q42 | Infrastructure sizing and performance targets | Non-blocking | Before design or implementation |

- Basis: summarises the gaps register in section 7 and the client question list in section 8; no new findings are introduced here.
- Items not listed above remain in the full gaps register and client question list. A further nine non-blocking items, G11 to G19, are recorded in section 7.

## 4. What Needs to Be Built or Changed

### Modules and Features

| Module / feature | Existing behaviour | Requested work | Users / roles | Phase / priority | Evidence |
| --- | --- | --- | --- | --- | --- |
| Vehicle-specific inspection checklist | Not applicable — carbon-copy paper books | Driver completes the checklist on a handheld device | Driver | Disputed: first release in both sources, but D2 limits it to Warrington | D1, requirement 1; D2, "Delivery approach" |
| Defect classification | Not applicable | Classify each defect as advisory or safety-critical | Driver | Disputed as above | D1, requirement 2 |
| Safety-critical blocking and notification | Not applicable | Prevent the vehicle being marked available and notify the depot manager | Driver, depot manager | Disputed as above | D1, requirement 3 |
| Rectification and re-authorisation | Not applicable | Depot manager records rectification and re-authorises the vehicle | Depot manager | D1: first release. D2 does not name it in the MVP | D1, requirement 4; D2, "Delivery approach" |
| Compliance search and CSV export | Not applicable | Search by vehicle, driver, depot, and date range; export to CSV | Compliance team | Conflicting: D1 first release, D2 second phase | D1, requirement 5; D2, "Delivery approach" |
| Tranman maintenance integration | Not applicable | Automatic integration with the fleet maintenance system | Not stated | Second phase, not required at initial delivery | D2, "Later phase" |

### Main Workflows and Business Rules

One workflow is described consistently across both sources at the level D1 provides: before leaving the depot, a driver completes the walkaround inspection for a specific vehicle; each defect is classified as advisory or safety-critical; a safety-critical defect immediately prevents the vehicle from being marked available and notifies the depot manager; the depot manager records rectification and re-authorises the vehicle; the compliance team searches and exports records for audits. Who is entitled to classify or override a classification, and how the checklist content is determined per vehicle, are not stated.

### Integrations and Data

No integration is required at initial delivery. D2 names automatic integration with the Tranman fleet maintenance system as a second-phase item, without describing what it must do. Neither document states where vehicle, driver, or depot reference data originates, or whether Tranman is the authoritative source for vehicle records. Authentication for managers and compliance staff is to use existing Active Directory accounts, which is a dependency on internal infrastructure rather than a described integration.

### Quality, Security, and Operational Requirements

Agreed across sources: Samsung Android handhelds, with D2 adding Android 11. Stated by D1 only, and contradicted by D2: offline completion with synchronisation on reconnect. Stated by D1: retention of 15 months. Stated by D2: retention of 7 years, single tenant with internal users only, Active Directory authentication for managers and compliance staff, driver authentication undecided, and that Calder IT will operate the system after handover with support hours and response targets not yet agreed. Not stated by either source: browsers, languages, accessibility, performance targets, availability targets, backup and recovery, monitoring, and environments.

### Exclusions and Deferred Work

Nothing is explicitly excluded. Deferred by D2: remaining depots and the compliance reporting module to a second phase, which D1 contradicts, and the Tranman integration to a second phase, which D1 does not address and which is therefore recorded as a phase statement rather than a contradiction. D2 states offline storage on the device is not required, which is a contradicted requirement rather than an exclusion.

## 5. Section-by-Section Answers

### Section 1 — Project Overview

### Q1. What is the project, and what business problem should it solve?
- Answer:
  - Project: FleetCheck, a new system for capturing daily driver walkaround inspections digitally, blocking vehicles with safety-critical defects, notifying depot managers, and giving the compliance team a searchable record.
  - Business problem: inspections are recorded on carbon-copy paper books across 260 vehicles, and the compliance team needs a searchable record for traffic commissioner audits.
- Status: Stated
- Evidence: D1, "Purpose": "These are currently recorded on carbon-copy paper books. FleetCheck is a new system to capture these inspections digitally and give the compliance team a searchable record."
- Uncertainty / clarification: None. D2 does not contradict the purpose.

### Q2. Who are the intended users, and what are their main goals?
- Answer:
  - Drivers, approximately 300 including agency drivers: complete inspections.
  - Depot managers: review defects and authorise vehicles for use.
  - Compliance team: run reports for traffic commissioner audits.
  - D2 adds that users are internal only.
- Status: Stated
- Evidence: D1, "Users" list; D2, "Platform notes": "Single tenant, internal users only."
- Uncertainty / clarification: None.

### Q3. Is this a new project, an enhancement, an extension, a migration, or a replacement of an existing system?
- Answer: A new project. The current process is paper carbon-copy books, and there is no existing digital system for inspections.
- Status: Stated
- Evidence: D1, "Purpose": "There is no existing digital system for inspections."
- Uncertainty / clarification: None. D2 describes deployment of a new system and does not contradict this.

### Q4. What outcomes would make this project successful, and how will they be measured?
- Answer: Stated outcomes are a digital record of inspections in place of paper and a searchable record that satisfies traffic commissioner audits. No measures, targets, or baselines are given.
- Status: Partially stated
- Evidence: D1, "Purpose": "give the compliance team a searchable record"; D1, "Non-functional requirements": "to satisfy audit requirements".
- Uncertainty / clarification: How success will be measured, for example inspection completion rates, defect turnaround, or audit outcomes, is not stated in either source.

### Q5. Who are the stakeholders, decision-makers, and final approvers?
- Answer: Sign-off on functionality sits with the Compliance Director; IT reviews technical decisions. D2 is authored by Calder IT. No budget approver or overall project owner is identified.
- Status: Partially stated
- Evidence: D1, "Roles and approval": "Sign-off on functionality sits with the Compliance Director. IT will review technical decisions."; D2 header: "prepared by Calder IT".
- Uncertainty / clarification: Unresolved and material: who arbitrates where a functional requirement and a technical constraint disagree, which is the situation in all four contradictions recorded under Q26. No budget approver is named, and budget is deferred (Q49).

### Section 2 — New Project

### Q6. Is the expected deliverable a prototype, proof of concept, MVP, or full production system?
- Answer:
  - D1: a full production system in use across all four depots at go-live, and phased depot rollout is explicitly not acceptable to the compliance team.
  - D2: an MVP for the Warrington depot only, with remaining depots and the compliance reporting module to follow in a second phase.
- Status: Conflicting
- Evidence: D1, "Delivery expectation": "We need a full production system in use across all four depots at go-live. Phased depot rollout is not acceptable to the compliance team."; D2, "Delivery approach": "Calder IT recommends delivering an MVP for the Warrington depot only... with the remaining depots and the compliance reporting module to follow in a second phase."
- Uncertainty / clarification: Directly incompatible statements about the same first delivery. No precedence is established between the sources. D2 uses the word "recommends", which may indicate a proposal rather than a decision, but it is presented as the deployment approach in a later dated document. See gaps G01 and G02.

### Q7. Which features are essential for the first release?
- Answer:
  - D1: all five functional requirements at go-live, namely checklist capture, defect classification, safety-critical blocking with manager notification, rectification and re-authorisation, and compliance search with CSV export, across all four depots.
  - D2: inspection capture and defect notification only, at the Warrington depot.
- Status: Conflicting
- Evidence: D1, "Functional requirements" 1 to 5, with "Delivery expectation" requiring all four depots at go-live; D2, "Delivery approach": "covering inspection capture and defect notification, with the remaining depots and the compliance reporting module to follow in a second phase."
- Uncertainty / clarification: The two first-release definitions differ in both feature set and depot coverage. Whether rectification and re-authorisation sits inside D2's MVP is not stated by D2 at all.

### Q8. Which features can wait for later phases?
- Answer:
  - D1: nothing; phased depot rollout is explicitly unacceptable.
  - D2: remaining depots and the compliance reporting module in a second phase.
  - Separately, D2 defers the Tranman fleet maintenance integration to a second phase. D1 does not mention Tranman, so this is a phase statement rather than a contradiction.
- Status: Conflicting
- Evidence: D1, "Delivery expectation": "Phased depot rollout is not acceptable to the compliance team."; D2, "Delivery approach" and "Later phase": "Automatic integration with the fleet maintenance system, Tranman, is planned for a second phase and is not required at initial delivery."
- Uncertainty / clarification: The conflict concerns depots and compliance reporting. Whether a second phase is inside this engagement or a separate future commitment is not stated in either source.

### Q9. Are designs, wireframes, reference products, or technical prototypes available?
- Answer: Not addressed in either source.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Whether an example paper inspection book, a reference application, or any prototype exists is unresolved, as is whether design production falls to the supplier (Q39).

### Q10. Does the first release need to support real customers and production data?
- Answer:
  - D1 establishes production use: a full production system in use across all four depots at go-live, producing the record used for traffic commissioner audits.
  - D2 does not state whether its Warrington MVP would carry real inspection records with compliance value.
- Status: Partially stated
- Evidence: D1, "Delivery expectation" and "Purpose"; D2, "Delivery approach" is silent on production data.
- Uncertainty / clarification: The unresolved part follows from the conflict recorded at Q6. If the MVP route is taken, whether Warrington drivers stop using paper books, and whether the MVP record is audit-valid, are unanswered.

### Section 3 — Existing Project

### Q11. Is the existing system live, in development, a demo, or currently unused?
- Answer: Not applicable. There is no existing digital system; inspections are on paper.
- Status: Not applicable
- Evidence: D1, "Purpose": "There is no existing digital system for inspections." Classification: New (Q3).
- Uncertainty / clarification: None.

### Q12. What functionality already exists, what needs to change, and what is the intended scope of the first release versus later releases?
- Answer: Not applicable for existing functionality. First-release and later-release scope are recorded under Q6, Q7, and Q8, where the sources conflict.
- Status: Not applicable
- Evidence: D1, "Purpose"; classification New (Q3).
- Uncertainty / clarification: None; release information is retained under Q6 to Q8 and Q49.

### Q13. Why is the change needed: missing features, technical limitations, performance, maintenance, or another reason?
- Answer: Not applicable as an existing-system question. The business motivation is recorded under Q1.
- Status: Not applicable
- Evidence: D1, "Purpose"; classification New (Q3).
- Uncertainty / clarification: None.

### Q14. What is the current technology stack, including versions?
- Answer: Not applicable. No existing digital system. Handheld device details are recorded under Q40.
- Status: Not applicable
- Evidence: D1, "Purpose"; classification New (Q3).
- Uncertainty / clarification: None.

### Q15. Are source code, documentation, environments, and a working demo available, and is access confirmed or merely referenced?
- Answer: Not applicable. No existing system exists to provide access to. Target infrastructure availability is recorded under Q31 and Q32, where the sources conflict.
- Status: Not applicable
- Evidence: D1, "Purpose"; classification New (Q3).
- Uncertainty / clarification: None.

### Q16. What existing behaviour, integrations, data, and URLs must be preserved?
- Answer: Not applicable. No existing system, integrations, or URLs. Whether historical paper records must be captured is recorded under Q34.
- Status: Not applicable
- Evidence: D1, "Purpose"; classification New (Q3).
- Uncertainty / clarification: None.

### Q17. Is the proposed approach to extend, refactor, migrate, or rebuild the system?
- Answer: Not applicable. There is no existing system to extend, refactor, migrate, or rebuild.
- Status: Not applicable
- Evidence: D1, "Purpose"; classification New (Q3).
- Uncertainty / clarification: None.

### Q18. Must the old and new systems run alongside each other, and are transition, cutover, downtime, or rollback expectations defined?
- Answer: Not applicable to an existing digital system. No cutover, downtime, or rollback expectations are stated.
- Status: Not applicable
- Evidence: D1, "Purpose"; classification New (Q3).
- Uncertainty / clarification: How long paper books run in parallel, particularly under D2's single-depot MVP route, is unresolved and is recorded under Q6 and Q10.

### Section 4 — Scope and Business Requirements

### Q19. What modules and features are explicitly included, including any commercial functionality such as pricing, plans, payments, subscriptions, invoicing, or usage limits where the sources describe them?
- Answer:
  - Included across the sources: vehicle-specific inspection checklist capture on a handheld; defect classification as advisory or safety-critical; immediate blocking of vehicle availability plus depot manager notification for safety-critical defects; depot manager rectification and re-authorisation; compliance search by vehicle, driver, depot, and date range with CSV export.
  - Commercial functionality is not raised in either source; FleetCheck is described as an internal compliance tool, so no pricing, payment, or subscription features are indicated.
  - Not addressed: how vehicle-specific checklists are defined and maintained, and any administration of vehicles, drivers, depots, and users.
- Status: Partially stated
- Evidence: D1, "Functional requirements" 1 to 5; D2, "Delivery approach" names inspection capture and defect notification.
- Uncertainty / clarification: The feature set itself is consistent between the sources; what they dispute is phasing, recorded at Q6 to Q8. Unresolved: checklist definition and maintenance, and administration functions, both of which the described features depend on. See gap G07.

### Q20. What is explicitly excluded?
- Answer: Nothing is explicitly excluded in either source.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: D2's statements that offline storage "is not required" and that Tranman "is not required at initial delivery" are a contradicted requirement and a deferral respectively, recorded at Q40 and Q8. Neither is a scope exclusion.

### Q21. What are the main user journeys and business workflows, including triggers, steps, and outcomes?
- Answer:
  - Trigger: before leaving the depot, each day, for a specific vehicle.
  - Steps: driver completes the vehicle-specific checklist; each defect is classified advisory or safety-critical; a safety-critical defect immediately prevents the vehicle from being marked as available and notifies the depot manager; the depot manager records rectification and re-authorises the vehicle.
  - Compliance journey: search inspections by vehicle, driver, depot, and date range, and export to CSV.
- Status: Stated
- Evidence: D1, "Purpose": "Drivers must complete a daily walkaround inspection before leaving the depot"; D1, "Functional requirements" 1 to 5.
- Uncertainty / clarification: None for the documented journey. Exception paths are addressed under Q24 and role boundaries under Q22.

### Q22. What user roles exist, and what can each role access or perform?
- Answer: Three roles: drivers complete inspections; depot managers review defects, record rectification, and authorise vehicles; the compliance team searches and exports records. Agency drivers are included in the driver population.
- Status: Partially stated
- Evidence: D1, "Users" list and "Functional requirements" 4 and 5.
- Uncertainty / clarification: Unresolved: whether depot managers and compliance staff see only their own depot or all four; whether a driver can view previous inspections; whether an administrator role exists for vehicles, users, and checklists; whether agency drivers have restricted access. Authentication per role is addressed under Q44.

### Q23. What business rules, validations, calculations, and approval processes apply, including any pricing, discount, tax, refund, or proration rules where charging is in scope?
- Answer:
  - Every defect must be classified as either advisory or safety-critical.
  - A safety-critical defect must immediately prevent the vehicle from being marked as available.
  - A safety-critical defect must notify the depot manager.
  - Vehicle re-authorisation follows recorded rectification by the depot manager, which is the one documented approval step.
  - Charging is not in scope, so pricing rules are not raised.
- Status: Partially stated
- Evidence: D1, "Functional requirements" 2 to 4.
- Uncertainty / clarification: Unresolved: who determines the classification and whether it can be overridden or escalated; whether all checklist items are mandatory; whether a driver may proceed with advisory defects; whether photographic or written evidence is required; whether the checklist content varies by vehicle type and who maintains it. See gap G07.

### Q24. Are exceptions covered, such as rejection, cancellation, duplicate submissions, and failed operations?
- Answer: No exception handling is described in either source.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Unresolved: an inspection started but not completed; two inspections for the same vehicle on the same day, for example on a shift change; a vehicle needed while blocked; a failed submission, which interacts directly with the offline conflict at Q40; rejection of a rectification record.

### Q25. Are administration, reporting, search, notifications, imports, and exports required, and what is specified for each?
- Answer:
  - Search: by vehicle, driver, depot, and date range, for the compliance team.
  - Exports: CSV export of inspection records.
  - Notifications: the depot manager must be notified of a safety-critical defect; the delivery channel is not stated.
  - Reporting: D1 refers to the compliance team running reports for audits; D2 refers to a "compliance reporting module" placed in a second phase.
  - Administration and imports: not specified.
- Status: Partially stated
- Evidence: D1, "Users" and "Functional requirements" 3 and 5; D2, "Delivery approach".
- Uncertainty / clarification: Unresolved: the notification channel, such as in-app, email, or SMS, and whether it must reach a manager who is away from a desk; what reports are required beyond search and CSV export; how vehicle, driver, and depot data enters the system; whether checklist templates are imported. The phasing of the reporting module is part of the conflict at Q6 to Q8.

### Q26. Are any requirements contradictory, ambiguous, or dependent on unstated assumptions?
- Answer:
  - Four direct contradictions between the sources: first delivery scope (Q6 to Q8); hosting and data location (Q31, Q32); offline capability and the resulting client application model (Q37, Q40, Q28); retention period, 15 months versus 7 years (Q46).
  - Both documents invoke internal policy on opposite sides of the hosting question: D1 cites a group IT standard for AWS, D2 states public cloud is not approved for operational data.
  - Not a contradiction: the Tranman integration is placed in a second phase by D2 and is not mentioned by D1; this is a phase statement.
  - Not a contradiction: D2's Android 11 detail refines D1's Samsung Android handhelds.
  - Ambiguity: D2 is labelled v0.3 and uses "recommends"; neither source states whether D2 is draft, interim, approved, or superseded, so its decision status is unclear.
  - Ambiguity: D1 assigns functional sign-off to the Compliance Director and technical review to IT, but the offline requirement and the retention period are functional requirements contradicted by the IT-authored document, so the arbitration path is undefined.
  - Unstated dependency: the described features rely on vehicle-specific checklists and on vehicle, driver, and depot reference data whose origin is not described.
  - Unstated dependency: the 1 September 2026 go-live date is stated by D1 alongside its full-rollout expectation; whether the date survives under D2's phased approach is not addressed.
- Status: Inferred
- Evidence: D1, "Hosting": "must be hosted on Amazon Web Services, in line with our group IT standard"; D2, "Deployment target": "We are not currently approved to place operational data in public cloud environments"; D1, "Non-functional requirements": "Inspections must be completable with no network connectivity"; D2, "Connectivity": "offline storage on the device is not required"; D1: "kept for 15 months"; D2: "retained for 7 years".
- Uncertainty / clarification: This answer is an analyst analysis of the cited statements, not a claim made by either source. No resolution is proposed here; see gaps G01 to G05.

### Section 5 — Technology and Architecture

### Q27. What target technology stack is mandatory, preferred, proposed, or explicitly open for recommendation, including versions where stated?
- Answer: D2 states .NET and SQL Server as a preference and explicitly notes this is a preference rather than a mandate. No versions are given. D1 states no stack.
- Status: Partially stated
- Evidence: D2, "Platform notes": "Preferred stack is .NET and SQL Server, though the addendum notes this is a preference rather than a mandate."
- Uncertainty / clarification: Unresolved: versions; whether the supplier may propose alternatives, and who would approve them; the client application technology, which is entangled with the offline conflict at Q28 and Q40.

### Q28. Which platforms are required: web, mobile, desktop, APIs, or background services?
- Answer:
  - D1 requires a handheld device experience with offline completion, which implies a device-resident application or equivalent offline-capable client.
  - D2 states the application "can therefore be built as a standard online-only web application".
  - Depot manager and compliance access channels are not specified in either source. APIs and background services are not mentioned.
- Status: Conflicting
- Evidence: D1, "Functional requirements" 1 and "Non-functional requirements": "Driver completes a vehicle-specific inspection checklist on a handheld device"; "Inspections must be completable with no network connectivity"; D2, "Connectivity": "The application can therefore be built as a standard online-only web application."
- Uncertainty / clarification: This conflict is a consequence of the offline disagreement recorded at Q40 and gap G04; the two are tracked as one issue in the gaps register. Manager and compliance platforms remain unspecified regardless of the outcome.

### Q29. Are there existing architecture standards, reusable components, or coding conventions to follow?
- Answer: None stated. Neither source describes architecture standards, reusable components, or coding conventions.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources. The related stack preference is recorded at Q27; the hosting and data-location policies, which the sources dispute, are recorded at Q31 and Q32.
- Uncertainty / clarification: Calder IT will operate the system after handover (Q47), so any conventions or infrastructure patterns they expect are material and unresolved.

### Q30. Is the system for one organisation or multiple tenants/customers, and are isolation or customer-specific configuration requirements defined?
- Answer: Single tenant, internal users only. Depot-level configuration is implied by four depots and depot-based search, but no isolation requirement is stated.
- Status: Stated
- Evidence: D2, "Platform notes": "Single tenant, internal users only."
- Uncertainty / clarification: Whether depot-level data separation is required is recorded as a role-visibility question at Q22, not as tenancy.

### Q31. Where must it run: cloud, on-premises, or a hybrid environment?
- Answer:
  - D1: must be hosted on Amazon Web Services, described as being in line with the group IT standard.
  - D2: will be deployed to the on-premises VMware cluster at the Warrington data centre, because public cloud is not currently approved for operational data.
- Status: Conflicting
- Evidence: D1, "Hosting": "The system must be hosted on Amazon Web Services, in line with our group IT standard."; D2, "Deployment target": "FleetCheck will be deployed to our on-premises VMware cluster at the Warrington data centre. We are not currently approved to place operational data in public cloud environments."
- Uncertainty / clarification: Both statements cite internal policy and are incompatible. Neither source is established as authoritative. See gaps G01 and G03.

### Q32. Are there restrictions on hosting, licensing, third-party services, data location, or technology choices?
- Answer:
  - Data location and hosting: contradicted. D1 mandates AWS as group standard; D2 states operational data is not approved for public cloud and specifies the Warrington on-premises cluster.
  - Technology: .NET and SQL Server preferred, not mandated (D2).
  - Licensing and third-party services: not addressed.
- Status: Conflicting
- Evidence: D1, "Hosting"; D2, "Deployment target"; D2, "Platform notes".
- Uncertainty / clarification: The unresolved restriction is the data-location approval position, which also determines the hosting answer at Q31. Licensing and third-party service constraints remain unaddressed in both sources.

### Section 6 — Data and Integrations

### Q33. What data must the system store, where does it originate, and which systems are authoritative for shared data?
- Answer:
  - Data: inspection records including checklist responses, defect entries with advisory or safety-critical classification, vehicle, driver, depot, date, vehicle availability state, and rectification and re-authorisation records.
  - Origin: entered by drivers on handhelds and by depot managers.
  - Authoritative systems: not stated. Tranman is named as the fleet maintenance system but is not described as authoritative for vehicle data.
- Status: Partially stated
- Evidence: D1, "Functional requirements" 1 to 5, and search fields "by vehicle, driver, depot, and date range"; D2, "Later phase" names Tranman.
- Uncertainty / clarification: Unresolved: where the 260-vehicle list, driver records including agency drivers, and depot data come from, and whether they are maintained in FleetCheck or sourced from Tranman or another system. Whether photographic evidence is stored is not addressed by either source. See gap G07.

### Q34. Is existing data migration required, including cleaning, mapping, and validation?
- Answer: Not addressed in either source.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources. Context: D1 states inspections are currently on carbon-copy paper books.
- Uncertainty / clarification: Unresolved: whether any historical paper records must be captured or retained digitally, which interacts with the retention conflict at Q46, and whether vehicle and driver reference data must be loaded from an existing system.

### Q35. Which external systems, APIs, or services must be integrated, and what is each integration expected to do?
- Answer: No integration is required at initial delivery. D2 names automatic integration with the Tranman fleet maintenance system as a second-phase item. Active Directory is named as the authentication source for managers and compliance staff.
- Status: Partially stated
- Evidence: D2, "Later phase": "Automatic integration with the fleet maintenance system, Tranman, is planned for a second phase and is not required at initial delivery."; D2, "Platform notes": "Authentication should use our existing Active Directory accounts for managers and compliance staff."
- Uncertainty / clarification: Unresolved: what the Tranman integration must actually do, for example raising defects as maintenance jobs or sourcing vehicle records; whether any part of it is needed at initial delivery to obtain vehicle data (see Q33); the Active Directory integration method and whether it is reachable from the chosen hosting environment, which depends on the conflict at Q31.

### Q36. Are integration documentation, sandbox access, credentials provisioning, and vendor support available or assigned to an owner?
- Answer: Not addressed in either source for Tranman or for Active Directory.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Unresolved even for the initial delivery, because Active Directory authentication is stated as a requirement and requires access, configuration detail, and an owner. No secret values appear in the sources or this report.

### Q37. Does data need to sync in real time, on a schedule, or manually, and in which direction?
- Answer:
  - D1: inspection records are captured on the device without connectivity and sync when the device reconnects, that is device to server, event-driven on reconnection.
  - D2: the application is online-only, so no device-side store-and-forward synchronisation exists.
  - External synchronisation: none at initial delivery, since Tranman is deferred.
- Status: Conflicting
- Evidence: D1, "Non-functional requirements": "Inspections must be completable with no network connectivity... Records sync when the device reconnects."; D2, "Connectivity": "The application can therefore be built as a standard online-only web application; offline storage on the device is not required."
- Uncertainty / clarification: Same underlying disagreement as Q40 and Q28, tracked as gap G04. If the D1 position holds, conflict handling, ordering, and duplicate protection on sync are undefined (see Q24 and Q38).

### Q38. What should happen when an integration fails, times out, returns incomplete data, or delivers duplicate events?
- Answer: Not addressed in either source.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Unresolved: behaviour on failed synchronisation or submission, which becomes central if offline capture is required; behaviour if Active Directory is unavailable at sign-in; duplicate inspection records arriving from a device after a retry.

### Section 7 — Design and Quality Requirements

### Q39. Are approved designs, branding, and content available, or is producing them part of the scope, and are ownership or licensing terms stated for client-supplied assets such as designs, fonts, imagery, copy, and third-party components?
- Answer: Not addressed in either source. No designs, branding, content, ownership, or licensing statements appear.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Unresolved: whether design production is in the supplier's scope; whether branding exists; who provides the checklist content, which is the core screen of the driver experience and is also raised at Q19 and Q23. Asset licensing is not raised by either source.

### Q40. What devices, browsers, languages, and accessibility requirements must be supported, including any stated locales, regional date/number/currency formats, time-zone handling, and offline or intermittent-connectivity expectations?
- Answer:
  - Devices: agreed across sources. Samsung Android handhelds already issued to drivers, with D2 adding Android 11.
  - Connectivity: contradicted. D1 requires inspections to be completable with no network connectivity, because two depots have no usable signal in the vehicle yard, with records syncing on reconnect. D2 states devices will operate on depot Wi-Fi and that offline storage on the device is not required.
  - Browsers, languages, locales, regional formats, time zones, and accessibility: not stated by either source. Locale and currency formats are not raised and no multi-country or currency scope is described.
- Status: Conflicting
- Evidence: D1, "Non-functional requirements": "Inspections must be completable with no network connectivity, because two depots have no usable signal in the vehicle yard. Records sync when the device reconnects."; "Must work on the Samsung Android handhelds already issued to drivers."; D2, "Connectivity": "Devices will operate on the depot Wi-Fi network. The application can therefore be built as a standard online-only web application"; D2, "Platform notes": "Handheld devices are Samsung Android units running Android 11."
- Uncertainty / clarification: The connectivity contradiction is factual as well as technical: D1 asserts no usable signal in two vehicle yards, D2 asserts depot Wi-Fi coverage. Resolution requires a statement about actual coverage where inspections are performed, not a preference. See gap G04. Browser and accessibility requirements remain unstated regardless of the outcome.

### Q41. What user numbers, concurrent usage, data volumes, and growth are expected?
- Answer:
  - 260 vehicles, approximately 300 drivers including agency drivers, four depots, and one inspection per vehicle per working day is implied by the daily requirement.
  - Depot manager and compliance user counts, concurrency, storage volumes, and growth are not stated.
- Status: Partially stated
- Evidence: D1, "Purpose": "operates 260 vehicles"; D1, "Users": "Drivers (approximately 300, including agency drivers)".
- Uncertainty / clarification: Unresolved and material: concurrency at the morning departure peak, when a large share of roughly 300 drivers would inspect within a short window. Storage volume depends on the retention conflict at Q46 and on whether photographic evidence is captured (Q33). See gap G10.

### Q42. Are performance, availability, and acceptable downtime targets defined?
- Answer: Not stated in either source.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Unresolved and consequential: an inspection is a precondition for a vehicle leaving the depot, so unavailability has an operational impact, and no fallback process is described. This interacts with the offline conflict at Q40.

### Q43. Are SEO, analytics, audit history, or activity tracking required, and what is specified for each?
- Answer: Not stated in either source. The compliance-facing searchable inspection record is a functional requirement recorded at Q19 and Q25, not a system audit history or activity-tracking requirement.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Unresolved: whether changes to inspection records, defect classifications, or re-authorisations must be audited and tamper-evident, which is usually material where records are produced to a regulator. Neither source raises analytics, and SEO is not relevant to an internal system.

### Section 8 — Security and Operations

### Q44. What authentication, permissions, and account-management requirements apply?
- Answer:
  - Managers and compliance staff: existing Active Directory accounts.
  - Drivers: authentication method is explicitly still under discussion.
  - Permissions and account management: not defined beyond the role actions at Q22.
- Status: Partially stated
- Evidence: D2, "Platform notes": "Authentication should use our existing Active Directory accounts for managers and compliance staff. Driver authentication method is still under discussion."
- Uncertainty / clarification: Unresolved and material: how approximately 300 drivers, including agency drivers, authenticate on shared handhelds, and how accounts are issued and revoked as agency drivers change. Whether drivers exist in Active Directory is not stated. See gap G06.

### Q45. Will the system handle sensitive information, and what privacy or compliance requirements are explicitly identified?
- Answer:
  - The system holds driver-identifiable inspection and defect records used for regulatory audits, and D2 describes the data as operational data subject to a cloud-approval restriction.
  - Compliance driver stated: records must satisfy traffic commissioner audit requirements.
  - No privacy or data-protection requirement is explicitly identified in either source.
- Status: Partially stated
- Evidence: D1, "Non-functional requirements": "inspection records must be kept for 15 months to satisfy audit requirements"; D1, search fields include driver; D2, "Deployment target": "not currently approved to place operational data in public cloud environments".
- Uncertainty / clarification: Unresolved: applicable data-protection obligations for driver records including agency staff, whether inspection records may be used in employment or disciplinary contexts, and what the cloud-approval restriction requires in practice. No requirement is assumed here beyond what is quoted.

### Q46. What backup, recovery, data retention, and deletion requirements exist?
- Answer:
  - Retention: contradicted. D1 requires inspection records to be kept for 15 months to satisfy audit requirements. D2 requires 7 years, consistent with the group data retention schedule.
  - Backup, recovery, and deletion: not stated in either source.
- Status: Conflicting
- Evidence: D1, "Non-functional requirements": "inspection records must be kept for 15 months to satisfy audit requirements."; D2, "Data retention": "Inspection records are to be retained for 7 years, consistent with our group data retention schedule."
- Uncertainty / clarification: Both cite an authority: audit requirements versus the group retention schedule. The difference is material for storage sizing, archiving, and the compliance position. Backup, recovery objectives, and deletion remain unaddressed by either source. See gap G05.

### Q47. Who will own or control hosting accounts, source code, data, deployments, third-party subscriptions and licences, and ongoing maintenance?
- Answer: Calder IT will operate the system after handover. Ownership of hosting accounts, source code, data, third-party licences, and pre-handover deployment responsibility are not stated.
- Status: Partially stated
- Evidence: D2, "Support": "Calder IT will operate the system after handover."
- Uncertainty / clarification: Unresolved: hosting account ownership, which also depends on the hosting conflict at Q31; source code and data ownership; who deploys before handover; what "operate" includes.

### Q48. What environments, deployment process, monitoring, incident handling, and support arrangements are required, including any stated support hours, response or resolution targets, and escalation paths?
- Answer:
  - Operations: Calder IT will operate the system after handover.
  - Support hours and response targets: explicitly not yet agreed.
  - Environments, deployment process, monitoring, incident handling, and escalation: not stated.
- Status: Partially stated
- Evidence: D2, "Support": "Calder IT will operate the system after handover. Support hours and response targets have not yet been agreed."
- Uncertainty / clarification: The support-target part is explicitly open in the source rather than merely absent. Unresolved: environment count and ownership, which depends on the hosting conflict at Q31; monitoring and alerting for failed inspection submissions, which matters most under the offline position at Q40.

### Section 9 — Delivery and Acceptance

### Q49. What are the budget, target dates, milestones, delivery priorities, release expectations, and any stated engagement commercial model?
- Answer:
  - Target date: go-live required by 1 September 2026 (D1).
  - Budget: not confirmed; to be discussed once scope is agreed (D1).
  - Milestones, delivery priorities, and commercial model: not stated.
  - Release expectations: contradicted between the sources, as recorded at Q6 to Q8.
- Status: Partially stated
- Evidence: D1, "Timeline and budget": "Go-live is required by 1 September 2026. Budget has not been confirmed and will be discussed once scope is agreed."
- Uncertainty / clarification: The date itself is not contradicted, so this is not recorded as a conflict. Unresolved: what the 1 September 2026 date covers if the phased approach in D2 is adopted, and whether the date is regulatory, operational, or preferred. In-product billing is not in scope (Q19).

### Q50. What dependencies or client-provided inputs could delay delivery, and who is responsible for them?
- Answer: No dependencies or client-provided inputs are identified in either source, and no owners are assigned.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Likely inputs implied but not documented: checklist content, the vehicle and driver data set, handheld device access for testing, Active Directory integration details, VMware or AWS environment provisioning depending on the hosting outcome, and a decision on driver authentication. None is stated or owned.

### Q51. What measurable acceptance criteria define completion?
- Answer: Not stated in either source.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Unresolved, and compounded by the conflict at Q6 to Q8: completion cannot be defined while the first delivery is disputed between four depots in production and a single-depot MVP. See gap G08.

### Q52. Who will test and approve the deliverables, what testing or sign-off process is specified, and who provides test data, test accounts, and UAT environment access?
- Answer: Sign-off on functionality sits with the Compliance Director, and IT reviews technical decisions. No testing process, testers, test data, test accounts, or UAT environment arrangements are described.
- Status: Partially stated
- Evidence: D1, "Roles and approval": "Sign-off on functionality sits with the Compliance Director. IT will review technical decisions."
- Uncertainty / clarification: Unresolved: who tests, particularly whether drivers participate in yard testing where the connectivity question is decided; who provides test data, test devices, and Active Directory test accounts; who hosts the UAT environment, which depends on the hosting conflict at Q31. Test-data and UAT ownership are not raised by either source.

### Q53. What documentation, training, handover, and post-launch support are expected?
- Answer: A handover to Calder IT is implied by their operating the system afterwards. No documentation, training, or post-launch support arrangements are specified, and support hours and response targets are explicitly not yet agreed.
- Status: Partially stated
- Evidence: D2, "Support": "Calder IT will operate the system after handover. Support hours and response targets have not yet been agreed."
- Uncertainty / clarification: Unresolved: operational and technical documentation for Calder IT; training for approximately 300 drivers moving from paper, including agency drivers who change frequently; training for depot managers; what handover comprises and how it is accepted.

### Q54. How will scope changes be reviewed and approved?
- Answer: Not stated in either source.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources. Functional sign-off and technical review responsibilities are recorded at Q5 and Q52 but no change process is described.
- Uncertainty / clarification: Unresolved and material given the four unresolved contradictions and the split approval responsibilities: no process or arbitrator for changes is defined.

### Section 10 — Document Completeness and Analyst Conclusions

### Q55. How many Q1–Q54 answers are Stated, Partially stated, Inferred, Missing, Conflicting, and Not applicable?
- Answer: Stated 5; Partially stated 18; Inferred 1; Missing 13; Conflicting 9; Not applicable 8. Total 54. See the dashboard in section 6.
- Basis: The tally reproduces the statuses assigned in section 5 without reclassification.
- Limitations: The nine Conflicting answers arise from four underlying disagreements, because several checklist questions touch the same disputed subjects. The gaps register deduplicates them into four conflict entries.

### Q56. Are referenced attachments, diagrams, designs, and supporting documents available and reviewed?
- Answer: Neither source references attachments, diagrams, designs, or supporting documents. Both supplied documents were read in full. D1 refers to a group IT standard and D2 to a group data retention schedule; neither underlying policy document was supplied, and both are cited on opposite sides of open questions.
- Basis: Source inventory in section 1; D1, "Hosting"; D2, "Data retention".
- Limitations: The two cited group policies could not be reviewed, so the hosting and retention contradictions cannot be assessed against the policies themselves. No paper inspection book, checklist template, or device inventory was supplied.

### Q57. Which unresolved items block specific estimation, architecture, implementation, acceptance, or release decisions, and why?
- Answer: Eight items are classified Blocking: G01 absence of established precedence between the documents, G02 the first-delivery conflict, G03 the hosting and data-location conflict, G04 the offline capability conflict, G05 the retention conflict, G06 driver authentication, G07 checklist and reference data, and G08 the absence of acceptance criteria. Each blocked activity and dependency is set out in section 7.
- Basis: Gaps register in section 7, derived from the statuses in section 5.
- Limitations: G01 is the precondition for G02 to G05: those four cannot be closed by choosing a technically preferable option, only by an authoritative client decision. The remaining items in the register are recorded as non-blocking with the point at which they become necessary.

### Q58. What must be clarified before committing to scope, cost, or timelines, and what can be resolved later?
- Answer: Before any commitment: which document governs and who arbitrates (G01); the first-delivery definition, whether four depots in production or a Warrington MVP (G02); hosting and data location (G03); offline capability, which requires a factual statement on yard connectivity (G04); and acceptance criteria (G08). Before design or implementation: retention period (G05), driver authentication (G06), checklist and reference data (G07), exception handling (G09), notification channel (G11), and role visibility across depots (G12). Before release: performance and availability targets (G13), backup and recovery (G14), audit trail expectations (G15), monitoring and support arrangements (G16), and documentation and training (G17). Questions are listed in section 8.
- Basis: Gaps register and client question list.
- Limitations: Sequencing is an analyst view. D2's approval status is unstated, so several disagreements may already be under internal discussion at the client, which the supplied documents would not show.

## 6. Answer Coverage Dashboard

| Status | Number of questions | Question IDs |
| --- | --- | --- |
| Stated | 5 | Q1, Q2, Q3, Q21, Q30 |
| Partially stated | 18 | Q4, Q5, Q10, Q19, Q22, Q23, Q25, Q27, Q33, Q35, Q41, Q44, Q45, Q47, Q48, Q49, Q52, Q53 |
| Inferred | 1 | Q26 |
| Missing | 13 | Q9, Q20, Q24, Q29, Q34, Q36, Q38, Q39, Q42, Q43, Q50, Q51, Q54 |
| Conflicting | 9 | Q6, Q7, Q8, Q28, Q31, Q32, Q37, Q40, Q46 |
| Not applicable | 8 | Q11, Q12, Q13, Q14, Q15, Q16, Q17, Q18 |
| Total | 54 | Q1–Q54 |

- Applicable questions: 46.
- Reading limitations affecting the tally: None for the supplied documents, both read in full. The group IT standard and group data retention schedule cited by the sources were not supplied, which limits Q31, Q32, and Q46.

## 7. Gaps, Conflicts, and Dependencies

### Blocking Items

| ID | Finding | Related Q IDs / sources | Decision or activity blocked | Why it is blocked | Clarification needed |
| --- | --- | --- | --- | --- | --- |
| G01 | Conflict precondition: no precedence is established between D1 and D2, D2's approval status is unstated (labelled v0.3; uses the word "recommends"), and D1 splits functional sign-off from technical review without an arbitrator | Q26, Q5, Q54; D1, D2 | Any commitment to scope, architecture, or cost | Four material disagreements cannot be closed by technical preference; they need an authoritative client decision, and no decision route exists in the documents | Which document governs, whether D2 v0.3 is approved, and who decides where a functional requirement and a technical constraint disagree |
| G02 | Conflict: full production across four depots at go-live, with phased rollout explicitly unacceptable, versus an MVP at Warrington only with remaining depots and compliance reporting in a second phase | Q6, Q7, Q8, Q19, Q25, Q10; D1 "Delivery expectation", D2 "Delivery approach" | Estimation, release planning, and definition of the first delivery | Scope, depot coverage, and feature set of the first delivery differ between the sources, so effort and completion cannot be defined | The agreed first-delivery definition, and whether a second phase is inside this engagement |
| G03 | Conflict: hosting on AWS "in line with our group IT standard" versus the on-premises VMware cluster because "we are not currently approved to place operational data in public cloud environments" | Q31, Q32, Q47, Q48; D1 "Hosting", D2 "Deployment target" | Architecture, infrastructure design, environment provisioning, and cost model | Both positions cite internal policy and are incompatible; hosting determines deployment, environments, Active Directory reachability, and operating model | The approved hosting position and data-location approval status, ideally with the group IT standard and approval record |
| G04 | Conflict: inspections must be completable with no network connectivity, because two depots have no usable signal in the vehicle yard, versus depot Wi-Fi making an online-only web application sufficient | Q37, Q40, Q28, Q24, Q38; D1 "Non-functional requirements", D2 "Connectivity" | Architecture and the client application model | Offline capture with sync changes the client technology, conflict handling, duplicate protection, testing, and effort; the sources also disagree on the factual coverage in the yards | A statement of actual connectivity where inspections are performed, and a decision on offline capability |
| G05 | Conflict: inspection records retained for 15 months "to satisfy audit requirements" versus 7 years "consistent with our group data retention schedule" | Q46, Q41, Q34; D1 "Non-functional requirements", D2 "Data retention" | Retention and storage design, and the stated compliance position | The two periods differ by a factor of roughly five for storage and archiving, and both cite an authority | The governing retention period, and whether audit and group retention obligations differ by record type |
| G06 | Driver authentication is explicitly still under discussion, and drivers are the largest user group including agency drivers on shared handhelds | Q44, Q22, Q35; D2 "Platform notes" | Implementation of driver access | Managers and compliance use Active Directory, but no method exists for approximately 300 drivers, and account issuance for changing agency staff is undefined | The driver authentication approach, whether drivers exist in Active Directory, and the account lifecycle for agency drivers |
| G07 | The source and maintenance of vehicle-specific checklists, and of vehicle, driver, and depot reference data, are not described | Q19, Q23, Q33, Q35; D1 "Functional requirements" 1 | Implementation of the inspection capture feature | The core screen is a vehicle-specific checklist, and 260 vehicles plus roughly 300 drivers must exist in the system; neither content nor data origin is defined | Who defines checklists, how they vary by vehicle, and where vehicle and driver data comes from, including any Tranman role |
| G08 | No acceptance criteria are defined, and budget is deferred until scope is agreed | Q51, Q49, Q4; D1 "Timeline and budget" | Commitment to scope and cost | Completion cannot be defined while the first delivery is disputed, and no measurable criteria exist for the agreed features | Measurable acceptance criteria for the agreed first delivery, and a budget range |

### Non-blocking Clarifications

| ID | Finding | Related Q IDs / sources | Why it matters | When needed |
| --- | --- | --- | --- | --- |
| G09 | Exception handling is not covered, including incomplete inspections, duplicate inspections for the same vehicle and day, a vehicle needed while blocked, and failed submissions | Q24, Q38; D1, D2 | These paths determine whether the system can be used operationally without a paper fallback, and become more complex if offline capture is required | Before design or implementation |
| G10 | Concurrency at the morning departure peak is not quantified | Q41, Q42; D1 "Users", "Purpose" | Roughly 300 drivers inspecting in a short window drives sizing and the performance target | Before design or implementation |
| G11 | The depot manager notification channel for safety-critical defects is not specified | Q25, Q23; D1 "Functional requirements" 3 | The notification must reach a manager who may not be at a desk; channel choice affects scope | Before design or implementation |
| G12 | Role visibility across depots, and whether an administrator role exists, are undefined | Q22, Q19; D1 "Users" | Determines access control scope across four depots and who maintains reference data | Before design or implementation |
| G13 | No performance, availability, or downtime targets are stated | Q42; D1, D2 | An inspection gates vehicle departure, so availability expectations are operationally significant and no fallback is described | Before release |
| G14 | Backup, recovery objectives, and deletion requirements are not stated | Q46; D1, D2 | Retention is disputed and the surrounding backup and deletion obligations are absent | Before release |
| G15 | Whether changes to inspection records and defect classifications must be audited and tamper-evident is not stated | Q43, Q23; D1, D2 | Records are produced to a regulator, so record integrity expectations affect design | Before design or implementation |
| G16 | Environments, monitoring, incident handling, support hours, and response targets are undefined, with support targets explicitly not yet agreed | Q48, Q47; D2 "Support" | Operational readiness and the handover to Calder IT | Before release |
| G17 | Documentation, training, and handover content are not specified, including training for approximately 300 drivers and changing agency staff | Q53; D2 "Support" | Adoption depends on driver training, and Calder IT inherits operation | Before release |
| G18 | Designs, branding, and content responsibility are not addressed | Q39, Q9; D1, D2 | Checklist content and the driver interface are central to the deliverable | Before design or implementation |
| G19 | Whether historical paper inspection records must be captured digitally, and whether the Tranman integration has any initial-delivery role, are unresolved | Q34, Q35; D2 "Later phase" | Affects migration scope and whether vehicle data can be sourced automatically | Before design or implementation |

### Documented Dependencies

| Dependency | Documented owner | Required timing | Evidence |
| --- | --- | --- | --- |
| Existing Active Directory accounts for managers and compliance staff | Client, Calder IT implied as system owner | Not stated | D2, "Platform notes" |
| Samsung Android handhelds already issued to drivers | Client | Not stated | D1, "Non-functional requirements"; D2, "Platform notes" |
| Deployment environment, either AWS or the Warrington VMware cluster | Client, subject to the unresolved hosting conflict G03 | Not stated | D1, "Hosting"; D2, "Deployment target" |
| Operation of the system after handover | Calder IT | After handover; date not stated | D2, "Support" |

Analyst note: the items above are analyst observations about what the documents leave unresolved or state incompatibly. No conflict has been resolved in this report, and classification indicates which activity is presently blocked rather than a prediction that a problem will occur.

## 8. Open Questions for the Client

### Document authority and conflicts

1. Which document governs where the Requirements Summary and the Technical Addendum disagree, and has Technical Addendum v0.3 been approved?
   - Needed: Before estimation.
   - References: Q26, Q5; G01.
2. Where a functional requirement from the compliance side and a technical constraint from IT disagree, who decides?
   - Needed: Before estimation.
   - References: Q5, Q54; G01.
3. Is the first delivery a full production rollout across all four depots, as the Requirements Summary requires, or an MVP at Warrington with a second phase, as the Technical Addendum proposes? If phased, is the second phase part of this engagement?
   - Needed: Before estimation.
   - References: Q6, Q7, Q8, Q10; G02.
4. Is the system to be hosted on AWS or on the Warrington VMware cluster, and what is the current approval position for placing operational data in public cloud? Could you share the group IT standard referred to in the Requirements Summary?
   - Needed: Before estimation.
   - References: Q31, Q32, Q56; G03.
5. Must inspections be completable with no connectivity? The two documents disagree on whether the vehicle yards have usable coverage, so a statement of actual coverage where inspections are performed would resolve it.
   - Needed: Before estimation.
   - References: Q37, Q40, Q28; G04.
6. Is the retention period for inspection records 15 months or 7 years, and do audit obligations and the group retention schedule apply to different record types?
   - Needed: Before design or implementation.
   - References: Q46; G05.

### Users, access, and roles

7. How should drivers authenticate, do driver records exist in Active Directory, and how are accounts issued and revoked for agency drivers?
   - Needed: Before design or implementation.
   - References: Q44, Q22; G06.
8. Do depot managers and compliance staff see all four depots or only their own, and is an administrator role required for vehicles, drivers, and checklists?
   - Needed: Before design or implementation.
   - References: Q22, Q19; G12.

### Inspection content and workflow

9. Who defines the vehicle-specific checklists, how do they vary by vehicle type, and who maintains them once live? An example of the current paper book would help.
   - Needed: Before estimation.
   - References: Q19, Q23, Q33, Q9; G07.
10. Where do the vehicle, driver, and depot records come from, and is Tranman the authoritative source for vehicle data?
    - Needed: Before design or implementation.
    - References: Q33, Q35; G07, G19.
11. Who classifies a defect as advisory or safety-critical, and can that classification be overridden or escalated?
    - Needed: Before design or implementation.
    - References: Q23.
12. What should happen when an inspection is started but not completed, when two inspections are recorded for the same vehicle on the same day, when a blocked vehicle is needed, and when a submission fails?
    - Needed: Before design or implementation.
    - References: Q24, Q38; G09.
13. How must a depot manager be notified of a safety-critical defect, given they may not be at a desk?
    - Needed: Before design or implementation.
    - References: Q25, Q23; G11.
14. Is photographic or written evidence required as part of an inspection or a rectification record?
    - Needed: Before design or implementation.
    - References: Q23, Q33.

### Scope and reporting

15. Beyond search and CSV export, what does the compliance reporting module need to produce?
    - Needed: Before estimation.
    - References: Q25, Q7; G02.
16. Must changes to inspection records, classifications, and re-authorisations be audited and tamper-evident for regulatory purposes?
    - Needed: Before design or implementation.
    - References: Q43, Q23; G15.
17. Does anything from the existing paper books need to be captured digitally at launch?
    - Needed: Before design or implementation.
    - References: Q34; G19.
18. What does the second-phase Tranman integration need to do, and is any part of it required at initial delivery?
    - Needed: Before design or implementation.
    - References: Q35, Q36; G19.

### Volumes, quality, and operations

19. How many drivers inspect within the morning departure window, and how many depot manager and compliance users are there?
    - Needed: Before design or implementation.
    - References: Q41; G10.
20. What performance and availability expectations apply, and what should happen operationally if the system is unavailable at departure time?
    - Needed: Before release.
    - References: Q42, Q24; G13.
21. What backup, recovery, and deletion requirements apply to inspection records?
    - Needed: Before release.
    - References: Q46; G14.
22. What data-protection obligations apply to driver-identifiable inspection records, including agency drivers?
    - Needed: Before release.
    - References: Q45.
23. What environments are required, who provisions them, and what monitoring, incident handling, support hours, and response targets should apply after handover to Calder IT?
    - Needed: Before release.
    - References: Q48, Q47; G16.
24. Who owns the source code, the data, and the hosting accounts, and who deploys before handover?
    - Needed: Before release.
    - References: Q47.
25. Are there architecture, infrastructure, or coding conventions Calder IT expects, given they will operate the system?
    - Needed: Before design or implementation.
    - References: Q29, Q47.

### Design and delivery

26. Is producing the interface design part of the supplier's scope, and is there branding to follow?
    - Needed: Before estimation.
    - References: Q39, Q9; G18.
27. What device support window applies, for example whether Android 11 is the minimum, and are there accessibility or language requirements?
    - Needed: Before design or implementation.
    - References: Q40, Q27.
28. What acceptance criteria would define completion for the agreed first delivery, and what budget range is available?
    - Needed: Before estimation.
    - References: Q51, Q49, Q4; G08.
29. Is the 1 September 2026 go-live date driven by a regulatory or operational commitment, and does it apply to the full rollout or to a first phase?
    - Needed: Before estimation.
    - References: Q49, Q6; G02.
30. Who will test the system, who provides test data, test devices, and Active Directory test accounts, and will drivers take part in yard testing?
    - Needed: Before release.
    - References: Q52.
31. What documentation, training, and handover do you expect, including training for approximately 300 drivers and for changing agency staff?
    - Needed: Before release.
    - References: Q53; G17.
32. What client-provided inputs will be needed from you, and who owns each?
    - Needed: Before estimation.
    - References: Q50.
33. How should scope changes be reviewed and approved once scope is agreed?
    - Needed: Later clarification.
    - References: Q54.

## 9. Document Assumptions and AI Inferences

### Assumptions Explicitly Stated in the Sources

| ID | Assumption | Source / Q IDs | What needs validation |
| --- | --- | --- | --- |
| A1 | D2 assumes depot Wi-Fi coverage is sufficient, and reasons from it that an online-only web application is acceptable: "Devices will operate on the depot Wi-Fi network. The application can therefore be built as a standard online-only web application" | D2, "Connectivity"; Q37, Q40 | The factual coverage where inspections are performed, which D1 contradicts by stating two depots have no usable signal in the vehicle yard |

### AI Inferences Requiring Confirmation

| ID | Inference | Supporting evidence / Q IDs | Why confirmation matters |
| --- | --- | --- | --- |
| I1 | Roughly one inspection per vehicle per working day is implied, giving an order of magnitude of 260 inspections per working day | D1, "Purpose": daily walkaround before leaving the depot, 260 vehicles; Q41 | Drives volume, storage, and retention sizing; neither source states a volume |
| I2 | Inspection volume concentrates in the morning departure window, so peak concurrency is materially higher than average | D1, "Purpose": "before leaving the depot"; Q41, Q42 | Sizing and performance targets depend on it, and no concurrency figure is stated |
| I3 | The offline requirement in D1 implies a device-resident or offline-capable client, whereas D2's online-only position implies a conventional web application | D1, "Non-functional requirements"; D2, "Connectivity"; Q28 | The client application model follows from the connectivity decision, not from a separate stated requirement |
| I4 | Storage volume is a function of the disputed retention period, so the retention conflict is also an infrastructure sizing question | D1 15 months versus D2 7 years; Q41, Q46 | Affects hosting cost under either hosting option |
| I5 | The disagreements follow the authorship split, with compliance-driven requirements in D1 and infrastructure-driven constraints in D2 | D1, "Roles and approval"; D2 header and content; Q5, Q26 | Suggests the conflicts need an arbitration decision rather than a technical recommendation, which is why G01 is treated as the precondition |
| I6 | Active Directory authentication implies the chosen hosting environment must be able to reach the client's directory service | D2, "Platform notes" and "Deployment target"; Q31, Q35, Q44 | Constrains the hosting decision, though neither source states this dependency |

## 10. Overall Understanding

- Project in plain language: a new system for Calder Logistics that lets drivers complete the daily walkaround inspection for a vehicle on an Android handheld, blocks vehicles with safety-critical defects and notifies the depot manager, lets managers record rectification and re-authorise, and gives the compliance team a searchable, exportable record for traffic commissioner audits.
- Clearly defined: the purpose, the three user groups and their goals, the core inspection and defect workflow including the safety-critical blocking rule, the device family, the single-tenant internal scope, and the absence of any existing digital system.
- Still uncertain: four direct contradictions between the two documents, covering first-delivery scope, hosting and data location, offline capability, and retention period, none of which can be resolved from the documents because no precedence is established. Beyond those, driver authentication, checklist and reference data, exception handling, acceptance criteria, and all performance, operational, and delivery arrangements are open.
- Before committing: establish which document governs and who arbitrates, then close the first-delivery, hosting, and connectivity questions and define acceptance criteria. The connectivity question in particular needs a factual statement about yard coverage rather than a preference.
- Review limitations: only the two supplied documents were reviewed. The group IT standard and the group data retention schedule that both documents cite were not supplied, so the hosting and retention contradictions could not be checked against the underlying policies. D2 is labelled v0.3 with no stated approval or draft status, so whether its statements represent a settled client position is unresolved.
