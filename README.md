# Expert Team Bootstrapper

`expert-team-bootstrapper` is a lean local skill that inspects a project,
builds a shared dossier, recommends a small set of useful experts, and
scaffolds reusable role skills such as `SEO Lead`, `CMO`, `Growth Lead`, or
other project-specific specialists.

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
- have the coding agent generate lean local skill folders for those experts
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
    ├── memory-schema.md
    ├── project-dossier-schema.md
    ├── role-catalog.json
    ├── role-template-schema.md
    ├── role-templates.md
    └── team-spec-example.json
```

## Requirements

- a local project you want to analyze
- an agent environment that supports `skills.sh` or Codex-style skills

## Install with skills.sh

For public distribution, install this skill with `skills.sh`:

```bash
npx skills add https://github.com/chentschel/expert-team-bootstrapper
```

That keeps installation simple and avoids any language-specific setup.

## Install in Codex

Codex-style environments can use this repo as a native skill.

If you want to install it manually instead of using `skills.sh`, copy or
symlink the repo root into your Codex skills directory:

```bash
git clone https://github.com/chentschel/expert-team-bootstrapper.git
cd expert-team-bootstrapper
mkdir -p ~/.codex/skills
ln -s "$PWD" ~/.codex/skills/expert-team-bootstrapper
```

Restart Codex after installing a new skill.

## Use in Codex

Once installed, invoke the skill explicitly:

```text
Use $expert-team-bootstrapper to inspect this project, recommend a lean team of experts, and scaffold the generated skills.
```

Typical workflow:

1. Ask the bootstrapper to inspect your repo and summarize the product.
2. Ask it to recommend only the experts that fit your project and current task.
3. Have it prepare a `team-spec.json` using a base role template for each persistent expert.
4. Have the agent create the generated team files directly in your workspace.
5. Install the generated expert folders you actually want to keep using.

Example prompts:

```text
Use $expert-team-bootstrapper to inspect this repo, identify what product it is, and recommend the smallest useful expert team.
```

```text
Use $expert-team-bootstrapper to create a team spec for this project with clear role boundaries and conservative external-data policies.
```

```text
Use $expert-team-bootstrapper to create persistent experts from the role catalog, specialize them for this project, and record only durable learnings in approved memory.
```

## Generate an Expert Team

The project analysis and file generation should be done by the coding agent
itself, not by a Python helper. The intended flow is:

1. the agent inspects your project
2. the agent drafts `team-spec.json`
3. the agent drafts `generated-team/memory/approved-memory.json` for durable confirmed learnings
4. the agent writes `generated-team/shared-references/*`
5. the agent writes `generated-team/skills/<expert-id>/*`
6. the agent updates `manifest.json` when the team changes

You can keep `team-spec.json` as the canonical source for regeneration and
`approved-memory.json` as the curated long-term memory source.

Generated output looks like:

```text
generated-team/
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
└── skills/
    ├── seo-lead/
    │   ├── SKILL.md
    │   └── agents/openai.yaml
    └── ...
```

## Install Generated Experts in Codex

The bootstrapper generates reusable expert skills, but it does not auto-install
them. Copy or symlink the generated expert folders you want into your Codex
skills directory:

```bash
ln -s "$PWD/generated-team/skills/seo-lead" ~/.codex/skills/seo-lead
```

Repeat for any other generated experts you want available as skills.

## Use with Claude, Gemini, and Other Assistants

The analysis should be done by the agent you are already using. This skill is
primarily a reusable instruction package that tells the agent how to inspect the
project, recommend roles, and generate the expert team files.

Codex can consume the base skill natively. Other assistants can still use the
generated outputs, even if they do not share the same folder-based skill
runtime.

The portable pieces are:

- `generated-team/memory/approved-memory.json`
- `generated-team/shared-references/*.md`
- each generated expert `SKILL.md`
- `manifest.json`

### Practical Claude or Gemini workflow

1. Run the bootstrapper and generate the team locally.
2. Add the shared reference files to your project knowledge or keep them
   open as reference material.
3. Use the generated expert `SKILL.md` content as the expert’s operating
   instruction set for a task-specific chat.
4. Keep the dossier as shared truth and avoid copying all project context into
   every chat.

In other words: Codex can consume the base skill directly, while Claude, Gemini,
and other assistants can consume the generated dossier and role definitions as
structured project instructions.

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

- manual regeneration
- JSON spec as canonical source of truth
- lightweight approved-memory source of truth
- shared dossier plus lean role skills
- agent-driven generation

Deliberately out of scope for now:

- automatic skill mutation from chat history
- persistent autonomous expert agents
- broad external tool access by default
- a heavy orchestration framework

## Publishing

For public distribution:

- the repo itself is the installable skill
- generated expert folders are outputs, not part of the base package
- the install flow should stay `npx skills add https://github.com/chentschel/expert-team-bootstrapper`
- the root README should explain both skills.sh installation and cross-agent usage

## License

This project is released under the MIT License.
