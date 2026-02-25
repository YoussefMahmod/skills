# Claude Code Skills

A collection of reusable skills for [Claude Code](https://claude.com/claude-code).

## Available Skills

| Skill | Description |
|-------|-------------|
| **smart-commits** | Analyze pending changes and create multiple focused, logical commits |
| **debt-audit** | Scan a codebase for technical debt and generate a prioritized report |

## Installation

### Install a specific skill

Use git's sparse checkout to clone only the skill you need directly into your project's `.claude/skills/` directory:

```bash
# From your project root
git clone --depth 1 --filter=blob:none --sparse \
  https://github.com/YoussefMahmod/skills.git /tmp/claude-skills && \
  cd /tmp/claude-skills && \
  git sparse-checkout set <skill-name> && \
  mkdir -p <your-project>/.claude/skills && \
  cp -r <skill-name> <your-project>/.claude/skills/ && \
  rm -rf /tmp/claude-skills
```

For example, to install **debt-audit**:

```bash
git clone --depth 1 --filter=blob:none --sparse \
  https://github.com/YoussefMahmod/skills.git /tmp/claude-skills && \
  cd /tmp/claude-skills && \
  git sparse-checkout set debt-audit && \
  mkdir -p /path/to/your-project/.claude/skills && \
  cp -r debt-audit /path/to/your-project/.claude/skills/ && \
  rm -rf /tmp/claude-skills
```

### Install all skills

```bash
git clone --depth 1 https://github.com/YoussefMahmod/skills.git /tmp/claude-skills && \
  mkdir -p /path/to/your-project/.claude/skills && \
  cp -r /tmp/claude-skills/smart-commits /path/to/your-project/.claude/skills/ && \
  cp -r /tmp/claude-skills/debt-audit /path/to/your-project/.claude/skills/ && \
  rm -rf /tmp/claude-skills
```

### Manual installation

1. Download the skill folder (e.g. `debt-audit/`) from this repo
2. Copy it into your project at `.claude/skills/<skill-name>/`
3. That's it — Claude Code will pick it up automatically

## Usage

Once installed, skills activate automatically based on your prompts. No configuration needed.

- **smart-commits** — Ask Claude to "commit my changes" or "split my changes into commits"
- **debt-audit** — Ask Claude to "audit technical debt" or "scan for code smells"

## Creating Your Own Skills

Each skill is a directory with a `SKILL.md` file containing YAML frontmatter (`name`, `description`) and markdown instructions. See any existing skill for reference.
