# Project Understanding Report — Northwind Institute of Surveyors, replacement of the MemberDesk portal

Analyzer: project-document-analyzer.md, version 1.1

## 1. Sources and Review Coverage

| Source ID | File / title | Stated version / date | Reviewed content | Limitations |
| --- | --- | --- | --- | --- |
| D1 | case-b-existing-system-replacement-brief.md — Statement of Requirements, reference SOR-2026-011 | Version 1.2, dated 2 February 2026 | Sections 1 to 8 in full | Appendix C is referenced in section 6 and listed in section 8 but was not supplied; its content was not reviewed |

- Review coverage: Partial. The supplied document was read in full, but its own Appendix C is missing.
- Source precedence: Not established; only one source supplied.
- Referenced but unavailable material: Appendix C, learning platform SSO integration notes (D1 section 6 and the appendix heading). Also referenced but not supplied: the GitLab repository, the staging environment, brand assets, and Stripe and Sage sandbox credentials, all of which D1 section 8 lists as client-provided inputs.

## 2. Project at a Glance

Northwind Institute of Surveyors wants to replace MemberDesk, its live member portal, with a new portal on a supported platform. The current system runs on ASP.NET Web Forms and .NET Framework 4.5 with SQL Server 2012 on a single on-premises VM, is out of support, has lost its maintaining contractor, and fails under load during the January renewal peak. Around 18,000 members in the UK and Ireland plus 22 staff use it. The requested work is a full rebuild on .NET 8 or later hosted in Azure, preserving member data, the `/members/directory/{id}` URLs, the Sage export, and SSO to the learning platform, with both portals running in parallel for roughly two months.

| Item | Finding | Evidence / checklist reference |
| --- | --- | --- |
| Project type | Existing | D1 section 1: objective is "to replace MemberDesk with a new portal on a supported platform". See Q3 |
| Classification basis | A live system is being replaced; a replacement remains an Existing project even though the codebase will be new | D1 sections 1–3; Q3, Q17 |
| Business objective | Move off an unsupported platform, remove contractor dependency, and let members renew and access CPD records without calling the office | D1 section 1; Q1, Q13 |
| Target users | Approximately 18,000 members (UK and Ireland) and 22 internal staff, in member, staff administrator, and finance roles | D1 sections 1 and 5; Q2, Q22 |
| Current system status | Live and used daily | D1 section 1; Q11 |
| First-release scope | Full production replacement of the listed functions in a single launch. The document does not describe a phased functional release | D1 sections 3, 4, 8; Q12, Q49 |
| Current technology stack | ASP.NET Web Forms on .NET Framework 4.5, SQL Server 2012, Windows Server 2012 R2 VM, on premises in London | D1 section 2; Q14 |
| Target technology stack | Required: .NET 8 or later and SQL Server, per client IT policy | D1 section 6; Q27 |
| Hosting expectations | Azure, under the client's existing tenant | D1 section 6; Q31 |
| Timeline / milestones | Design sign-off 30 April 2026; member-facing functions in UAT 31 August 2026; launch before 1 November 2026; cutover must avoid the January renewal peak | D1 sections 3 and 8; Q49 |
| Budget / currency | £180,000 excluding VAT, fixed price | D1 section 8; Q49 |

## 3. Key Clarifications

Ten most consequential unresolved items, most consequential first. Selection and ordering are analyst judgments about consequence; classifications and timing are reused unchanged from sections 7 and 8.

| Ref | Unresolved item | Question IDs | Decision or activity affected | Classification | Needed by |
| --- | --- | --- | --- | --- | --- |
| G01 | Appendix C, the learning platform SSO integration notes, was not supplied | Q35, Q36 | Estimation and implementation of the SAML SSO integration | Blocking | Before estimation |
| G02 | Duplicate member records are acknowledged but cleansing and mapping rules are "to be agreed", while acceptance requires migrated records to reconcile to current database counts | Q34, Q51, Q26 | Migration design and the migration acceptance test | Blocking | Before estimation |
| G07 | How the existing 18,000 member credentials are carried into the new portal is not described | Q44, Q34 | Implementation of authentication and the migration cutover | Blocking | Before design or implementation |
| G04 | The mechanism for the monthly EUR rate used to invoice Irish members is not defined | Q23, Q40 | Implementation of renewal pricing and invoicing | Blocking | Before design or implementation |
| G06 | Cutover downtime tolerance and rollback trigger criteria are not defined, although a rollback plan is required | Q18 | Release and cutover planning | Blocking | Before release |
| G03 | Client-provided inputs are owned by an IT team stated to be at capacity until mid-April, against a 30 April design sign-off milestone | Q50, Q49, Q15 | Commitment to the milestone dates | Blocking | Before estimation |
| G13 | Failure, timeout, and duplicate-event handling for the Sage export and Stripe confirmations is not specified | Q38, Q37 | Implementation of the integrations | Non-blocking | Before design or implementation |
| G11 | Notifications, including renewal reminders and payment failure messages, are not specified | Q25, Q23 | Scope of the renewal module | Non-blocking | Before design or implementation |
| G05 | Whether the "Trelawney Sans" web licence extends to this portal is stated as unconfirmed | Q39 | Design sign-off and front-end asset use | Non-blocking | Before design or implementation |
| G10 | Recovery objectives are not stated, although nightly backups and 30-day retention are | Q46 | Backup and recovery design | Non-blocking | Before release |

- Basis: summarises the gaps register in section 7 and the client question list in section 8; no new findings are introduced here.
- Items not listed above remain in the full gaps register and client question list. A further eight non-blocking items, G08, G09, G12, G14, G15, G16, G17, and G18, are recorded in section 7.

## 4. What Needs to Be Built or Changed

### Modules and Features

| Module / feature | Existing behaviour | Requested work | Users / roles | Phase / priority | Evidence |
| --- | --- | --- | --- | --- | --- |
| Member login and account management | Member login exists | Rebuild, with MFA mandatory for staff accounts | Member, staff administrator | In scope | D1 sections 2, 4, 7 |
| Annual renewal with payment | Annual renewal with card payment exists; renewal page fails under January load | Rebuild with Stripe, fee rules, VAT invoice, and one retry on failure | Member, staff administrator, finance | In scope; must sustain 900 concurrent users | D1 sections 2, 4, 5, 7 |
| CPD activity log | CPD activity log exists | Rebuild, with evidence upload | Member | In scope | D1 sections 2, 4 |
| Member directory | Member directory exists at `/members/directory/{id}` | Rebuild; existing URLs must continue to resolve | Member | In scope | D1 sections 2, 3, 4 |
| Certificate generation | Downloadable certificates exist | Rebuild | Member | In scope | D1 sections 2, 4 |
| Staff admin area | Staff admin area exists | Rebuild, including manual renewals | Staff administrator | In scope | D1 sections 2, 4, 5 |
| Membership reporting | Not described in the current system | Provide membership reporting | Staff administrator, finance | In scope | D1 section 4 |
| Data migration | Data in SQL Server 2012 | Migrate member records, CPD history since 2016, and payment history | Not applicable | In scope; cleansing rules to be agreed | D1 sections 4, 6 |
| Automated CPD assessment | Does not exist | Possible later phase, not in this engagement | Not stated | Excluded from this engagement | D1 section 4 |

### Main Workflows and Business Rules

Renewal is the dominant documented workflow. Stated rules: the standard annual fee is £245; retired members pay 40% of the standard fee; members joining after 1 October pay a pro-rated fee for the remainder of the year; VAT at the prevailing UK rate applies and must appear on the invoice; a failed payment is retried once after 72 hours; a renewal is complete only when payment is confirmed; refunds are handled manually by Finance and are outside the portal (D1 section 5). Irish members are invoiced in EUR at a rate set monthly by Finance (D1 section 7), but the mechanism for capturing and applying that rate is not described. Other workflows named without step-level detail are CPD logging with evidence upload, certificate download, staff manual renewal, and membership reporting.

### Integrations and Data

Three integrations are required: Stripe for card payments, with confirmations reflected immediately; a nightly Sage accounting export that must keep working; and SAML SSO to the learning platform, which must also keep working. Stripe and Sage sandbox credentials are to be provided by the client IT team; the SSO notes sit in the unsupplied Appendix C. Migration covers member records, CPD history since 2016, and payment history from SQL Server 2012, with known duplicate member records and cleansing rules still to be agreed. Card data must not be stored by the client.

### Quality, Security, and Operational Requirements

Documented: current Chrome, Edge, and Safari on desktop and mobile; English only; GBP with EUR invoicing for Irish members; DD/MM/YYYY dates; WCAG 2.1 AA; 18,000 members with up to 900 concurrent users at the January peak; renewal pages responding within 2 seconds at peak; 99.5% monthly availability excluding an agreed monthly maintenance window; audit history of staff actions on member records; email and password authentication with mandatory MFA for staff; UK GDPR; no card data stored; nightly backups with 30-day retention; deletion on request; development, staging, and production environments; support 08:00–18:00 UK time on business days with a 4-hour response target for renewal-blocking issues and next-business-day response otherwise.

### Exclusions and Deferred Work

Explicitly out of scope: the public marketing website, the events booking system, and any mobile app (D1 section 4). Named as a possible later phase and not part of this engagement: automated CPD assessment (D1 section 4). Named as a second-phase integration in the addendum context of this document set: none. Refunds remain a manual Finance process outside the portal (D1 section 5).

## 5. Section-by-Section Answers

### Section 1 — Project Overview

### Q1. What is the project, and what business problem should it solve?
- Answer:
  - Project: replace the live MemberDesk member portal with a new portal on a supported platform, preserving member data and public URLs.
  - Business problem: the 2014 platform is out of support, the maintaining contractor left in 2025, the renewal page fails under load every January, and members call the office to renew and access CPD records.
- Status: Stated
- Evidence: D1 section 1: "The objective is to replace MemberDesk with a new portal on a supported platform, without losing member data or public URLs"; D1 section 2: "the platform is out of support, the contractor has gone, and the renewal page fails under load every January".
- Uncertainty / clarification: None.

### Q2. Who are the intended users, and what are their main goals?
- Answer:
  - Approximately 18,000 members across the UK and Ireland: renew membership, log CPD activity, download certificates, use the member directory, manage their own profile.
  - 22 internal staff: administer membership without contractor involvement, process manual renewals, run reports.
  - Finance users: read-only access to payment records and exports.
- Status: Stated
- Evidence: D1 section 1: "used daily by approximately 18,000 members across the United Kingdom and Ireland, plus 22 internal staff"; D1 section 5 role list.
- Uncertainty / clarification: None.

### Q3. Is this a new project, an enhancement, an extension, a migration, or a replacement of an existing system?
- Answer: A replacement of an existing live system, delivered as a full rebuild.
- Status: Stated
- Evidence: D1 section 1: "replace MemberDesk with a new portal"; D1 section 3: "We want a full rebuild, not a refactor."
- Uncertainty / clarification: None.

### Q4. What outcomes would make this project successful, and how will they be measured?
- Answer: Members can renew and access CPD records without calling the office, measured by a reduction in renewal-related support calls of at least 30% in the first renewal cycle after launch. Staff can administer membership without contractor involvement.
- Status: Stated
- Evidence: D1 section 1: "a reduction in renewal-related support calls of at least 30% in the first renewal cycle after launch".
- Uncertainty / clarification: The current baseline call volume is not given in the document, but the measure itself is defined.

### Q5. Who are the stakeholders, decision-makers, and final approvers?
- Answer: Daniel Okafor, Head of Digital, is project owner and document author; Fiona Grant, Finance Director, approves spend; the Chief Executive holds final launch sign-off. The Membership Manager leads client testing.
- Status: Stated
- Evidence: D1 section 1: "Decision-makers: Daniel Okafor (project owner) and the Finance Director, Fiona Grant, who approves spend. Final sign-off for launch sits with the Chief Executive."; D1 section 8 names the Membership Manager as test lead.
- Uncertainty / clarification: None.

### Section 2 — New Project

### Q6. Is the expected deliverable a prototype, proof of concept, MVP, or full production system?
- Answer: Not applicable under Existing-project routing. Release expectations are recorded under Q12 and Q49: a full production replacement of a live portal, launching before 1 November 2026.
- Status: Not applicable
- Evidence: Classification Existing (Q3), based on D1 section 1 and section 3.
- Uncertainty / clarification: None; release information is retained under Q12 and Q49.

### Q7. Which features are essential for the first release?
- Answer: Not applicable under Existing-project routing. The in-scope function list for the single launch is recorded under Q12 and Q19.
- Status: Not applicable
- Evidence: Classification Existing (Q3); in-scope list at D1 section 4.
- Uncertainty / clarification: None.

### Q8. Which features can wait for later phases?
- Answer: Not applicable under Existing-project routing. Deferred and excluded work is recorded under Q12 and Q20, including automated CPD assessment as a possible later phase.
- Status: Not applicable
- Evidence: Classification Existing (Q3); D1 section 4.
- Uncertainty / clarification: None.

### Q9. Are designs, wireframes, reference products, or technical prototypes available?
- Answer: Not applicable under Existing-project routing. Design availability is recorded under Q39: brand guidelines and logos will be supplied, page designs do not exist and are expected from the supplier.
- Status: Not applicable
- Evidence: Classification Existing (Q3); D1 section 7.
- Uncertainty / clarification: None.

### Q10. Does the first release need to support real customers and production data?
- Answer: Not applicable under Existing-project routing. The equivalent finding is recorded under Q12: the release replaces a live portal serving approximately 18,000 members, with migrated production data.
- Status: Not applicable
- Evidence: Classification Existing (Q3); D1 sections 1, 3, 4.
- Uncertainty / clarification: None.

### Section 3 — Existing Project

### Q11. Is the existing system live, in development, a demo, or currently unused?
- Answer: Live and used daily by approximately 18,000 members and 22 internal staff. A staging copy also exists, roughly 14 months behind production.
- Status: Stated
- Evidence: D1 section 1: "It is live and used daily"; D1 section 2: "There is a staging copy of the system that is roughly 14 months behind production."
- Uncertainty / clarification: None.

### Q12. What functionality already exists, what needs to change, and what is the intended scope of the first release versus later releases?
- Answer:
  - Exists: member login, annual renewal with card payment, CPD activity log, member directory, downloadable certificates, staff admin area.
  - Needs to change: all of the above are to be rebuilt on a supported platform, with membership reporting added and data migrated.
  - Release scope: a single full production launch before 1 November 2026, with automated CPD assessment named as a possible later phase outside this engagement.
- Status: Partially stated
- Evidence: D1 section 2 functionality list; D1 section 4 in-scope list; D1 section 8: "member-facing functions in UAT by 31 August 2026, launch before 1 November 2026"; D1 section 4: "Automated CPD assessment is a possible later phase".
- Uncertainty / clarification: Whether the in-scope functions may be released incrementally, or must all launch together, is not stated. The milestone wording distinguishes member-facing functions in UAT but does not confirm a staged live release.

### Q13. Why is the change needed: missing features, technical limitations, performance, maintenance, or another reason?
- Answer: Three reasons are stated: the platform is out of support, the maintaining contractor left in 2025 so maintenance capability is gone, and the renewal page fails under load every January.
- Status: Stated
- Evidence: D1 section 2: "the platform is out of support, the contractor has gone, and the renewal page fails under load every January."
- Uncertainty / clarification: None.

### Q14. What is the current technology stack, including versions?
- Answer: ASP.NET Web Forms on .NET Framework 4.5, SQL Server 2012, hosted on a single on-premises Windows Server 2012 R2 VM in the client's London office. Built in 2014.
- Status: Stated
- Evidence: D1 section 2, first bullet.
- Uncertainty / clarification: None.

### Q15. Are source code, documentation, environments, and a working demo available, and is access confirmed or merely referenced?
- Answer:
  - Source code: in a self-hosted GitLab instance; the client states read access can be provided. Access is offered, not yet granted.
  - Documentation: none exists.
  - Environments: live production plus a staging copy roughly 14 months behind production.
  - Working system: the live portal itself.
- Status: Stated
- Evidence: D1 section 2: "Source code is in a self-hosted GitLab instance. We can provide read access. There is no technical documentation. There is a staging copy of the system that is roughly 14 months behind production."
- Uncertainty / clarification: Access has not yet been granted and D1 section 8 lists GitLab access as a client input owned by an IT team at capacity until mid-April. The absence of documentation and the outdated staging copy increase reliance on reading production code and data.

### Q16. What existing behaviour, integrations, data, and URLs must be preserved?
- Answer:
  - URLs: `/members/directory/{id}` must continue to resolve.
  - Integrations: the existing Sage accounting export and the existing SAML SSO link to the learning platform must keep working.
  - Data: member data must not be lost; member records, CPD history since 2016, and payment history are migrated.
  - Behaviour: the renewal, CPD, directory, certificate, and admin functions continue to be provided.
- Status: Stated
- Evidence: D1 section 1: "without losing member data or public URLs"; D1 section 3: "Existing public URLs of the form `/members/directory/{id}` must continue to resolve. The existing Sage accounting export and the existing SSO link to our learning platform must keep working."
- Uncertainty / clarification: Whether other public URLs beyond the directory pattern must also be preserved is not enumerated, though the general statement about public URLs is recorded above.

### Q17. Is the proposed approach to extend, refactor, migrate, or rebuild the system?
- Answer: A full rebuild, explicitly not a refactor, with data migration from the existing SQL Server 2012 database.
- Status: Stated
- Evidence: D1 section 3: "We want a full rebuild, not a refactor."
- Uncertainty / clarification: None.

### Q18. Must the old and new systems run alongside each other, and are transition, cutover, downtime, or rollback expectations defined?
- Answer:
  - Parallel running: yes. The old portal remains live until the new portal has completed one renewal cycle, approximately two months.
  - Cutover: must avoid the January renewal peak.
  - Rollback: a rollback plan is required.
  - Downtime: no cutover downtime tolerance is stated.
- Status: Partially stated
- Evidence: D1 section 3: "The old portal must remain live until the new portal has completed one renewal cycle, so both will run in parallel for approximately two months. Cutover must avoid the January renewal peak. A rollback plan is required."
- Uncertainty / clarification: Unresolved: acceptable cutover downtime; which system is authoritative for member and CPD data during parallel running; whether members renew in one portal only; the criteria that would trigger rollback and the point after which rollback is no longer possible.

### Section 4 — Scope and Business Requirements

### Q19. What modules and features are explicitly included, including any commercial functionality such as pricing, plans, payments, subscriptions, invoicing, or usage limits where the sources describe them?
- Answer: Member login and account management; annual renewal with payment; CPD activity log with evidence upload; member directory; certificate generation; staff admin area; membership reporting; data migration from SQL Server 2012. Commercial functionality is in scope and described: annual fee with retired and pro-rata variations, Stripe card payment, VAT on invoices, and EUR invoicing for Irish members.
- Status: Stated
- Evidence: D1 section 4 in-scope list; D1 section 5 renewal rules; D1 section 7: "Irish members are invoiced in EUR".
- Uncertainty / clarification: None for the module list. Specific pricing mechanics are addressed under Q23.

### Q20. What is explicitly excluded?
- Answer: The public marketing website, the events booking system, and any mobile app. Automated CPD assessment is a possible later phase and not part of this engagement. Refunds are handled manually by Finance and are outside the portal.
- Status: Stated
- Evidence: D1 section 4: "Explicitly out of scope: the public marketing website, the events booking system, and any mobile app"; D1 section 5: "Refunds are handled manually by Finance and are outside the portal."
- Uncertainty / clarification: None.

### Q21. What are the main user journeys and business workflows, including triggers, steps, and outcomes?
- Answer:
  - Renewal: annual, with fee calculation, payment via Stripe, invoice with VAT, one retry after a failed payment, and completion only on payment confirmation.
  - CPD logging with evidence upload; certificate download; member directory use; staff manual renewal; staff reporting; finance read-only access to payment records and exports.
- Status: Partially stated
- Evidence: D1 section 5 rules and role list; D1 section 4 in-scope list.
- Uncertainty / clarification: Step-level flows are not documented for any journey. Unresolved: what triggers renewal for a member, such as a reminder or a due date; the steps and outcomes of the staff manual renewal process; what a member sees after a failed payment; how CPD evidence is reviewed, if at all.

### Q22. What user roles exist, and what can each role access or perform?
- Answer:
  - Member: manage own profile, renew, log CPD, download certificates.
  - Staff administrator: manage member records, process manual renewals, run reports.
  - Finance user: read-only access to payment records and exports.
- Status: Stated
- Evidence: D1 section 5 role list.
- Uncertainty / clarification: None. Authentication requirements per role are addressed under Q44.

### Q23. What business rules, validations, calculations, and approval processes apply, including any pricing, discount, tax, refund, or proration rules where charging is in scope?
- Answer:
  - Standard annual fee £245; retired members pay 40% of the standard fee; joiners after 1 October pay pro rata for the remainder of the year.
  - VAT at the prevailing UK rate applies and must appear on the invoice.
  - Failed payments retried once after 72 hours; renewal complete only when payment is confirmed.
  - Refunds are manual and outside the portal.
  - Irish members are invoiced in EUR at a rate set monthly by Finance.
- Status: Partially stated
- Evidence: D1 section 5 renewal rules; D1 section 7: "Irish members are invoiced in EUR at a rate set monthly by Finance."
- Uncertainty / clarification: Unresolved: how the monthly EUR rate is entered, stored, and applied to an invoice, and which rate applies to a renewal spanning a rate change; how retired status is established and verified; whether VAT treatment differs for Irish members; whether staff manual renewals require any approval; validation rules for CPD evidence uploads such as file type and size.

### Q24. Are exceptions covered, such as rejection, cancellation, duplicate submissions, and failed operations?
- Answer: Payment failure is covered by a single retry after 72 hours and by the rule that renewal completes only on payment confirmation. Refund handling is defined as a manual out-of-portal process.
- Status: Partially stated
- Evidence: D1 section 5: "Failed payments must be retried once after 72 hours, and a renewal is only complete when payment is confirmed."
- Uncertainty / clarification: Unresolved: what happens after the single retry also fails; membership cancellation or lapse handling; duplicate CPD entries or duplicate renewal attempts; rejection of CPD evidence; failed certificate generation; failed Sage export runs.

### Q25. Are administration, reporting, search, notifications, imports, and exports required, and what is specified for each?
- Answer:
  - Administration: staff admin area for member records and manual renewals.
  - Reporting: membership reporting; finance read-only access to payment records and exports.
  - Exports: the nightly Sage accounting export must keep working.
  - Imports: one-off migration of member, CPD, and payment data.
  - Search and notifications: not specified.
- Status: Partially stated
- Evidence: D1 section 4 in-scope list; D1 section 5 finance role; D1 sections 3 and 6 on the Sage export; D1 section 6 on migration.
- Uncertainty / clarification: Unresolved: which reports are required and their contents; whether member directory search and admin search are required and on which fields; whether renewal reminders, payment-failure notices, or certificate emails are required, which is material to the stated goal of reducing renewal-related calls.

### Q26. Are any requirements contradictory, ambiguous, or dependent on unstated assumptions?
- Answer:
  - Tension: acceptance requires migrated records to "reconcile to the current database counts", while the document also acknowledges duplicate member records whose cleansing rules are still to be agreed. Deduplication and count reconciliation may not be simultaneously satisfiable as written.
  - Ambiguity: "the prevailing UK rate" of VAT is not fixed to a date or mechanism.
  - Ambiguity: the monthly EUR rate set by Finance has no stated capture or application mechanism.
  - Ambiguity: the monthly maintenance window is described as "agreed" but is not defined.
  - Unstated dependency: the UAT environment is to be hosted by the supplier, while the client owns the Azure subscription and post-launch deployments; the boundary between the two is not described.
  - Unstated dependency: how existing member credentials are carried into the new portal is not addressed anywhere, although 18,000 members must continue to log in.
  - Schedule dependency: client inputs are owned by an IT team "at capacity until mid-April", against a design sign-off milestone of 30 April 2026.
- Status: Inferred
- Evidence: D1 section 8: "migrated records reconcile to the current database counts"; D1 section 6: "We know there are duplicate member records; cleansing rules will need to be agreed"; D1 section 7: "at a rate set monthly by Finance"; "excluding an agreed monthly maintenance window"; D1 section 8: "Our IT team owns these and is currently at capacity until mid-April."
- Uncertainty / clarification: These are analyst observations grounded in the quoted text, not statements by the client. No contradiction between separate sources arises, as only one document was supplied.

### Section 5 — Technology and Architecture

### Q27. What target technology stack is mandatory, preferred, proposed, or explicitly open for recommendation, including versions where stated?
- Answer: Mandatory by client IT policy: .NET 8 or later and SQL Server. Hosting must move to Azure under the existing tenant.
- Status: Stated
- Evidence: D1 section 6: "our IT policy requires .NET 8 or later and SQL Server. Hosting must move to Azure under our existing tenant."
- Uncertainty / clarification: The SQL Server edition and deployment form, such as Azure SQL Database or SQL Server on a VM, are not specified; this is recorded as a technology detail rather than an open stack question.

### Q28. Which platforms are required: web, mobile, desktop, APIs, or background services?
- Answer:
  - Web portal accessed on desktop and mobile browsers is required.
  - A mobile app is explicitly excluded.
  - A scheduled background process is implied by the nightly Sage export, and inbound handling is implied by immediate Stripe confirmations, but neither is stated as a platform requirement.
  - Public or partner APIs are not mentioned.
- Status: Partially stated
- Evidence: D1 section 7: "Must support current Chrome, Edge, and Safari on desktop and mobile"; D1 section 4: "any mobile app" is out of scope; D1 section 6: "The Sage export runs nightly."
- Uncertainty / clarification: Unresolved: whether any API surface must be exposed, for example to the learning platform beyond SSO, and whether background processing is expected within the same hosted application.

### Q29. Are there existing architecture standards, reusable components, or coding conventions to follow?
- Answer: None stated. The document states a technology policy (.NET 8 or later, SQL Server, Azure) but no architecture standards, reusable components, or coding conventions.
- Status: Missing
- Evidence: No supporting statement found in the reviewed sources. The related technology policy is recorded under Q27 and Q31.
- Uncertainty / clarification: Whether the client's internal IT team, which will own deployments after launch, imposes conventions, repository standards, or infrastructure patterns is unresolved and matters because they inherit the codebase.

### Q30. Is the system for one organisation or multiple tenants/customers, and are isolation or customer-specific configuration requirements defined?
- Answer: Single organisation, no multi-tenancy.
- Status: Stated
- Evidence: D1 section 6: "Single organisation, no multi-tenancy."
- Uncertainty / clarification: None.

### Q31. Where must it run: cloud, on-premises, or a hybrid environment?
- Answer: Cloud. Hosting must move to Azure under the client's existing tenant, replacing the current on-premises London VM.
- Status: Stated
- Evidence: D1 section 6: "Hosting must move to Azure under our existing tenant"; D1 section 2 describes the current on-premises VM.
- Uncertainty / clarification: None.

### Q32. Are there restrictions on hosting, licensing, third-party services, data location, or technology choices?
- Answer:
  - Hosting: Azure under the client's tenant.
  - Technology: .NET 8 or later and SQL Server required by policy.
  - Third-party services: Stripe, Sage, and the learning platform SSO are fixed by the preservation requirements.
  - Licensing: the "Trelawney Sans" web licence is held, but its extension to this portal is unconfirmed.
  - Data handling: card data must not be stored by the client.
- Status: Partially stated
- Evidence: D1 section 6 platform bullets; D1 section 7: "We hold a commercial licence for the 'Trelawney Sans' typeface for web use and will confirm whether it extends to this portal"; D1 section 7: "Card data must not be stored by us."
- Uncertainty / clarification: Unresolved: data residency. UK GDPR is stated but no region or data-location constraint is given for the Azure deployment, which matters for members in Ireland.

### Section 6 — Data and Integrations

### Q33. What data must the system store, where does it originate, and which systems are authoritative for shared data?
- Answer:
  - Data: member records and profiles, CPD activity with uploaded evidence, payment history and payment references, generated certificates, membership reporting data, and an audit history of staff actions.
  - Origin: the existing SQL Server 2012 database via migration, members and staff through the portal, and Stripe for payment confirmations.
  - Authoritative systems: not stated.
- Status: Partially stated
- Evidence: D1 sections 2 and 4 on existing data and migration; D1 section 6 on integrations; D1 section 7: "Audit history of staff actions on member records is required"; "The portal holds member personal data and payment references."
- Uncertainty / clarification: Unresolved: which system is authoritative for member identity during parallel running and after cutover, given the old portal remains live for approximately two months; whether Sage or the portal is authoritative for financial records; where uploaded CPD evidence files are stored.

### Q34. Is existing data migration required, including cleaning, mapping, and validation?
- Answer:
  - Required: member records, CPD history since 2016, and payment history.
  - Cleaning: duplicate member records are known to exist; cleansing rules will need to be agreed.
  - Validation: acceptance requires migrated records to reconcile to the current database counts.
  - Mapping: not described.
- Status: Partially stated
- Evidence: D1 section 6: "Data migration covers member records, CPD history since 2016, and payment history. We know there are duplicate member records; cleansing rules will need to be agreed."; D1 section 8: "migrated records reconcile to the current database counts".
- Uncertainty / clarification: Unresolved: cleansing rules; field mapping; whether CPD history before 2016 is discarded or retained elsewhere; how deduplication reconciles with count-based acceptance (see Q26 and gap G02); how member credentials are migrated (see Q44).

### Q35. Which external systems, APIs, or services must be integrated, and what is each integration expected to do?
- Answer:
  - Stripe: card payments for renewal, with confirmations reflected immediately.
  - Sage: nightly accounting export, which must keep working.
  - Learning platform: SAML SSO, which must keep working.
- Status: Stated
- Evidence: D1 section 6: "Integrations: Stripe for card payments, Sage for the accounting export, and SAML SSO to the learning platform"; D1 section 3 on preservation.
- Uncertainty / clarification: The integrations are identified with a stated purpose. Their technical detail depends on the unavailable Appendix C for SSO; see Q36 and gap G01.

### Q36. Are integration documentation, sandbox access, credentials provisioning, and vendor support available or assigned to an owner?
- Answer:
  - Sandbox and credentials: Stripe and Sage sandbox credentials will be provided by the client IT team.
  - Documentation: learning platform SSO notes are in Appendix C, which was not supplied. No Stripe or Sage integration documentation specific to the current implementation is offered.
  - Vendor support: not addressed for any of the three integrations.
- Status: Partially stated
- Evidence: D1 section 6: "Stripe and Sage sandbox credentials will be provided by our IT team. Integration notes for the learning platform SSO are in Appendix C."; the Appendix C heading in D1 records it as not supplied.
- Uncertainty / clarification: Appendix C is unavailable, so the SSO configuration, identity provider, attribute mapping, and endpoints are unknown. No secret values are reproduced in this report. Sage export file format documentation is not offered, and the current export is implemented in code with no technical documentation (Q15).

### Q37. Does data need to sync in real time, on a schedule, or manually, and in which direction?
- Answer:
  - Sage: scheduled nightly, outbound export from the portal.
  - Stripe: payment confirmations must be reflected immediately, inbound to the portal.
  - Learning platform: SSO only; no data synchronisation is described.
- Status: Partially stated
- Evidence: D1 section 6: "The Sage export runs nightly. Stripe payment confirmations must be reflected immediately."
- Uncertainty / clarification: Unresolved: whether member or CPD data must flow to the learning platform beyond authentication; whether the Sage export is one-way or requires acknowledgement; whether any manual re-run or catch-up capability is expected.

### Q38. What should happen when an integration fails, times out, returns incomplete data, or delivers duplicate events?
- Answer: Only the business-level payment rule is stated: failed payments are retried once after 72 hours. No technical failure handling is specified for any integration.
- Status: Partially stated
- Evidence: D1 section 5: "Failed payments must be retried once after 72 hours."
- Uncertainty / clarification: Unresolved: behaviour when the nightly Sage export fails or produces a partial file; handling of duplicate or out-of-order Stripe confirmations; behaviour when the learning platform SSO is unavailable; whether alerts are raised to staff and to whom.

### Section 7 — Design and Quality Requirements

### Q39. Are approved designs, branding, and content available, or is producing them part of the scope, and are ownership or licensing terms stated for client-supplied assets such as designs, fonts, imagery, copy, and third-party components?
- Answer:
  - Available: brand guidelines and logo files will be supplied by the client.
  - Not available: page designs do not exist; the client expects the supplier to produce them.
  - Licensing: a commercial web licence is held for the "Trelawney Sans" typeface, and the client will confirm whether it extends to this portal.
  - Content: not addressed.
- Status: Partially stated
- Evidence: D1 section 7: "Our brand guidelines and logo files will be supplied... We hold a commercial licence for the 'Trelawney Sans' typeface for web use and will confirm whether it extends to this portal. Page designs do not exist and we expect the supplier to produce them."
- Uncertainty / clarification: Unresolved: the outcome of the typeface licence check and the fallback if it does not extend (gap G05); who provides page copy and member-facing content for the new portal, and whether existing portal content is reused; ownership of the delivered designs.

### Q40. What devices, browsers, languages, and accessibility requirements must be supported, including any stated locales, regional date/number/currency formats, time-zone handling, and offline or intermittent-connectivity expectations?
- Answer:
  - Browsers and devices: current Chrome, Edge, and Safari on desktop and mobile.
  - Languages: English only.
  - Currency and regional formats: GBP, with EUR invoicing for Irish members at a monthly rate set by Finance; dates displayed as DD/MM/YYYY.
  - Accessibility: WCAG 2.1 AA, required by client policy.
  - Time-zone handling and offline use are not raised in the sources; the portal is a browser-based system for the UK and Ireland, which share a time zone, so these are recorded as not raised rather than as requirements.
- Status: Stated
- Evidence: D1 section 7: "Must support current Chrome, Edge, and Safari on desktop and mobile. English only. Amounts are in GBP; Irish members are invoiced in EUR at a rate set monthly by Finance. Dates must display in DD/MM/YYYY. WCAG 2.1 AA is required by our accessibility policy."
- Uncertainty / clarification: The mechanism for the EUR rate is an unresolved business rule and is recorded under Q23 rather than here. Whether "current" browsers implies a specific support window is not defined.

### Q41. What user numbers, concurrent usage, data volumes, and growth are expected?
- Answer:
  - Users: approximately 18,000 members and 22 internal staff.
  - Concurrency: up to 900 concurrent users during the January renewal peak.
  - Data volumes: CPD history since 2016 and payment history are to be migrated, but no record counts or storage sizes are given.
  - Growth: not stated.
- Status: Partially stated
- Evidence: D1 section 1 user numbers; D1 section 7: "Expected load: 18,000 members, with up to 900 concurrent users during the January renewal peak."
- Uncertainty / clarification: Unresolved: record counts and storage volumes for migration and for CPD evidence uploads; expected membership growth; evidence file size limits.

### Q42. Are performance, availability, and acceptable downtime targets defined?
- Answer:
  - Performance: renewal pages should respond within 2 seconds at peak.
  - Availability: 99.5% monthly, excluding an agreed monthly maintenance window.
  - Downtime: the maintenance window is referenced but not defined; cutover downtime is not stated (Q18).
- Status: Partially stated
- Evidence: D1 section 7: "Renewal pages should respond within 2 seconds at peak. Target availability is 99.5% monthly, excluding an agreed monthly maintenance window."
- Uncertainty / clarification: Unresolved: the duration, timing, and frequency of the maintenance window; whether the 2-second target applies to all pages or only renewal; how availability is measured and by whom.

### Q43. Are SEO, analytics, audit history, or activity tracking required, and what is specified for each?
- Answer:
  - Audit history: required for staff actions on member records.
  - SEO and analytics: not addressed. Preservation of `/members/directory/{id}` URLs is stated as a continuity requirement rather than an SEO requirement.
- Status: Partially stated
- Evidence: D1 section 7: "Audit history of staff actions on member records is required."; D1 section 3 on URL preservation.
- Uncertainty / clarification: Unresolved: retention and visibility of the audit history; whether member-side activity must be tracked; whether analytics tooling is expected; whether the directory pages are publicly indexed today, which affects the URL-preservation requirement.

### Section 8 — Security and Operations

### Q44. What authentication, permissions, and account-management requirements apply?
- Answer:
  - Authentication: email and password, with mandatory multi-factor authentication for staff accounts.
  - Permissions: three roles with the accesses recorded under Q22.
  - Account management: not described.
- Status: Partially stated
- Evidence: D1 section 7: "Authentication: email and password with mandatory multi-factor authentication for staff accounts."; D1 section 5 role list.
- Uncertainty / clarification: Unresolved and material: how existing member credentials are carried into the new portal, or whether 18,000 members must reset passwords at cutover (gap G07). Also unresolved: whether MFA is available to members, self-service registration and password reset, account lockout, staff account provisioning and de-provisioning, and whether staff authenticate against an existing directory.

### Q45. Will the system handle sensitive information, and what privacy or compliance requirements are explicitly identified?
- Answer: Yes. The portal holds member personal data and payment references. UK GDPR applies, and card data must not be stored by the client.
- Status: Stated
- Evidence: D1 section 7: "The portal holds member personal data and payment references. UK GDPR applies. Card data must not be stored by us."
- Uncertainty / clarification: Only UK GDPR is identified. Members are also based in Ireland, which may carry additional obligations, but the document identifies none and none is assumed here. Payment card compliance scope beyond "no card data stored" is not described.

### Q46. What backup, recovery, data retention, and deletion requirements exist?
- Answer:
  - Backup: nightly, with 30-day retention.
  - Deletion: member data must be deletable on request.
  - Recovery and data retention beyond backups: not stated.
- Status: Partially stated
- Evidence: D1 section 7: "Nightly backups with a 30-day retention are required. Member data must be deletable on request."
- Uncertainty / clarification: Unresolved: recovery point and recovery time objectives; restore testing expectations; how long member, CPD, and payment records are retained after membership ends; how deletion interacts with payment history required for accounting and with the Sage export.

### Q47. Who will own or control hosting accounts, source code, data, deployments, third-party subscriptions and licences, and ongoing maintenance?
- Answer:
  - Client owns: the Azure subscription, the source code repository, and all data.
  - Deployments after launch: run by the client's internal IT team.
  - Not stated: ownership of the Stripe and Sage accounts, ownership of the delivered designs and any third-party components or licences, and who maintains the application after the three-month post-launch support period.
- Status: Partially stated
- Evidence: D1 section 7: "We will own the Azure subscription, the source code repository, and all data. Deployments after launch will be run by our internal IT team."; D1 section 8 on three months of post-launch support.
- Uncertainty / clarification: Unresolved: deployment responsibility before launch and for the supplier-hosted UAT environment; ongoing maintenance arrangements after the support period; licence ownership for any third-party components introduced by the supplier.

### Q48. What environments, deployment process, monitoring, incident handling, and support arrangements are required, including any stated support hours, response or resolution targets, and escalation paths?
- Answer:
  - Environments: development, staging, production. A UAT environment hosted by the supplier is also referenced under Q52.
  - Support: 08:00–18:00 UK time on business days; 4-hour response target for issues preventing renewal; next-business-day response otherwise.
  - Deployment: after launch, run by the client's internal IT team.
  - Monitoring, incident handling, and escalation: not stated.
- Status: Partially stated
- Evidence: D1 section 7: "Environments required: development, staging, production. Support hours are 08:00–18:00 UK time on business days, with a 4-hour response target for issues that prevent renewal and next-business-day response for everything else."
- Uncertainty / clarification: Unresolved: resolution targets as distinct from response targets; monitoring and alerting expectations and who receives alerts; the escalation path during the January peak, which falls outside the three-month post-launch support period if launch occurs in late October; the deployment process and pipeline expectations before launch.

### Section 9 — Delivery and Acceptance

### Q49. What are the budget, target dates, milestones, delivery priorities, release expectations, and any stated engagement commercial model?
- Answer:
  - Budget: £180,000 excluding VAT, approved.
  - Commercial model: fixed price.
  - Milestones: design sign-off by 30 April 2026; member-facing functions in UAT by 31 August 2026; launch before 1 November 2026.
  - Release expectations: cutover must avoid the January renewal peak; parallel running for approximately two months until one renewal cycle completes.
  - Priorities: member-facing functions are sequenced first, per the UAT milestone.
- Status: Stated
- Evidence: D1 section 8: "Budget is approved at £180,000 excluding VAT, on a fixed-price basis."; milestone list in D1 section 8; D1 section 3 on parallel running and cutover timing.
- Uncertainty / clarification: The relationship between a launch before 1 November 2026 and a parallel-running period covering the January renewal cycle is stated but implies support obligations into January; this is recorded under Q48.

### Q50. What dependencies or client-provided inputs could delay delivery, and who is responsible for them?
- Answer: Client-provided inputs are GitLab access, brand assets, Stripe and Sage sandbox credentials, and Appendix C. The client's IT team owns these and is stated to be at capacity until mid-April. The client also provides anonymised test data.
- Status: Stated
- Evidence: D1 section 8: "Client-provided inputs: GitLab access, brand assets, Stripe and Sage sandbox credentials, and Appendix C. Our IT team owns these and is currently at capacity until mid-April."; "We will provide anonymised test data".
- Uncertainty / clarification: The stated IT capacity constraint sits close to the 30 April design sign-off milestone; the timing consequence is recorded as gap G03 rather than as a missing answer.

### Q51. What measurable acceptance criteria define completion?
- Answer: Three criteria are stated: a member can complete renewal end to end; migrated records reconcile to the current database counts; and the renewal page sustains 900 concurrent users in a load test.
- Status: Stated
- Evidence: D1 section 8: "Acceptance: a member can complete renewal end to end, migrated records reconcile to the current database counts, and the renewal page sustains 900 concurrent users in a load test."
- Uncertainty / clarification: The count-reconciliation criterion interacts with the duplicate-record cleansing intention; recorded under Q26 and gap G02. Whether acceptance also covers CPD, directory, certificates, admin, reporting, WCAG 2.1 AA, and the 2-second response target is not stated.

### Q52. Who will test and approve the deliverables, what testing or sign-off process is specified, and who provides test data, test accounts, and UAT environment access?
- Answer:
  - Testing: the client's internal team, led by the Membership Manager.
  - Approval: final launch sign-off sits with the Chief Executive (Q5).
  - Test data: anonymised test data provided by the client.
  - UAT environment: hosted by the supplier.
  - Process: not described.
- Status: Partially stated
- Evidence: D1 section 8: "Testing will be carried out by our internal team, led by our Membership Manager. We will provide anonymised test data and the UAT environment will be hosted by the supplier."; D1 section 1 on Chief Executive sign-off.
- Uncertainty / clarification: Unresolved: the UAT process, duration, and entry and exit criteria; defect severity classification and turnaround; how anonymised data supports migration reconciliation testing, which depends on real record counts; who supplies test accounts for Stripe, Sage, and SSO testing; whether load testing is performed by the supplier or the client.

### Q53. What documentation, training, handover, and post-launch support are expected?
- Answer: Administrator documentation, two staff training sessions, and three months of post-launch support.
- Status: Partially stated
- Evidence: D1 section 8: "Deliverables include administrator documentation, two staff training sessions, and three months of post-launch support."
- Uncertainty / clarification: Unresolved: technical or architecture documentation for the internal IT team that will own deployments and maintenance; member-facing help content; the handover process and its acceptance; what happens after the three-month support period, particularly across the January renewal peak.

### Q54. How will scope changes be reviewed and approved?
- Answer: Through a written change request approved by Daniel Okafor, and additionally by Fiona Grant where cost is affected.
- Status: Stated
- Evidence: D1 section 8: "Scope changes will be handled through a written change request approved by Daniel Okafor, and by Fiona Grant where cost is affected."
- Uncertainty / clarification: None.

### Section 10 — Document Completeness and Analyst Conclusions

### Q55. How many Q1–Q54 answers are Stated, Partially stated, Inferred, Missing, Conflicting, and Not applicable?
- Answer: Stated 24; Partially stated 23; Inferred 1; Missing 1; Conflicting 0; Not applicable 5. Total 54. See the dashboard in section 6.
- Basis: The tally reproduces the statuses assigned in section 5 without reclassification.
- Limitations: The high Partially stated count reflects a document that states requirements at summary level across most topics while leaving mechanisms undefined. Appendix C was unavailable, which affects Q35, Q36, and Q37.

### Q56. Are referenced attachments, diagrams, designs, and supporting documents available and reviewed?
- Answer: Appendix C, the learning platform SSO integration notes, is referenced twice in D1 but was not supplied and was not reviewed. Other referenced material not supplied: the GitLab repository, the staging environment, brand guidelines and logo files, and Stripe and Sage sandbox credentials. The document states that no technical documentation exists for the current system, and that page designs do not exist. No diagrams are referenced.
- Basis: Source inventory in section 1; D1 sections 2, 6, 7, 8 and the Appendix C heading.
- Limitations: Because Appendix C is unavailable, the SSO integration cannot be characterised beyond "SAML SSO to the learning platform". Statements about the current implementation rest on the document's own description, since neither the code nor an environment was reviewed.

### Q57. Which unresolved items block specific estimation, architecture, implementation, acceptance, or release decisions, and why?
- Answer: Six items are classified Blocking: G01 unavailable Appendix C, G02 migration cleansing versus count-based acceptance, G07 member credential migration, G04 the EUR rate mechanism, G06 cutover downtime and rollback criteria, and G03 client input timing against the design sign-off milestone. Each blocked activity and dependency is set out in section 7.
- Basis: Gaps register in section 7, derived from the statuses in section 5.
- Limitations: The engagement is fixed price, so scope-affecting unknowns carry more commercial weight than they would under a time-and-materials arrangement; this is an analyst observation, not a statement in the document.

### Q58. What must be clarified before committing to scope, cost, or timelines, and what can be resolved later?
- Answer: Before commitment: Appendix C and the SSO configuration (G01), migration cleansing rules and how they reconcile with count-based acceptance (G02), the EUR rate mechanism (G04), and the availability timing of client inputs against the 30 April design sign-off (G03). Before design or implementation: member credential migration (G07), integration failure handling (G13), notifications (G11), the typeface licence outcome (G05), content responsibility (G15), and search and reporting detail (G12). Before release: cutover downtime and rollback criteria (G06), recovery objectives (G10), monitoring and escalation (G09), and the maintenance window definition (G17). Questions are listed in section 8.
- Basis: Gaps register and client question list.
- Limitations: Sequencing is an analyst view. The client may hold answers, particularly for migration and SSO detail, that were not written into this document.

## 6. Answer Coverage Dashboard

| Status | Number of questions | Question IDs |
| --- | --- | --- |
| Stated | 24 | Q1, Q2, Q3, Q4, Q5, Q11, Q13, Q14, Q15, Q16, Q17, Q19, Q20, Q22, Q27, Q30, Q31, Q35, Q40, Q45, Q49, Q50, Q51, Q54 |
| Partially stated | 23 | Q12, Q18, Q21, Q23, Q24, Q25, Q28, Q32, Q33, Q34, Q36, Q37, Q38, Q39, Q41, Q42, Q43, Q44, Q46, Q47, Q48, Q52, Q53 |
| Inferred | 1 | Q26 |
| Missing | 1 | Q29 |
| Conflicting | 0 | None |
| Not applicable | 5 | Q6, Q7, Q8, Q9, Q10 |
| Total | 54 | Q1–Q54 |

- Applicable questions: 49.
- Reading limitations affecting the tally: Appendix C was not supplied, which limits Q35, Q36, and Q37. No code or environment was inspected, so all current-system statements rest on the document.

## 7. Gaps, Conflicts, and Dependencies

### Blocking Items

| ID | Finding | Related Q IDs / sources | Decision or activity blocked | Why it is blocked | Clarification needed |
| --- | --- | --- | --- | --- | --- |
| G01 | Appendix C, the learning platform SSO integration notes, is referenced but not supplied | Q35, Q36, Q56; D1 sections 6, 8 | Estimation and implementation of the SAML SSO integration | The identity provider, attribute mapping, endpoints, and existing configuration are unknown, and the integration must keep working after replacement | Supply Appendix C, or confirm the SSO configuration and who administers the learning platform |
| G02 | Duplicate member records are acknowledged with cleansing rules "to be agreed", while acceptance requires migrated records to reconcile to current database counts | Q34, Q51, Q26; D1 sections 6, 8 | Migration design and the migration acceptance test | Deduplication changes record counts, so the cleansing approach and the acceptance measure cannot both be satisfied as currently written | Cleansing and merge rules, and a reconciliation definition that accounts for deduplication |
| G07 | How the existing 18,000 member credentials are carried into the new portal is not described | Q44, Q34; D1 sections 4, 7 | Implementation of authentication and cutover planning | Credential migration versus a forced reset changes the authentication design, the cutover communications, and the support load during renewal | Whether member passwords migrate, and if not, the reset approach at cutover |
| G04 | The mechanism for the monthly EUR rate applied to Irish member invoices is not defined | Q23, Q40; D1 sections 5, 7 | Implementation of renewal pricing and invoicing | The rate source, entry point, effective period, and treatment of a renewal spanning a rate change are all undetermined, and invoices are a stated requirement | Who sets the rate, how it is entered and stored, and which rate applies to a given renewal |
| G06 | Cutover downtime tolerance and rollback trigger criteria are undefined, although a rollback plan is required and parallel running is mandated | Q18; D1 section 3 | Release and cutover planning | Rollback design and data-authority rules during parallel running depend on these; a two-month parallel period with live renewals raises data-divergence questions | Acceptable downtime, which system is authoritative during parallel running, and rollback triggers and cut-off point |
| G03 | Client-owned inputs are held by an IT team stated to be at capacity until mid-April, against a design sign-off milestone of 30 April 2026 | Q50, Q49, Q15; D1 section 8 | Commitment to the stated milestone dates on a fixed-price basis | Repository access, brand assets, credentials, and Appendix C are prerequisites for design and integration work that must complete before sign-off | Confirmed dates for each input, or a revised milestone plan |

### Non-blocking Clarifications

| ID | Finding | Related Q IDs / sources | Why it matters | When needed |
| --- | --- | --- | --- | --- |
| G05 | The "Trelawney Sans" web licence may not extend to this portal; the client will confirm | Q39, Q32; D1 section 7 | Determines whether the licensed typeface can be used or a fallback is required in the designs | Before design or implementation |
| G08 | No technical documentation exists and the staging copy is roughly 14 months behind production | Q15; D1 section 2 | Understanding current behaviour, especially the Sage export and SSO, will rely on production code and data | Before design or implementation |
| G09 | Monitoring, incident handling, and escalation paths are not stated, and resolution targets are not defined | Q48; D1 section 7 | Operational readiness and the support arrangement across the January peak | Before release |
| G10 | Recovery objectives are not stated, although nightly backups and 30-day retention are | Q46; D1 section 7 | Backup and recovery design, and restore expectations for a live membership system | Before release |
| G11 | Notifications, including renewal reminders, payment-failure notices, and certificate delivery, are not specified | Q25, Q23; D1 sections 4, 5 | Directly affects the stated goal of reducing renewal-related support calls | Before design or implementation |
| G12 | Required reports, and search behaviour for the directory and admin area, are not specified | Q25; D1 section 4 | Reporting and search are in scope but undefined, which affects effort and acceptance | Before design or implementation |
| G13 | Failure, timeout, partial-data, and duplicate-event handling for the Sage export and Stripe confirmations is not specified | Q38, Q37; D1 sections 5, 6 | Financial data integrity and staff visibility of failures | Before design or implementation |
| G14 | Ownership of the Stripe and Sage accounts, of delivered designs, and of maintenance after the three-month support period is not stated | Q47; D1 sections 7, 8 | Operational handover and post-support responsibility | Before release |
| G15 | Responsibility for page copy and member-facing content is not stated | Q39; D1 section 7 | Content production can be a material effort item, and reuse of existing portal content is unconfirmed | Before design or implementation |
| G16 | Whether member or CPD data must flow to the learning platform beyond SSO is unresolved | Q37, Q28; D1 section 6 | Determines whether an additional integration or API surface is in scope | Before design or implementation |
| G17 | The monthly maintenance window is referred to as agreed but is not defined | Q42; D1 section 7 | Needed to express the availability target and plan deployments | Before release |
| G18 | The UAT process, defect handling, and test-account provisioning for Stripe, Sage, and SSO are not described, and anonymised test data may not support count reconciliation | Q52, Q51; D1 section 8 | Acceptance execution, particularly for the migration criterion | Before release |

### Documented Dependencies

| Dependency | Documented owner | Required timing | Evidence |
| --- | --- | --- | --- |
| GitLab read access to the current source code | Client IT team | Not stated; IT at capacity until mid-April | D1 sections 2, 8 |
| Brand guidelines and logo files | Client | Not stated | D1 sections 7, 8 |
| Stripe and Sage sandbox credentials | Client IT team | Not stated; IT at capacity until mid-April | D1 sections 6, 8 |
| Appendix C, learning platform SSO notes | Client | Not stated; not yet supplied | D1 sections 6, 8 |
| Anonymised test data | Client | Before UAT, which is milestoned at 31 August 2026 | D1 section 8 |
| UAT environment hosting | Supplier | Before UAT | D1 section 8 |
| Post-launch deployments | Client internal IT team | After launch | D1 section 7 |
| Typeface licence confirmation | Client | Not stated | D1 section 7 |

Analyst note: the items above are analyst observations about what the document leaves unresolved. Classification indicates which activity is presently blocked, not a prediction that a problem will occur.

## 8. Open Questions for the Client

### Integrations

1. Please supply Appendix C, or confirm the learning platform SSO configuration, including the identity provider, the SAML attributes exchanged, endpoints, and who administers the learning platform.
   - Needed: Before estimation.
   - References: Q35, Q36, Q56; G01.
2. Is there any documentation or specification of the current nightly Sage export file format, or must it be derived from the existing code?
   - Needed: Before estimation.
   - References: Q36, Q15; G08.
3. What should happen when the nightly Sage export fails or produces a partial file, and when a Stripe confirmation is duplicated, delayed, or missing? Who should be alerted?
   - Needed: Before design or implementation.
   - References: Q38, Q37; G13.
4. Beyond single sign-on, does any member or CPD data need to be exchanged with the learning platform?
   - Needed: Before design or implementation.
   - References: Q37, Q28; G16.

### Migration and data

5. What cleansing and merge rules should apply to the known duplicate member records, and how should the acceptance criterion of reconciling to current database counts be measured once duplicates are merged?
   - Needed: Before estimation.
   - References: Q34, Q51, Q26; G02.
6. How should the existing 18,000 member credentials be handled: migrated, or reset at cutover?
   - Needed: Before design or implementation.
   - References: Q44, Q34; G07.
7. What are the approximate record counts and storage volumes for member records, CPD history since 2016, payment history, and CPD evidence files?
   - Needed: Before estimation.
   - References: Q41, Q34.
8. Is CPD history before 2016 to be discarded, archived elsewhere, or retained in the old database?
   - Needed: Before design or implementation.
   - References: Q34.
9. During the two-month parallel period, which system is authoritative for member and CPD data, and can members renew in both portals?
   - Needed: Before release.
   - References: Q18, Q33; G06.

### Renewal, pricing, and workflow

10. How is the monthly EUR rate for Irish members set, entered, and stored, and which rate applies to a renewal that spans a rate change?
    - Needed: Before design or implementation.
    - References: Q23, Q40; G04.
11. How is retired status established and verified for the 40% fee, and does VAT treatment differ for Irish members?
    - Needed: Before design or implementation.
    - References: Q23.
12. What happens after the single 72-hour payment retry also fails, and how are lapsed or cancelled memberships handled?
    - Needed: Before design or implementation.
    - References: Q23, Q24.
13. Which notifications are required, for example renewal reminders, payment-failure notices, and certificate delivery?
    - Needed: Before design or implementation.
    - References: Q25, Q23; G11.
14. What are the steps and outcomes of the staff manual renewal process, and does it require any internal approval?
    - Needed: Before design or implementation.
    - References: Q21, Q23.
15. Are there validation rules for CPD evidence uploads, such as accepted file types, size limits, or review by staff?
    - Needed: Before design or implementation.
    - References: Q23, Q24.

### Scope detail

16. Which membership reports are required, and what should each contain?
    - Needed: Before estimation.
    - References: Q25; G12.
17. What search capability is required in the member directory and the staff admin area, and on which fields?
    - Needed: Before design or implementation.
    - References: Q25; G12.
18. Are the in-scope functions expected to launch together, or may they be released incrementally?
    - Needed: Before estimation.
    - References: Q12, Q49.
19. Beyond the `/members/directory/{id}` pattern, are there other existing URLs that must continue to resolve, and are the directory pages publicly indexed today?
    - Needed: Before design or implementation.
    - References: Q16, Q43.

### Design, content, and quality

20. Has the "Trelawney Sans" licence been confirmed for this portal, and if it does not extend, what fallback is acceptable?
    - Needed: Before design or implementation.
    - References: Q39, Q32; G05.
21. Who provides page copy and member-facing content for the new portal, and can existing portal content be reused?
    - Needed: Before design or implementation.
    - References: Q39; G15.
22. Does the 2-second response target apply beyond renewal pages, and does acceptance include WCAG 2.1 AA and the performance target alongside the three stated criteria?
    - Needed: Before estimation.
    - References: Q42, Q51.
23. What are the duration, timing, and frequency of the agreed monthly maintenance window?
    - Needed: Before release.
    - References: Q42; G17.
24. What retention applies to the audit history, and is any member-side activity tracking or analytics required?
    - Needed: Before design or implementation.
    - References: Q43.

### Security and operations

25. Should staff authenticate against an existing directory, and is MFA to be offered to members as well as mandated for staff?
    - Needed: Before design or implementation.
    - References: Q44.
26. What self-service account management is required, for example registration, password reset, and lockout handling?
    - Needed: Before design or implementation.
    - References: Q44.
27. Are there data residency requirements for the Azure deployment, given members in Ireland and the stated UK GDPR obligation?
    - Needed: Before estimation.
    - References: Q32, Q45.
28. What recovery point and recovery time objectives apply, and how long must member, CPD, and payment records be retained after membership ends?
    - Needed: Before release.
    - References: Q46; G10.
29. How does deletion on request interact with payment history required for accounting and with records already sent to Sage?
    - Needed: Before release.
    - References: Q46, Q37.
30. What monitoring, alerting, incident handling, and escalation are expected, and are resolution targets required alongside the stated response targets?
    - Needed: Before release.
    - References: Q48; G09.
31. Who owns the Stripe and Sage accounts, and who maintains the application after the three-month post-launch support period, particularly across the January renewal peak?
    - Needed: Before release.
    - References: Q47, Q48, Q53; G14.
32. Does the internal IT team have architecture, repository, or infrastructure standards the new system must follow, given they will own deployments?
    - Needed: Before design or implementation.
    - References: Q29, Q47.

### Delivery and acceptance

33. When can each client input realistically be provided, given the IT team is at capacity until mid-April and design sign-off is milestoned for 30 April 2026?
    - Needed: Before estimation.
    - References: Q50, Q49, Q15; G03.
34. What UAT process, duration, and defect turnaround do you expect, and who provides test accounts for Stripe, Sage, and the learning platform SSO?
    - Needed: Before release.
    - References: Q52; G18.
35. Given test data is anonymised, how should migration count reconciliation be verified, and who performs the 900-user load test?
    - Needed: Before release.
    - References: Q51, Q52; G18.
36. Is technical or architecture documentation required for the internal IT team alongside the administrator documentation, and what does handover acceptance involve?
    - Needed: Before release.
    - References: Q53.
37. Can you confirm the current baseline volume of renewal-related support calls, so the 30% reduction measure has a reference point?
    - Needed: Later clarification.
    - References: Q4.

## 9. Document Assumptions and AI Inferences

### Assumptions Explicitly Stated in the Sources

| ID | Assumption | Source / Q IDs | What needs validation |
| --- | --- | --- | --- |
| A1 | "We assume the existing Sage export file format will not change during the project" | D1 section 8; Q36, Q37 | Who confirms this, and what happens to scope and cost if Sage or the client's finance process changes the format mid-project |

### AI Inferences Requiring Confirmation

| ID | Inference | Supporting evidence / Q IDs | Why confirmation matters |
| --- | --- | --- | --- |
| I1 | The deliverable is a full production system rather than a staged or limited release, since it replaces a live portal for approximately 18,000 members with migrated production data | D1 sections 1, 3, 4, 8; Q12 | Determines release, cutover, and acceptance expectations; the document never uses the terms prototype, MVP, or production |
| I2 | A scheduled background process is required for the nightly Sage export, and inbound webhook or callback handling for immediate Stripe confirmations | D1 section 6: "The Sage export runs nightly"; "Stripe payment confirmations must be reflected immediately"; Q28, Q37 | Affects architecture and hosting components, which are not stated as platform requirements |
| I3 | Deduplication of member records and reconciliation to current record counts are in tension as written | D1 sections 6, 8; Q26, Q34, Q51 | If the tension is real, either the cleansing scope or the acceptance criterion needs restating before a fixed-price commitment |
| I4 | Member-facing functions are prioritised ahead of staff functions, based on the UAT milestone wording | D1 section 8: "member-facing functions in UAT by 31 August 2026"; Q49 | Sequencing affects delivery planning; the document does not state a priority order explicitly |
| I5 | Support obligations extend into the January renewal peak, since parallel running continues until one renewal cycle completes while post-launch support is three months from a launch before 1 November 2026 | D1 sections 3, 8; Q48, Q53 | Determines whether peak-period support is inside or outside the engagement |
| I6 | Uploaded CPD evidence implies file storage in addition to database records | D1 section 4: "CPD activity log with evidence upload"; Q33, Q41 | Affects storage design, volumes, backup, and retention, none of which are quantified |

## 10. Overall Understanding

- Project in plain language: rebuild Northwind's live member portal on .NET 8 or later in Azure, migrate member, CPD, and payment data from SQL Server 2012, keep the Sage export, the learning platform SSO, and the existing directory URLs working, and run the old portal in parallel until one renewal cycle completes.
- Clearly defined: the business drivers, the current stack, the in-scope and out-of-scope function lists, roles, the core renewal pricing rules, the mandated target stack and hosting, the three integrations, the performance and availability targets, the budget and fixed-price basis, the milestones, the acceptance criteria, the client inputs, and the change-control process.
- Still uncertain: the SSO configuration held in the unsupplied Appendix C, migration cleansing rules and how they square with count-based acceptance, member credential migration, the EUR rate mechanism, cutover downtime and rollback criteria, integration failure handling, notifications, and the timing of client-provided inputs.
- Before committing: resolve the six blocking items in section 7. Three of them, Appendix C, migration cleansing versus acceptance, and input timing, bear directly on a fixed-price commitment.
- Review limitations: only the requirements document was reviewed. Appendix C was unavailable, and no code, database, or environment was inspected, so every statement about current behaviour rests on the client's own description.
