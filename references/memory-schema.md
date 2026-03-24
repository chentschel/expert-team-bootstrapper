# Approved Memory Schema

Long-term memory should be lightweight, reviewed, and external to the generated
role `SKILL.md` files.

Store approved memory in a file such as:

```text
.expert-team/memory/approved-memory.json
```

## Why this exists

Do not update role skills directly from raw chats. That creates prompt drift.

Approved memory should capture only durable information such as:

- project facts discovered after initial generation
- explicit founder preferences
- strategies that were tried and rejected
- strategies or patterns that have worked well
- stable constraints that should keep shaping role behavior

## Suggested structure

```json
{
  "version": 1,
  "approved_facts": [],
  "preferences": [],
  "rejected_strategies": [],
  "successful_patterns": [],
  "open_questions": []
}
```

Each memory entry should be a small object with:

- `id`: stable identifier
- `summary`: concise durable learning
- `source`: where it came from, such as founder decision or repeated project work
- `confidence`: `high`, `medium`, or `low`
- `applies_to`: `global` or a list of role ids/template ids

Optional fields:

- `evidence`
- `notes`
- `last_confirmed`

## Promotion rules

Only promote something into approved memory when it is:

- likely to matter again
- more durable than a one-off task detail
- explicitly confirmed or strongly evidenced

Do not promote:

- stylistic noise from one conversation
- speculative ideas that have not been accepted
- temporary tactical instructions

## Generation rules

When generating or regenerating roles:

- use approved memory to narrow or sharpen the role
- do not let memory override core template boundaries unless explicitly intended
- if memory conflicts with the current dossier, surface the conflict instead of
  silently resolving it
