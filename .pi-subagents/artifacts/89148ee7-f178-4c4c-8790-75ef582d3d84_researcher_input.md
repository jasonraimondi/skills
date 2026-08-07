# Task for researcher

Research the consolidation proposal in /var/folders/bs/v8tgtdv17gs_r38126hngln80000gn/T/handoff-typescript-skill-consolidation.XXXXXX.md.vtExDKDJkj. Work in /Users/jraimondi/Code/jason/skills, read the relevant local skill manifests/config/discovery rules and any authoritative Pi documentation/source necessary to answer the handoff's open questions. Use primary sources. Determine the exact current registration mechanism, compare unique useful content between vendor-auto/typescript-advanced-types and vendor-auto/typescript-magician (including rules), and recommend a safe concrete implementation that meets acceptance criteria. Write a single cited Markdown research brief in the repository following existing research-note conventions (if none, choose a sensible docs/research path). Do not change manifests or delete skills. In your final response give the output file path and concise findings.

---
**Output:**
Write your findings to exactly this path: /Users/jraimondi/Code/jason/skills/.pi-subagents/artifacts/outputs/89148ee7-f178-4c4c-8790-75ef582d3d84/research-typescript-skill-consolidation.md
This path is authoritative for this run.
Ignore any other output filename or output path mentioned elsewhere, including output destinations in the base agent prompt, system prompt, or task instructions.

## Acceptance Contract
Acceptance level: checked
Completion is not accepted from prose alone. End with a structured acceptance report.

Criteria:
- criterion-1: Return concrete findings with file paths and severity when applicable

Required evidence: changed-files, tests-added, commands-run, residual-risks, no-staged-files, manual-notes

Finish with a fenced JSON block tagged `acceptance-report` in this shape:
Use empty arrays when no items apply; array fields contain strings unless object entries are shown.
`criteriaSatisfied[].status` must be exactly one of: satisfied, not-satisfied, not-applicable.
`commandsRun[].result` must be exactly one of: passed, failed, not-run.
`manualNotes` and `notes` are optional strings; an empty string means no note and does not satisfy `manual-notes` evidence.
```acceptance-report
{
  "criteriaSatisfied": [
    {
      "id": "criterion-1",
      "status": "satisfied",
      "evidence": "specific proof"
    }
  ],
  "changedFiles": [
    "src/file.ts"
  ],
  "testsAddedOrUpdated": [
    "test/file.test.ts"
  ],
  "commandsRun": [
    {
      "command": "command",
      "result": "passed",
      "summary": "short result"
    }
  ],
  "validationOutput": [
    "validation output or concise summary"
  ],
  "residualRisks": [
    "none"
  ],
  "noStagedFiles": true,
  "diffSummary": "short description of the diff",
  "reviewFindings": [
    "blocker: file.ts:12 - issue found, or no blockers"
  ],
  "manualNotes": "anything else the parent should know"
}
```