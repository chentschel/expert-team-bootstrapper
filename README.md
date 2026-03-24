# Expert Team Bootstrapper

`expert-team-bootstrapper` is a lean local skill that inspects a project,
builds a shared dossier, recommends a small set of useful experts, and
automatically generates, installs, and maintains reusable role skills such as
`SEO Lead`, `CMO`, `Growth Lead`, or other project-specific specialists.

It is designed to stay simple:

- repo-first, not questionnaire-first
- shared project knowledge, not duplicated prompt blobs
- a few differentiated experts, not an artificial org chart
- explicit capability tiers for external data access
- persistent experts derived from template + dossier + approved memory

This repo is the installable skill.

## Publishing Identity

Final names:

- GitHub repo: `expert-team-bootstrapper`
- Published skill name: `expert-team-bootstrapper`
- GitHub path: `chentschel/expert-team-bootstrapper`

This keeps the repo name, skill name, install command, and explicit skill
invocation aligned.

## What It Does

The bootstrapper helps you:

- inspect a local project and infer what the product is
- build a structured shared dossier for product, audience, domain, growth, and constraints
- keep a lightweight approved-memory file for durable project learnings
- recommend which experts are actually worth creating
- have the coding agent generate and install managed specialists for the current runtime
- regenerate the team later from a canonical JSON spec

Generated experts are reusable role definitions, not always-on autonomous
agents.

## Repo Layout

```text
.
├── SKILL.md
├── README.md
├── LICENSE
├── agents/openai.yaml
└── references/
    ├── approved-memory-example.json
    ├── capability-policy.md
    ├── managed-specialist-example/
    │   ├── SKILL.md
    │   └── agents/openai.yaml
    ├── managed-specialist-spec.md
    ├── manifest-example.json
    ├── manifest-schema.md
    ├── memory-schema.md
    ├── project-dossier-schema.md
    ├── role-catalog.json
    ├── role-template-schema.md
    ├── role-templates.md
    ├── runtime-management.md
    └── team-spec-example.json
```

## Requirements

- a local project you want to analyze
- an agent environment that supports `skills.sh`

## Install with skills.sh

For public distribution, install this skill with `skills.sh`:

```bash
npx skills add https://github.com/chentschel/expert-team-bootstrapper
```

That keeps installation simple and avoids any language-specific setup.

## How to Use

After installation, invoke the skill using your agent's skill syntax:

```text
[OpenAI/Codex-style example]
Use $expert-team-bootstrapper to inspect this project, recommend a lean team of experts, and install or update the managed specialists.
```

Typical workflow:

1. Ask the bootstrapper to inspect your repo and summarize the product.
2. Ask it to recommend only the experts that fit your project and current task.
3. Have it prepare a `team-spec.json` using a base role template for each persistent expert.
4. Run in `preview` mode when you want to inspect planned changes first.
5. Run in `apply` mode to update `.expert-team/` and install or update the managed specialists automatically.
6. Re-run the bootstrapper later to keep the team aligned with project changes and approved memory.

Example prompts:

OpenAI/Codex-style examples:

```text
Use $expert-team-bootstrapper to inspect this repo, identify what product it is, and recommend the smallest useful expert team.
```

```text
Use $expert-team-bootstrapper to create a team spec for this project with clear role boundaries and conservative external-data policies.
```

```text
Use $expert-team-bootstrapper to create persistent experts from the role catalog, specialize them for this project, and record only durable learnings in approved memory.
```

```text
Use $expert-team-bootstrapper in preview mode to show which specialists would be created, updated, kept, or removed before applying changes.
```

If your agent uses a different skill invocation syntax, use the equivalent
runtime-specific form while keeping the same intent.

## Managed Team Lifecycle

The project analysis, canonical-state generation, and runtime installation
should be done by the coding agent itself. The intended flow is:

1. the agent inspects your project
2. the agent drafts `.expert-team/team-spec.json`
3. the agent drafts `.expert-team/memory/approved-memory.json` for durable confirmed learnings
4. the agent writes `.expert-team/shared-references/*`
5. the agent updates `.expert-team/manifest.json`
6. the agent installs or updates managed specialists for the current runtime

You can keep `.expert-team/team-spec.json` as the canonical source for
regeneration and `.expert-team/memory/approved-memory.json` as the curated
long-term memory source.

Canonical project state looks like:

```text
.expert-team/
├── manifest.json
├── team-spec.json
├── memory/
│   └── approved-memory.json
├── shared-references/
│   ├── product.md
│   ├── audience.md
│   ├── workflows.md
│   ├── domain.md
│   ├── business-model.md
│   ├── growth.md
│   ├── constraints.md
│   └── assumptions.md
```

Managed runtime specialists are installed from that state for the current
runtime. This keeps the project-owned canonical state separate from
runtime-installed artifacts.

Two operating modes are expected:

- `preview`: show planned create, update, remove, and keep actions without mutating anything
- `apply`: write canonical state and reconcile managed specialists

## Cross-Agent Use

The analysis should be done by the agent you are already using. This skill is
primarily a reusable instruction package that tells the agent how to inspect the
project, recommend roles, and manage the expert team lifecycle.

Codex can consume the base skill natively. Other assistants can still use the
canonical project state, even if they do not share the same folder-based skill
runtime.

The portable project-owned pieces are:

- `.expert-team/memory/approved-memory.json`
- `.expert-team/shared-references/*.md`
- `.expert-team/team-spec.json`
- `.expert-team/manifest.json`

### Practical Claude or Gemini workflow

1. Run the bootstrapper and generate the team locally.
2. Add the shared reference files to your project knowledge or keep them
   open as reference material.
3. Let the bootstrapper manage runtime specialists for the current tool when supported.
4. If a tool does not support local skill installation, use the canonical files
   in `.expert-team/` as structured operating context.
5. Keep the dossier as shared truth and avoid copying all project context into
   every chat.

In other words: the bootstrapper should install and maintain specialists for the
current runtime where possible, while the canonical `.expert-team/` state stays
portable across agents.

## How Roles Stay Consistent

Persistent experts should be composed from three inputs:

1. a base template from [`references/role-catalog.json`](./references/role-catalog.json)
2. the current project dossier
3. approved long-term memory

That means a role like `growth-lead` should not be invented from scratch each
time. It should start from the canonical template, then be narrowed by project
facts and durable learnings.

Approved memory should stay lightweight and reviewed. Keep it in a separate file
instead of appending raw chat history to role prompts.

## How Specialist Management Works

The bootstrapper should behave like a control plane:

- canonical state lives in `.expert-team/`
- runtime-specific installed specialists are derived artifacts
- create and update operations should apply automatically
- stale managed specialists should be removed or archived during reconciliation
- unrelated user-created skills should not be touched

The current runtime policy is documented in
[`references/runtime-management.md`](./references/runtime-management.md).
The manifest contract is documented in
[`references/manifest-schema.md`](./references/manifest-schema.md).
The installed specialist artifact contract is documented in
[`references/managed-specialist-spec.md`](./references/managed-specialist-spec.md).

Concrete examples are provided in:

- [`references/manifest-example.json`](./references/manifest-example.json)
- [`references/managed-specialist-example/`](./references/managed-specialist-example)

## Capability Tiers

Generated experts support explicit capability policies:

- `local-only`: repo and shared dossier only
- `research-enabled`: may use external search/docs when needed
- `browser-enabled`: may inspect live sites or apps when a task truly requires it

The default should be conservative. External data should augment the project
dossier, not replace it.

## Design Constraints

This repo intentionally avoids overengineering.

Current scope:

- managed regeneration from canonical state
- JSON spec as canonical source of truth
- lightweight approved-memory source of truth
- shared dossier plus lean role skills
- agent-driven generation and runtime installation

Deliberately out of scope for now:

- automatic skill mutation from chat history
- persistent autonomous expert agents
- broad external tool access by default
- a heavy orchestration framework

## Publishing

For public distribution:

- the repo itself is the installable skill
- runtime-installed specialists are derived artifacts, not part of the base package
- the install flow should stay `npx skills add https://github.com/chentschel/expert-team-bootstrapper`
- the root README should explain both skills.sh installation and cross-agent usage

## License

This project is released under the MIT License.
