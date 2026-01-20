# /learn — Deep Dive for Developers

## Architecture Overview

`/learn` uses **3 parallel Haiku agents** to explore codebases efficiently.

```
┌─────────────────────────────────────────────────────┐
│                    Main Agent                        │
│                   (Orchestrator)                     │
└──────────────┬──────────────┬──────────────┬────────┘
               │              │              │
               ▼              ▼              ▼
        ┌──────────┐   ┌──────────┐   ┌──────────┐
        │ Agent 1  │   │ Agent 2  │   │ Agent 3  │
        │ Arch     │   │ Code     │   │ Quick    │
        │ Explorer │   │ Snippets │   │ Ref      │
        └──────────┘   └──────────┘   └──────────┘
               │              │              │
               ▼              ▼              ▼
        ┌──────────────────────────────────────────┐
        │              Output Files                 │
        │  ψ/learn/[REPO]/                         │
        │  ├── [DATE]_ARCHITECTURE.md              │
        │  ├── [DATE]_CODE-SNIPPETS.md             │
        │  ├── [DATE]_QUICK-REFERENCE.md           │
        │  └── [REPO].md (hub file)                │
        └──────────────────────────────────────────┘
```

---

## Agent Responsibilities

### Agent 1: Architecture Explorer

```typescript
// Task prompt
{
  subagent_type: "Explore",
  model: "haiku",
  prompt: `
    Analyze repository structure:
    - Directory layout
    - Entry points (main, index, app)
    - Core abstractions (classes, modules)
    - Dependencies (package.json, go.mod, etc.)
    - Design patterns used
  `
}
```

**Output**: `YYYY-MM-DD_ARCHITECTURE.md`

### Agent 2: Code Snippets Collector

```typescript
{
  subagent_type: "Explore",
  model: "haiku",
  prompt: `
    Extract key code examples:
    - Main entry point code
    - Core function implementations
    - Interesting patterns
    - API usage examples
  `
}
```

**Output**: `YYYY-MM-DD_CODE-SNIPPETS.md`

### Agent 3: Quick Reference Builder

```typescript
{
  subagent_type: "Explore",
  model: "haiku",
  prompt: `
    Create quick reference:
    - What the project does
    - Installation steps
    - Key features
    - Common usage patterns
    - CLI commands (if any)
  `
}
```

**Output**: `YYYY-MM-DD_QUICK-REFERENCE.md`

---

## Input Resolution

```bash
# URL input
/learn https://github.com/owner/repo
  → GHQ_ROOT="ψ/learn/repo" ghq get "$URL"
  → Resolves to ψ/learn/repo/github.com/owner/repo

# Slug input (from slugs.yaml)
/learn my-project
  → grep "^my-project:" ψ/memory/slugs.yaml | cut -d: -f2

# Direct path
/learn /path/to/repo
  → Uses path directly
```

---

## Output Structure

```
ψ/learn/
├── repo-name/
│   ├── repo-name.md              # Hub file (index)
│   ├── 2024-01-20_ARCHITECTURE.md
│   ├── 2024-01-20_CODE-SNIPPETS.md
│   └── 2024-01-20_QUICK-REFERENCE.md
└── another-repo/
    └── ...
```

### Hub File Format

```markdown
# [REPO_NAME] Learning Index

## Latest Exploration
**Date**: 2024-01-20

**Files**:
- [[2024-01-20_ARCHITECTURE|Architecture]]
- [[2024-01-20_CODE-SNIPPETS|Code Snippets]]
- [[2024-01-20_QUICK-REFERENCE|Quick Reference]]

## Timeline
### 2024-01-20 (First exploration)
- Initial discovery
- Core: [main pattern identified]

### 2024-01-25 (Second pass)
- Deeper dive into X
- Found Y pattern
```

---

## Why 3 Agents?

| Aspect | Benefit |
|--------|---------|
| **Parallel execution** | 3x faster than sequential |
| **Haiku model** | Cost effective (~$0.001/exploration) |
| **Focused prompts** | Each agent specializes |
| **Main reviews** | Quality gate before output |

---

## Cost Analysis

```
Per /learn invocation:
- 3 Haiku agents: ~3 × $0.0003 = $0.0009
- Main agent synthesis: ~$0.002
- Total: ~$0.003 per repo exploration
```

---

## Integration with /project

```bash
# Study mode (read-only)
/project learn https://github.com/owner/repo
  → ghq clone
  → symlink to ψ/learn/
  → Ready for /learn

# Then explore
/learn repo-name
  → 3 agents analyze
  → Documentation generated
```

---

## Best Practices

### 1. Re-explore periodically

```bash
# First exploration
/learn cool-project

# After major updates
/learn cool-project  # Creates new dated files
```

Hub file accumulates timeline of explorations.

### 2. Combine with Oracle

```bash
# After learning
oracle_learn({
  pattern: "cool-project uses X pattern for Y",
  concepts: ["architecture", "cool-project"]
})
```

### 3. Use for PR review prep

```bash
# Before reviewing external PR
/learn https://github.com/owner/repo
# Now you understand the codebase
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Repo not found" | Check URL, run `ghq get` manually |
| Slow exploration | Large repos take longer, be patient |
| Missing output | Check ψ/learn/ directory exists |
| Agent timeout | Retry, or explore smaller scope |

---

## Advanced: Custom Exploration

Override agent prompts for specific needs:

```
/learn repo-name

Focus on:
- Authentication flow only
- Database schema
- API endpoints
```

Main agent will adjust sub-agent prompts accordingly.
