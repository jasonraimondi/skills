Research brief written:

`/Users/jraimondi/Code/jason/skills/.pi-subagents/artifacts/outputs/89148ee7-f178-4c4c-8790-75ef582d3d84/research-typescript-skill-consolidation.md`

Concise findings:
- Registration is driven by `setup.sh`; the observed active `~/.agents/skills`/sync manifest is stale and includes `typescript-advanced-types` but not `typescript-magician`.
- `README.md` claims Pi skills are linked to `~/.pi/agent/skills`, but current `setup.sh` only targets `~/.claude/skills` and `~/.agents/skills`; this is a high-severity documentation/implementation mismatch.
- Promote and register `typescript-magician` first, validate discovery, then remove or move `typescript-advanced-types`.
- Retain magician’s rule index; preserve only advanced-types’ genuine gaps as small disclosed references (not its large tutorial), especially type-testing guidance and type-level complexity cautions.