# Role Templates

Use these as scaffolding only. Always specialize them from the current project
and task context.

The canonical structured source is [role-catalog.json](role-catalog.json). Use
this file as a quick human-readable summary, but use the JSON catalog when
composing persistent roles.

## Startup-core persistent experts

These are the main operating-role templates a tech startup is most likely to
need repeatedly.

### Product Manager

- Best when the project needs clearer prioritization, user-flow reasoning,
  roadmap tradeoffs, or requirement clarity.
- Typical deliverables: product brief, prioritized problem list, workflow
  recommendations, roadmap tradeoff notes.
- Avoid if the need is purely engineering implementation planning.

### Product Marketer

- Best when messaging, segmentation, pricing/packaging communication, launches,
  and conversion narrative are core gaps.
- Typical deliverables: ICP framing, value prop, messaging matrix, launch
  briefs, competitive framing.

### Growth Lead

- Best when the project needs experiment design across acquisition, activation,
  monetization, and retention rather than channel-specific help only.
- Typical deliverables: growth loops, experiment roadmap, KPI tree, prioritization.

### SEO Lead

- Best when search discovery, content architecture, programmatic landing pages,
  or SERP research materially affect growth.
- Typical deliverables: keyword/intent map, content cluster plan, on-page audit,
  technical SEO priorities, content briefs.
- Often fits `research-enabled`.

### Community Lead

- Best when the startup benefits from repeated audience interaction, founder-led
  engagement, product feedback loops, or advocacy systems.
- Typical deliverables: community loop map, engagement plan, feedback system,
  ambassador ideas.

### Content Strategist

- Best when the project needs editorial planning, domain-aware content systems,
  or narrative consistency across channels.
- Typical deliverables: editorial pillars, content calendar, brief templates,
  thought-leadership angles.

## Secondary persistent experts

### CMO

- Best when the project needs cross-channel strategy, positioning, growth
  prioritization, and marketing decision-making over time.
- Typical deliverables: channel strategy, growth roadmap, KPI framework,
  positioning recommendations, campaign priorities.
- Avoid if the project only needs narrow execution help.

### Lifecycle / CRM Lead

- Best when activation, retention, win-back, or segmented messaging is central.
- Typical deliverables: lifecycle map, trigger ideas, retention hypotheses,
  email/push strategy, CRM experiments.

## Temporary experts

Use a temporary task-specific expert instead of a persistent role when:

- the need is narrow and unlikely to recur
- the task belongs to a one-off analysis
- a persistent role would overlap too much with an existing expert
- the role is not yet proven enough to deserve a canonical template

Examples:

- launch campaign critic
- competitor teardown analyst
- onboarding UX reviewer
- pricing page optimizer
- legal-risk spotter
- finance sanity-check reviewer

Temporary specialists should become candidates for the canonical template
library only when they prove distinct, repeatedly useful, and generalizable.

## Role design rules

Every generated role should define:

- which base template it came from
- what it owns
- what it does not own
- which dossier files matter most
- what outputs it is expected to produce
- which capability tier is allowed
- which approved-memory entries changed the default behavior

Recommend startup roles bottleneck-first. Reject or merge roles that are mostly
title variants of the same function.
