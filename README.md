# Horntail Loot

เว็บจองและแบ่งของบอส Horntail ของกลุ่ม (MapleStory Old School, GMS v83)

- ลงบอสได้ +1 แต้มทุกคนในตี้ · จองได้สูงสุด 3 ชิ้น ราคารวมไม่เกินแต้ม
- คนแต้มมากสุดได้ก่อน (เท่ากันสุ่ม) · หัวตี้กด "ปิดจอง · ลงบอส" ก่อนตีบอส
- ของที่ไม่มีใครเอาเข้าคลัง ขอแลกได้ตลอด ส่งตอนมีตี้กำลังเล่น

## โครงสร้าง
| ไฟล์ | หน้าที่ |
|---|---|
| `index.html` | ทั้งเว็บ (HTML/CSS/JS ไฟล์เดียว ไม่ต้อง build) |
| `database.rules.json` | Rules ของ Firebase Realtime Database |
| `FIREBASE-SETUP.md` | วิธีตั้งค่า Firebase · แก้ข้อมูลใน console · สำรองข้อมูล |

## Deploy
GitHub Pages จาก branch `main` โฟลเดอร์ root · แก้ `index.html` แล้ว push ขึ้น `main` รอ 1–2 นาที

## รันในเครื่อง
```bash
python3 -m http.server 8766
```
แล้วเปิด http://localhost:8766 (ใช้ Firebase ตัวจริง ถ้าอยากลองแบบไม่แตะข้อมูลจริง ให้ตั้ง `FIREBASE_CONFIG = null` ในสำเนาไฟล์)
