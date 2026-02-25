# Debt Report Template

Use this exact structure for the output report. Save to `DEBT-REPORT.md` in the project root.

```markdown
# Technical Debt Report
> Project: {project_name}
> Scanned: {date}
> Files analyzed: {count}
> Source directories: {dirs}

## Executive Summary

| Category | Critical | High | Medium | Low | Total |
|----------|----------|------|--------|-----|-------|
| TODOs/FIXMEs | — | — | — | — | — |
| Oversized Files | — | — | — | — | — |
| Duplicated Logic | — | — | — | — | — |
| Unused Exports | — | — | — | — | — |
| Circular Deps | — | — | — | — | — |
| Code Smells | — | — | — | — | — |
| **Total** | — | — | — | — | — |

**Overall debt level:** {Low / Moderate / High / Critical}
- Low: < 10 total findings, no criticals
- Moderate: 10–30 findings or 1–2 criticals
- High: 30–60 findings or 3+ criticals
- Critical: 60+ findings or systemic issues

## Top 10 Quick Wins

Highest-priority items sorted by priority score descending.

| # | Category | Finding | File | Severity | Effort | Score |
|---|----------|---------|------|----------|--------|-------|
| 1 | ... | ... | ... | ... | ... | ... |

## Detailed Findings

### 1. TODOs / FIXMEs / HACKs
| File | Line | Tag | Text | Severity | Effort |
|------|------|-----|------|----------|--------|

### 2. Oversized Files
| File | Lines | Severity | Effort | Notes |
|------|-------|----------|--------|-------|

### 3. Duplicated Logic
| Description | Files | Severity | Effort |
|-------------|-------|----------|--------|

### 4. Unused Exports
| Symbol | File | Type | Severity | Effort |
|--------|------|------|----------|--------|

### 5. Circular Dependencies
| Cycle | Severity | Effort |
|-------|----------|--------|

### 6. Code Smells
| Smell | File | Line | Severity | Effort |
|-------|------|------|----------|--------|

## Effort Summary

| Effort | Count | Estimated Total |
|--------|-------|-----------------|
| S (< 30 min) | — | — |
| M (1–3 hours) | — | — |
| L (half day) | — | — |
| XL (1+ days) | — | — |

## Recommendations

{3–5 prioritized recommendations focused on systemic improvements, not individual fixes.}
```
