---
name: skill-router
description: Route a request to the most appropriate installed skill using the local catalog, trigger exclusions, invocation mode, and precedence rules.
---

# Skill Router

Use the local skill catalog as the source of routing metadata.

Read [the catalog contract](references/catalog-v1.md) before resolving a route.

## Workflow

1. Read the installed skill catalog and identify candidate skills from the user's intent.
2. Prefer the narrowest skill whose trigger matches the task.
3. Apply explicit precedence and exclusion rules before selecting a skill.
4. Distinguish user-invoked orchestration from model-invoked reusable discipline.
5. Explain the selected skill and any safer alternative when routing is ambiguous.

The router must not perform the selected skill's work. It only selects or explains the route. Report missing, duplicate, stale, or contradictory catalog entries.
