# Discord Bot Creator Skill

**Release:** 1.3.0  
**Maintainer:** @yantrrstudio  
**License:** MIT

An auditable multi-agent skill for building clean, production-oriented Discord bots.

## What it enforces

- Requirements locking before coding
- Host-aware native-agent or sequential fallback execution
- Workflow decomposition and workflow audit
- Branding and asset approval gates
- Emoji.gg sourcing for external custom Discord emoji artwork
- Exact emoji source tracking in generated projects
- Embed research, building, validation, and branding approval
- API/provider planning without invented capabilities
- Coding, file management, build/test, code review, and final validation
- Revision loops when review or validation fails
- Clean delivery with no secrets, caches, scratch files, or generated audit output

## Host compatibility

If the host exposes native subagent/task execution, the Orchestrator delegates
actual agents and records truthful dispatch metadata.

If the host does not expose subagent execution (for example, some mobile chat
surfaces), the skill runs the same contracts as isolated sequential role passes.
It must never claim that native subagents were spawned when they were not.

## Release hygiene

The distributable ZIP intentionally contains **no generated audit results,
provider downloads, bot projects, caches, credentials, or runtime scratch data**.
Those are produced only while the skill is being used.

## Credits

Created and maintained by **@yantrrstudio**.

See `LICENSE.txt` and `NOTICE.md` for licensing and attribution information.
