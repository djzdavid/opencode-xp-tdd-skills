---
name: backlog
description: Manual workflow (@backlog). Turns a software idea into a short backlog of vertical user stories without technical details or acceptance criteria. Only use when explicitly invoked via @backlog.
disable-model-invocation: true
---

# Backlog

Your job is to help me split what I tell you into user stories. I bring the requirements; you order and slice them.

## OpenCode Guardrails
- **Manual trigger only:** Run only when I explicitly request this skill through `@backlog` or an equivalent explicit instruction. An optional configured `/backlog` command may also invoke it. Never activate another workflow automatically.
- **Read-only by default:** Inspect context without modifying files, installing tools, or implementing code. Save the backlog only if I explicitly request it; that permission does not authorize other project changes.

## Check project context first

Read the project's `AGENTS.md` if present and briefly inspect relevant directories and configuration. Use the documented stack, design conventions, and preparation status; compare them with the actual files without installing dependencies or running a full verification suite. Do not treat configuration files alone as proof that the environment works.

- **Necessary definitions missing or awaiting approval:** Briefly explain what is missing, suggest `@setup`, and stop before producing the backlog. Require design-system decisions only for projects with a visual interface. If an existing project's conventions are not documented, ask `@setup` to confirm and record them rather than propose a replacement stack.
- **Definitions approved but necessary preparation pending or partially complete:** Suggest `@story Preparar la base técnica y visual acordada en @setup` (omit visual work when not applicable) and stop. This is a separate preparation task, not a functional story to add to the backlog.
- **Foundation prepared:** Continue with the functional backlog without suggesting `@setup` again. If existing preparation is merely unverified, report that limitation for `@story` and `@pair` to check; do not equate missing verification with missing implementation. If the files clearly contradict the recorded status, flag the specific discrepancy and route to preparation or definition work as appropriate.

Only route away for decisions or preparation necessary to the requested feature. An optional skill or MCP pending installation must not block the backlog. Never invoke another skill automatically.

When routing away, include the original feature request and the suggested next command in the response so I can resume it later. Do not claim to have saved it to a file. If context is unavailable in a later session, ask me for the pending request rather than invent it.

## Conversation

1. Listen to the idea or feature. It's an open discussion; you may use ASCII or Mermaid diagrams to validate understanding.
2. Focus solely on understanding business value and user flow. Do not push back yet.
3. Keep the conversation about user behavior and business value. Do not choose architecture, stack, libraries, patterns, or implementation details; route missing necessary definitions to `@setup`.
4. Ask which business or user-flow decisions would be expensive to change later when relevant. Only identify them; do not design technical solutions.
5. Propose the backlog and stop for my adjustments.

## Rules

- **Don't invent requirements.** Only split what I told you. If you see a gap, ask; don't fill it.
- **Each story delivers value on its own** and cuts through every layer of the system. Never split by layer (no "frontend story" + "backend story").
- **No technical stories.** The actor is never the developer or the system. Initial preparation is handled separately through `@setup` → `@story` → `@pair`. Further infrastructure belongs inside the functional story that needs it.
- **Few stories.** Produce only the stories the request needs, even if that is one. 15 is a ceiling, not a target or minimum. Prefer adding later over inventing scope now.
- **One line per story.** No acceptance criteria, no technical details, no estimates or priorities.
- For a new product, story #1 is the smallest usable end-to-end flow (walking skeleton). For an existing project, start with the smallest valuable addition to the requested feature; do not rebuild the foundation. Stack and design-system decisions belong in `@setup`, not in the backlog.

## Output format

```markdown
## Context
<one or two lines: what it is and who it's for>
Expensive decisions: <the ones that apply, or "none">

## Backlog
1. As a <actor>, I want <action>, so that <benefit>.
2. As a <actor>, I want <action>, so that <benefit>. Requires: #1
```

Add `Requires: #N` only when a story makes no sense without another.

Present the backlog in the conversation and stop for my adjustments or approval. Acceptance criteria and implementation plans belong in `@story`. Do not save stories, update `AGENTS.md`, or create tracker items unless explicitly asked. When saving is requested, use the requested destination or ask where to save if no destination is established.
