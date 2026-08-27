# Test Fixture C, Document 1 — Requirements Summary

> Synthetic test fixture for validating project-document-analyzer.md. Not a real client document.
> Purpose: paired with case-c-doc2-technical-addendum.md to exercise conflict detection, including
> one difference that is a phase difference rather than a genuine contradiction.

---

## Requirements Summary — "FleetCheck" Driver Inspection System

**Client:** Calder Logistics
**Document:** Requirements Summary, undated, circulated by email on 6 May 2026

### Purpose

Calder Logistics operates 260 vehicles. Drivers must complete a daily walkaround inspection before
leaving the depot. These are currently recorded on carbon-copy paper books. FleetCheck is a new
system to capture these inspections digitally and give the compliance team a searchable record.

There is no existing digital system for inspections.

### Users

- Drivers (approximately 300, including agency drivers) complete inspections.
- Depot managers review defects and authorise vehicles for use.
- The compliance team runs reports for audits by the traffic commissioner.

### Delivery expectation

We need a **full production system** in use across all four depots at go-live. Phased depot rollout
is not acceptable to the compliance team.

### Functional requirements

1. Driver completes a vehicle-specific inspection checklist on a handheld device.
2. Any defect must be classified as either advisory or safety-critical.
3. A safety-critical defect must immediately prevent the vehicle from being marked as available and
   must notify the depot manager.
4. Depot manager records the rectification and re-authorises the vehicle.
5. Compliance team searches inspections by vehicle, driver, depot, and date range, and exports to CSV.

### Non-functional requirements

- Inspections must be completable with no network connectivity, because two depots have no usable
  signal in the vehicle yard. Records sync when the device reconnects.
- Must work on the Samsung Android handhelds already issued to drivers.
- Retention: inspection records must be kept for 15 months to satisfy audit requirements.

### Hosting

The system must be hosted on **Amazon Web Services**, in line with our group IT standard.

### Timeline and budget

Go-live is required by **1 September 2026**. Budget has not been confirmed and will be discussed once
scope is agreed.

### Roles and approval

Sign-off on functionality sits with the Compliance Director. IT will review technical decisions.
