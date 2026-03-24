# Runtime Management

The bootstrapper should manage specialists for the current runtime, not just
generate files and stop.

## Canonical state

Keep the project-owned source of truth in:

```text
.expert-team/
```

Recommended layout:

```text
.expert-team/
├── manifest.json
├── team-spec.json
├── memory/
│   └── approved-memory.json
└── shared-references/
    ├── product.md
    ├── audience.md
    ├── workflows.md
    ├── domain.md
    ├── business-model.md
    ├── growth.md
    ├── constraints.md
    └── assumptions.md
```

The runtime-installed specialist skills are generated artifacts. The
`.expert-team/` directory is the durable source of truth.

## Default install targets

### Codex

- default install root: `~/.codex/skills/`
- install as managed user-level skills
- use a project-prefixed skill id to avoid collisions

Example:

```text
~/.codex/skills/fantasypulse-seo-lead/
```

### Claude Code

- default install root: `.claude/skills/`
- prefer project-level installation for project-specific specialists
- use the same project-prefixed skill id

Example:

```text
.claude/skills/fantasypulse-seo-lead/
```

If the user explicitly wants shared cross-project specialists, user-level
installation can be chosen instead.

## Reconciliation rules

When the bootstrapper runs in apply mode, it should:

- create missing managed specialists
- update existing managed specialists when the spec, dossier, or approved memory changes
- remove or archive managed specialists that are no longer in the manifest
- keep the manifest aligned with the current installed set

## Safety rules

- default to apply for create and update operations
- show a preview before destructive removals when practical
- do not modify unrelated user-created skills
- mark managed skills clearly so they can be reconciled safely

## Managed skill identity

Each managed specialist should have:

- a stable role id
- a runtime install path
- provenance back to `.expert-team/team-spec.json`
- a project-specific prefixed installed name

This lets the bootstrapper update the right specialist without guessing.
