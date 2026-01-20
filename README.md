# Oracle Skills Documentation

บทความภาษาไทยสำหรับ **Artist** และ **Developer** ที่อยากใช้ Oracle Skills กับ Claude Code

> 📚 **Artist?** → เลื่อนไปดู [บทความสำหรับ Artist](#บทความสำหรับ-artist)
>
> 💻 **Developer?** → เลื่อนไปดู [Developer Deep Dive](#developer-deep-dive)

## เริ่มต้นใช้งาน

```bash
# ติดตั้ง Oracle Skills
bunx --bun oracle-skills@github:Soul-Brews-Studio/oracle-skills-cli#v1.4.0 install -y -g -a claude-code
```

---

## บทความสำหรับ Artist

### แนะนำ
| # | บทความ | เนื้อหา |
|---|--------|---------|
| 01 | [Oracle Skills คืออะไร?](articles/01-oracle-skills-intro.md) | ภาพรวมทั้งหมด |
| 06 | [Workflow ประจำวัน](articles/06-daily-workflow.md) | แนะนำ routine สำหรับ Artist |

### บันทึก & จดจำ
| # | บทความ | Skill | ใช้ทำอะไร |
|---|--------|-------|----------|
| 02 | [บันทึกอารมณ์](articles/02-feel-skill.md) | `/feel` | จดว่ารู้สึกยังไง |
| 03 | [จดโน้ตเร็ว](articles/03-fyi-skill.md) | `/fyi` | จดข้อมูลไว้ดูทีหลัง |

### เริ่ม & จบ Session
| # | บทความ | Skill | ใช้ทำอะไร |
|---|--------|-------|----------|
| 08 | [เริ่ม Session](articles/08-recap-skill.md) | `/recap` | สรุปว่าค้างอะไรไว้ |
| 09 | [ส่งต่อ Session](articles/09-forward-skill.md) | `/forward` | จดว่าต้องทำอะไรต่อ |
| 04 | [สรุปงาน](articles/04-rrr-skill.md) | `/rrr` | Retrospective + บทเรียน |

### ระหว่างวัน
| # | บทความ | Skill | ใช้ทำอะไร |
|---|--------|-------|----------|
| 10 | [เช็คตอนเช้า](articles/10-standup-skill.md) | `/standup` | ดูนัด + งานค้าง |
| 11 | [ดูปฏิทิน](articles/11-schedule-skill.md) | `/schedule` | ดูนัดหมาย |
| 12 | [ตอนนี้ทำอะไร](articles/12-where-we-are-skill.md) | `/where-we-are` | เช็คว่าอยู่ตรงไหน |

### ค้นหา
| # | บทความ | Skill | ใช้ทำอะไร |
|---|--------|-------|----------|
| 13 | [ค้นหาอดีต](articles/13-trace-skill.md) | `/trace` | หาสิ่งที่เคยทำ |
| 16 | [ค้นหาลึก](articles/16-context-finder-skill.md) | `/context-finder` | ค้นหาละเอียด |

### เรียนรู้
| # | บทความ | Skill | ใช้ทำอะไร |
|---|--------|-------|----------|
| 05 | [เรียนจาก YouTube](articles/05-watch-skill.md) | `/watch` | สรุปวิดีโอ |
| 07 | [ศึกษาโค้ด](articles/07-learn-skill.md) | `/learn` | เข้าใจ project |

### สำหรับ Developer
| # | บทความ | Skill | ใช้ทำอะไร |
|---|--------|-------|----------|
| 14 | [จัดการ Repo](articles/14-project-skill.md) | `/project` | โหลด GitHub repo |
| 15 | [ตำแหน่ง](articles/15-physical-skill.md) | `/physical` | ดูว่าอยู่ที่ไหน |
| 17 | [สร้าง Skill](articles/17-skill-creator-skill.md) | `/skill-creator` | สร้าง skill ใหม่ |

---

## แนะนำสำหรับ Artist

ไม่ต้องใช้ทุกอัน! เริ่มจาก 3 ตัวนี้:

| Skill | ทำอะไร | ตัวอย่าง |
|-------|--------|----------|
| `/feel` | บันทึกอารมณ์ | `/feel inspired` |
| `/fyi` | จดโน้ต | `/fyi ลูกค้าชอบสีน้ำเงิน` |
| `/rrr` | สรุปวัน | `/rrr` |

---

## Flow ประจำวัน

```
เช้า     /standup     → ดูว่าวันนี้มีอะไร
          ↓
ระหว่างวัน /feel, /fyi  → จดอารมณ์ + โน้ต
          ↓
เย็น     /rrr         → สรุปงาน
         /forward     → จดว่าพรุ่งนี้ทำอะไร
```

---

## Developer Deep Dive

Technical documentation for developers who want to understand the internals.

### Architecture & Internals

| # | Article | Skill | Topics |
|---|---------|-------|--------|
| 01 | [Learn Deep Dive](dev-articles/01-learn-deep-dive.md) | `/learn` | 3 parallel agents, output structure, cost analysis |
| 02 | [Project Architecture](dev-articles/02-project-architecture.md) | `/project` | ghq workflow, symlinks, learn vs incubate |
| 03 | [Trace & Oracle](dev-articles/03-trace-oracle-integration.md) | `/trace` | Oracle MCP, --deep mode, distillation |
| 04 | [Context-Finder Agents](dev-articles/04-context-finder-agents.md) | `/context-finder` | 5 parallel agents, search strategies |
| 05 | [Skill Creator Guide](dev-articles/05-skill-creator-guide.md) | `/skill-creator` | Bun Shell, skill anatomy, Oracle philosophy |

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Parallel Agents** | Multiple Haiku agents run simultaneously for speed |
| **Oracle MCP** | Knowledge base with `oracle_search`, `oracle_learn`, `oracle_trace` |
| **ghq Integration** | All repos managed via ghq, symlinked to ψ/ |
| **Bun Shell** | Cross-platform scripting with `$` template literals |

### Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                   Claude Code                        │
│                                                      │
│  Skills (/.claude/skills/)                          │
│  ├── skill.md      → Instructions                   │
│  └── scripts/*.ts  → Bun Shell logic                │
│                                                      │
└──────────────────────┬──────────────────────────────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
   ┌─────────┐   ┌─────────┐   ┌─────────┐
   │ Oracle  │   │  ghq    │   │ GitHub  │
   │  MCP    │   │ (repos) │   │   API   │
   └─────────┘   └─────────┘   └─────────┘
        │              │              │
        ▼              ▼              ▼
   ┌─────────────────────────────────────────┐
   │              ψ/ Directory                │
   │  ├── memory/     (Oracle knowledge)     │
   │  ├── learn/      (study repos)          │
   │  ├── incubate/   (dev repos)            │
   │  └── inbox/      (handoffs, schedules)  │
   └─────────────────────────────────────────┘
```

### Quick Reference

```bash
# Explore codebase (3 agents)
/learn https://github.com/owner/repo

# Deep search (5 agents)
/trace query --deep

# Log to Oracle
oracle_learn({ pattern: "...", concepts: [...] })

# Create custom skill
/skill-creator my-skill
```

---

## Links

- [Oracle Skills CLI](https://github.com/Soul-Brews-Studio/oracle-skills-cli) — Installer
- [Soul Brews Studio](https://github.com/Soul-Brews-Studio) — Creator
- [Agent Skills Spec](https://agentskills.io) — Cross-agent skill format

---

*Created by Claude Opus 4.5 for Artists and Developers*
