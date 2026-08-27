# Project Understanding Report — Harbour Facilities Services site visit app

Analyzer: project-document-analyzer.md, version 1.1

## 1. Sources and Review Coverage

| Source ID | File / title | Stated version / date | Reviewed content | Limitations |
| --- | --- | --- | --- | --- |
| D1 | case-a-thin-new-project-brief.md — "Enquiry Email — Site visit app" from Priya Nair | Email dated 14 March 2026; no version | Full email text | Short enquiry email only; no specification, appendices, or attachments supplied |

- Review coverage: Complete for supplied sources.
- Source precedence: Not established; only one source supplied.
- Referenced but unavailable material: None identified.

## 2. Project at a Glance

Harbour Facilities Services manages cleaning and maintenance contracts for around 40 commercial buildings. Supervisors currently record site visits on paper checklists that are later retyped into a spreadsheet. Forms are lost and the company cannot readily evidence visits to its clients. The enquiry asks for an app in which a supervisor opens the checklist for a building, ticks items, attaches photos, and submits, with office staff able to view submissions. The source is an enquiry email, so most delivery, technical, and commercial requirements are not yet documented.

| Item | Finding | Evidence / checklist reference |
| --- | --- | --- |
| Project type | New | D1: "We have not built anything like this before, so there is no existing system." See Q3 |
| Classification basis | Source states no existing system is being changed or replaced | D1; Q3 |
| Business objective | Replace paper site-visit checklists to stop losing forms, remove weekly retyping, and evidence visits to clients | D1; Q1 |
| Target users | Supervisors completing checklists; office staff viewing submissions. Whether the building clients receive access is not stated | D1; Q2 |
| Current system status | Not applicable — paper checklists and a spreadsheet, no existing digital system | D1; Q3, Q11 |
| First-release scope | Not specified. Wording suggests an initial trial with a small group of supervisors (inferred, not stated) | D1; Q6 |
| Current technology stack | Not applicable | Q14 |
| Target technology stack | Not stated | Q27 |
| Hosting expectations | Not stated | Q31 |
| Timeline / milestones | Not stated | Q49 |
| Budget / currency | Not stated | Q49 |

## 3. Key Clarifications

Ten most consequential unresolved items, most consequential first. Selection and ordering are analyst judgments about consequence; classifications and timing are reused unchanged from sections 7 and 8.

| Ref | Unresolved item | Question IDs | Decision or activity affected | Classification | Needed by |
| --- | --- | --- | --- | --- | --- |
| G01 | Whether the deliverable is a trial/prototype or a production system used for real client evidence | Q6, Q10 | Estimation and definition of completion | Blocking | Before estimation |
| G02 | Whether inspections must be completable without connectivity, given the stated basement signal problem | Q37, Q40 | Architecture: offline-capable client versus online-only application | Blocking | Before estimation |
| G09 | How building-specific checklists are defined, maintained, and by whom | Q19, Q23 | Implementation of the checklist feature | Blocking | Before design or implementation |
| G11 | Whether producing designs is part of the supplier's scope, since none exist | Q9, Q39 | Estimation of design effort | Blocking | Before estimation |
| G06 | Hosting location, platform, and any technology or licensing restrictions | Q27, Q31, Q32 | Architecture and hosting decisions | Blocking | Before estimation |
| G05 | Authentication method and what each role may access | Q22, Q44 | Implementation of access control | Blocking | Before design or implementation |
| G04 | Measurable acceptance criteria for the first release | Q51 | Commitment to a fixed scope or price | Blocking | Before estimation |
| G03 | Budget, target dates, and any commercial model for the engagement | Q49 | Scheduling and commercial commitment | Non-blocking | Before estimation |
| G07 | Whether the building clients will be given access to visit evidence | Q2, Q30 | Scope sizing and external-access model | Non-blocking | Before design or implementation |
| G10 | Number of supervisors, submission volumes, and photo storage volumes | Q41 | Infrastructure sizing | Non-blocking | Before design or implementation |

- Basis: summarises the gaps register in section 7 and the client question list in section 8; no new findings are introduced here.
- Items not listed above remain in the full gaps register and client question list. One further non-blocking item, G08, is recorded in section 7.

## 4. What Needs to Be Built or Changed

### Modules and Features

| Module / feature | Existing behaviour | Requested work | Users / roles | Phase / priority | Evidence |
| --- | --- | --- | --- | --- | --- |
| Site visit checklist capture | Not applicable — paper carbon checklists | Open the checklist for a building, tick items, submit | Supervisor | Not stated | D1: "open the checklist for a building, tick the items ... and submit" |
| Photo evidence | Not applicable | Attach one or more photos to a visit | Supervisor | Not stated | D1: "add a photo or two" |
| Submitted visit visibility | Not applicable — weekly spreadsheet retyping | Office staff can see submitted visits | Office staff | Not stated | D1: "Office staff should be able to see submitted visits" |

### Main Workflows and Business Rules

One partial workflow is described: a supervisor opens a building's checklist, completes items, adds photos, and submits; office staff then view the submission (D1). What triggers a visit, what happens after submission, and how defects or failed checks are handled are not described. No business rules, validations, calculations, or approval steps are stated (Q23). No exception handling is described (Q24).

### Integrations and Data

Data to be stored is implied by the described features: checklist responses, photos, the building concerned, and the submission itself (Q33). The source of the building list and who maintains it are not stated. No external systems or integrations are mentioned (Q35). The existing spreadsheet is described as current practice; no migration of historical records is requested (Q34).

### Quality, Security, and Operational Requirements

Documented items are limited to the device context: supervisors mostly use their own Android phones, and some buildings are basements with very poor signal, which the source gives as a reason paper has survived (D1). The source does not state this as an offline requirement. No performance, availability, security, privacy, backup, or support requirements are documented.

### Exclusions and Deferred Work

No work is explicitly excluded (Q20). "Nothing fancy for now" indicates restraint in the first release but does not identify which features are deferred (Q8).

## 5. Section-by-Section Answers

### Section 1 — Project Overview

### Q1. What is the project, and what business problem should it solve?
- Answer:
  - Project: a mobile-oriented app for recording site visits, in which a supervisor opens a building's checklist, ticks items, attaches photos, and submits; office staff can view submitted visits.
  - Business problem: paper checklists are lost, results are retyped into a spreadsheet weekly, and clients ask for proof that a visit happened.
- Status: Stated
- Evidence: D1, enquiry email: "we lose forms, and clients keep asking for proof that a visit happened"; "a simple app where a supervisor can open the checklist for a building, tick the items, add a photo or two, and submit".
- Uncertainty / clarification: None.

### Q2. Who are the intended users, and what are their main goals?
- Answer:
  - Supervisors: complete and submit checklists for buildings they visit.
  - Office staff: view submitted visits, replacing weekly manual retyping.
  - Clients of the business are described as asking for proof of visits, but no client access is requested.
- Status: Partially stated
- Evidence: D1: "a supervisor can open the checklist for a building"; "Office staff should be able to see submitted visits"; "clients keep asking for proof that a visit happened".
- Uncertainty / clarification: Whether clients are intended users of the system is unresolved. Office staff goals beyond viewing submissions, such as reporting to clients, are not stated.

### Q3. Is this a new project, an enhancement, an extension, a migration, or a replacement of an existing system?
- Answer: A new project. No existing digital system is in scope; the current process is paper checklists plus a spreadsheet.
- Status: Stated
- Evidence: D1: "We have not built anything like this before, so there is no existing system."
- Uncertainty / clarification: None.

### Q4. What outcomes would make this project successful, and how will they be measured?
- Answer:
  - Stated outcomes: stop using paper, and establish whether supervisors will actually use the app.
  - Implied outcome from the problem statement: being able to evidence visits to clients.
  - No measures, targets, or baselines are stated.
- Status: Partially stated
- Evidence: D1: "we just want to stop using paper and get something in front of a few supervisors to see if they will actually use it".
- Uncertainty / clarification: How adoption or success will be measured is not stated. No measurement of form loss, retyping effort, or client evidence requests is defined.

### Q5. Who are the stakeholders, decision-makers, and final approvers?
- Answer: Priya Nair, Operations Manager, is the stated contact and author. No other stakeholders, decision-makers, or approvers are identified.
- Status: Partially stated
- Evidence: D1, signature block: "Priya Nair, Operations Manager, Harbour Facilities Services".
- Uncertainty / clarification: Whether Priya Nair is the decision-maker and final approver is not stated. Budget authority and any supervisor or office-staff representation are not identified.

### Section 2 — New Project

### Q6. Is the expected deliverable a prototype, proof of concept, MVP, or full production system?
- Answer: Not labelled in the source. The description of putting something in front of a few supervisors to test whether they will use it is consistent with a trial or prototype-style first delivery rather than a full production rollout.
- Status: Inferred
- Evidence: D1: "Nothing fancy for now — we just want to stop using paper and get something in front of a few supervisors to see if they will actually use it."
- Uncertainty / clarification: The source does not use any of these terms. The same wording could describe a limited production pilot. Confirmation is required; recorded as inference I1.

### Q7. Which features are essential for the first release?
- Answer: Open the checklist for a building; tick checklist items; attach one or two photos; submit the visit; office staff view submitted visits.
- Status: Stated
- Evidence: D1: "open the checklist for a building, tick the items, add a photo or two, and submit"; "Office staff should be able to see submitted visits."
- Uncertainty / clarification: None for the list itself. Supporting capabilities such as checklist setup are addressed under Q19.

### Q8. Which features can wait for later phases?
- Answer: No deferred features are identified.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources. "Nothing fancy for now" signals restraint but names no specific deferred feature.
- Uncertainty / clarification: The intended boundary between the first release and later work is unresolved.

### Q9. Are designs, wireframes, reference products, or technical prototypes available?
- Answer: Designs are explicitly not available. Wireframes, reference products, and technical prototypes are not addressed.
- Status: Partially stated
- Evidence: D1: "We do not have designs."
- Uncertainty / clarification: Whether any reference app, sample paper checklist, or existing template exists is unresolved. Whether producing designs falls to the supplier is addressed under Q39.

### Q10. Does the first release need to support real customers and production data?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources. The reference to a few supervisors trying the app does not state whether real site visits and client-facing evidence are in scope for that release.
- Uncertainty / clarification: This determines whether the first release must meet production data-handling, retention, and availability expectations.

### Section 3 — Existing Project

### Q11. Is the existing system live, in development, a demo, or currently unused?
- Answer: Not applicable. There is no existing digital system.
- Status: Not applicable
- Evidence: D1: "there is no existing system." Classification: New (Q3).
- Uncertainty / clarification: None. The current paper and spreadsheet process is described under Q1 and Q34.

### Q12. What functionality already exists, what needs to change, and what is the intended scope of the first release versus later releases?
- Answer: Not applicable for existing functionality. First-release and later-release scope are answered under Q7 and Q8.
- Status: Not applicable
- Evidence: D1: "there is no existing system." Classification: New (Q3).
- Uncertainty / clarification: None; release-scope information is retained under Q7, Q8, and Q49.

### Q13. Why is the change needed: missing features, technical limitations, performance, maintenance, or another reason?
- Answer: Not applicable as an existing-system question. The business motivation is recorded under Q1.
- Status: Not applicable
- Evidence: D1: "there is no existing system." Classification: New (Q3).
- Uncertainty / clarification: None.

### Q14. What is the current technology stack, including versions?
- Answer: Not applicable. No existing digital system, therefore no current stack. Supervisors' Android phones are recorded under Q40.
- Status: Not applicable
- Evidence: D1: "there is no existing system." Classification: New (Q3).
- Uncertainty / clarification: None.

### Q15. Are source code, documentation, environments, and a working demo available, and is access confirmed or merely referenced?
- Answer: Not applicable. There is no existing system to provide access to.
- Status: Not applicable
- Evidence: D1: "there is no existing system." Classification: New (Q3).
- Uncertainty / clarification: None.

### Q16. What existing behaviour, integrations, data, and URLs must be preserved?
- Answer: Not applicable. No existing system, integrations, or URLs are in scope.
- Status: Not applicable
- Evidence: D1: "there is no existing system." Classification: New (Q3).
- Uncertainty / clarification: None. Whether historical spreadsheet records must be carried over is addressed under Q34.

### Q17. Is the proposed approach to extend, refactor, migrate, or rebuild the system?
- Answer: Not applicable. There is no existing system to extend, refactor, migrate, or rebuild.
- Status: Not applicable
- Evidence: D1: "there is no existing system." Classification: New (Q3).
- Uncertainty / clarification: None.

### Q18. Must the old and new systems run alongside each other, and are transition, cutover, downtime, or rollback expectations defined?
- Answer: Not applicable to an existing digital system. No parallel running, cutover, or rollback expectations are stated.
- Status: Not applicable
- Evidence: D1: "there is no existing system." Classification: New (Q3).
- Uncertainty / clarification: How long paper checklists continue in parallel during a trial is not stated and is captured under Q6 and Q10.

### Section 4 — Scope and Business Requirements

### Q19. What modules and features are explicitly included, including any commercial functionality such as pricing, plans, payments, subscriptions, invoicing, or usage limits where the sources describe them?
- Answer:
  - Included: checklist completion against a building, photo attachment, submission, and office-staff visibility of submitted visits.
  - Not addressed: how building-specific checklists are created and maintained, how buildings and supervisors are set up, and any office-staff administration module.
  - Commercial functionality is not raised in the sources; the app is described as internal operational tooling, so no pricing, payment, or subscription features are indicated.
- Status: Partially stated
- Evidence: D1: "open the checklist for a building, tick the items, add a photo or two, and submit"; "Office staff should be able to see submitted visits."
- Uncertainty / clarification: Checklist template definition and building/user setup are unresolved even though the described features depend on them.

### Q20. What is explicitly excluded?
- Answer: Nothing is explicitly excluded.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: "Nothing fancy for now" is a general constraint, not an exclusion, and does not define a scope boundary.

### Q21. What are the main user journeys and business workflows, including triggers, steps, and outcomes?
- Answer:
  - Supervisor journey steps are described: open the building's checklist, tick items, add photos, submit.
  - Office-staff journey is described only as viewing submitted visits.
  - Triggers, such as scheduled visits or contract obligations, and post-submission outcomes are not described.
- Status: Partially stated
- Evidence: D1: "a supervisor can open the checklist for a building, tick the items, add a photo or two, and submit"; "Office staff should be able to see submitted visits."
- Uncertainty / clarification: What initiates a visit, what happens when a check fails, and what office staff do with a submission are unresolved.

### Q22. What user roles exist, and what can each role access or perform?
- Answer: Two roles are indicated, supervisor and office staff. Their permitted actions are described only at the level of Q21; no access boundaries are defined.
- Status: Partially stated
- Evidence: D1: references to "a supervisor" and "Office staff".
- Uncertainty / clarification: Whether supervisors can view others' submissions, whether office staff can edit or delete submissions, and whether an administrator role exists are unresolved.

### Q23. What business rules, validations, calculations, and approval processes apply, including any pricing, discount, tax, refund, or proration rules where charging is in scope?
- Answer: No business rules, validations, calculations, or approval processes are stated. Charging is not in scope in the sources, so pricing rules are not raised.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Whether all checklist items are mandatory, whether photos are required, whether submissions can be edited after submission, and whether any approval follows submission are unresolved.

### Q24. Are exceptions covered, such as rejection, cancellation, duplicate submissions, and failed operations?
- Answer: No exception handling is described.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Behaviour for incomplete visits, duplicate submissions for the same building and day, and failed submissions is unresolved. This interacts with the connectivity question under Q40.

### Q25. Are administration, reporting, search, notifications, imports, and exports required, and what is specified for each?
- Answer:
  - Viewing submitted visits by office staff is stated.
  - Administration, reporting, search, notifications, imports, and exports are not specified.
- Status: Partially stated
- Evidence: D1: "Office staff should be able to see submitted visits."
- Uncertainty / clarification: The current weekly spreadsheet is described as existing practice, not as a required export. Whether clients need reports or evidence packs is unresolved, and relates to Q2.

### Q26. Are any requirements contradictory, ambiguous, or dependent on unstated assumptions?
- Answer:
  - No contradictions were found; only one source was supplied.
  - Ambiguity: "Nothing fancy for now" does not define a scope boundary.
  - Ambiguity: the intended deliverable type is not stated (Q6).
  - Unstated dependency: the described features depend on building-specific checklists existing in the system, but their creation and maintenance are not described (Q19).
  - Unstated dependency: poor signal in basement locations is presented as background, yet the requested workflow occurs in those locations, so connectivity behaviour is an unresolved dependency (Q40).
- Status: Inferred
- Evidence: D1: "Nothing fancy for now"; "Some of the buildings are basements with very poor signal, which is one reason the paper forms have survived this long."
- Uncertainty / clarification: This answer is an analyst observation grounded in the quoted text, not a statement made by the client.

### Section 5 — Technology and Architecture

### Q27. What target technology stack is mandatory, preferred, proposed, or explicitly open for recommendation, including versions where stated?
- Answer: No target stack is stated as mandatory, preferred, proposed, or open.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: The reference to supervisors' Android phones constrains the client platform but does not state a stack. Whether the client expects to choose or delegate this decision is unresolved.

### Q28. Which platforms are required: web, mobile, desktop, APIs, or background services?
- Answer:
  - A mobile experience for supervisors is indicated, since supervisors use Android phones and the request is for an "app".
  - The office-staff access channel is not specified.
  - APIs and background services are not addressed.
- Status: Partially stated
- Evidence: D1: "a simple app where a supervisor can open the checklist"; "Supervisors mostly use their own Android phones."
- Uncertainty / clarification: Whether "app" means a native Android application or a mobile web application is not stated, and neither is the office-staff platform.

### Q29. Are there existing architecture standards, reusable components, or coding conventions to follow?
- Answer: None stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Whether the client has any IT standards or an internal IT function is unresolved.

### Q30. Is the system for one organisation or multiple tenants/customers, and are isolation or customer-specific configuration requirements defined?
- Answer: The described users all belong to Harbour Facilities Services, which points to a single-organisation system. Building-level configuration is implied by building-specific checklists. No isolation or tenancy requirements are stated.
- Status: Inferred
- Evidence: D1: users are described as "our supervisors" and "Office staff" of Harbour Facilities Services; buildings are managed under client contracts.
- Uncertainty / clarification: If the building clients are later given access to visit evidence, external-access and data-separation requirements would arise. Recorded as inference I2 and gap G07.

### Q31. Where must it run: cloud, on-premises, or a hybrid environment?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: No hosting preference, existing tenant, or data-location expectation is documented.

### Q32. Are there restrictions on hosting, licensing, third-party services, data location, or technology choices?
- Answer: None stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Whether any client contract imposes data-location or evidence-retention constraints is unresolved.

### Section 6 — Data and Integrations

### Q33. What data must the system store, where does it originate, and which systems are authoritative for shared data?
- Answer:
  - Implied data: checklist responses, attached photos, the building concerned, the submitting supervisor, and the submission date.
  - Origin: entered by supervisors on site.
  - The authoritative source of the building list, contract data, and supervisor records is not stated.
- Status: Partially stated
- Evidence: D1: "open the checklist for a building, tick the items, add a photo or two, and submit".
- Uncertainty / clarification: Whether building and staff data comes from an existing system, the spreadsheet, or manual entry is unresolved.

### Q34. Is existing data migration required, including cleaning, mapping, and validation?
- Answer: No migration is requested. A spreadsheet of retyped visit results exists in the current process, but the source does not ask for historical records to be loaded.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources regarding migration. Context: D1 describes "someone types them into a spreadsheet at the end of the week".
- Uncertainty / clarification: Whether historical visit records must be retained, imported, or left in the spreadsheet is unresolved.

### Q35. Which external systems, APIs, or services must be integrated, and what is each integration expected to do?
- Answer: No integrations are mentioned.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Whether the app must feed the existing spreadsheet, a client portal, or any accounting or scheduling system is unresolved.

### Q36. Are integration documentation, sandbox access, credentials provisioning, and vendor support available or assigned to an owner?
- Answer: Not addressed. No integrations have been identified, and the source does not state that none are required.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: This cannot be resolved as Not applicable, because the absence of integrations is silence rather than a documented exclusion. Depends on Q35.

### Q37. Does data need to sync in real time, on a schedule, or manually, and in which direction?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: The description of very poor signal in basement buildings makes device-to-server synchronisation timing a material unknown, but the source states no requirement. See Q40 and gap G02.

### Q38. What should happen when an integration fails, times out, returns incomplete data, or delivers duplicate events?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Depends on Q35 and Q37. Submission failure behaviour on a device with no signal is a related unresolved point.

### Section 7 — Design and Quality Requirements

### Q39. Are approved designs, branding, and content available, or is producing them part of the scope, and are ownership or licensing terms stated for client-supplied assets such as designs, fonts, imagery, copy, and third-party components?
- Answer:
  - Designs are explicitly not available.
  - Whether producing designs is part of the supplier's scope is not stated.
  - Branding assets, checklist content, ownership, and licensing of any supplied assets are not addressed.
- Status: Partially stated
- Evidence: D1: "We do not have designs."
- Uncertainty / clarification: Design ownership drives estimation (gap G11). Checklist content, which is the core of the app, is not supplied or described. Asset licensing is not raised in the sources.

### Q40. What devices, browsers, languages, and accessibility requirements must be supported, including any stated locales, regional date/number/currency formats, time-zone handling, and offline or intermittent-connectivity expectations?
- Answer:
  - Devices: supervisors mostly use their own Android phones. Device models, Android versions, and any bring-your-own-device policy are not stated.
  - Connectivity: some buildings are basements with very poor signal, given as a reason paper has persisted. The source does not state an offline requirement.
  - Browsers, languages, locale or regional formats, time zones, and accessibility requirements are not stated.
- Status: Partially stated
- Evidence: D1: "Supervisors mostly use their own Android phones. Some of the buildings are basements with very poor signal, which is one reason the paper forms have survived this long."
- Uncertainty / clarification: Connectivity is an applicable unresolved part because the sources raise it and the described workflow happens in those locations. Locale and regional formatting are not raised and are recorded as not raised rather than as requirements. Office-staff browser support is unresolved.

### Q41. What user numbers, concurrent usage, data volumes, and growth are expected?
- Answer:
  - Approximately 40 commercial buildings are under management.
  - The first group is described only as "a few supervisors"; total supervisor and office-staff numbers are not stated.
  - Concurrency, visit frequency, photo volumes, and growth are not stated.
- Status: Partially stated
- Evidence: D1: "We manage cleaning and maintenance contracts for about 40 commercial buildings"; "get something in front of a few supervisors".
- Uncertainty / clarification: Photo storage volume is unresolved and depends on visit frequency and photos per visit.

### Q42. Are performance, availability, and acceptable downtime targets defined?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: No response-time, availability, or maintenance-window expectations are documented.

### Q43. Are SEO, analytics, audit history, or activity tracking required, and what is specified for each?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: The stated need to prove that a visit happened suggests audit or evidence expectations, but no tracking requirement is documented. Related to Q1 and Q43 clarifications.

### Section 8 — Security and Operations

### Q44. What authentication, permissions, and account-management requirements apply?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: How supervisors sign in on personal devices, how accounts are issued and revoked, and whether agency or temporary staff are included are unresolved. See gap G05.

### Q45. Will the system handle sensitive information, and what privacy or compliance requirements are explicitly identified?
- Answer: No privacy or compliance requirements are identified in the source.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: The described data includes named staff activity and site photographs, which may raise privacy considerations, but the source states no requirement and none is assumed here.

### Q46. What backup, recovery, data retention, and deletion requirements exist?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Retention of visit evidence may be governed by client contracts; this is unresolved.

### Q47. Who will own or control hosting accounts, source code, data, deployments, third-party subscriptions and licences, and ongoing maintenance?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Ownership of any app-store accounts, if a native Android app is chosen, is also unresolved and depends on Q28.

### Q48. What environments, deployment process, monitoring, incident handling, and support arrangements are required, including any stated support hours, response or resolution targets, and escalation paths?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Support hours and response targets are not raised in the sources and are recorded as not raised, not as requirements. Environment and deployment expectations are unresolved.

### Section 9 — Delivery and Acceptance

### Q49. What are the budget, target dates, milestones, delivery priorities, release expectations, and any stated engagement commercial model?
- Answer: None stated. No budget, dates, milestones, priorities, or commercial model appear in the source.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: "Nothing fancy for now" implies urgency or cost sensitivity but states no constraint. In-product billing is not in scope (Q19).

### Q50. What dependencies or client-provided inputs could delay delivery, and who is responsible for them?
- Answer: No dependencies or inputs are identified. The source instead asks what the supplier would need.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources. Context: D1: "Could you let us know what you would need from us to move forward?"
- Uncertainty / clarification: Checklist content, building data, and supervisor access to devices are likely client inputs, but none are documented or assigned an owner.

### Q51. What measurable acceptance criteria define completion?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Whether supervisor adoption is intended as an acceptance measure is unresolved; the source frames it as a purpose rather than a criterion (Q4).

### Q52. Who will test and approve the deliverables, what testing or sign-off process is specified, and who provides test data, test accounts, and UAT environment access?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources. The reference to putting the app in front of a few supervisors is described as an adoption trial, not a testing or sign-off process.
- Uncertainty / clarification: Test data ownership and UAT arrangements are not raised in the sources and are recorded as not raised.

### Q53. What documentation, training, handover, and post-launch support are expected?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: Supervisor training needs may be material given a move from paper, but nothing is documented.

### Q54. How will scope changes be reviewed and approved?
- Answer: Not stated.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources.
- Uncertainty / clarification: No change process or approver is identified; related to the unresolved approver question under Q5.

### Section 10 — Document Completeness and Analyst Conclusions

### Q55. How many Q1–Q54 answers are Stated, Partially stated, Inferred, Missing, Conflicting, and Not applicable?
- Answer: Stated 3; Partially stated 13; Inferred 3; Missing 27; Conflicting 0; Not applicable 8. Total 54. See the dashboard in section 6.
- Basis: The tally reproduces the statuses assigned in section 5 without reclassification.
- Limitations: The high Missing count reflects that the only source is a short enquiry email, not that the client has no requirements.

### Q56. Are referenced attachments, diagrams, designs, and supporting documents available and reviewed?
- Answer: No attachments, diagrams, or supporting documents are referenced or supplied. The source explicitly states designs do not exist. The email text was reviewed in full.
- Basis: Source inventory in section 1; D1: "We do not have designs."
- Limitations: None affecting reading coverage. The paper checklist used today is central to the requested app but was not supplied and is not referenced as an attachment.

### Q57. Which unresolved items block specific estimation, architecture, implementation, acceptance, or release decisions, and why?
- Answer: Seven items are classified Blocking: G01 deliverable type, G02 connectivity behaviour, G09 checklist definition, G11 design ownership, G06 hosting and technology constraints, G05 authentication and permissions, and G04 acceptance criteria. Each blocked activity and dependency is set out in section 7.
- Basis: Gaps register in section 7, derived from the statuses in section 5.
- Limitations: Blocking classification reflects the activities named in the register. Items not listed are recorded as non-blocking with the point at which they become necessary.

### Q58. What must be clarified before committing to scope, cost, or timelines, and what can be resolved later?
- Answer: Before any scope or cost commitment: deliverable type and production expectations (G01), connectivity behaviour (G02), checklist definition and maintenance (G09), design ownership (G11), hosting and technology constraints (G06), and acceptance criteria (G04). Access control (G05) is required before design or implementation. Resolvable later: budget and dates for scheduling purposes (G03), client access (G07), retention and privacy expectations (G08), and volume figures (G10). Questions are listed in section 8.
- Basis: Gaps register and client question list.
- Limitations: This is an analyst view of sequencing based on one enquiry email; the client may hold answers that were simply not written down.

## 6. Answer Coverage Dashboard

| Status | Number of questions | Question IDs |
| --- | --- | --- |
| Stated | 3 | Q1, Q3, Q7 |
| Partially stated | 13 | Q2, Q4, Q5, Q9, Q19, Q21, Q22, Q25, Q28, Q33, Q39, Q40, Q41 |
| Inferred | 3 | Q6, Q26, Q30 |
| Missing | 27 | Q8, Q10, Q20, Q23, Q24, Q27, Q29, Q31, Q32, Q34, Q35, Q36, Q37, Q38, Q42, Q43, Q44, Q45, Q46, Q47, Q48, Q49, Q50, Q51, Q52, Q53, Q54 |
| Conflicting | 0 | None |
| Not applicable | 8 | Q11, Q12, Q13, Q14, Q15, Q16, Q17, Q18 |
| Total | 54 | Q1–Q54 |

- Applicable questions: 46.
- Reading limitations affecting the tally: None. The supplied source was read in full; its brevity, not reading coverage, explains the Missing count.

## 7. Gaps, Conflicts, and Dependencies

### Blocking Items

| ID | Finding | Related Q IDs / sources | Decision or activity blocked | Why it is blocked | Clarification needed |
| --- | --- | --- | --- | --- | --- |
| G01 | Deliverable type and production expectations are not stated | Q6, Q10; D1 | Estimation and definition of completion | A supervisor trial and a production system used as client evidence differ materially in security, retention, availability, and effort | Whether the first release is a trial or must support real visits and client-facing evidence |
| G02 | Connectivity behaviour is undefined although basement sites with very poor signal are described | Q37, Q40; D1 | Architecture decision on offline capability | Offline capture with later synchronisation changes the client architecture, conflict handling, and effort | Whether supervisors must be able to complete and submit visits with no signal |
| G09 | Checklist definition, per-building variation, and maintenance responsibility are not described | Q19, Q23; D1 | Implementation of the core checklist feature | Checklists are the central artefact; without their structure and ownership the feature cannot be built | Who defines checklists, whether they differ per building or contract, and who maintains them |
| G11 | Designs do not exist and design ownership is unstated | Q9, Q39; D1 | Estimation of design and content effort | Design and checklist content production may or may not sit with the supplier, which changes scope | Whether the supplier produces designs and checklist content |
| G06 | Hosting, platform, and technology or licensing constraints are absent | Q27, Q31, Q32; D1 | Architecture and hosting decisions | No stack, hosting location, or constraint is known, and native versus web delivery is also open | Any IT constraints, hosting preferences, and whether a native Android app is expected |
| G05 | Authentication, permissions, and account management are undefined | Q22, Q44; D1 | Implementation of access control | Roles are named but their permissions and sign-in method are unknown; personal devices add account-management questions | Sign-in method, per-role permissions, and account issue and revocation process |
| G04 | No measurable acceptance criteria | Q51, Q4; D1 | Commitment to a fixed scope or price | Completion cannot be evidenced or agreed without criteria | What must be demonstrated for the first release to be accepted |

### Non-blocking Clarifications

| ID | Finding | Related Q IDs / sources | Why it matters | When needed |
| --- | --- | --- | --- | --- |
| G03 | Budget, dates, milestones, and commercial model are not stated | Q49; D1 | Affects sequencing and commercial commitment; an estimate can still be prepared from clarified scope | Before estimation is presented for approval |
| G07 | Whether building clients receive access to visit evidence is unresolved | Q2, Q30; D1 | Client-facing access would add scope and an external-access model | Before design or implementation |
| G08 | Retention, privacy, and deletion expectations for visit records and site photos are absent | Q45, Q46; D1 | Evidence value depends on retention; site photos and named staff activity may carry privacy considerations | Before release |
| G10 | Supervisor numbers, visit frequency, and photo volumes are not stated | Q41; D1 | Needed for storage and infrastructure sizing | Before design or implementation |

### Documented Dependencies

| Dependency | Documented owner | Required timing | Evidence |
| --- | --- | --- | --- |
| None identified in the reviewed sources | — | — | D1 asks the supplier what inputs are required rather than committing to any |

Analyst note: the items above are analyst observations about what the documents leave unresolved. Classification indicates which activity is presently blocked, not a prediction that a problem will occur.

## 8. Open Questions for the Client

### Deliverable and release

1. Is the first release intended as a trial with a small group of supervisors, or must it support real site visits and serve as client-facing evidence from day one?
   - Needed: Before estimation.
   - References: Q6, Q10; G01.
2. Which capabilities are needed in the first release, and which are you content to defer?
   - Needed: Before estimation.
   - References: Q7, Q8, Q20.
3. What would you need to see for the first release to be considered complete and acceptable?
   - Needed: Before estimation.
   - References: Q4, Q51; G04.

### Checklists and workflow

4. Who defines the checklists, do they vary by building or contract, and who will maintain them once the app is live?
   - Needed: Before design or implementation.
   - References: Q19, Q23; G09.
5. Could you share an example of a current paper checklist?
   - Needed: Before design or implementation.
   - References: Q9, Q39, Q56.
6. What should happen after a supervisor submits a visit, and what should happen when a check fails or an item cannot be completed?
   - Needed: Before design or implementation.
   - References: Q21, Q23, Q24.
7. What do office staff need to do with submissions beyond viewing them, for example search, report, or send evidence to a client?
   - Needed: Before design or implementation.
   - References: Q25, Q43.

### Devices and connectivity

8. Must supervisors be able to complete and submit a visit with no signal, with records sent once the device reconnects?
   - Needed: Before estimation.
   - References: Q37, Q40; G02.
9. Which Android devices and versions are in use, and are these personal devices or company-issued?
   - Needed: Before design or implementation.
   - References: Q40, Q44.
10. How will office staff use the system, on a desktop browser or on mobile?
    - Needed: Before design or implementation.
    - References: Q28.

### Users and access

11. How should supervisors and office staff sign in, and who is responsible for creating and removing accounts?
    - Needed: Before design or implementation.
    - References: Q22, Q44; G05.
12. Will the buildings' clients ever need access to visit records, either now or later?
    - Needed: Before design or implementation.
    - References: Q2, Q30; G07.

### Technology, hosting, and data

13. Are there any IT constraints on hosting, technology choices, or third-party services, and do you have a preferred hosting arrangement?
    - Needed: Before estimation.
    - References: Q27, Q31, Q32; G06.
14. Where should building, contract, and supervisor data come from, and does anything need to be carried over from the current spreadsheet?
    - Needed: Before design or implementation.
    - References: Q33, Q34.
15. Does the app need to connect to any other system you use, such as scheduling, client reporting, or accounting?
    - Needed: Before design or implementation.
    - References: Q35, Q36, Q37.
16. How long must visit records and photos be kept, and do any client contracts set retention or privacy obligations?
    - Needed: Before release.
    - References: Q45, Q46; G08.

### Volumes and operations

17. How many supervisors and office staff will use the system, how many visits are recorded per week, and how many photos would you expect per visit?
    - Needed: Before design or implementation.
    - References: Q41; G10.
18. What environments, ongoing support, and training would you expect after launch, and who will own the hosting accounts and data?
    - Needed: Later clarification.
    - References: Q47, Q48, Q53.

### Commercial and governance

19. What budget range and target dates are you working towards?
    - Needed: Before estimation.
    - References: Q49; G03.
20. Who approves scope and cost decisions, and who would sign off the delivered app?
    - Needed: Before estimation.
    - References: Q5, Q52, Q54.

## 9. Document Assumptions and AI Inferences

### Assumptions Explicitly Stated in the Sources

| ID | Assumption | Source / Q IDs | What needs validation |
| --- | --- | --- | --- |
| None identified in the reviewed sources | — | — | — |

### AI Inferences Requiring Confirmation

| ID | Inference | Supporting evidence / Q IDs | Why confirmation matters |
| --- | --- | --- | --- |
| I1 | The first delivery is intended as a trial or prototype rather than a full production rollout | D1: "get something in front of a few supervisors to see if they will actually use it"; Q6 | Determines security, retention, availability, and effort expectations |
| I2 | The system serves one organisation, Harbour Facilities Services | D1: users described as the company's supervisors and office staff; Q30 | Client-facing access would introduce external-access and data-separation requirements |
| I3 | A mobile experience for supervisors is required | D1: "a simple app"; "Supervisors mostly use their own Android phones"; Q28 | Native versus mobile web delivery affects architecture, app-store ownership, and effort |
| I4 | Checklist responses, photos, building identity, supervisor identity, and submission date are the core stored data | D1 feature description; Q33 | The data model shapes storage, retention, and reporting scope |
| I5 | Basement signal problems are relevant to the requested workflow, not only to the history of paper forms | D1: "Some of the buildings are basements with very poor signal"; Q26, Q40 | Drives the offline-capability decision in G02 |

## 10. Overall Understanding

- Project in plain language: a new app for Harbour Facilities Services that lets supervisors complete a building's site-visit checklist on a phone with photo evidence, and lets office staff see submissions, replacing paper forms and weekly retyping.
- Clearly defined: the business problem, the core first-release features, the fact that no existing system or designs exist, and the Android phone context.
- Still uncertain: whether this is a trial or a production system, how checklists are defined and maintained, whether offline capture is required, design ownership, hosting and technology constraints, access control, acceptance criteria, budget, and dates.
- Before committing: resolve the seven blocking items in section 7, particularly deliverable type, connectivity behaviour, checklist definition, and design ownership.
- Review limitations: the analysis rests on a single short enquiry email. The large number of Missing answers reflects the document, not a judgment about the client's readiness.
