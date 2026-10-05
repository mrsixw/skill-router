---
name: skill-router
description: Route a request to the most appropriate already-installed skill using the local catalogue, trigger exclusions, invocation mode, and precedence rules, without executing it. Use when the user asks which installed skill should handle a task. Not for discovering or installing new skills (use find-skills where it is installed).
---

# Skill router

Resolve a request to the narrowest installed skill that actually owns the work.
Routing is the complete task; do not execute the selected workflow. This skill
chooses among installed skills. Discovering or installing a new skill is out of
scope: defer to `find-skills` where it is installed, and otherwise report that
no installed skill owns the request rather than routing to the closest
approximation.

Read [the catalogue contract] before resolving a route.

## Resolve from installed state

1. Read the local skill catalogue, commonly `~/.agents/skill-catalog.json`, or
   a path the user supplies. Identify candidates from the user's outcome,
   target, and requested operation.
2. Confirm that each candidate is installed and that its instructions are
   readable. Catalogue presence alone is not proof of availability.
3. Apply trigger exclusions, explicit precedence, and scope boundaries.
4. Prefer a specific owner over a broad advisory skill. Use orchestration only
   when the request genuinely spans several workflows.
5. Distinguish a user-invoked workflow from a reusable discipline that the model
   applies automatically.

Report the selected skill, the matching trigger, invocation mode, mutation
boundary, and any prerequisite skill. When two candidates remain credible,
explain the material difference instead of choosing silently.

Report missing, duplicate, stale, contradictory, or unreadable catalogue
entries. Do not substitute a similarly named skill without saying so.

---

[the catalogue contract]: references/catalog-v1.md
