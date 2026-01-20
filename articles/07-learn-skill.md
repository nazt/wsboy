# /learn — ศึกษาโปรเจคใหม่

## มันคืออะไร?

`/learn` ช่วยให้ Claude **ศึกษาโค้ดโปรเจค** แล้วสรุปให้เข้าใจง่าย

เหมือนมี**ผู้ช่วย 3 คน**อ่านโค้ดพร้อมกัน แล้วมาเล่าให้ฟัง

---

## ใช้ยังไง?

### ศึกษาจาก GitHub
```
/learn https://github.com/someone/cool-project
```

### ศึกษาโปรเจคที่มีอยู่แล้ว
```
/learn my-project-name
```

---

## ได้อะไรออกมา?

Claude จะสร้าง 3 ไฟล์:

| ไฟล์ | เนื้อหา |
|------|---------|
| `ARCHITECTURE.md` | โครงสร้างโปรเจค |
| `CODE-SNIPPETS.md` | ตัวอย่างโค้ดสำคัญ |
| `QUICK-REFERENCE.md` | วิธีใช้งานเบื้องต้น |

---

## สำหรับ Artist ใช้ทำอะไรได้?

### 1. ศึกษา tools ที่ใช้
```
/learn https://github.com/excalidraw/excalidraw
```
→ เข้าใจว่า Excalidraw ทำงานยังไง

### 2. ดู plugin ที่สนใจ
```
/learn https://github.com/someone/procreate-brushes
```
→ ดูว่า brush pack นี้มีอะไรบ้าง

### 3. เรียนรู้ automation
```
/learn https://github.com/someone/art-workflow
```
→ ดูว่าคนอื่นจัด workflow ยังไง

---

## ต่างจาก /watch ยังไง?

| /watch | /learn |
|--------|--------|
| เรียนจาก **YouTube** | เรียนจาก **GitHub** |
| วิดีโอ | โค้ด |
| ได้สรุป | ได้ documentation |

---

## ข้อจำกัด

- เหมาะกับ**โปรเจคโค้ด**มากกว่า
- ถ้าไม่เคยเขียนโค้ด อาจอ่านยาก
- แต่ถามให้ Claude อธิบายเพิ่มได้!

---

## Tips สำหรับ non-dev

หลัง `/learn` เสร็จ ลองถาม:

```
อธิบายโปรเจคนี้แบบง่ายๆ ให้คนไม่เขียนโค้ดเข้าใจหน่อย
```

Claude จะสรุปให้ใหม่แบบเข้าใจง่าย

---

*บทความสำหรับเพื่อน Artist ที่อยากรู้จัก tools มากขึ้น*
