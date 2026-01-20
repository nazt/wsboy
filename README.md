# Oracle Skills สำหรับ Artist

บทความภาษาไทยสำหรับคนที่ไม่ถนัดโปรแกรม อยากใช้ Oracle Skills กับ Claude Code

## เริ่มต้นใช้งาน

```bash
# ติดตั้ง Oracle Skills
bunx --bun oracle-skills@github:Soul-Brews-Studio/oracle-skills-cli#v1.4.0 install -y -g -a claude-code
```

---

## บทความทั้งหมด

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

## Links

- [Oracle Skills CLI](https://github.com/Soul-Brews-Studio/oracle-skills-cli) — ตัวติดตั้ง
- [Soul Brews Studio](https://github.com/Soul-Brews-Studio) — ผู้สร้าง

---

*สร้างโดย Claude Opus 4.5 สำหรับเพื่อน Artist ที่อยากใช้ AI ช่วยงาน*
