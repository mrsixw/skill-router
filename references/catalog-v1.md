# Skill Catalog Contract v1

The catalog is a local JSON document with a top-level `version: 1` and a
`skills` object keyed by skill name. Each entry records `source`, `sourceUrl`,
`visibility`, `invocation`, `mutationScope`, and `installedAt`. Optional
`precedence` and `excludes` fields may express relationships that cannot be
inferred safely from names alone:

- `precedence`: skills that this entry **outranks**. When this entry and a
  listed skill both match a request, route to this entry. Typically a
  specialised adapter lists the generic skills it wraps or narrows.
- `excludes`: skills whose triggers are mutually exclusive with this entry.
  Never route to both for the same request; if both appear to match, report
  the ambiguity. Record exclusions on both entries so they stay symmetric.
  Do not use `excludes` for skills that compose (one wraps or invokes the
  other); use `precedence` instead.

Routing rules:

1. Reject entries with missing required fields or unsupported invocation values.
2. Prefer an exact, narrow trigger over a broad one.
3. Prefer a specialized adapter over a generic core when both match.
4. Honor explicit exclusions and never route around a skill's invocation mode.
5. Treat mutation scope as a safety signal and surface it before routing to a
   mutating skill.
6. Report ties, stale sources, and contradictory metadata instead of guessing.

The catalog is routing metadata, not a credential store and not proof that a
remote source is reachable.
