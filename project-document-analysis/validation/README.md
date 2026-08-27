# Validation — Project Document Analyzer

Synthetic fixtures and sample reports used to check that [`../project-document-analyzer.md`](../project-document-analyzer.md) produces a correctly structured Project Understanding Report.

These files are **examples for validating the analyzer**. They are **not** real client documents and are **not project evidence** unless you explicitly select a fixture in [`../PROMPT.md`](../PROMPT.md) for a test run.

For real analyses, use [`../PROMPT.md`](../PROMPT.md) and list only the project documents to analyze.

---

## Layout

```text
validation/
  fixtures/   # synthetic input documents
  reports/    # sample reports generated against those fixtures
```

---

## Sample cases

| Case | Fixture(s) | Report | What it exercises |
| --- | --- | --- | --- |
| A — Thin new-project brief | `fixtures/case-a-thin-new-project-brief.md` | `reports/case-a-report.md` | New-project routing; sparse content; many Missing answers; extraction cues that are "not raised" |
| B — Existing-system replacement | `fixtures/case-b-existing-system-replacement-brief.md` | `reports/case-b-report.md` | Existing-project routing for a replacement; release detail retained under Q12/Q49; billing, asset licensing, locale, support targets, UAT ownership cues; unavailable Appendix C; explicit document assumption |
| C — Conflicting documents | `fixtures/case-c-doc1-requirements-summary.md`, `fixtures/case-c-doc2-technical-addendum.md` | `reports/case-c-report.md` | Multi-source Conflicting statuses; no established precedence; phase differences kept distinct from contradictions |

---

## Checks performed

Against each sample report:

1. **Checklist completeness** — Q1–Q58 appear exactly once, in order.
2. **Status discipline** — each of Q1–Q54 has exactly one Status, Evidence, and Uncertainty field; Q55–Q58 use Answer / Basis / Limitations and are excluded from the dashboard.
3. **Routing** — New cases mark Section 3 Not applicable; Existing cases mark Section 2 Not applicable; useful release information is not discarded.
4. **Evidence** — Stated, Partially stated, and Inferred answers cite a source; Missing uses the prescribed evidence sentence; Conflicting cites both sides; quoted fragments match the fixtures.
5. **Partial answers** — Partially stated answers disclose the unresolved parts.
6. **Dashboard and Q55** — status counts and question-ID lists total 54 and match the assigned answers.
7. **Key Clarifications** — every row reuses a gaps-register ID and classification; no new findings appear only in the summary.
8. **No invention** — requirements, blockers, estimates, and recommendations are not invented beyond the fixture evidence.

---

## Limitations

- These three cases were written for this analyzer and analysed in a single run. They show that the report format is internally consistent and self-checkable; they do **not** prove consistency across AI models or across repeated runs.
- One underlying conflict can produce several Conflicting question statuses (Case C). The gaps register is the place to see the deduplicated issues.
- Blocking classification and whether an extraction cue is materially relevant remain analyst judgments, so those areas are where run-to-run variation is most likely.
- Fixtures and reports must stay separate from the reusable analyzer. Do not promote a sample report into the analyzer, and do not add real client documents here.

---

## Provenance

All fixtures and reports in this folder were authored as synthetic validation material for analyzer version 1.1. They contain no real client information or credentials. Mentions of credentials, sandbox access, or passwords in Case B describe fictional process requirements only; no secret values are present.
