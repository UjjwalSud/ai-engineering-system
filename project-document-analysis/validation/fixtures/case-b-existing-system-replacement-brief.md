# Test Fixture B — Existing-System Replacement Brief

> Synthetic test fixture for validating project-document-analyzer.md. Not a real client document.
> Purpose: exercise the Existing-project route on a replacement, the new extraction cues
> (billing, asset licensing, locale, support targets, UAT ownership), an explicit document
> assumption, and a referenced-but-unavailable attachment.

---

## Statement of Requirements — Replacement of "MemberDesk" Portal

**Client:** Northwind Institute of Surveyors
**Document reference:** SOR-2026-011, version 1.2, dated 2 February 2026
**Prepared by:** Daniel Okafor, Head of Digital

### 1. Background and objective

MemberDesk is our member portal. It is live and used daily by approximately 18,000 members across
the United Kingdom and Ireland, plus 22 internal staff. It was built in 2014 and has been maintained
by a contractor who left in 2025.

The objective is to replace MemberDesk with a new portal on a supported platform, without losing
member data or public URLs. Success means members can renew and access CPD records without calling
the office, and that staff can administer membership without contractor involvement. We will judge
this on a reduction in renewal-related support calls of at least 30% in the first renewal cycle after
launch.

Decision-makers: Daniel Okafor (project owner) and the Finance Director, Fiona Grant, who approves
spend. Final sign-off for launch sits with the Chief Executive.

### 2. Current system

- ASP.NET Web Forms on .NET Framework 4.5, SQL Server 2012, hosted on a single on-premises Windows
  Server 2012 R2 VM in our London office.
- Functionality today: member login, annual renewal with card payment, CPD activity log, member
  directory, downloadable certificates, and a staff admin area.
- The change is needed because the platform is out of support, the contractor has gone, and the
  renewal page fails under load every January.
- Source code is in a self-hosted GitLab instance. We can provide read access. There is no technical
  documentation. There is a staging copy of the system that is roughly 14 months behind production.

### 3. Replacement approach and transition

We want a full rebuild, not a refactor. The old portal must remain live until the new portal has
completed one renewal cycle, so both will run in parallel for approximately two months. Cutover must
avoid the January renewal peak. A rollback plan is required.

Existing public URLs of the form `/members/directory/{id}` must continue to resolve. The existing
Sage accounting export and the existing SSO link to our learning platform must keep working.

### 4. Scope

In scope: member login and account management, annual renewal with payment, CPD activity log with
evidence upload, member directory, certificate generation, staff admin area, membership reporting,
and data migration from SQL Server 2012.

Explicitly out of scope: the public marketing website, the events booking system, and any mobile
app. Automated CPD assessment is a possible later phase and is not part of this engagement.

### 5. Roles and rules

- Member: manage own profile, renew, log CPD, download certificates.
- Staff administrator: manage member records, process manual renewals, run reports.
- Finance user: read-only access to payment records and exports.

Renewal rules: the standard annual fee is £245. Retired members pay 40% of the standard fee.
Members joining after 1 October pay a pro-rated fee for the remainder of the year. VAT at the
prevailing UK rate applies and must appear on the invoice. Failed payments must be retried once
after 72 hours, and a renewal is only complete when payment is confirmed. Refunds are handled
manually by Finance and are outside the portal.

### 6. Platform, data, and integrations

- Target stack: our IT policy requires .NET 8 or later and SQL Server. Hosting must move to Azure
  under our existing tenant.
- Single organisation, no multi-tenancy.
- Integrations: Stripe for card payments, Sage for the accounting export, and SAML SSO to the
  learning platform. Stripe and Sage sandbox credentials will be provided by our IT team.
  Integration notes for the learning platform SSO are in Appendix C.
- Data migration covers member records, CPD history since 2016, and payment history. We know there
  are duplicate member records; cleansing rules will need to be agreed.
- The Sage export runs nightly. Stripe payment confirmations must be reflected immediately.

### 7. Design, quality, and operations

- Our brand guidelines and logo files will be supplied. We hold a commercial licence for the
  "Trelawney Sans" typeface for web use and will confirm whether it extends to this portal.
  Page designs do not exist and we expect the supplier to produce them.
- Must support current Chrome, Edge, and Safari on desktop and mobile. English only. Amounts are in
  GBP; Irish members are invoiced in EUR at a rate set monthly by Finance. Dates must display in
  DD/MM/YYYY. WCAG 2.1 AA is required by our accessibility policy.
- Expected load: 18,000 members, with up to 900 concurrent users during the January renewal peak.
  Renewal pages should respond within 2 seconds at peak. Target availability is 99.5% monthly,
  excluding an agreed monthly maintenance window.
- Audit history of staff actions on member records is required.
- Authentication: email and password with mandatory multi-factor authentication for staff accounts.
- The portal holds member personal data and payment references. UK GDPR applies. Card data must not
  be stored by us.
- Nightly backups with a 30-day retention are required. Member data must be deletable on request.
- We will own the Azure subscription, the source code repository, and all data. Deployments after
  launch will be run by our internal IT team.
- Environments required: development, staging, production. Support hours are 08:00–18:00 UK time on
  business days, with a 4-hour response target for issues that prevent renewal and next-business-day
  response for everything else.

### 8. Delivery and acceptance

- Budget is approved at £180,000 excluding VAT, on a fixed-price basis.
- Target milestones: design sign-off by 30 April 2026, member-facing functions in UAT by 31 August
  2026, launch before 1 November 2026.
- Client-provided inputs: GitLab access, brand assets, Stripe and Sage sandbox credentials, and
  Appendix C. Our IT team owns these and is currently at capacity until mid-April.
- Acceptance: a member can complete renewal end to end, migrated records reconcile to the current
  database counts, and the renewal page sustains 900 concurrent users in a load test.
- Testing will be carried out by our internal team, led by our Membership Manager. We will provide
  anonymised test data and the UAT environment will be hosted by the supplier.
- Deliverables include administrator documentation, two staff training sessions, and three months of
  post-launch support.
- Assumption: we assume the existing Sage export file format will not change during the project.
- Scope changes will be handled through a written change request approved by Daniel Okafor, and by
  Fiona Grant where cost is affected.

### Appendix C — Learning platform SSO integration notes

*(Appendix C is referenced in this document but has not been supplied.)*
