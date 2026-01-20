# /skill-creator — Building Custom Skills

## Skill Anatomy

```
skills/my-skill/
├── skill.md              ← Instructions for Claude
└── scripts/
    └── main.ts           ← Bun Shell logic (optional)
```

---

## skill.md Structure

```markdown
---
name: my-skill
description: One line. Use when user says "trigger1", "trigger2".
---

# /my-skill - Title

What it does in one sentence.

## Usage

\`\`\`
/my-skill [args]
\`\`\`

## Step 0: Timestamp

\`\`\`bash
date "+🕐 %H:%M (%A %d %B %Y)"
\`\`\`

## Step 1: [First Action]

[Instructions for Claude]

## Step 2: [Second Action]

[Instructions for Claude]

## Output

[What to show user]

---

## Oracle Integration

When skill completes:
\`\`\`
oracle_learn({ pattern: "[What was learned]" })
\`\`\`

---

ARGUMENTS: $ARGUMENTS
```

---

## Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Skill identifier |
| `description` | Yes | One-line + trigger words |
| `trigger` | No | Explicit trigger (default: /name) |
| `version` | No | Skill version |

### Description Format

```yaml
description: v1.0.0 (Global) | What it does. Use when user says "x", "y", "z".
```

The `Use when user says` part helps Claude auto-invoke.

---

## Bun Shell Scripts

### Basic Template

```typescript
#!/usr/bin/env bun
import { $ } from "bun"

const args = process.argv.slice(2)
const input = args[0] || ""

// Your logic
const result = await $`echo "Processing: ${input}"`.text()

console.log(JSON.stringify({ input, result }, null, 2))
```

### With Error Handling

```typescript
#!/usr/bin/env bun
import { $ } from "bun"

try {
  const args = process.argv.slice(2)
  const [command, ...rest] = args

  switch (command) {
    case "list":
      const files = await $`ls -la`.text()
      console.log(files)
      break

    case "search":
      const query = rest.join(" ")
      const results = await $`grep -ril "${query}" .`.text()
      console.log(results)
      break

    default:
      console.log("Usage: main.ts [list|search] [args]")
  }
} catch (error) {
  console.error("Error:", error.message)
  process.exit(1)
}
```

### With Fetch

```typescript
#!/usr/bin/env bun

const API_URL = "https://api.example.com"

async function fetchData(query: string) {
  const response = await fetch(`${API_URL}/search?q=${query}`)
  return response.json()
}

const args = process.argv.slice(2)
const data = await fetchData(args[0])
console.log(JSON.stringify(data, null, 2))
```

---

## Oracle Philosophy Integration

Every skill should consider:

### 1. Nothing is Deleted

```typescript
// Log meaningful actions
oracle_trace({
  query: "skill action",
  foundFiles: [...]
})

// Preserve history
oracle_learn({
  pattern: "User did X which resulted in Y"
})
```

### 2. Patterns Over Intentions

```markdown
## Verification

After completing:
1. Check output exists
2. Verify format correct
3. Report success/failure
```

### 3. External Brain, Not Command

```markdown
## Options

Present choices to user:
1. Option A (recommended)
2. Option B
3. Option C

Let user decide.
```

---

## Creation Workflow

### Interactive Mode

```bash
/skill-creator my-skill
```

Claude asks:
1. **Name**: Folder name, command name
2. **Purpose**: What does it do?
3. **Triggers**: When to invoke?
4. **Inputs**: What arguments?
5. **Outputs**: What to show?

### Quick Mode

```bash
/skill-creator my-skill --quick
```

Uses defaults:
- Triggers: "my-skill", "run my-skill"
- Output: JSON
- Oracle: Yes (trace + learn)

---

## Testing Skills

### Test Script Directly

```bash
bun skills/my-skill/scripts/main.ts "test input"
```

### Test via Claude

```
/my-skill test input
```

### Debug Mode

```typescript
// Add to script
const DEBUG = process.env.DEBUG === "1"

if (DEBUG) {
  console.log("Args:", args)
  console.log("Config:", config)
}
```

```bash
DEBUG=1 bun skills/my-skill/scripts/main.ts
```

---

## Registration

### Local (Project)

Place in `.claude/skills/my-skill/`

### Global (User)

Place in `~/.claude/skills/my-skill/`

### Marketplace

Add to `marketplace.json`:

```json
{
  "skills": [
    "./skills/my-skill"
  ]
}
```

---

## Examples

### Example 1: Daily Prompt Generator

```markdown
---
name: daily-prompt
description: Generate art prompts. Use when user says "prompt", "art idea".
---

# /daily-prompt

Generate creative prompts for artists.

## Usage

\`\`\`
/daily-prompt [theme]
\`\`\`

## Step 1: Generate

Based on theme (or random if none):
- Style: [realistic, abstract, minimalist, ...]
- Subject: [nature, portrait, urban, ...]
- Mood: [calm, energetic, mysterious, ...]
- Technique: [watercolor, digital, sketch, ...]

## Output

\`\`\`
🎨 Daily Prompt

Subject: [subject]
Style: [style]
Mood: [mood]
Challenge: [specific challenge]

💡 Tip: [one helpful tip]
\`\`\`

ARGUMENTS: $ARGUMENTS
```

### Example 2: Git Stats

```markdown
---
name: git-stats
description: Show repo statistics. Use when user says "stats", "repo info".
---

# /git-stats

Quick repository statistics.

## Step 1: Gather Data

\`\`\`bash
echo "Commits: $(git rev-list --count HEAD)"
echo "Contributors: $(git shortlog -sn | wc -l)"
echo "Files: $(git ls-files | wc -l)"
echo "First commit: $(git log --reverse --format=%ci | head -1)"
echo "Latest commit: $(git log -1 --format=%ci)"
\`\`\`

## Output

| Metric | Value |
|--------|-------|
| Total commits | X |
| Contributors | X |
| Files tracked | X |
| Age | X days |

ARGUMENTS: $ARGUMENTS
```

### Example 3: With Script

**skill.md**:
```markdown
---
name: color-palette
description: Generate color palettes. Use when "colors", "palette".
---

# /color-palette

Generate harmonious color palettes.

## Usage

\`\`\`
/color-palette [base-color]
\`\`\`

## Step 1: Run Script

\`\`\`bash
bun .claude/skills/color-palette/scripts/generate.ts $ARGUMENTS
\`\`\`

## Step 2: Display Results

Show the generated palette with hex codes and preview.

ARGUMENTS: $ARGUMENTS
```

**scripts/generate.ts**:
```typescript
#!/usr/bin/env bun

function hexToHSL(hex: string) {
  // Convert hex to HSL
  // ... implementation
}

function generatePalette(base: string) {
  const hsl = hexToHSL(base)
  return {
    primary: base,
    secondary: rotate(hsl, 30),
    accent: rotate(hsl, 180),
    light: lighten(hsl, 40),
    dark: darken(hsl, 40)
  }
}

const base = process.argv[2] || "#6366f1"
const palette = generatePalette(base)
console.log(JSON.stringify(palette, null, 2))
```

---

## Checklist

Before publishing:

- [ ] Clear purpose (one sentence)
- [ ] Good triggers (3+ natural phrases)
- [ ] Oracle integration (trace/learn)
- [ ] Cross-platform (Bun Shell)
- [ ] Testable (can run directly)
- [ ] Error handling
- [ ] Documentation complete

---

## Philosophy

> "Skills extend Claude's capabilities, not replace human judgment"

Skills should:
- **Assist**, not decide
- **Present options**, not force choices
- **Log meaningfully**, not spam
- **Fail gracefully**, not crash
