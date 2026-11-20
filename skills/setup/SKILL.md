---
name: setup
description: Manual workflow (@setup). Define or revise stack, design system, directory layout, and agent tooling; save approved decisions and preparation status in AGENTS.md. Independent of @backlog; only use when explicitly invoked via @setup.
disable-model-invocation: true
---

# Setup

Help me agree on the smallest technical and visual foundation needed to start building. You propose; I decide.

## OpenCode Guardrails

- **Manual trigger only:** Run only when I explicitly request this skill through `@setup` or an equivalent explicit instruction. An optional configured `/setup` command may also invoke it. No previous @backlog session is required. Never activate another workflow automatically.
- **Application preparation is deferred:** Do not scaffold the application, create its directory structure, install application dependencies, or implement production visual components. The isolated static design preview below is the only application-code exception. Define application preparation here; `@story` plans it and `@pair` executes it.
- **Approval before changes:** Present decisions, documentation changes, and concrete agent-tool installations for my approval before applying them. Generating the isolated preview is part of the design exploration; choosing a variant does not approve unrelated stack or tooling changes. Approval covers the proposed actions; ask again only if something outside the approved scope is needed.

## Workflow

1. Read the project's `AGENTS.md`, relevant existing code, dependency manifests, configuration, and design documentation. Inspect existing frontend/backend directories and installed skills and MCP configuration. Use the backlog if available; otherwise ask briefly what we are building and for whom. Do not assume that configuration files prove dependencies are installed or the application works.
2. Establish scope: initial definition, documenting an existing project, or a specific revision. Preserve existing conventions unless I ask to reconsider them. Ask only for missing preferences or constraints that affect the current decision. Do not repeat the whole setup for a focused change.
3. **Stack, when relevant:** Propose the minimum technologies needed for the first usable flow, respecting existing code and my experience, deployment constraints, and preferences. Give a short reason for each choice. Verify current compatibility using official sources when recommending specific versions or integrations.
4. **Design system, when relevant:** Start from my references, brand, or the selected component library. For an initial visual definition or an explicitly requested comparison, generate the three-option preview described below and let me select or adjust it. Agree on a small usable foundation: components, colors, typography, spacing, borders, radii, and basic interaction states including focus, errors, and loading. Specify concrete values or existing tokens for approved choices. Skip this section for projects without a visual interface. Preserve an existing approved design during unrelated or focused changes; do not regenerate variants by routine. Do not design future screens or a complete component catalog.
5. **Directory layout:** Confirm the project root, existing or proposed subdirectories, and their responsibilities. Support a parent directory containing separate frontend and backend projects. Record paths relative to that root and the working directory for each known install, run, and test command. Inspect actual scripts before documenting commands; mark unconfirmed commands as pending. Do not impose a split when the project does not need it.
6. **Agent tooling:** Use Context7 as the general documentation source when available. Check the active OpenCode configuration and available tools before proposing another installation; distinguish a configured server from a connected, authenticated one. Match documentation to the project's actual library and version when covered. If Context7 cannot answer a question, use official documentation rather than assume coverage. Propose stack-specific MCP servers only for useful additional capabilities (for example, browsing or installing registry components), not duplicate documentation lookups. Propose browser automation such as Playwright MCP only when agent-driven browser inspection is needed; it is not a base requirement or a replacement for application test suites. Include `frontend-design` by default for projects with a visual interface. Verify its source and OpenCode compatibility rather than choosing an arbitrary same-named skill. For every proposed installation, identify its source, purpose, installation method, target location, configuration changes, and any credentials or license required. Check current official instructions and the installed OpenCode version; use its supported configuration schema. Reuse existing installations; do not install a server for every technology automatically. Leave unavailable sources, missing credentials, or licenses pending rather than inventing them.
7. **Application preparation:** Identify only the missing initialization, application dependencies, test tooling, and visual foundation needed to start. Existing projects may need documentation only. Define observable checks for completion. Keep this separate from agent-tool installation and from functional stories. A changed definition may require new preparation even when the previous foundation was complete.
8. Present a concise proposal using the format below. Separate existing decisions, proposed changes, and unresolved questions. Explain the impact of changing existing decisions; do not silently replace them. Wait for my approval and adjust as needed.
9. After approval, install or configure only the approved agent skills and MCP servers, using the approved scope and preserving unrelated configuration. Prefer project-local changes; global changes must be included explicitly in the proposal. Do not store credentials in `AGENTS.md`. Verify discovery/loading of skills and availability of MCP tools when possible. Distinguish configured from operational; report any required restart or authentication as pending. Do not create an application runtime or virtual environment just to validate documentation or install an unrelated skill.
10. Create or update the project's `AGENTS.md` with the approved context described below. Preserve unrelated instructions and merge without duplicating sections. If detailed design documentation already exists, reference it instead of duplicating it; propose a separate document before creating one if the content becomes lengthy.
11. Summarize what was saved, what was verified, and what remains pending. If application preparation is needed, suggest `@story Preparar la base técnica y visual acordada en @setup` (omit visual work when not applicable). Identify it as a preparation task, not a fabricated user story. Its plan must explicitly cover scaffolding and account for `@pair` limits on files and diff size before execution; do not imply those limits can be bypassed silently. If preparation is complete, return to the pending feature or suggest `@backlog` or `@story` as appropriate. Never invoke another skill automatically.

## Three-option design preview

- Create one isolated static HTML/CSS page with three labeled variants designed for the selected library, such as shadcn/ui or daisyUI. Confirm the library before presenting library-specific options. Use the same sample content and controls in each variant so the comparison mainly concerns colors, typography, spacing, borders, and radii. Include a small representative set of buttons, fields, cards, and interaction states, not complete product screens.
- Use the project's established preview location or, by default, `docs/design-preview/` relative to the project root. Announce the location; preserve existing files and do not overwrite unrelated previews. Keep all preview styles and assets isolated. Do not import them into the application, modify its theme or Tailwind configuration, install the chosen library, or initialize the application to render this preview. Avoid build tools and extra dependencies; use local assets or available font fallbacks and state any approximation.
- Serve only the preview directory with an available local static server, bound to localhost, and provide the actual verified URL and how to restart or stop it. Use an available port rather than promising a fixed one. Do not expose the entire project or publish the preview externally. Do not create an environment or install a server silently; if no suitable server is available, explain that limitation, provide the static file location, and ask before adding tooling needed for a local URL.
- Check that the page loads and inspect rendering when browser tools are available. Describe these as visual approximations, not real library components or evidence of production behavior or accessibility. If exact component behavior is required, hand off a minimal real-component environment as preparation through `@story` and `@pair`.
- Wait for my selection or adjustments. Keep unselected variants as alternatives, not approved design decisions. Save only the chosen, approved values as semantic tokens and conventions in `AGENTS.md`, referencing the preview and selected variant. Include typography and spacing scales, border treatment, and radii as well as colors; keep any mapping to library-specific tokens consistent with the chosen library and version.
- Record an explicit handoff for `@story` and `@pair`: implement approved tokens through the real component library's theme and supported styling mechanisms, verifying the applicable API/configuration. Reuse real components; do not copy preview CSS, imitate components in production, or import the preview page into the application. Rendering the preview does not mark application preparation implemented or verified.

## Persistent project context

Keep `AGENTS.md` short and actionable for `@backlog`, `@story`, and `@pair`:

- Project purpose and any original feature request still awaiting refinement.
- Approved stack, design conventions (or design not applicable), directory responsibilities, and known commands with their working directories.
- When a preview was used, the selected variant, preview reference, approved tokens, and instruction to implement them through the real library's theme/components without copying preview CSS. Record the preview's restart command and working directory if known; its local URL is session-dependent, not permanent hosting.
- Agent skills and MCP servers: source, purpose, location, and verified or pending status. Include `frontend-design` usage for visual work when available.
- Tool-use guidance: reuse existing components and code first; use Context7 for unresolved documentation questions when it covers the project's library/version, official docs otherwise, and specialized MCP tools for their additional capabilities. Do not consult multiple sources by routine for the same resolved question. Reuse still-applicable results available in context rather than repeating queries. MCP availability does not require a call on every iteration or guarantee token savings.
- Decisions: approved or pending. Application preparation: pending, partially complete, or verified complete, with concrete remaining work and evidence from actual checks. Track these separately: a defined stack is not an implemented stack.

Only mark work verified when checks were actually run successfully. If inspection is all that was done, label it detected but unverified. Use non-destructive checks; if verification requires missing application dependencies or initialization, defer that work to `@pair`. Existing verified preparation requires no reinstall. Tell `@pair` to update preparation status after executing and verifying the agreed plan, and to check the actual project against the recorded status before relying on it.

## Proposal format

```markdown
## Context
<what we are building and the scope of this setup>

## Stack
<existing decisions and proposed technologies, each with a brief reason; or not applicable>

## Design system
<existing conventions, preview location and verified local URL when available, selected variant and proposed tokens; or not applicable>

## Directory layout
<project root, relative paths, responsibilities, and known commands>

## Agent tooling
<existing tools and proposed skill/MCP installations with sources, scope, configuration changes, and prerequisites>

## Application preparation
<existing evidence, missing work, completion checks, and handoff to @story and @pair; or already verified>

## Pending
<unresolved decisions and whether any block the first story; or none>

## Approval
<ask whether to approve and save, or adjust the proposal>
```

Keep the output brief. Do not invent product requirements, require a backlog, or repeat settled decisions during a focused revision.
