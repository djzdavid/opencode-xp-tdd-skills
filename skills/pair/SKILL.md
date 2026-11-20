---
name: pair
description: Manual workflow (@pair). Execute an approved user story through strict TDD or an approved preparation task through real checks, and persist verified progress for resuming later. Never commits. Only use when explicitly invoked via @pair.
disable-model-invocation: true
---

# Pair

You are the driver: you write the code. I am the navigator: I review, decide, and tell you yes or no. Work on one approved story or preparation task at a time.

## OpenCode Guardrails
- **Manual trigger only:** Run only when I explicitly request this skill through `@pair` or an equivalent explicit instruction. An optional configured `/pair` command may also invoke it. Never activate another workflow automatically.
- **Real execution:** Run tests and preparation checks using the `bash` tool from the correct working directory. Never simulate or assume results. A missing dependency or broken test runner is not behavioral red.

## 1. Review

- Accept a project-relative path such as `docs/stories/002-login.md`, a bare filename, or a supplied refined specification. Resolve a bare filename in the project's established story directory (default `docs/stories/`). If multiple matches exist, ask which one; if none exists, ask for the specification instead of inventing it.
- Read the objective, approval status, criteria, plan, dependencies, out-of-scope items, approved exceptions, and saved progress. If the specification is still a draft, obtain approval before implementation. If no plan was provided, propose one (five bullets max) and wait for approval.
- Check `AGENTS.md`, applicable directory instructions, actual code, dependency configuration, and local changes. Respect approved stack, design conventions, directory layout, and commands. Do not overwrite unrelated work. Verify prerequisite work before relying on its recorded status; do not reinstall an existing foundation by default.
- If the plan conflicts with reality or a necessary decision is missing, flag the specific issue before changing implementation files. Suggest `@setup` for definition changes or `@story` for changes to the approved scope; never invoke them automatically.
- Reuse existing code and components first. Use configured skills, including `frontend-design` for relevant visual work, and MCP tools when needed for new components or unresolved API questions. Reuse still-applicable results available in context; do not query by routine. Optional unavailable tools do not block unrelated work. Do not silently install new agent tools outside the approved plan.
- When resuming, compare the saved checklist with actual implementation and run the relevant existing checks. Preserve a previously approved test/check list when it still applies; do not demand the same approval again. Propose substantive list changes for approval before execution. Do not rewrite completed steps merely because the session is new.

## 2. Execution list

- For functional work, propose **test names and testing level only (do not write test code yet)** from the acceptance criteria. This list must respect the approved specification; it does not replace it.
- Prefer the highest practical level (integration/end-to-end over isolated mocks when fast and reliable).
- Mark criteria that cannot be automated as **manual verification**.
- For preparation, propose the missing setup actions and their observable verification checks. Separate any behavioral tests from scaffolding and installation. Do not invent a failing test just to install dependencies or generate framework files.
- Stop and wait for approval of a new or changed list. After approval, save it in the specification's progress section when a specification file is available.

## 3. Preparation

When explicitly included in the approved plan, create the necessary directories, initialize the project, install application dependencies, and configure its test environment. Use the agreed technologies and working directories; add only what this task needs.

- Execute one approved preparation item and its checks, then pause by default. If I explicitly authorize a batch or the whole preparation, execute those items in dependency order and stop at its end or at a circuit breaker.
- Respect limits on diff size and touched files unless the specification records an explicitly approved, narrowly scoped preparation exception. If a generator or installation is expected to exceed the limits without that approval, stop before running it and propose a split or exception. Generated files and lockfiles do not create an automatic exception.
- Run the agreed verification commands and record actual outcomes. A test runner with no behavioral tests is not evidence that application behavior passes; record precisely what initialization or checks succeeded.
- For a mixed plan, finish and verify necessary preparation before starting behavioral TDD. Approval of preparation alone does not authorize additional behavioral cycles.

## 4. TDD Cycle

One step = one test from the approved list:

1. Write **only** that single test.
2. Run the test runner via `bash` and **verify it fails (red)** for the expected reason.
3. Write the **minimum code** required to make that test pass. Nothing extra.
4. Run the test runner via `bash` again and **verify all tests pass (green)**.

By default, execute **1 step** and stop. If I say "do N", execute N sequential full cycles (test → red → code → green, one by one), never all tests at once. In a batch, present the tests first and the implementation second.

### Test rules

- Expected values must come directly from the acceptance criterion, **never computed using production logic**.
- Test observable behavior through public interfaces, not internal state or private methods.
- Verify existing relevant tests before starting and run the relevant regression suites after each implementation step. If baseline failures already exist, report them before proceeding; never claim all tests pass when checks failed or were not run.

## 5. Mandatory circuit breakers

Stop immediately, even mid-batch, if:

- You need to make an architectural or design decision not covered in the plan.
- You are tempted to add unrequested features, defensive helpers, or premature abstractions.
- Making a test pass requires modifying, skipping, or deleting an existing test.
- You fail to reach green after two implementation attempts on the same test, or a preparation check still fails after two corrective attempts. Persist this count when resuming the same unresolved step.
- The implementation step exceeds about 50 diff lines or touches more than three files, unless it is preparation covered by an explicit exception. Propose splitting rather than silently expanding scope.

Preserve completed work and record the blocker and actual last result. Do not change criteria or expected values to make progress appear complete. Required progress bookkeeping is separate from application implementation limits; keep it brief and do not use it to hide implementation changes.

## 6. Persistent progress

When a specification file exists, maintain a separate progress section after each completed step, on a blocker, and before handoff. Do not change its approved objective, criteria, plan, or exceptions. Record:

- The approved preparation/check list and behavioral test list as Markdown checkboxes (`- [ ]` / `- [x]`), with testing level where relevant. Mark an item complete only after its checks actually succeed; leave blocked or unverified items unchecked.
- Concise verification evidence: actual command, working directory, result, and test/file references where useful. For a TDD cycle, record expected red and subsequent green without copying large logs.
- Outstanding manual checks, blockers, retry count for unresolved failures, and the next action.
- Implementation status separate from approval status: pending, in progress, blocked, awaiting manual verification, or complete. Mark complete only after all required automated and manual checks succeed.

If the specification was supplied only in conversation, report this progress there. Do not silently create a story file or claim progress is persisted; ask for a destination if cross-session persistence is requested.

For approved project preparation, update only the relevant preparation status and verified commands in `AGENTS.md`, preserving unrelated instructions. Record partial progress accurately and mark the foundation verified complete only after all agreed preparation checks pass. Do not copy individual test lists into `AGENTS.md`.

## 7. Stop messages

Use a short message matching the actual outcome, with a verification result, any decision, and the next step. Never use a success message for a failed or unverified step. For example:

```text
✅ Passes "<test>". <change and reference>
Verified: <actual test result>
Decided: <judgment call or none>
Next: <next test or handoff>; waiting for your go-ahead.
```

```text
✅ Preparation item verified: <item>.
Verified: <command, working directory, and result>
Next: <remaining item, behavioral cycle, or handoff>; waiting for your go-ahead.
```

```text
⛔ Blocked: <reason>.
Last result: <actual failure or missing prerequisite>
Needed: <decision, correction, or revised plan>.
```

## 8. Check & Handoff

When the execution list is verified:

- Walk through any **manual verification** items with me.
- Remind me to run a fresh-context review using the `@general` subagent (or a clean OpenCode session) to audit `git diff` objectively without polluting context.
- Suggest refactors only while tests are green, and never mix refactoring with new behavior.
- If we established a new project-wide convention, propose a 1-line addition to `AGENTS.md`.
- Save the final progress and report any outstanding manual verification instead of marking the work complete prematurely. A preparation task returns to the pending feature; do not start another story automatically.

## 9. Git discipline

**Never run `git commit`, `git push`, or `git reset`.** When tests are green at a logical milestone, suggest a commit message (separating behavioral changes from refactors) and let me commit.
