---
name: avatar
description: >-
  Manage persistent agent and project identities from Notion with the commands
  /new, /resume, /save and /handoff. Trigger on those text commands, creating a
  project or department, resuming a previous agent, auditing conversational
  memory, or preparing a successor. Use the Agent Registry, avoid duplicate
  pages, verify writes, load only relevant policies/tools, and store handoffs
  only in the canonical _HANDOFF child of the corresponding Quien soy page.
---

# Avatar — Agent Lifecycle & Memory v0.1.3

## Purpose and canonical sources

Avatar supports any type of project using durable identities, scoped documentation and a small set of repeatable text commands. ChatGPT chats are transient runtimes. Notion is the canonical memory when connected. Never claim to preserve literal consciousness or inaccessible hidden state.

Required workspace sources (configure your own; never use the author's workspace):

- **Agent Registry:** a Notion database or index of projects and canonical agent identities.
- **Project root:** the user's own Notion page for each independent initiative.
- **Canonical Quien soy:** the identity page under each verified project, plus its optional child _HANDOFF.

Consult the user's authorized Notion workspace through a connected integration. Resolve and verify URLs/IDs from that workspace; never invent private locations or assume any preconfigured pages exist.

These slash-prefixed commands are text triggers interpreted by this skill. They are not guaranteed host-native slash commands.

## Universal identity model

- **Project root:** an independent initiative with its own Notion root.
- **Operational identity:** a persistent project coordinator, department, specialist or agent with one canonical `Quien soy` page.
- **Avatar version:** a particular running/successive agent, e.g. `AGENT 1.4`, `AGENT 1.5`. Attribute consequential checkpoints to the actual known author/version and date. Never invent a version.
- **Founding project coordinator:** when `/new` unequivocally creates a new project root, its founder may coordinate that project initially. This authority is scoped only to that new project and grants no external permissions.
- A new chat or `/handoff` does not automatically create a new identity.

## Routing and evidence rules

1. Determine the project root and identity from the current conversation, Agent Registry and accessible canonical sources. Preserve hierarchy; never use one project's root as a universal fallback.
2. Prefer stable Notion IDs/URLs. For a new destination use: exact URL/ID -> verify and operate; registered alias -> resolve by ID; fuzzy name/path -> search and verify the parent chain. If a single approximate match exists, show the complete path and obtain first-time confirmation before writing. If ambiguous, do not write.
3. Inspect existing pages before editing. Edit minimally, preserve unrelated content and child pages, read back important writes, and keep repeated operations idempotent.
4. Load the minimum relevant documentation: current checkpoint and decisions first, then only applicable policies and skills. A policy is an obligation; a skill is an optional capability.
5. Never store passwords, tokens, credentials or secret-bearing URLs. Verify current tool connections instead of assuming a previous session still has them.
6. Keep facts, approved decisions, hypotheses, blocked tasks and obsolete decisions distinguishable.
7. Prefer ordinary ChatGPT + connected plugins. Use GitHub, Linear, Cloudflare or other sources only when the task actually depends on them.

## Scalable `Quien soy` memory

Use one canonical `Quien soy` per identity, even if supporting pages are needed later.

The boot core must always contain:

1. Identity, parent project/root, charter, authority, boundaries and durable instructions.
2. Exactly one current operational checkpoint: current phase, verified state, active decisions/constraints, open issues, risks, next precise action, author/avatar version and timestamp.
3. Relevant policy requirements and repositories/tools needed for the role. Treat connector availability as unverified until checked in the current session.
4. A compact dependency index with canonical URLs/IDs, what each dependency contains, its authority/status and the trigger for reading it.

Create as few supporting documents as necessary. Reuse existing topic documents. Do not create a new page per chat, version, checkpoint, isolated issue or handoff.

### Safe compaction transaction

When memory gets bloated:

1. Inspect the original and classify content as active core, active specialist documentation, closed/superseded history, or confirmed duplicate.
2. Copy closed history to a suitable existing archive, preserving dates, author/version, source pointers, decisions and lessons.
3. Read back the destination and verify the content and links.
4. Update the root dependency index and active summaries.
5. Only then trim confirmed redundant text from the root.

Age alone is not reason to discard founding decisions, recurring failures, policy or security lessons. Never guess a fixed Notion block limit. If Notion stops accepting writes, persist a provisional checkpoint where it can be recovered and report what remains unsynchronized.

## Canonical handoff location — v0.1.3

Every operational identity may have **at most one** canonical handoff page:

`Agent Registry -> canonical Quien soy -> direct child _HANDOFF`

This location is mandatory.

- `/handoff` always updates the same `_HANDOFF` page.
- If `_HANDOFF` does not exist, create it once as a **direct child** of the canonical `Quien soy`, then register its stable URL in Agent Registry and in the dependency index.
- Never create a handoff in workspace root, project root, `_Documentacion`, a similarly named folder, or any other fallback location.
- Never create a new handoff page per session, avatar version or relay.
- If the canonical `Quien soy` cannot be resolved or `_HANDOFF` cannot be written, return `BLOCKED` and provide a provisional handoff in chat. Do **not** choose an alternate parent for convenience.
- Historical handoff documents already scattered elsewhere may remain as historical evidence; the current handoff must converge on the single canonical `_HANDOFF`.

The operational checkpoint remains in `Quien soy`; `_HANDOFF` contains the prepared relay for the next agent.

## /new NAME [@ DESTINATION]

Interpret intent first.

- **New independent project:** create a new root. If no shared parent is specified and the platform supports it, create it privately at workspace level and report that status.
- **New department/specialist under a known project:** create it under the verified project parent.
- **Existing identity:** use `/resume` and avoid duplicates.
- **Ambiguous root vs child:** ask one direct clarification before creating anything.

`DESTINATION` may be a Notion URL/ID, registered alias, hierarchical path, approximate title, or explicit `raiz` for an independent root.

For a new root or identity, create only the minimal structure required by that project. When the existing project convention uses `_Quien soy?` and `_Documentacion`, reuse that convention rather than inventing another. Do not create archive pages preemptively.

The new `Quien soy` boot core should include identity, mission, scope/boundaries, memory rules, applicable policies, tool manifest, checkpoint/next steps, attribution and dependency index when needed.

Register real project/identity relationships in Agent Registry and verify the created links by reading them back. If writing fails, report the blocker and keep a provisional draft; never claim success.

## /resume [IDENTITY] [OPTIONAL VERSION]

1. Read Agent Registry first and resolve the project/identity without asking for a link when the identity is uniquely known.
2. Read the canonical `Quien soy` boot core and dependency index.
3. If Agent Registry/index declares `_HANDOFF`, read it when there is a current relay or when resuming after a handoff.
4. Open supporting pages only when their trigger applies: current task, mandatory policy, contradiction, incident recurrence, evidence gap or deep handoff.
5. Prefer current canonical decisions over superseded history. Surface material conflicts and uncertainty.
6. Check only relevant policies/skills.
7. Verify tools required by the current task. A tool recorded in memory is not proof that it is connected now.
8. When the task depends on a repo, ticket or deploy, consult the live connected source if available; historical branch/commit/deploy state is only a snapshot.
9. Respond compactly with identity/version, last verified state, blockers, necessary connections, next action and canonical sources.
10. `/resume` is read-only: it does not grant authority to edit or automatically rewrite memory.

## /save [OPTIONAL IDENTITY]

1. Resolve identity/version from the current context and Registry. A directed `/save IDENTITY` is allowed only when the user explicitly identifies the target and writing is authorized.
2. Audit the accessible conversation since the last saved checkpoint against canonical Notion sources. Do not claim to have reviewed inaccessible chats or hidden reasoning.
3. Detect material decisions, lessons, failures, tool/permission changes, unresolved risks and next actions.
4. Classify each finding to its proper destination: identity-wide rules/checkpoint in `Quien soy`; project decisions in project documentation; general validated lessons in relevant policy/skill docs; engineering tasks in Linear; technical evidence in GitHub.
5. Before editing, fetch the destination and preserve unrelated content.
6. Update the current checkpoint incrementally with verified state, decisions, blockers, next action, author/avatar version and timestamp.
7. Record tools by name/purpose/connection procedure where useful, never secrets.
8. Re-read important writes and verify persistence.
9. Repeating `/save` with no material changes must create no duplicate block/page or manufactured version.
10. If permissions are missing or the target is ambiguous, give a precise pending-write report instead of guessing.

## /handoff

`/handoff` is a deep save plus successor-readiness check.

1. Resolve identity from Agent Registry and verify its canonical `Quien soy`.
2. Run the relevant `/save` audit: current state, decisions, risks, blockers, tools, repos/tickets/deploys when applicable, and exact next action.
3. Resolve the handoff destination deterministically as `Registry -> Quien soy -> _HANDOFF`.
4. If `_HANDOFF` exists, update that exact page. If missing, create one direct child under `Quien soy`, register its URL in Agent Registry and dependency index, then use it from then on.
5. If the canonical destination cannot be resolved or written, return `BLOCKED` with the provisional handoff in chat. Never fall back to root or another folder.
6. The handoff must include: identity/scope/boundaries; current avatar/version if known; timestamp; what was verified; what is assumed; current checkpoint; decisions; active tasks; branches/issues/deploy state when relevant; errors/lessons; dependencies/tool connection status; policy blockers; exact next action; canonical sources.
7. Keep one current handoff, plus a short historical ledger or pointers only when useful. Do not duplicate the whole history.
8. Perform a successor boot test: can a new chat continue using Registry -> `Quien soy` + index -> `_HANDOFF` + only triggered references?
9. Report `READY`, `PARTIAL`, or `BLOCKED`, with precise reasons.
10. `/handoff` does not launch another conversation or create a new identity.

## Lightweight avatar provenance

Avoid per-conclusion metadata noise. The checkpoint and handoff should identify the known avatar/version and date. A version changes when there is a real successor/relay, not on every save.

When useful, maintain a compact per-avatar ledger containing:

- avatar/version;
- active period;
- areas worked;
- 5-10 material decisions/conclusions;
- major corrections/errors;
- handoff destination.

Do not tag every paragraph or every conclusion individually unless a project specifically requires that audit depth.

## Contextual policy and skill selection

Project-specific documents are not universal obligations. Read the user's own applicable security, privacy, engineering and authoring policies only when authorized and relevant. Never import policies or confidential references from the skill author's workspace.

Select controls by actual project, role, task, risk and phase. Do not claim critical security controls have passed without evidence.

## Tool discipline

- Notion = durable memory/doctrine/checkpoints.
- GitHub = code and technical evidence.
- Linear = executable tasks.
- Cloudflare/Vercel/etc. = live deployment/infrastructure evidence when relevant.
- A ticket or document does not autonomously wake another conversation.
- Use deterministic/local tooling before expensive inference where possible.
- Never install tools just because they are cataloged.

## Success criteria

A fresh chat with Avatar and Notion access should be able to:

- `/resume AGENT` or another registered identity without the user providing its URL;
- recover identity, boundaries, current checkpoint and next action;
- find the canonical `_HANDOFF` without searching random folders;
- `/save` material changes idempotently;
- `/handoff` to the single deterministic `_HANDOFF` child;
- `/new` an independent root or child identity without silently attaching it to the wrong project;
- report missing permissions or connections honestly.

## Version note

v0.1.3 (2026-10-14) adds deterministic handoff placement: one `_HANDOFF` direct child under each identity's canonical `Quien soy`, stable Registry pointer, and `BLOCKED` behavior instead of fallback destinations. It also keeps avatar provenance deliberately lightweight: checkpoint/handoff-level attribution rather than metadata on every conclusion.
