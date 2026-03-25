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

Use [manifest-schema.md](manifest-schema.md) for the manifest contract and
[managed-specialist-spec.md](managed-specialist-spec.md) for the installed
specialist artifact shape. Use [security-model.md](security-model.md) for trust
boundaries, redaction, and path-safety rules.

## Default install targets

Generated specialists should inherit the active runtime's preferred scope when
that scope can be detected. If the bootstrapper was installed into a known
project-level or user-level location for that runtime, use the same scope style
for managed specialists unless the user explicitly asks for something else. For
project-specific teams, prefer project-local installation. Use user-level
installation only when the user explicitly asks for it or when the runtime does
not support project-local skills.

Install targets must be treated as constrained runtime roots, not arbitrary file
system paths.

### Codex

- prefer the same scope style the bootstrapper itself is using when that is discoverable
- if the runtime exposes only a user-level skills directory, use that directory
- otherwise prefer a project-local managed location for project-specific teams
- use a project-prefixed skill id to avoid collisions

Example user-level install:

```text
~/.codex/skills/fantasypulse-seo-lead/
```

### Claude Code

- prefer project-level installation for project-specific specialists
- fall back to user-level installation only when explicitly requested
- use the same project-prefixed skill id

Example project-level install:

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
- validate every target path before writing, archiving, or removing
- compute safe install paths from approved roots and sanitized managed names instead of trusting raw manifest paths

When the bootstrapper runs in preview mode, it should:

- calculate the same create, update, remove, and keep decisions
- show the planned changes without mutating canonical state or installed specialists

## Safety rules

- default to preview before runtime-scope writes or removals
- require explicit user intent for apply when runtime-installed specialists will change
- show a preview before destructive removals when practical
- do not modify unrelated user-created skills
- mark managed skills clearly so they can be reconciled safely
- do not follow symlinks or path traversal outside approved runtime roots
- reject unsafe slugs or managed names rather than attempting to normalize silently

## Managed skill identity

Each managed specialist should have:

- a stable role id
- a runtime install path
- an install scope such as `project` or `user`
- provenance back to `.expert-team/team-spec.json`
- a project-specific prefixed installed name

This lets the bootstrapper update the right specialist without guessing.

Do not update or remove a skill based on name matching alone. Require matching
managed marker, project slug, role id, template id, and source-of-truth path.
