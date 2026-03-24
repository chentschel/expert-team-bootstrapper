# Capability Policy

External data is optional and role-scoped.

## Capability tiers

### `local-only`

Allowed:

- repo files
- generated shared references
- local notes or assets

Use as the default for most generated experts.

### `research-enabled`

Allowed:

- web search
- external documentation
- market or competitor research

Use when the role routinely needs outside facts, such as SEO, market research,
or competitive positioning.

### `browser-enabled`

Allowed:

- browser inspection of live sites, flows, or products
- browser-based UX or competitor review

Use only when the task clearly requires interactive inspection.

## Operating rules

- project facts remain primary truth
- external findings must be distinguishable from repo-derived facts
- outside research should be proportional to the task
- do not assign broad capabilities just because the tool exists
- prefer the lowest capability tier that still lets the role do the work

## Suggested defaults

- CMO: `local-only`, sometimes `research-enabled`
- SEO Lead: `research-enabled`
- Product Marketer: `local-only`, sometimes `research-enabled`
- Lifecycle Lead: `local-only`
- Growth Lead: `local-only`, sometimes `research-enabled`
- UX Reviewer: `browser-enabled` only when explicitly needed
