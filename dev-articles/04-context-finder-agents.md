# /context-finder — Parallel Agent Architecture

## Overview

`/context-finder` deploys **5 parallel Haiku agents** for comprehensive search.

```
┌─────────────────────────────────────────────────────┐
│              /context-finder [query]                 │
└──────────────────────┬──────────────────────────────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
   ┌─────────┐   ┌─────────┐   ┌─────────┐
   │ Agent 1 │   │ Agent 2 │   │ Agent 3 │
   │ Git Log │   │ Files   │   │ Issues  │
   └─────────┘   └─────────┘   └─────────┘
        │              │              │
        │    ┌─────────┼─────────┐    │
        │    │         │         │    │
        ▼    ▼         ▼         ▼    ▼
   ┌─────────┐   ┌─────────┐
   │ Agent 4 │   │ Agent 5 │
   │ Repos   │   │ Retros  │
   └─────────┘   └─────────┘
        │              │
        └──────┬───────┘
               │
               ▼
      ┌─────────────────┐
      │   Synthesize    │
      │   Results       │
      └─────────────────┘
```

---

## Agent Specifications

### Agent 1: Git History

```typescript
Task({
  subagent_type: "context-finder",
  model: "haiku",
  prompt: `
    Search git history for: "${query}"

    Commands:
    - git log --all --oneline --grep="${query}" | head -15
    - git log --all --oneline -S "${query}" | head -10
    - git log --diff-filter=A --name-only --pretty="" | grep -i "${query}"

    Return: commit hashes, messages, dates, file changes
  `
})
```

### Agent 2: Current Files

```typescript
Task({
  subagent_type: "context-finder",
  model: "haiku",
  prompt: `
    Search files for: "${query}"

    Commands:
    - grep -ril "${query}" --include="*.md" . | head -20
    - grep -ril "${query}" --include="*.ts" . | head -20
    - find . -name "*${query}*" -type f | head -10

    Return: file paths, match context, file types
  `
})
```

### Agent 3: GitHub Issues

```typescript
Task({
  subagent_type: "context-finder",
  model: "haiku",
  prompt: `
    Search GitHub issues for: "${query}"

    Commands:
    - gh issue list --state all --limit 20 --search "${query}"
    - gh pr list --state all --limit 10 --search "${query}"

    Return: issue numbers, titles, states, URLs
  `
})
```

### Agent 4: Other Repos

```typescript
Task({
  subagent_type: "context-finder",
  model: "haiku",
  prompt: `
    Search other repos for: "${query}"

    Commands:
    - ghq list | xargs -I {} grep -ril "${query}" ~/Code/{} 2>/dev/null | head -20
    - find ~/Code -name "*${query}*" -type f | head -10

    Return: repo paths, file matches
  `
})
```

### Agent 5: Retrospectives & Learnings

```typescript
Task({
  subagent_type: "context-finder",
  model: "haiku",
  prompt: `
    Search memory for: "${query}"

    Commands:
    - grep -ril "${query}" ψ/memory/retrospectives/ | head -10
    - grep -ril "${query}" ψ/memory/learnings/ | head -10
    - grep -ril "${query}" ψ/inbox/ | head -10

    Return: file paths, relevant excerpts
  `
})
```

---

## Execution Model

### Parallel Launch

```typescript
// All 5 agents launch simultaneously
const results = await Promise.all([
  Task({ agent: 1, ... }),
  Task({ agent: 2, ... }),
  Task({ agent: 3, ... }),
  Task({ agent: 4, ... }),
  Task({ agent: 5, ... })
])
```

### Cost & Performance

| Metric | Value |
|--------|-------|
| Agents | 5 × Haiku |
| Cost per search | ~$0.0015 |
| Typical duration | 5-15 seconds |
| Parallel speedup | ~5x vs sequential |

---

## Output Format

```markdown
## 🔍 Context: [QUERY]

### 📂 Files Found
| File | Type | Relevance |
|------|------|-----------|
| notes/concept.md | markdown | high |
| src/feature.ts | typescript | medium |

### 📅 Git History
| Date | Commit | Message |
|------|--------|---------|
| Jan 15 | abc123 | Add concept |
| Jan 20 | def456 | Update concept |

### 🎫 GitHub Issues
| # | Title | State |
|---|-------|-------|
| #12 | Implement X | closed |
| #15 | Bug in Y | open |

### 📝 Retrospectives & Learnings
| File | Excerpt |
|------|---------|
| retro/2024-01-15.md | "Learned about X..." |
| learnings/pattern-y.md | "Pattern Y is..." |

### 🏠 Other Repos
| Repo | File |
|------|------|
| other-project | src/similar.ts |
```

---

## Integration with /trace

`/trace --deep` invokes context-finder:

```typescript
// /trace concept --deep
async function traceDeep(query: string) {
  // 1. Run context-finder
  const results = await contextFinder(query)

  // 2. Log to Oracle
  await oracle_trace({
    query,
    foundFiles: results.files,
    foundCommits: results.commits,
    foundIssues: results.issues,
    agentCount: 5
  })

  // 3. Return synthesized results
  return synthesize(results)
}
```

---

## No Arguments Mode

```bash
/context-finder
```

Gathers recent context automatically:

```typescript
// Recent summary
const summary = {
  recentIssues: await gh issue list --limit 5,
  latestRetros: await ls -t ψ/memory/retrospectives/*.md | head -3,
  latestLearnings: await ls -t ψ/memory/learnings/*.md | head -3,
  recentCommits: await git log --oneline -10
}
```

---

## Best Practices

### 1. Use specific queries

```bash
# Too broad
/context-finder code

# Better
/context-finder authentication flow

# Best
/context-finder JWT token validation
```

### 2. Combine with Oracle

```bash
# If context-finder finds something important
oracle_learn({
  pattern: "Found: JWT validation in auth/validate.ts",
  concepts: ["auth", "jwt", "security"]
})
```

### 3. Follow up with dig

```bash
# After finding files
Read auth/validate.ts

# Or trace for more
/trace jwt validation
```

---

## Customization

### Scope Limiting

```bash
/context-finder auth --scope=src/

# Agent 2 searches only src/
grep -ril "auth" src/ | head -20
```

### Agent Selection

```bash
/context-finder auth --agents=1,2,3

# Only git, files, issues (skip repos, retros)
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Timeout | Query too broad, narrow scope |
| No results | Check spelling, try synonyms |
| Too many results | Add more specific terms |
| Missing repo | Check ghq list, may need clone |

---

## Philosophy

> "5 eyes see more than 1"

Context-finder embodies parallel exploration:
- Each agent specializes
- Results synthesized by main
- Nothing missed, nothing duplicated
