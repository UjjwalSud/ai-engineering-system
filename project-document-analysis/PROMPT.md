# Project Document Analysis — Invocation Prompt

This is the **canonical invocation prompt** for [`project-document-analyzer.md`](./project-document-analyzer.md).

Copy the block below into an AI conversation or coding assistant after filling in the placeholders. Do not paste the analyzer text into the prompt; reference its path instead.

## What counts as evidence

| Role | Files |
| --- | --- |
| Instructions | `project-document-analyzer.md`, this `PROMPT.md` |
| Project evidence | Only the files you list under **Project documents to analyze**, plus any explicit project clarifications you add in the conversation |
| Examples / not evidence | Repository READMEs, `validation/fixtures/`, `validation/reports/`, and other repo docs — unless you explicitly select a fixture for a test |

---

## Placeholders

Replace every bracketed token before sending:

| Placeholder | Replace with |
| --- | --- |
| `[ANALYZER_FILE_PATH]` | Absolute or workspace-relative path to `project-document-analyzer.md` |
| `[EXPLICIT_FILE_PATHS_OR_ATTACHMENT_NAMES]` | One path or attachment name per line for each project document to analyze |
| `[OUTPUT_DIRECTORY]` | Directory where `project-understanding-report.md` should be saved |

---

## Copy / paste block

```text
Use [ANALYZER_FILE_PATH] as the analysis instructions and checklist,
never as project evidence.

Project documents to analyze:
[EXPLICIT_FILE_PATHS_OR_ATTACHMENT_NAMES]

Output:
[OUTPUT_DIRECTORY]/project-understanding-report.md

Read the analyzer and every listed project document. Generate the
complete report using the analyzer's rules and exact report format.

Only the explicitly listed project documents and my explicit project
clarifications are evidence. Do not treat repository READMEs, the
analyzer, this prompt, validation fixtures, or sample reports as
project evidence unless I explicitly select a fixture for a test.

Disclose unreadable or unavailable content. If no project documents
are specified or accessible, ask me to provide them.

Complete the analysis from available evidence and put clarification
questions inside the report. Do not invent requirements, recommend
solutions, estimate effort, or write code.

Preserve full checklist coverage, but keep answers concise and use
cross-references instead of repeating explanations.

Save the report if file creation is supported; otherwise return
Markdown. Do not modify source documents or analyzer files.
If the output file already exists, ask before replacing it.
```
