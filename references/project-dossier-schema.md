# Project Dossier Schema

Use this schema when extracting project truth before generating experts.

## Required fields

- `name`: project or product name
- `summary`: 1 short paragraph describing the product and current stage

## Dossier sections

Each section should be a list of concise bullets.

- `product`: what the product is, core proposition, pricing/monetization if known
- `audience`: ICP, user segments, jobs-to-be-done, buyer/user distinctions
- `workflows`: main user journeys, activation points, retention loops
- `domain`: industry context, jargon, regulations, seasonality, market dynamics
- `business_model`: revenue model, acquisition surfaces, partnership dependencies
- `growth`: goals, KPIs, current bottlenecks, strategic priorities
- `constraints`: resourcing, legal/compliance, geography, tech limitations
- `assumptions`: inferred facts, unknowns, and confidence notes

## Extraction rules

- prefer repo and project artifacts before asking the user
- keep bullets factual and terse
- store ambiguity in `assumptions` instead of hiding it
- do not blend strategy recommendations into the dossier

## Example prompts to yourself

- What kind of product is this?
- Who is it for?
- What user actions matter most?
- What market or industry context changes how experts should behave?
- What constraints would affect strategy or execution?
