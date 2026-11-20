# AI-Assisted Development in OpenCode

Four skills for defining a project and implementing features in small steps. The AI proposes and writes; you approve decisions and choose when to continue.

## The skills

| Skill | What it does | Where the result goes |
| --- | --- | --- |
| [`@setup`](skills/setup/SKILL.md) | Defines or revises the stack, design system, directories, and agent tools. | Approved decisions in `AGENTS.md`. |
| [`@backlog`](skills/backlog/SKILL.md) | Proposes the necessary small, vertical user stories. | The conversation, unless you ask to save them. |
| [`@story`](skills/story/SKILL.md) | Refines a user story or preparation task: up to three criteria and five plan points. | One file per approved specification, under `docs/stories/` by default. |
| [`@pair`](skills/pair/SKILL.md) | Executes the specification, verifies results, and records progress. | Code, tests, and a checklist in the story file. |

```text
Define the foundation:  @setup → approve decisions
Prepare the foundation: @story preparation → approve → @pair preparation
Build features:         @backlog → @story one story → approve → @pair file
```

You can start with `@backlog I want to add login`. If necessary decisions are missing, it suggests `@setup`; if the approved foundation still needs preparation, it suggests `@story` for that work. Skills never invoke each other automatically.

`@setup` is independent. Use it initially and to revise decisions, not for every new feature. It distinguishes an agreed stack from pending preparation and a verified environment.

## Install with npx

You need **OpenCode V2**, a configured provider/model, Git, and a Node.js version supported by the [skills CLI](https://github.com/vercel-labs/skills) (currently 22.20 or later).

From your project root, install the four skills using this repository's GitHub URL:

```bash
npx skills add https://github.com/djzdavid/opencode-xp-tdd-skills -a opencode --skill setup backlog story pair
```

Add `-g` to install them across projects. The installer manages skill files; it does not install OpenCode, copy [opencode.json](opencode.json), configure Context7, or install your application's stack. See the [CLI options](https://github.com/vercel-labs/skills/blob/main/docs/commands.md).

Open or restart OpenCode and mention a skill by its ID, for example `@setup Review this project`. All four skills disable automatic invocation through `disable-model-invocation: true` and remain available through explicit mentions. No slash commands need to be registered. See the [OpenCode skills documentation](https://opencode.ai/docs/skills/).

For separate frontend and backend directories, open the session in their parent directory. The skill records the layout, and the agent runs each command in the appropriate location.

[opencode.json](opencode.json) is an **optional** example for users who prefer `/setup`, `/backlog`, `/story`, and `/pair` commands. Do not overwrite an existing configuration with it: it includes a model, permissions, and paths that you must adapt. The `@` mentions work without that file and remain subject to your OpenCode permissions.

### Context7 authentication

Context7 is optional and configured separately from skill installation. To use **OAuth**, merge this snippet into your OpenCode V2 configuration, preserving other settings:

```json
{
  "mcp": {
    "servers": {
      "context7": {
        "type": "remote",
        "url": "https://mcp.context7.com/mcp/oauth"
      }
    }
  }
}
```

Then run these commands in a terminal from the project root:

```bash
opencode mcp auth context7
opencode mcp list
```

Complete sign-in in your browser and check that the server is connected. You can also sign in through `/mcps` in OpenCode. No local server or additional plugin is needed. References: [OpenCode MCP servers](https://opencode.ai/docs/mcp-servers/) and [Context7 for OpenCode](https://context7.com/docs/clients/opencode).

Signing in does not require a paid plan: a [free plan with a quota](https://context7.com/plans) is available. If authentication is missing, the quota is exhausted, or the service is unavailable, the agent reports the limitation and uses official documentation through its available tools. An API key is another option, but requires configuration changes; never store credentials in the repository.

## Example 1: an existing project

Open OpenCode in the parent directory of `project-frontend` and `project-backend`. Send messages separately, reviewing each proposal.

**1. Document the existing foundation**, if it has not already been agreed in `AGENTS.md`:

```text
@setup The frontend and backend are already installed in project-frontend
and project-backend. The current directory is the project root.
Inspect and preserve the existing stack and design. Identify commands
and their working directories. Propose what to record in AGENTS.md,
without reinstalling dependencies or generating new visual variants.
```

After reviewing:

```text
I approve the decisions and saving them. Do not install additional tools.
```

Finding files does not prove the environment works. The agent identifies anything it has not verified; if only documentation was missing, it does not create an initialization task.

**2. Request a feature and choose a user story:**

```text
@backlog I want email and password login: successful access and a message
for invalid credentials. No registration or password recovery.
```

```text
@story Refine the story for signing in with valid credentials.
Use AGENTS.md and the current code.
```

```text
I approve the criteria, plan, and proposed path. Save the story.
```

**3. Implement using the path returned by the agent:**

```text
@pair docs/stories/001-login.md
```

After reviewing the test names and levels:

```text
I approve the list. Run one TDD cycle and stop.
```

To move forward, reply `Continue with the next test`. File names and identifiers in these examples are illustrative; use the ones from your project.

## Example 2: a project from scratch

Open OpenCode in the directory intended for the application, with the four skills installed.

**1. Agree on the foundation and design:**

```text
@setup I want a booking application for a hair salon.
The first flow will show available time slots. The project is empty.
I know React and Python, and prefer PostgreSQL and shadcn/ui.

Use the current directory as the root, with project-frontend and
project-backend inside. Propose the minimum stack, three visual
variants, agent tools, and the preparation needed.
```

The preview compares colors, typography, spacing, borders, and radii using the same sample controls. It stays isolated, under `docs/design-preview/` by default. If a server is available, the agent provides a verified local URL and instructions to restart or stop it; otherwise, it asks before adding tools.

```text
I choose option 2. Reduce the border radii and show me the adjustment.
```

When you are satisfied:

```text
I approve the stack, directories, adjusted option 2, and the agent-tool
installations described in the proposal. Save the decisions in AGENTS.md.
```

`@setup` can install approved skills and MCP servers, but leaves application creation to `@pair`. The approved design is recorded as tokens and conventions. The preview is approximate: `@pair` applies those values through the real theme and components without copying the preview CSS.

**2. Plan the preparation:**

```text
@story Prepare the technical and visual foundation agreed in @setup:
the necessary initialization, dependencies, theme, and test environment.
Respect the frontend and backend directories.
```

```text
I approve the criteria, plan, path, and preparation exceptions described
in the proposal. Save the specification.
```

Generators may exceed the usual limit of three files or about 50 diff lines per step. The plan must split the work or include an explicit, narrowly scoped exception before execution.

**3. Create and verify the foundation:**

```text
@pair docs/stories/001-initial-preparation.md
```

After reviewing the actions and checks:

```text
I approve the list. Execute all approved preparation, verify each part,
and stop when finished or if blocked.
```

The AI creates directories, initializes projects, and installs what was agreed. It uses real checks rather than manufacturing failing tests for installations. Preparation is only marked verified after the agreed checks pass.

**4. Continue with features:**

```text
@backlog I want customers to view available time slots.
Do not implement booking yet.
```

With the foundation ready, continue with `@story` and `@pair` as in the first example, without repeating `@setup`.

## Progress and new sessions

`AGENTS.md` holds general decisions and preparation status. Each story file contains its criteria, plan, scope, and a progress section maintained by `@pair`, for example:

```markdown
## Progress
- [x] Sign-in with valid credentials — integration.
- [ ] Reject an incorrect password — integration.

## Last verification
Command, working directory, and actual results of the tests executed.

## Next step
Implement rejection of an incorrect password.
```

Items are marked `[x]` only with actual evidence. Approving a story does not mean it is implemented; pending manual verification prevents completion. Established project locations are respected, and existing stories are not overwritten to add new ones.

To resume in another session:

```text
@pair docs/stories/002-login.md
```

`@pair 002-login.md` also works if the name is unambiguous. The agent compares saved progress with the code and checks before continuing. If you did not save the specification, you will need to provide it again.

## Tools and execution rules

- **Context7:** documentation for specific questions; reuse useful answers available in context.
- **Specialized MCP servers:** added when they provide additional capabilities. **Playwright MCP** is optional for browser inspection and does not replace the test suite.
- **`frontend-design`:** proposed by default for projects with a UI; it is not bundled, and its source and compatibility are checked before installation with approval.
- **TDD:** one test → expected red → minimum code → green → pause. You can request several consecutive cycles, always one at a time. An environment failure does not count as behavioral red.
- **Blockers:** `@pair` stops for decisions outside the plan, two attempts without green, a need to alter existing tests to make them pass, or steps exceeding limits without an approved exception.
- **Git:** you handle commits and pushes; `@pair` never runs `git commit`, `git push`, or `git reset`.

Queries and tools consume model context. The goal is clear scope and verifiable results, without promising token or quota savings.
