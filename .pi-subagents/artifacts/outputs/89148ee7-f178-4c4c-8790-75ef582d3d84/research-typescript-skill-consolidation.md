# Research: TypeScript skill consolidation

## Summary
`vendor-auto/typescript-magician` is not missing because of its frontmatter: it is a valid auto-invoked skill by repository policy. The current active catalogue is instead stale: `setup.sh` discovers every `SKILL.md` below `skills/`, `vendor/`, and `vendor-auto/`, but the last-sync manifest and live `~/.agents/skills/` contain `typescript-advanced-types` and not `typescript-magician`. Safely make `typescript-magician` canonical first, then run the installer and verify discovery before retiring the old skill.

The two skills substantially overlap, but `typescript-magician` is the better canonical entry because it has a task loop and explicit progressive-disclosure rule links. Preserve the handful of advanced-types topics not already covered as small, purpose-specific references or branches—not by moving its tutorial into the canonical manifest.

## Findings

1. **[High] Current registration is installer-driven, and the active list is stale.** A directory is a skill exactly when it contains `SKILL.md`; `setup.sh` enumerates `skills/`, `vendor/`, then `vendor-auto/`, applies invocation policy, and links each resulting directory to `$HOME/.claude/skills` and `$HOME/.agents/skills`. It records names in `$XDG_STATE_HOME` (or `~/.local/state`)/`jason-skills/synced-skills`. The installed state file lists `typescript-advanced-types` but not `typescript-magician`; the former resolves at `~/.agents/skills/typescript-advanced-types/SKILL.md`, while the latter path does not exist. Thus the immediate cause is that the installer has not been rerun since `typescript-magician` was added (or since the old list was created), not a name collision or `disable-model-invocation` setting. [Installer source](../../../../setup.sh) [Current sync manifest](/Users/jraimondi/.local/state/jason-skills/synced-skills) [Live old-skill path](/Users/jraimondi/.agents/skills/typescript-advanced-types/SKILL.md)

2. **[High] The repository’s Pi-registration documentation and implementation disagree.** `README.md` says `setup.sh` links individual skills into `~/.pi/agent/skills/`; the current script’s `SKILLS_TARGETS` contains only `~/.claude/skills` and `~/.agents/skills`. It does link `AGENT.md` and extensions to `~/.pi/agent`, but neither TypeScript skill exists at the documented Pi path. Treat `~/.agents/skills` as the observed active discovery location for this session, and resolve this documentation/implementation discrepancy before claiming portable Pi registration. [README registration statement](../../../../README.md#install) [Script targets](../../../../setup.sh)

3. **`vendor-auto` makes both current skills model-invoked.** `setup.sh` assigns `auto` policy to `vendor-auto` and strips `disable-model-invocation` from a skill’s YAML frontmatter. Both TypeScript directories are in that root and neither manifest disables invocation. Retaining both means two model-invoked descriptions own overlapping triggers; merely changing README’s catalogue row would not satisfy the one-owner criterion. [Policy implementation](../../../../setup.sh) [Advanced-types manifest](../../../../vendor-auto/typescript-advanced-types/SKILL.md) [Magician manifest](../../../../vendor-auto/typescript-magician/SKILL.md)

4. **Coverage comparison: most advanced-types material is already represented by magician rules; magician also has several useful exclusive operational references.**

   | Topic | `typescript-advanced-types` | `typescript-magician` | Consolidation result |
   | --- | --- | --- | --- |
   | Generics, constraints, inference, `keyof` | Main tutorial | `rules/generics-basics.md`; `rules/deep-inference.md` | Covered; magician is more focused. |
   | Conditional/distributive types, `infer`, mapped/key-remapped, template literals | Main tutorial | Individual conditional, infer, mapped, and template-literal rule files | Covered; retain magician rules. |
   | Built-in utilities, `as const`/`typeof`, array element indexing | Brief utilities section | Dedicated utility, `as-const-typeof`, and array-index references | Magician has more useful, discoverable coverage. |
   | Guards, assertions, discriminated unions | Main tutorial | `rules/type-narrowing.md` | Covered, including exhaustive switching. |
   | Builder and deep transforms | Builder and DeepReadonly/DeepPartial examples | `rules/builder-pattern.md`; conditional/mapped rule material | Covered. |
   | Brands/opaque identifiers, overloads, error diagnosis | Not materialized as focused references | Dedicated opaque, overload, and error-diagnosis rule files | Unique useful magician material. |
   | Type-safe API-client example | Detailed example | No dedicated reference found | Useful advanced-types-only example; preserve only if API-client type design is an intended branch. |
   | Type-safe form-validation example | Detailed example | No dedicated reference found | Useful only as an application-pattern reference. |
   | State-machine/reducer example | Detailed example | No dedicated reference found | Useful only as a state-modeling reference. |
   | Type-level equality/expected-error test helpers | Dedicated section | Manifest asks for tests, but no linked type-test reference | Useful advanced-types-only material; preserve as a concise type-testing reference. |
   | Performance limits for recursive/conditional types | Brief section | Not surfaced by linked rules | Useful advanced-types-only caution; add to an appropriate type-level rule, not the main workflow. |

   The table is based on the old tutorial’s headings and examples and the canonical manifest’s explicit links; the latter is the authoritative reachable-reference list. [Advanced-types tutorial](../../../../vendor-auto/typescript-advanced-types/SKILL.md) [Magician reference index](../../../../vendor-auto/typescript-magician/SKILL.md#reference) [Generics rule](../../../../vendor-auto/typescript-magician/rules/generics-basics.md) [Conditional-types rule](../../../../vendor-auto/typescript-magician/rules/conditional-types.md) [Mapped-types rule](../../../../vendor-auto/typescript-magician/rules/mapped-types.md) [Narrowing rule](../../../../vendor-auto/typescript-magician/rules/type-narrowing.md) [Diagnosis rule](../../../../vendor-auto/typescript-magician/rules/error-diagnosis.md)

5. **[Medium] The canonical instructions need narrow safety corrections before promotion.** The current step 1 assumes `tsc --noEmit` is appropriate from the current directory, which fails for repositories that typecheck via a package script, project reference, workspace package, or non-root config. Step 4 commands removal of *all* `any`, which conflicts with sound, documented interop boundaries. The main body also requires theory, multiple approaches, before/after representations, and comprehensive type tests for *every* challenge—too expensive for simple compiler-error repairs. The repository’s own skill-writing guidance favors instructions that form a reliable process and progressive disclosure behind explicit branches. [Current workflow](../../../../vendor-auto/typescript-magician/SKILL.md#instructions) [Skill-writing guidance](../../../../vendor-auto/writing-for-agents/SKILL.md)

## Recommended implementation

Perform this in the following order; do not delete or disable the old skill before step 4 verifies the replacement is discoverable.

1. **Refine `vendor-auto/typescript-magician/SKILL.md` as the sole trigger.** Keep its current description as the one broad TypeScript-type/type-error trigger surface, but streamline body instructions to:
   1. inspect `package.json`, workspace layout, and `tsconfig*.json` (or ask when unavailable) and select the narrowest project-native typecheck command;
   2. run that command and record the relevant errors;
   3. locate the root cause and load only the relevant `rules/*.md` link(s);
   4. implement the smallest sound type change, using `unknown`/narrowing at untrusted boundaries and retaining a documented intentional `any` only where it is the correct boundary;
   5. rerun the same typecheck command and any focused existing test/type-test command.

   Retain the present explicit rule index. Make explanations, alternatives, and type tests conditional: provide them when requested, when changing public/reusable type APIs, or when the project has an established type-test harness.

2. **Preserve only genuine old-skill gaps as disclosed references.** Add a compact `rules/type-tests.md` (equality assertions plus `@ts-expect-error`, with the project’s existing harness when present) and add a short complexity/recursion caution to `conditional-types.md` or `mapped-types.md`. Add an `application-patterns` reference only if API-client, form-validation, and reducer/state-machine designs are supported use cases; otherwise retire those tutorial examples. Do not copy the 724-line tutorial into `SKILL.md`.

3. **Update the active catalogue documentation.** Replace the README’s `typescript-advanced-types` row under “Language deep-dives” with `typescript-magician` and its task-oriented summary. Also correct the README’s Pi-target claim to match the installer, or add `~/.pi/agent/skills` to `SKILLS_TARGETS` if that is the intended Pi discovery contract. This is a separate registration bug from the consolidation itself.

4. **Register and prove canonical discovery.** Run `./setup.sh` after the canonical manifest exists. Confirm that the state manifest and each intended live target contain `typescript-magician`, its resolved `SKILL.md` has no `disable-model-invocation: true`, and the active Pi catalogue exposes it. Only after that proof, remove `vendor-auto/typescript-advanced-types` (preferred) or move it to `vendor/` with a minimal user-invoked compatibility pointer that delegates to magician and does not repeat triggers. A pointer remaining in `vendor-auto` would violate the one-model-invoked-owner acceptance criterion.

5. **Validate after removal/move.** Rerun `./setup.sh`, verify stale-link cleanup removes the advanced-types target, check all links in magician’s `SKILL.md`, and inspect the active catalogue for exactly one TypeScript skill in this trigger space.

## Sources

- Kept: [setup.sh](../../../../setup.sh) — primary implementation of skill discovery, target linking, invocation normalization, precedence, and stale-link removal.
- Kept: [README.md](../../../../README.md) — stated repository convention and current active-catalogue entry; exposes the Pi-target discrepancy.
- Kept: [synced-skills state manifest](/Users/jraimondi/.local/state/jason-skills/synced-skills) — direct evidence of the last successful installer catalogue.
- Kept: [live advanced-types manifest](/Users/jraimondi/.agents/skills/typescript-advanced-types/SKILL.md) — direct evidence of current old-skill exposure.
- Kept: [typescript-advanced-types](../../../../vendor-auto/typescript-advanced-types/SKILL.md) and [typescript-magician](../../../../vendor-auto/typescript-magician/SKILL.md) — primary comparison sources.
- Kept: [magician rules](../../../../vendor-auto/typescript-magician/rules/) — primary progressive-disclosure source inventory.
- Kept: [writing-for-agents](../../../../vendor-auto/writing-for-agents/SKILL.md) — repository-local authoritative guidance for invocation boundaries and progressive disclosure.
- Dropped: external TypeScript tutorials and secondary blog posts — they cannot establish this repository’s actual registration or catalogue behavior.

## Gaps

- This research environment has no shell/process execution capability, so it could not run `./setup.sh --dry-run`, inspect symlink metadata, query Pi’s runtime skill list, or inspect git’s index. The direct state file and readable live path establish the observed stale state, but the exact Pi version-specific relationship between `~/.agents/skills` and the runtime catalogue still needs an implementation-time check.
- Rule-file coverage was compared from the canonical manifest’s explicit reference index and representative primary rule contents. Before deleting the old tutorial, perform a final file-level inventory of every rule to ensure no referenced file is missing and decide whether the three application examples merit a new disclosed reference.

```acceptance-report
{
  "criteriaSatisfied": [
    {
      "id": "criterion-1",
      "status": "satisfied",
      "evidence": "Concrete high/medium findings identify setup.sh target and policy code, the stale state manifest, live ~/.agents path, both TypeScript manifests, and the exact safe registration/removal sequence."
    }
  ],
  "changedFiles": [
    ".pi-subagents/artifacts/outputs/89148ee7-f178-4c4c-8790-75ef582d3d84/research-typescript-skill-consolidation.md"
  ],
  "testsAddedOrUpdated": [],
  "commandsRun": [
    {
      "command": "./setup.sh --dry-run",
      "result": "not-run",
      "summary": "No shell/process-execution tool was available to this research subagent; recommended as the first implementation validation."
    }
  ],
  "validationOutput": [
    "Read primary local setup.sh, README.md, both TypeScript SKILL.md files, representative magician rules, ~/.local/state/jason-skills/synced-skills, and the live ~/.agents/skills/typescript-advanced-types/SKILL.md path.",
    "Confirmed state manifest contains typescript-advanced-types and omits typescript-magician; confirmed live old-skill path is readable and live magician path is absent."
  ],
  "residualRisks": [
    "Pi-specific discovery of ~/.agents/skills versus the README-claimed ~/.pi/agent/skills remains unexecuted and must be checked after installer changes.",
    "Do not remove or move typescript-advanced-types until typescript-magician is demonstrably present in the active catalogue.",
    "Repository git index was not queryable; no files were staged by this research task."
  ],
  "noStagedFiles": true,
  "diffSummary": "Added one research brief only; no skill manifest, registration script, catalogue, or skill directory was modified.",
  "reviewFindings": [
    "high: README.md installation claim and setup.sh targets disagree about ~/.pi/agent/skills.",
    "high: current synced catalogue exposes typescript-advanced-types but not typescript-magician.",
    "medium: magician workflow assumes root tsc --noEmit and mandates removal of every any."
  ],
  "manualNotes": "No manifests were changed and no skills were deleted. Run setup.sh and validate active Pi discovery before carrying out the proposed retirement."
}
```