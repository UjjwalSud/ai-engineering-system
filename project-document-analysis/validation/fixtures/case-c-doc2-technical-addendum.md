# Test Fixture C, Document 2 — Technical Addendum

> Synthetic test fixture for validating project-document-analyzer.md. Not a real client document.
> Purpose: paired with case-c-doc1-requirements-summary.md. Contradicts it on hosting, deliverable
> type, offline capability, and retention. Also states a later-phase item that must not be reported
> as a contradiction.

---

## Technical Addendum — FleetCheck

**Client:** Calder Logistics
**Document:** Technical Addendum v0.3, dated 19 May 2026, prepared by Calder IT

### Deployment target

FleetCheck will be deployed to our **on-premises VMware cluster** at the Warrington data centre. We
are not currently approved to place operational data in public cloud environments.

### Delivery approach

Calder IT recommends delivering an **MVP for the Warrington depot only**, covering inspection capture
and defect notification, with the remaining depots and the compliance reporting module to follow in a
second phase.

### Connectivity

Devices will operate on the depot Wi-Fi network. The application can therefore be built as a standard
**online-only web application**; offline storage on the device is not required.

### Data retention

Inspection records are to be retained for **7 years**, consistent with our group data retention
schedule.

### Platform notes

- Preferred stack is .NET and SQL Server, though the addendum notes this is a preference rather than
  a mandate.
- Single tenant, internal users only.
- Authentication should use our existing Active Directory accounts for managers and compliance staff.
  Driver authentication method is still under discussion.
- Handheld devices are Samsung Android units running Android 11.

### Later phase

Automatic integration with the fleet maintenance system, Tranman, is planned for a **second phase**
and is not required at initial delivery.

### Support

Calder IT will operate the system after handover. Support hours and response targets have not yet
been agreed.
