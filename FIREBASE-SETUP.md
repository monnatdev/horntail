# ตั้งค่า Firebase สำหรับ Horntail Loot (แผนฟรี Spark)

ใช้เวลาราว 10 นาที ต้องมีบัญชี Google อย่างเดียว ไม่ต้องผูกบัตร
> ชื่อเมนูอาจต่างจากนี้เล็กน้อยถ้า Firebase เปลี่ยนหน้าตา (เขียนเมื่อ 17 พ.ย. 2026)
> console แบบใหม่ (พ.ย. 2026) ไม่มีหมวด "Build" แล้ว: Realtime Database อยู่ใต้ **Databases & Storage** · Authentication อยู่ใต้ **Security** · หรือพิมพ์ชื่อในช่องค้นหาซ้ายบน

แผน Spark (ฟรี) ของ Realtime Database ณ วันที่เขียน: ต่อพร้อมกันได้ 100 คน · เก็บได้ 1 GB · ดาวน์โหลด 10 GB/เดือน
(ที่มา: firebase.google.com/pricing ซึ่งเป็นหน้าของ Google เอง) — ตี้ของเราใช้ไม่ถึง 1% ของโควตา

---

## 1. สร้างโปรเจกต์
1. เข้า https://console.firebase.google.com แล้วล็อกอินด้วยบัญชี Google
2. กด **Create a project** (หรือ Add project)
3. ตั้งชื่อ เช่น `horntail-loot` → Continue
4. หน้า Google Analytics → **ปิด** (ไม่ต้องใช้) → Create project → รอเสร็จแล้วกด Continue

## 2. เปิด Realtime Database
1. เมนูซ้าย **Build → Realtime Database** → **Create Database**
2. Location: เลือก **Singapore (asia-southeast1)** (ใกล้ไทยสุด เร็วสุด)
3. Security rules: เลือก **Start in locked mode** → Enable
4. จดที่อยู่ database ด้านบน (หน้าตาแบบ `https://horntail-loot-default-rtdb.asia-southeast1.firebasedatabase.app`)

## 3. เปิดล็อกอินแบบไม่ระบุตัวตน (Anonymous)
1. เมนูซ้าย **Build → Authentication** → **Get started**
2. แท็บ **Sign-in method** → เลือก **Anonymous** → เปิด **Enable** → Save
> ผู้ใช้ไม่เห็นอะไรเพิ่ม เว็บจะล็อกอินให้อัตโนมัติ มีไว้ให้ Rules กันบอทและสคริปต์ที่ยิงเข้ามาตรงๆ

## 4. ใส่ Rules
1. กลับไป **Realtime Database** → แท็บ **Rules**
2. ลบของเดิมทั้งหมด แล้ววางเนื้อหาจากไฟล์ `database.rules.json` (อยู่ในโฟลเดอร์นี้)
3. กด **Publish**

Rules นี้ทำอะไร:
- อ่าน/เขียนได้เฉพาะคนที่เปิดผ่านเว็บ (ล็อกอินแบบ Anonymous แล้ว)
- ห้ามลบฐานข้อมูลทั้งก้อน
- แต้มและราคาต้องเป็นตัวเลข ≥ 0 · ชื่อยาวไม่เกิน 30 ตัว

⚠️ **ข้อจำกัด:** คนที่เปิดเว็บได้ก็เขียนข้อมูลได้ (ไม่มีระบบ login จริง) Rules กันคนตั้งใจป่วนที่รู้เทคนิคไม่ได้ทั้งหมด → สำรองข้อมูลตามข้อ 7 เป็นระยะ

## 5. เอา config มาใส่เว็บ
1. กดรูปฟันเฟือง ⚙️ ข้าง Project Overview → **Project settings**
2. แท็บ General เลื่อนลงไปที่ **Your apps** → กดไอคอน **</>** (Web)
3. ตั้งชื่อแอป เช่น `horntail-web` → **ไม่ต้อง**ติ๊ก Firebase Hosting → Register app
4. จะเห็นโค้ดแบบนี้ ให้ copy เฉพาะก้อน `firebaseConfig`:
   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "horntail-loot.firebaseapp.com",
     databaseURL: "https://horntail-loot-default-rtdb.asia-southeast1.firebasedatabase.app",
     projectId: "horntail-loot",
     storageBucket: "...",
     messagingSenderId: "...",
     appId: "..."
   };
   ```
   ถ้าไม่มีบรรทัด `databaseURL` ให้เติมเองด้วยที่อยู่จากข้อ 2.4
5. ส่งก้อนนี้ให้ผม หรือแก้ใน `index.html` เอง: หาบรรทัด `const FIREBASE_CONFIG = null;` แล้วเปลี่ยน `null` เป็นก้อน `{ ... }` ข้างบน

> config นี้ **ไม่ใช่ความลับ** ใครเปิดเว็บก็เห็นอยู่แล้ว สิ่งที่ป้องกันข้อมูลคือ Rules ในข้อ 4

## 6. ใช้งานครั้งแรก
1. เปิดเว็บ → จะเจอหน้า **"ตั้งค่าครั้งแรก"** (เพราะฐานข้อมูลยังว่าง)
2. วาง JSON รายชื่อ (รูปแบบอยู่บนหน้านั้น) → ตรวจตาราง → กด **นำเข้า** (ทำได้ครั้งเดียว)
3. แต่ละคนเลือกชื่อตัวเอง แล้วตั้งรหัส PIN 4 ตัวเองตอนเข้าครั้งแรก

## 6.1 แก้ข้อมูลเอง (ไม่มีหน้าดูแลในเว็บ)
เข้า Realtime Database → แท็บ **Data** แล้วคลิกไล่ลงไปตามชื่อ:

| อยากทำ | ไปที่ | ทำยังไง |
|---|---|---|
| แก้แต้มใครสักคน | `members` → รหัสคนนั้น (เช่น `m3`) → `points` | คลิกตัวเลข แก้ แล้วกด Enter |
| คนลืมรหัส PIN | `members` → รหัสคนนั้น → `pinHash` | ชี้แล้วกดถังขยะ 🗑 ลบทิ้ง → เจ้าตัวเข้าเว็บแล้วตั้งรหัสใหม่ได้ |
| แก้ราคาของ | `prices` → รหัสของ (เช่น `1113149` = Silver Blossom Ring) | คลิกตัวเลข แก้ แล้วกด Enter |
| ลบคนออก | `members` → รหัสคนนั้น | กดถังขยะ (ประวัติเก่ายังอยู่ แต่ชื่อจะขึ้นว่า "ไม่มีชื่อนี้แล้ว") |
| ลบของออกจากคลัง | `vault` → รหัสของชิ้นนั้น | กดถังขยะ |

> หารหัสคนได้จากช่อง `name` ที่อยู่ข้างใน · หารหัสของได้จากหน้า "ราคาของ" หรือไฟล์ `index.html` (ส่วน DROPS)
> ⚠️ แก้ผิดแล้วย้อนไม่ได้ → Export JSON สำรอง (ข้อ 7) ก่อนแก้ทุกครั้ง

## 7. สำรองข้อมูล (แนะนำทำทุกสัปดาห์)
Realtime Database → แท็บ Data → เมนู ⋮ มุมขวา → **Export JSON** → เก็บไฟล์ไว้
กู้คืน: เมนูเดียวกัน → **Import JSON** (ทับข้อมูลทั้งหมด)

---

## ถ้ามีปัญหา
| อาการบนเว็บ | แก้ |
|---|---|
| "เชื่อมต่อฐานข้อมูลไม่ได้ (auth/operation-not-allowed)" | ยังไม่ได้เปิด Anonymous (ข้อ 3) |
| "เชื่อมต่อฐานข้อมูลไม่ได้ (auth/admin-restricted-operation)" | Anonymous ยังไม่ได้ Enable+Save (ข้อ 3) หรือ Authentication → Settings → User actions → ช่อง **Enable create (sign-up)** ถูกปิดอยู่ ให้ติ๊กเปิดแล้ว Save |
| "อ่านข้อมูลไม่ได้ (PERMISSION_DENIED)" | Rules ยังไม่ได้ Publish หรือวางผิด (ข้อ 4) |
| "… (auth/unauthorized-domain)" | Authentication → Settings → Authorized domains → Add domain → ใส่ `ชื่อบัญชี.github.io` |
| ขึ้นแถบแดง "ออฟไลน์" | เน็ตหลุด เว็บจะต่อใหม่เอง |
