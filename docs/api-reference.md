# 🌐 API Reference (v8.1)

คู่มือการส่งข้อมูลเพื่อ Encode และ Decode ผ่าน GhostPass API

## 🏠 Endpoint พื้นฐาน
เมือคุณ Deploy บน Vercel หรือ Server อื่นๆ URL พื้นฐานจะเป็น:
`https://your-domain.com/api`

---

## 🔒 การตั้งค่าความปลอดภัย (Environment Variable)
ก่อนใช้งาน API คุณต้องมั่นใจว่าเซิร์ฟเวอร์มีการกำหนดรหัสลับใน Environment Variable แล้ว:
- **Key:** `GHOSTPASS_MASTER_SECRET`
- **Value:** รหัสลับของคุณ (เช่น `my_private_salt_2026`)

---

## 🔑 1. การสร้างกุญแจ (Encode API)
ใช้เพื่อรับข้อมูล Licensing แล้วแปลงเป็น 12 คำศัพท์

- **Method:** `POST`
- **URL:** `/api/encode`
- **Body (JSON):**
```json
{
  "role": 1,
  "type": 0,
  "name": 500,
  "version": 1,
  "model": 88,
  "prodDate": 260102,
  "actDate": 0,
  "expiryDate": 2047,
  "sku": 777
}
```

- **Response ตัวอย่าง:**
```json
{
  "success": true,
  "phrase": ["abandon", "ability", "able", ...],
  "words": "abandon ability able ..."
}
```

---

## 🔍 2. การตรวจสอบกุญแจ (Decode API)
ใช้เพื่อรับ 12 คำศัพท์ แล้วถอดรหัสออกมาเป็นข้อมูล พร้อมเช็คความถูกต้อง (รปภ)

- **Method:** `POST`
- **URL:** `/api/decode`
- **Body (JSON):**
```json
{
  "phrase": "abandon ability able ..."
}
```
*(หรือจะส่งเป็น Array ของคำศัพท์ก็ได้)*

- **Response ตัวอย่าง:**
```json
{
  "success": true,
  "valid": true,
  "data": {
    "role": 1,
    "type": 0,
    "name": 500,
    "version": 1,
    ...
  }
}
```

---

## 💻 ตัวอย่างการเรียกใช้ (JavaScript Fetch)
```javascript
const response = await fetch('https://your-api.com/api/decode', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ phrase: "word1 word2 ... word12" })
});

const result = await response.json();
if (result.valid) {
  console.log("เข้าใช้งานได้! บทบาทคือ:", result.data.role);
} else {
  console.log("กุญแจไม่ถูกต้อง!");
}
```
