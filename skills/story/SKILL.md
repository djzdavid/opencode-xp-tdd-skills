---
name: story
description: Manual workflow (@story). Refine one user story or an agreed preparation task with up to three acceptance criteria and a five-point plan, then save the approved specification for @pair. Only use when explicitly invoked via @story.
disable-model-invocation: true
---

# Story

We refine one user story or preparation task right before building it. You propose and I decide.

## OpenCode Guardrails
- **Manual trigger only:** Run only when I explicitly request this skill through `@story` or an equivalent explicit instruction. An optional configured `/story` command may also invoke it. Never activate another workflow automatically.
- **Read-only until approval:** Read project context while drafting. After my approval, save the approved specification in the agreed location. Do not implement code, create application scaffolding, install dependencies, or change application configuration.
- **Explicit handoff:** Approval authorizes saving this specification, not starting implementation. Never invoke `@pair` or another skill automatically.

## Steps

1. **Read context:** Read the supplied story, preparation request, or referenced specification. Inspect `AGENTS.md` (if present), relevant code, manifests, and existing story documents. Respect approved stack, design, directory responsibilities, commands, and agent tooling. Compare recorded preparation status with actual files without treating inspection as proof that runtime checks passed. If a necessary decision is missing or conflicts with the project, identify it, suggest a focused `@setup`, and stop rather than silently choose a new stack. Optional unavailable tooling must not block unrelated work.
2. **Identify the work:** For functional work, refine exactly one user story. For preparation approved in `@setup`, use a technical objective, not a fabricated business actor. Include only missing work; do not reinstall or recreate existing foundations. If preparation blocks a functional story, explain the dependency and propose refining that preparation first rather than silently replacing the requested story.
3. **Acceptance criteria:** For a user story, use Given/When/Then to describe observable user outcomes, not implementation details. For preparation, define observable technical outcomes and checks, such as successful initialization, dependency availability, test-runner execution, or agreed visual foundations. Distinguish checks that `@pair` must execute from manual verification. Do not mark criteria complete during planning or invent meaningless tests just to manufacture a TDD cycle for scaffolding.
4. **Enforce small scope:** Use at most three criteria. If more are needed, propose splitting and wait for my approval before refining the first item. Split functional work into small vertical stories and preparation into clearly named preparation tasks. Do not hide unrelated outcomes inside one compound criterion to evade the limit.
5. **Mini plan:** Five bullets max; no application code snippets or function signatures. For a functional story, explain the slice across the relevant parts of the application without prescribing individual source files. When preparation is needed, explicitly identify scaffolding, necessary dependencies, working directories, and verification before behavioral TDD begins. Flag any step expected to exceed `@pair` limits (about 50 diff lines or three files) and propose splitting or a narrowly scoped exception for my approval. Do not assume an exception or begin executing it. Already approved decisions need not be approved again unless the proposal changes them.
6. **Checkpoint and destination:** Present the complete draft, including out-of-scope items, any exceptions requiring approval, and its proposed save path. Stop and wait for my explicit approval. Iterate until I approve; substantive changes after approval require approval of the revised specification.
7. **Save and hand off:** Save exactly the approved specification following the persistence rules below. Report the actual saved path and the corresponding `@pair <path>` command. Do not claim that the application or preparation has been implemented.

## Rules

- **Don't invent requirements.** If edge cases or behaviors are ambiguous, ask me instead of guessing.
- **The simplest thing that meets the criteria.** No abstractions, layers, configs, or patterns "for the future".
- **Suggest, don't decide.** If a design pattern or refactor could help, list it under "Suggestions" with a concrete signal from the existing code.

## Persistence

- Use the project's established story location or an explicit destination I provide. Otherwise use `docs/stories/` relative to the project root, shared by frontend and backend when they belong to the same project.
- Save one approved story or preparation task per Markdown file, for example `001-preparacion-inicial.md` or `002-login.md`. Inspect existing files and select the next unused numeric identifier and a short descriptive slug; do not assume the example identifiers are available.
- Include the proposed relative path in the draft so approval covers creating that document and its parent directories. If I explicitly ask for conversation-only output, do not save it.
- Never overwrite or renumber another story. When refining an existing file, show the proposed revision and save it back only after explicit approval to update that specification; preserve unrelated content.
- The saved document must stand on its own in a new session: include its identifier, type (user story or preparation task), approved status, objective, acceptance criteria, plan, dependencies when applicable, out-of-scope items, and any explicitly approved preparation exceptions. Reference existing project context instead of duplicating the full stack and design system. Preserve approval separately from implementation status; new specifications have implementation pending, not complete.
- Do not put individual stories or their plans into `AGENTS.md`, or create tracker items unless explicitly asked. Do not mark project preparation complete; `@pair` records that only after executing and verifying it.

## Output format

Present a concise draft using the following structure. After approval, save the approved content and record its approval status:

```markdown
## Specification
ID: <proposed identifier>
Type: <user story or preparation task>
Approval: draft
Implementation: pending
Save path: <proposed project-relative Markdown path>

## Objective
<As a user, I want an action, so that a benefit; or the technical preparation objective>

## Dependencies
<prerequisite specifications or relevant approved project context; or none>

## Acceptance criteria
- [ ] Given <context>, when <action>, then <result>.
- [ ] ...

## Plan
- ...

## Preparation exceptions
<specific exception requiring approval and its scope; or none>

## Out of scope
- <what's explicitly excluded from this story so it doesn't sneak in>

## Suggestions (your call)
- <optional, only if grounded in current code>
```

End the draft by asking for approval or adjustments. After saving, give the actual invocation, for example `@pair docs/stories/002-login.md`. This path allows the user to resume in another session; do not rely on the current conversation as the only specification.
