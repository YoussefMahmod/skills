---
name: debt-audit
description: "Technical Debt Scanner — Scan any codebase for TODOs, FIXMEs, duplicated logic, unused exports, oversized files, circular dependencies, and code smells. Generates a prioritized debt report with estimated effort. Use when the user asks to: (1) audit technical debt, (2) find TODOs or FIXMEs, (3) detect code duplication, (4) find unused exports or dead code, (5) check for circular dependencies, (6) assess code quality or code smells, (7) generate a debt report, or any request about codebase health or maintainability."
---

# Technical Debt Audit

Systematically scan the current project and produce a prioritized debt report saved to `DEBT-REPORT.md`.

## Workflow Overview

1. Detect project context (language, source dirs, file extensions)
2. Run 6 scan phases (parallelize where independent)
3. Score and prioritize all findings
4. Generate the report using the template in [references/report-template.md](references/report-template.md)

## Pre-Scan Setup

1. Detect language/framework from config files (`package.json`, `tsconfig.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, etc.)
2. Identify source directories (typically `src/`, `lib/`, `app/`, `packages/`)
3. Determine file extensions — see [references/patterns.md](references/patterns.md) for per-language extensions and excluded directories
4. Glob all source files to establish the working file set

---

## Phase 1: TODO / FIXME / HACK Comments

Grep with pattern `(?i)\b(TODO|FIXME|HACK|XXX|WORKAROUND)\b` across all source files, output_mode "content".

**Severity by tag:**

| Tag | Severity |
|-----|----------|
| FIXME | high |
| HACK / WORKAROUND | high |
| XXX | medium |
| TODO | medium |
| DEPRECATED (in comments) | low |

**Effort:** S if clear action, M if references external system, L if vague.

---

## Phase 2: Oversized Files

Count lines for all source files using `wc -l` via Bash. Sort descending, take top 40.

| Lines | Severity |
|-------|----------|
| > 800 | critical |
| > 500 | high |
| > 300 | medium |

**Effort:** M if one large function to extract, L if many sections to split, XL if deeply interconnected.

---

## Phase 3: Duplicated Logic

Three detection strategies:

**A. Repeated function names** — Grep for function declarations (see [references/patterns.md](references/patterns.md)). Flag identical names in different files.

**B. Repeated code blocks** — Search for string literals, fetch URLs, regex patterns, or error messages appearing 3+ times across different files.

**C. Similar utilities** — Look for multiple files implementing date formatting, string helpers, HTTP wrappers, auth token handling, or error formatting.

**Severity:** high if 3+ places, medium if 2 places.
**Effort:** S to extract shared utility, M to consolidate with interface changes, L for significant refactor.

---

## Phase 4: Unused Exports

Two-pass approach:

1. **Collect** all exported symbols (see [references/patterns.md](references/patterns.md) for export patterns per language)
2. **Verify** each symbol has at least one import/reference elsewhere in the codebase

Exclude entry points and public API files — see [references/patterns.md](references/patterns.md) for the list.

**Severity:** medium for unused functions/classes, low for unused types or barrel re-exports.
**Effort:** S for simple removal, M if usage is unclear (library code).

On large codebases (100+ files), limit to the 20 largest/most central modules and note the scope.

---

## Phase 5: Circular Dependencies

1. Build a partial import graph for the top 30–50 most imported files (see [references/patterns.md](references/patterns.md) for import patterns)
2. Resolve relative paths to project-absolute paths
3. Detect cycles up to 4 levels deep, prioritizing direct A ↔ B cycles

| Cycle Length | Severity |
|-------------|----------|
| 2 files | critical |
| 3 files | high |
| 4 files | medium |

**Effort:** M to extract shared types, L to restructure, XL for deep entanglement.

---

## Phase 6: Code Smells

Quick heuristic pass:

- **Long parameter lists** — 5–6 params: medium/S, 7+: high/M
- **Loose typing** (TS only) — Grep `:\s*any` and `as any`. 4+ per file: medium/S
- **Deep nesting** — 4+ indentation levels in largest files. 4: medium/S, 5+: high/M
- **Magic numbers/strings** — Hardcoded values (excluding 0, 1, -1) repeated across files: low/S

---

## Scoring & Prioritization

```
Severity weights:  critical=4, high=3, medium=2, low=1
Effort weights:    S=1, M=2, L=4, XL=8

Priority Score = severity_weight × (4 / effort_weight)
```

| Score | Priority |
|-------|----------|
| 12–16 | P0 — Fix immediately |
| 6–11 | P1 — Fix soon |
| 3–5 | P2 — Next sprint |
| 1–2 | P3 — Backlog |

---

## Output

Generate the report following [references/report-template.md](references/report-template.md). Save to `DEBT-REPORT.md` in the project root.

## Execution Notes

- Phases 1, 2, and 6 are independent — run in parallel
- Mark uncertain findings with `?` suffix on severity
- Skip irrelevant phases (e.g., no TS-specific smells for Python projects)
- For 100+ file projects, sample strategically and note scope in report
