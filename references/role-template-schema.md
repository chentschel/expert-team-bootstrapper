# Role Template Schema

Use structured role templates as the first source for any generated persistent
expert. The current catalog lives in
[role-catalog.json](role-catalog.json).

## Why this exists

Role titles alone are too weak. A generated role should come from:

1. template
2. project dossier
3. approved project memory

That keeps responsibilities consistent across projects while still allowing
specialization.

The startup-focused canonical catalog should prioritize operating roles a tech
startup needs repeatedly. Temporary specialists may still exist outside the
catalog, but they are not canonical templates.

## Required template fields

- `id`: stable identifier used in specs and manifests
- `role_class`: `core` or `secondary` for canonical startup-role templates
- `title`: human-facing role title
- `purpose`: canonical summary of why the role exists
- `when_to_recommend`: conditions that justify creating the role
- `when_not_to_recommend`: conditions that argue against creating the role
- `owns`: default responsibilities
- `does_not_own`: default exclusions and boundaries
- `deliverables`: default expected outputs
- `success_metrics`: default evaluation signals
- `default_capability_tier`: default external-data tier
- `default_reads`: default dossier files the role should consult

## Generation rules

When generating a persistent role:

- start from exactly one base template
- specialize the template using project dossier facts
- refine it using approved memory only
- keep the generated role narrower than the template, not broader
- if no template fits cleanly, either recommend no role or create a temporary
  task-specific expert instead
- recommend roles bottleneck-first, with startup stage used only as a modifier

## Output traceability

Each generated persistent expert should record:

- `template_id`
- which dossier sections most influenced the role
- which approved-memory entries changed the default template behavior

That traceability should live in `team-spec.json` and optionally in the
generated manifest.

## Promotion rules

Temporary specialists should become candidates for canonical templates only when:

- they solve a distinct recurring role need
- they stay clearly differentiated from existing templates
- they prove useful across multiple iterations or projects
