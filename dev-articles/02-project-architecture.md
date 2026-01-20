# /project — Architecture & Workflow

## Golden Rule

> **ghq owns the clone → ψ/ owns the symlink**

```
┌─────────────────────────────────────────────────────┐
│                   Source of Truth                    │
│                                                      │
│   ~/Code/  (ghq root)                               │
│   └── github.com/                                   │
│       └── owner/                                    │
│           └── repo/  ← actual files here            │
│                                                      │
└─────────────────────────────────────────────────────┘
                         │
                         │ symlink
                         ▼
┌─────────────────────────────────────────────────────┐
│                   ψ/ References                      │
│                                                      │
│   ψ/learn/repo-name    → ~/Code/.../repo            │
│   ψ/incubate/repo-name → ~/Code/.../repo            │
│                                                      │
└─────────────────────────────────────────────────────┘
```

---

## Why Symlinks?

| Approach | Problem |
|----------|---------|
| Copy files | Duplicates, out of sync |
| Clone to ψ/ | ghq can't manage |
| **Symlink** | Single source, ghq manages |

---

## Directory Structure

```
~/Code/                          # ghq root
├── github.com/
│   ├── vercel/
│   │   └── next.js/            # cloned repo
│   ├── tailwindlabs/
│   │   └── tailwindcss/        # cloned repo
│   └── myorg/
│       └── my-project/         # cloned repo

ψ/
├── learn/
│   ├── next.js     → ~/Code/github.com/vercel/next.js
│   └── tailwindcss → ~/Code/github.com/tailwindlabs/tailwindcss
│
└── incubate/
    └── my-project  → ~/Code/github.com/myorg/my-project
```

---

## Learn vs Incubate

| Mode | Purpose | Typical Actions |
|------|---------|-----------------|
| **learn** | Study, read-only | Browse code, run /learn |
| **incubate** | Active development | Edit, commit, PR |

### Learn Flow

```bash
/project learn https://github.com/vercel/next.js

# Behind the scenes:
ghq get -u https://github.com/vercel/next.js
ln -sf ~/Code/github.com/vercel/next.js ψ/learn/next.js

# Output:
✓ Linked next.js to ψ/learn/next.js
```

### Incubate Flow

```bash
/project incubate https://github.com/myorg/my-fork

# Behind the scenes:
ghq get -u https://github.com/myorg/my-fork
ln -sf ~/Code/github.com/myorg/my-fork ψ/incubate/my-fork

# Output:
✓ Linked my-fork to ψ/incubate/my-fork
```

---

## Commands Reference

### Add Project

```bash
# Study mode
/project learn <url|slug>

# Dev mode
/project incubate <url|slug>
```

### Find Project

```bash
/project find next

# Searches:
# 1. ghq list | grep -i "next"
# 2. ls ψ/learn/ ψ/incubate/ | grep -i "next"
```

### List All

```bash
/project list

# Output:
📚 Learn
  next.js → ~/Code/github.com/vercel/next.js
  tailwindcss → ~/Code/github.com/tailwindlabs/tailwindcss

🌱 Incubate
  my-project → ~/Code/github.com/myorg/my-project

🏠 External (ghq)
  github.com/golang/go
  github.com/rust-lang/rust
  ...
```

---

## Health Check

### Detect Broken Symlinks

```bash
find ψ/learn ψ/incubate -type l ! -exec test -e {} \; -print
```

### Fix Broken Symlinks

```bash
# Re-clone source
ghq get -u https://github.com/owner/repo

# Symlink auto-works again
```

---

## Integration with Other Skills

### /project → /learn

```bash
# 1. Clone and link
/project learn https://github.com/cool/project

# 2. Generate documentation
/learn cool-project
```

### /project → /trace

```bash
# Find where project went
/trace cool-project

# Results show:
# - ψ/learn/cool-project (current)
# - git history of interactions
# - Related issues
```

---

## Slug System

### slugs.yaml

```yaml
# ψ/memory/slugs.yaml
next: github.com/vercel/next.js
tw: github.com/tailwindlabs/tailwindcss
myproj: github.com/myorg/my-project
```

### Usage

```bash
# Instead of full URL
/project learn next
/learn tw
```

---

## Anti-Patterns

| ❌ Wrong | ✅ Right |
|----------|----------|
| `git clone` to ψ/ | `ghq get` then symlink |
| Nested: `ψ/learn/repo/github.com/...` | Flat: `ψ/learn/repo-name` |
| Copy files | Symlink always |
| Manual path management | Use ghq |

---

## Graduation Flow

When project moves from incubate to production:

```bash
# 1. Project is ready
/project incubate my-startup

# 2. After months of dev, it graduates
rm ψ/incubate/my-startup  # Remove symlink only

# 3. Log graduation
oracle_learn({
  pattern: "my-startup graduated from incubation",
  concepts: ["graduation", "my-startup"]
})

# 4. Source still exists in ghq
ghq list | grep my-startup  # Still there
```

---

## Best Practices

### 1. Keep ghq updated

```bash
# Update all repos
ghq list | xargs -I {} ghq get -u {}
```

### 2. Clean up periodically

```bash
# Find unused symlinks
/project list

# Remove if no longer needed
rm ψ/learn/old-project
```

### 3. Use slugs for frequent projects

```yaml
# Add to slugs.yaml
claude: github.com/anthropics/claude-code
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "ghq not found" | `brew install ghq` |
| Broken symlink | `ghq get -u <url>` |
| Wrong ghq root | Check `ghq root` output |
| Duplicate entries | Clean up ψ/ symlinks |
