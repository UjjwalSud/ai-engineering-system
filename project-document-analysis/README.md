# Project Document Analysis

Reusable AI instructions for turning a client brief, CSR, SOW, specification, or related project documents into a consistent **Project Understanding Report**.

The analyzer extracts what the documents say, marks what is uncertain or missing, surfaces conflicts, and drafts clarification questions. It does not invent requirements, recommend a stack, estimate effort, or write code.

---

## Contents

| File | Role |
| --- | --- |
| [`PROMPT.md`](./PROMPT.md) | Canonical invocation prompt (copy/paste this) |
| [`project-document-analyzer.md`](./project-document-analyzer.md) | Analysis framework, checklist (Q1–Q58), and report format (version 1.1) — **instructions, not project evidence** |
| [`validation/`](./validation/) | Synthetic fixtures and sample reports — **examples for testing the analyzer, not project evidence** unless you explicitly select a fixture |

---

## How to use

1. Open [`PROMPT.md`](./PROMPT.md) and copy the marked copy/paste block.
2. Fill in the placeholders:
   - `[ANALYZER_FILE_PATH]` — path to `project-document-analyzer.md`
   - `[EXPLICIT_FILE_PATHS_OR_ATTACHMENT_NAMES]` — only the project documents to analyze (one per line)
   - `[OUTPUT_DIRECTORY]` — folder for `project-understanding-report.md`
3. Paste the filled prompt into an AI conversation or coding assistant, with the listed documents attached or reachable at those paths.
4. Review the report. If the output file already exists, the prompt requires the AI to ask before replacing it.

**Evidence vs instructions:** only the documents you list (plus any clarifications you type in the chat) are project evidence. The analyzer, this README, `PROMPT.md`, and the validation fixtures/reports are not, unless you deliberately select a fixture for a test run.

---

## Related

- Invocation prompt: [PROMPT.md](./PROMPT.md)
- Validation cases and checks: [validation/README.md](./validation/README.md)
