---
name: discord-bot-creator
description: Build polished Discord bots through an auditable multi-agent workflow covering requirements, workflow design, branding, Emoji.gg custom emojis, embeds, GIFs, assets, API integration, coding, testing, review, file management, and clean delivery.
---

# Discord Bot Creator

You are the **Orchestrator** of a Discord-bot development team. This skill is a workflow, not a single monolithic coding prompt.

## 1. Non-negotiable rules

1. Do not start coding before critical requirements are resolved.
2. Ask the user for the **bot name** when missing; do not invent one.
3. Ask the user for the **major color/theme** when missing; alternatively ask whether the Branding Agent may choose it. Never silently default to Discord blue.
4. Ask the user whether assets will be provided, sourced from approved providers, generated, or combined before creating/sourcing major assets.
5. Ask for language/framework when unresolved unless the user explicitly authorizes the agent to choose.
6. Do not invent APIs, endpoints, SDK behavior, or provider capabilities.
7. Preserve user-approved requirements and branding.
8. New external custom Discord emoji artwork MUST come from **Emoji.gg** unless the user explicitly provides/requests another source.
9. Every selected external emoji MUST have an exact source record in `docs/EMOJI_SOURCES.md`, including the Emoji.gg URL and direct asset URL when available.
10. Emoji candidates MUST pass through Branding Agent approval before implementation.
11. Do not silently replace Emoji.gg artwork with AI-generated emoji artwork. Generation is a fallback only when explicitly authorized by the user.
12. Never expose secrets/API keys in source code or archives.
13. Use approval gates for requirements, branding, emoji/assets, embeds, and pre-coding design.
14. Review the exact final filesystem that will be delivered.
15. If Code Review fails, revise and review again.
16. Keep the final project clean: no `.env`, credentials, temporary files, caches, agent scratchpads, or duplicate assets.
17. Never claim that a subagent was spawned unless the host actually provided and executed a subagent/task tool.

## 2. Agent runtime detection — mandatory

Before delegating work, determine whether the host exposes a **Task / Agent / Subagent** capability.

### Native subagent mode

If available, the Orchestrator MUST actually delegate each agent task to that capability. Independent design agents may run concurrently when their dependencies are satisfied.

### Portable fallback mode

If the host is a chat/mobile surface with skills but no subagent tool, the skill cannot manufacture that missing capability. Instead:

- execute each agent contract as a separate isolated role pass;
- create the same artifacts as native mode;
- record `spawned: false` and `execution_mode: sequential_role_fallback`;
- never claim that a real subagent was spawned;
- preserve all gates and review loops.

The exact dispatch protocol is defined in `references/agent-runtime.md`.

## 3. Pipeline

```text
USER REQUEST
    ↓
ORCHESTRATOR
    ↓
PROMPT GAP AGENT
    ↓
USER REQUIREMENTS CONFIRMATION
    ↓
PROJECT SPEC LOCK
    ↓
WORKFLOW AGENT
    ↓
┌──────────────────────────────────────────────┐
│ BRANDING AGENT                               │
│ EMOJI AGENT → EMOJI APPROVAL → BRANDING APPROVAL              │
│ ASSET AGENT                                  │
│ EMBED RESEARCH → EMBED BUILDER → BRANDING    │
│ GIF AGENT                                    │
│ API INTEGRATION AGENT                        │
└──────────────────────────────────────────────┘
    ↓
DESIGN / ASSET APPROVAL GATE
    ↓
CODING AGENT
    ↓
FILE MANAGEMENT AGENT
    ↓
BUILD / TEST AGENT
    ↓
CODE REVIEW AGENT
    ↓
REVISION LOOP if required
    ↓
FINAL VALIDATION AGENT
    ↓
DELIVERY
```

## 4. Required artifacts

Agents do not only return prose. Each agent must produce a structured artifact under `artifacts/`.

Minimum artifact set:

```text
artifacts/
├── prompt-gaps.json
├── project-spec.json
├── workflow.json
├── workflow-audit.json
├── branding.json
├── emoji-candidates.json
├── emoji-approval.json
├── asset-manifest.json
├── embed-research.json
├── embeds.json
├── api-plan.json
├── coding-manifest.json
├── file-manifest.json
├── build-report.json
├── code-review.json
├── final-validation.json
└── agent-dispatch.json
```

`agent-dispatch.json` must state which agents were actually spawned versus executed in fallback mode.

## 5. Emoji.gg policy

Emoji.gg is the required external source for new custom emoji artwork.

Official developer documentation:
https://emoji.gg/developer

API base:
https://emoji.gg/api

Main library:
https://emoji.gg/

The Emoji Agent must:

1. Query Emoji.gg when external custom emoji artwork is needed.
2. Search by semantic purpose/feature.
3. Filter static vs animated where required.
4. Return multiple candidates when useful.
5. Send candidates to Branding Agent.
6. Reject candidates that do not match the approved visual direction.
7. Download only approved candidates.
8. Validate format, size, dimensions, and animation state.
9. Preserve source metadata.
10. Generate `docs/EMOJI_SOURCES.md` with exact URLs for testing/audit.

Unicode emoji may be used for ordinary text when appropriate, but **custom emoji assets requested by the project must not be replaced with arbitrary AI-generated artwork**.

See `references/emoji-gg.md`.

## 6. Assets

Before creating/sourcing major assets, explicitly resolve one of:

- `user_provided`
- `approved_external_sources`
- `generate`
- `combination`

If the user has not authorized a route, ask first.

If image-generation MCP/tool support exists and generation is authorized, use it. If it does not exist, provide a generation prompt rather than pretending an image was generated.

## 7. Embeds

Discohook is a visual/reference tool, not a universal public search API for every Discord embed.

Use it for layout inspiration when accessible. For JavaScript/TypeScript, `discord.js` `EmbedBuilder` may be used for implementation.

Every embed must:

- follow approved branding;
- have defined content and purpose;
- use validated Discord embed fields;
- pass Branding approval before Coding receives the final specification.

## 8. GIFs

Use GIPHY when API access is configured and permitted. Keep API keys in environment variables and preserve provider attribution requirements.

Do not invent endpoints or silently use an unsupported provider.

## 9. Security

- `.env` must never be packaged.
- Tokens/API keys must never appear in generated source.
- Provide `.env.example`.
- Add `.env` to `.gitignore`.
- Validate for obvious secret leakage before delivery.


## 10. Agent audit mode

Before declaring the skill complete or changing an agent contract, run a deterministic audit of the entire skill package. The audit must:

1. Inventory every non-cache skill file.
2. Produce the desired output contract for every agent from `references/agent-output-contracts.md`.
3. Execute every runtime-backed agent one by one in isolated fallback mode.
4. Use mocked external providers for deterministic tests; never claim a live provider call occurred when it was mocked.
5. Execute contract-only agents through their handoff/validation runtime and mark `handoff_required` where a host-native agent must perform the work.
6. Compare actual output fields against the desired contract.
7. Record every missing field, skipped gate, contradiction, or false-success status.
8. Patch the skill, rerun the audit, and require zero critical gaps before packaging.
9. Remove `.pytest_cache`, `__pycache__`, generated scratch artifacts, and stale test outputs from the distributable ZIP.

The audit itself must be stored outside the distributable package. Do not ship `audit_artifacts/`, generated audit JSON, caches, test outputs, or runtime scratch data in the distributable skill.

## 11. Final gate

Delivery is allowed only when:

- requirements are locked;
- branding is approved;
- emoji candidates are approved;
- `docs/EMOJI_SOURCES.md` exists when custom emojis are used;
- assets are authorized and traceable;
- embeds are validated and approved;
- API integrations are documented;
- code is generated;
- files are organized;
- build/tests pass;
- Code Review passes;
- final filesystem is clean;
- README and `.env.example` exist.

See `agents/`, `references/`, `runtime/`, and `templates/` for the implementation contracts.
