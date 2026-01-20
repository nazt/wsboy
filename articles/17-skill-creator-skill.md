# /skill-creator — สร้าง Skill ใหม่

## มันคืออะไร?

`/skill-creator` ช่วยสร้าง**skill ใหม่**สำหรับ Claude

เหมือนสอน Claude ให้ทำอะไรใหม่ๆ ได้

---

## ใช้ยังไง?

### สร้าง skill ใหม่
```
/skill-creator my-new-skill
```

### แบบเร็ว (ใช้ค่าเริ่มต้น)
```
/skill-creator my-skill --quick
```

### ดูไอเดีย skill
```
/skill-creator list-ideas
```

---

## Claude จะถามอะไร?

| คำถาม | ตัวอย่างตอบ |
|-------|-------------|
| ชื่อ skill | `daily-art` |
| ทำอะไร | สุ่ม prompt วาดรูปให้ทุกวัน |
| ใช้ตอนไหน | พิมพ์ "daily art", "วาดอะไรดี" |
| รับ input อะไร | theme ที่อยากวาด |
| แสดงผลอะไร | prompt วาดรูป |

---

## ได้อะไรออกมา?

Claude สร้างไฟล์ให้:

```
skills/daily-art/
├── SKILL.md          ← คำอธิบาย skill
└── scripts/
    └── main.ts       ← โค้ดที่ทำงาน
```

---

## สำหรับ Artist

นี่คือ skill ที่ให้คุณ**สอน Claude ให้ทำอะไรใหม่**!

### ไอเดีย skill สำหรับ Artist

| ชื่อ | ทำอะไร |
|------|--------|
| `daily-prompt` | สุ่ม prompt วาดรูปทุกวัน |
| `color-palette` | สร้าง palette สีให้ |
| `art-timer` | จับเวลาวาดรูป |
| `reference-finder` | หา reference ให้ |

---

## ตัวอย่าง: สร้าง daily-prompt

```
/skill-creator daily-prompt

Claude: ชื่อ skill?
You: daily-prompt

Claude: ทำอะไร?
You: สุ่ม prompt วาดรูปให้ทุกวัน

Claude: ใช้ตอนไหน?
You: พิมพ์ "daily prompt", "วาดอะไรดี", "prompt"

Claude: รับ input?
You: theme เช่น "fantasy", "sci-fi"

Claude: แสดงผล?
You: prompt + คำแนะนำสั้นๆ
```

หลังจากนั้นคุณก็ใช้ `/daily-prompt` ได้เลย!

---

## ข้อจำกัด

- ต้องเข้าใจพื้นฐานว่า skill ทำงานยังไง
- บาง skill อาจต้องเขียนโค้ดเพิ่ม

---

## Tips

1. เริ่มจาก skill ง่ายๆ ก่อน
2. ดู skill ที่มีอยู่เป็นตัวอย่าง
3. ถาม Claude ช่วยได้เสมอ

---

*บทความสำหรับเพื่อน Artist ที่อยากปรับแต่ง Claude ให้เหมาะกับตัวเอง*
