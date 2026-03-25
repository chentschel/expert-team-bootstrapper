# Manifest Schema

The manifest is the runtime-facing inventory for the managed team. It should
live at:

```text
.expert-team/manifest.json
```

## Purpose

Use the manifest to reconcile canonical project state with runtime-installed
specialists.

Treat the manifest as runtime-facing state, not as an authority to bypass path
validation or managed-identity checks.

It should answer:

- which specialists are currently managed
- where they are installed
- which install scope was chosen
- which spec and memory state they were derived from
- which runtime the manifest applies to

## Suggested structure

```json
{
  "version": 1,
  "project": {
    "name": "Example",
    "slug": "example"
  },
  "management": {
    "runtime": "codex",
    "mode": "apply",
    "install_scope": "project",
    "install_root": ".codex/skills",
    "managed_prefix": "example-"
  },
  "state": {
    "team_spec_path": ".expert-team/team-spec.json",
    "approved_memory_path": ".expert-team/memory/approved-memory.json"
  },
  "specialists": []
}
```

## Required specialist fields

Each entry in `specialists` should include:

- `id`: role id from the team spec
- `managed_name`: installed skill name, usually `<project>-<role>`
- `template_id`: base template used
- `install_path`: runtime-specific path where the skill is installed
- `install_scope`: `project` or `user`
- `capability_tier`: effective capability tier
- `reads`: dossier files used by the specialist
- `memory_refs`: approved-memory entries used to specialize the role
- `managed_by`: fixed value `expert-team-bootstrapper`
- `last_applied`: last successful reconciliation timestamp

Optional fields:

- `status`: `active`, `stale`, `archived`, or `error`
- `notes`

## Validation rules

- `install_root` must be an approved runtime skill root for the active runtime
- `install_path` is derived metadata and must be recomputed or validated before use
- `managed_name` should be a safe slug using lowercase letters, digits, and hyphens
- reject path traversal, symlink escapes, and unapproved absolute paths

## Reconciliation rules

On apply:

- create entries for new specialists
- update changed entries after writing the new specialist artifact
- mark removed specialists as `stale` before deleting or archiving them
- keep only one active manifest entry per managed specialist

On preview:

- do not mutate installed specialists
- produce the same structural decisions that apply would produce
- include planned `create`, `update`, `remove`, or `keep` actions
