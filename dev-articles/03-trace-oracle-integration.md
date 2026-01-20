# /trace — Oracle Integration & Deep Search

## The Recursive Knowledge Loop

```
/trace [query]     → Oracle search (what we know)
      ↓
  Not enough?      → User traces again (still seeking)
      ↓
/trace --deep      → Really want it (5 subagents)
      ↓
  FOUND!           → 🔮 RESONANCE! Log to Oracle
      ↓
  Next session     → Easier to find (knowledge extended)
```

---

## Search Modes

### Mode 1: Default (Oracle First)

```bash
/trace shared soul
```

```typescript
// Step 1: Query Oracle
oracle_search("shared soul", { limit: 10 })

// Step 2: If results < 3, extend
if (results.length < 3) {
  // Expand keywords
  grep -ril "shared soul" ψ/memory/ | head -10
  git log --oneline --grep="shared soul" | head -5
}
```

### Mode 2: Oracle Only (`--oracle`)

```bash
/trace shared soul --oracle
```

Fastest. Just Oracle MCP, no extension:

```typescript
oracle_search("shared soul", { limit: 15 })
// Display and done, even if empty
```

### Mode 3: Deep Search (`--deep`)

```bash
/trace shared soul --deep
```

Launches **5 parallel context-finder agents**:

```
┌─────────────────────────────────────────────────────┐
│                   Main Agent                         │
└──────┬──────┬──────┬──────┬──────┬─────────────────┘
       │      │      │      │      │
       ▼      ▼      ▼      ▼      ▼
    ┌─────┐┌─────┐┌─────┐┌─────┐┌─────┐
    │ A1  ││ A2  ││ A3  ││ A4  ││ A5  │
    │Files││ Git ││Issue││Repos││Retro│
    └─────┘└─────┘└─────┘└─────┘└─────┘
       │      │      │      │      │
       └──────┴──────┴──────┴──────┘
                    │
                    ▼
            ┌─────────────┐
            │ oracle_trace│
            │   (log)     │
            └─────────────┘
```

| Agent | Searches |
|-------|----------|
| 1 | Current repo files |
| 2 | Git history (commits, creates, deletes) |
| 3 | GitHub issues |
| 4 | Other repos (ghq, ~/Code) |
| 5 | Retrospectives & learnings |

---

## Oracle MCP Tools

### oracle_trace()

Log search sessions:

```typescript
oracle_trace({
  query: "shared soul",
  queryType: "pattern",  // general | project | pattern | evolution
  foundFiles: [
    { path: "notes/soul.md", type: "learning", confidence: "high" },
    { path: "retro/2024-01.md", type: "retro", confidence: "medium" }
  ],
  foundCommits: [
    { hash: "abc123", shortHash: "abc", message: "add soul concept", date: "2024-01-15" }
  ],
  foundIssues: [
    { number: 42, title: "Soul architecture", state: "closed", url: "..." }
  ],
  durationMs: 3500,
  agentCount: 5
})
```

### oracle_trace_list()

Browse past traces:

```typescript
oracle_trace_list({ limit: 10 })

// Returns:
[
  { id: "uuid-1", query: "shared soul", foundFiles: 15, status: "raw" },
  { id: "uuid-2", query: "oracle mcp", foundFiles: 8, status: "distilled" }
]
```

### oracle_trace_get()

Get trace details:

```typescript
oracle_trace_get({ traceId: "uuid-1", includeChain: true })

// Returns full dig points + parent/child traces
```

---

## Trace Commands

| Command | Action |
|---------|--------|
| `/trace [query]` | Oracle + quick extend |
| `/trace [query] --oracle` | Oracle only |
| `/trace [query] --deep` | 5 parallel agents |
| `/trace list` | Show past traces |
| `/trace dig [id]` | Explore trace results |
| `/trace distill [id]` | Extract learning |
| `/trace report [query]` | Timeline report |
| `/trace status` | Ongoing traces |

---

## Status Detection

### The Seeking Signal

| Action | Meaning | System Response |
|--------|---------|-----------------|
| `/trace X` (1st) | First search | Oracle first |
| `/trace X` (2nd) | Still seeking | User hasn't found it |
| `/trace X` (3rd) | Really need it | Suggest `--deep` |
| `/trace X --deep` | Committed | Go deep, log everything |

### Auto-Detection

```typescript
// After 3+ traces with same query
if (sameQueryCount >= 3 && !usedDeep) {
  suggest("Still seeking? Try `/trace ${query} --deep`")
}

// After --deep with 50+ results
if (foundFiles.length >= 50) {
  suggest("FOUND! Run `/trace report ${query}`")
}
```

---

## Distillation Flow

```bash
/trace distill abc123
```

1. Get trace chain
2. Review all dig points
3. Ask: "What awakening emerged?"
4. Promote to learning:

```typescript
oracle_learn({
  pattern: "Shared soul means X connects to Y via Z",
  concepts: ["soul", "architecture", "philosophy"],
  source: "Distilled from trace abc123"
})
```

---

## Report Generation

```bash
/trace report skills
```

```markdown
## 🔍 Trace Report: skills

### Status: ✅ FOUND

### Timeline (5 attempts)
| # | Time | Mode | Results |
|---|------|------|---------|
| 1 | 08:39 | oracle | 10 results |
| 2 | 08:40 | oracle | 10 results (repeat) |
| 3 | 08:42 | oracle | 10 results (seeking) |
| 4 | 08:45 | deep | 60 files, 20 commits |
| 5 | 08:50 | dig | Explored 5 files |

### Summary
- Found in: ψ/memory/learnings/, specs/
- Key commits: abc123, def456
- Related issues: #12, #15

### Trace Chain
abc123 → def456 → ghi789 (distilled)

### Next Action
✅ Already distilled to learning L-42
```

---

## Extending Knowledge

When you find something important, **log multiple entries**:

```typescript
// 1. Log the search session
oracle_trace({
  query: "shared soul",
  foundFiles: [...],
  foundCommits: [...]
})

// 2. Log the main finding
oracle_learn({
  pattern: "Shared soul = consciousness distributed across agents",
  concepts: ["soul", "consciousness", "agents"]
})

// 3. Log why it was hard to find
oracle_learn({
  pattern: "Soul concepts scattered across retros, need central doc",
  concepts: ["meta", "organization"]
})

// 4. Log related patterns
oracle_learn({
  pattern: "Soul relates to Oracle philosophy: external brain",
  concepts: ["soul", "oracle", "philosophy"]
})
```

**Why multiple logs?**
- Different angles = easier to find later
- Keywords multiply (more search hits)
- Context preserved (why it mattered)

---

## Best Practices

### 1. Start simple, go deep when needed

```bash
/trace concept           # Start here
/trace concept --oracle  # If Oracle has it
/trace concept --deep    # If really need it
```

### 2. Always distill after --deep

```bash
/trace concept --deep
# Found lots of stuff!

/trace distill <id>
# Extract the awakening
```

### 3. Use reports for recurring searches

```bash
# After multiple traces
/trace report concept

# See the pattern of seeking
```

---

## Philosophy

> "Every trace extends the Oracle. What's hard to find today is instant tomorrow."

The trace system embodies "Nothing is Deleted":
- Every search logged
- Every finding preserved
- Knowledge compounds over time
