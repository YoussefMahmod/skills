# Language-Specific Patterns

Grep patterns and conventions per language. Adapt to the detected project language.

## Function Declarations

| Language | Pattern |
|----------|---------|
| JS/TS | `(function\|const\|let\|var)\s+\w+\s*=?\s*(async\s*)?\(` |
| Python | `def \w+\(` |
| Go | `func \w+\(` |
| Rust | `fn \w+\(` |
| Java | `(public\|private\|protected)\s+.*\w+\s*\(` |

## Import Statements

| Language | Pattern |
|----------|---------|
| JS/TS | `import .+ from ['"](.+)['"]` and `require\(['"](.+)['"]\)` |
| Python | `from (\S+) import` and `import (\S+)` |
| Go | `"(.+)"` inside import blocks |
| Rust | `use (.+);` |
| Java | `import (.+);` |

## Export Declarations

| Language | Pattern | Notes |
|----------|---------|-------|
| JS/TS | `export (default \|)(function\|class\|const\|let\|var\|type\|interface\|enum)\s+(\w+)` | Also check `export \{ .+ \}` |
| Python | `__all__` definitions; public functions (no `_` prefix) | |
| Go | Capitalized function/type names | |
| Rust | `pub fn`, `pub struct`, `pub enum` | |

## Entry Points to Exclude

Files that should be excluded from unused-export analysis:

- JS/TS: `index.ts`, `main.ts`, `app.ts`, `mod.ts`, `.d.ts` files
- Python: `__init__.py`, `__main__.py`
- Go: `main.go`
- Rust: `main.rs`, `lib.rs`, `mod.rs`
- Java: classes with `public static void main`

## File Extensions by Ecosystem

| Ecosystem | Extensions |
|-----------|-----------|
| JS/TS | `.ts`, `.tsx`, `.js`, `.jsx`, `.mjs`, `.cjs` |
| Python | `.py` |
| Go | `.go` |
| Rust | `.rs` |
| Java/Kotlin | `.java`, `.kt` |
| C/C++ | `.c`, `.cpp`, `.h`, `.hpp` |
| Ruby | `.rb` |
| PHP | `.php` |

## Excluded Directories

Always exclude from scanning:
`node_modules`, `dist`, `build`, `.next`, `out`, `__pycache__`, `target`, `vendor`, `.git`, `coverage`, `.claude`, `.venv`, `env`, `.tox`, `bin` (for Go)
