# 🪐 คอร์สแวร์: ระบบสุริยะและสื่อเทคโนโลยีความจริงเสริม (CAI: AR & Solar System)

ระบบบทเรียนคอมพิวเตอร์ช่วยสอน (Courseware) สำหรับนักเรียนชั้นมัธยมศึกษาปีที่ 5 รายวิชาระบบสุริยะ (เนื้อหาเน้นเทคโนโลยีความจริงเสริม - AR) พัฒนาในรูปแบบ Single Page Application (SPA) ด้วย HTML/JS บริสุทธิ์ และใช้ [Supabase](https://supabase.com) เป็นระบบหลังบ้านแบบ Serverless

## ✨ ฟีเจอร์หลัก (Features)

### 👨‍🎓 สำหรับผู้เรียน (Students)
* **ระบบบัญชีผู้ใช้:** ล็อกอินด้วยรหัสนักศึกษา (11 หลัก) เฉพาะผู้ที่มีรายชื่อในระบบเท่านั้น
* **แบบทดสอบก่อนเรียน/หลังเรียน:** ข้อสอบคู่ขนานชุด A และ B (สลับตามเลขท้ายรหัสนักศึกษา)
* **บทเรียน 5 หน่วย:** พร้อมคำถามแทรก (Mini Quiz) ที่ต้องตอบให้ถูกก่อนผ่านไปบทถัดไป
* **WebAR Practice:** รองรับการแสดงผลโมเดล 3 มิติ ผ่าน `<model-viewer>` และโหมด AR บนมือถือ
* **ส่งงานปฏิบัติ:** อัปโหลดไฟล์ภาพ/วิดีโอ (ไม่เกิน 20MB) หรือส่งเป็นลิงก์ พร้อมดูคะแนนและข้อเสนอแนะจากผู้สอน
* **ศูนย์รวมสื่อการเรียน:** ดาวน์โหลดหรือดูสื่อเพิ่มเติมที่ผู้สอนอัปโหลดไว้
* **แดชบอร์ดส่วนตัว:** แสดงความก้าวหน้า และคะแนน Normalized Gain

### 👩‍🏫 สำหรับผู้สอน (Teachers)
* **แผงควบคุม (Dashboard):** ดูภาพรวมคะแนนและความก้าวหน้าของผู้เรียนทั้งหมด
* **ระบบตรวจงาน:** ตรวจงานปฏิบัติ (Unit 4) ให้คะแนน และพิมพ์ข้อเสนอแนะ
* **จัดการสื่อ (Materials):** อัปโหลดไฟล์ (PDF, DOCX, PPTX ไม่เกิน 50MB) หรือแนบลิงก์สื่อภายนอก
* **ส่งออกข้อมูล (Export):** ดาวน์โหลดผลการเรียนทั้งหมดเป็นไฟล์ CSV (รองรับภาษาไทยสำหรับ Excel)

## 🛠️ เทคโนโลยีที่ใช้ (Tech Stack)
* **Frontend:** HTML5, CSS3, Vanilla JavaScript, `<model-viewer>`
* **Backend (BaaS):** Supabase (PostgreSQL, Auth, Storage)
* **Security:** Row Level Security (RLS), Database Triggers, Role-based Access

## 🚀 ขั้นตอนการติดตั้งและการตั้งค่า (Setup Guide)

### 1. ตั้งค่า Supabase (Backend)
1. สมัครสมาชิกและสร้างโปรเจกต์ใหม่ที่ [Supabase](https://supabase.com)
2. ไปที่เมนู **Authentication > Providers > Email** และ **ปิดสวิตช์** `Confirm email`
3. ไปที่เมนู **SQL Editor** สร้าง Query ใหม่ แล้วคัดลอกโค้ดจากไฟล์ `setup.sql` ไปวางและกด **Run** เพื่อสร้างตาราง นโยบาย RLS และ Storage Buckets
4. ไปที่เมนู **Project Settings > API** เพื่อคัดลอก `Project URL` และ `anon public key`

### 2. ตั้งค่า Frontend
1. เปิดไฟล์ `courseware.html` ด้วย Text Editor (เช่น VS Code, Notepad)
2. นำค่าที่คัดลอกมาไปใส่ในตัวแปรส่วนต้นของไฟล์:
   ```javascript
   const SUPABASE_URL = 'https://xxxx.supabase.co'; 
   const SUPABASE_ANON_KEY = 'eyJh...'; 
   ```
3. บันทึกไฟล์

### 3. การตั้งค่าบัญชีผู้สอน (Teacher Account)
1. ใน Supabase ไปที่เมนู **Authentication > Users** กด **Add User** แล้วสร้างบัญชีด้วยอีเมลของคุณ (เช่น `teacher@school.ac.th`)
2. กลับไปที่ **SQL Editor** นำโค้ดด้านล่างนี้ไปรัน (เปลี่ยนอีเมลให้ตรงกับที่คุณสมัคร):
   ```sql
   INSERT INTO public.teachers (user_id) 
   SELECT id FROM auth.users WHERE email = 'teacher@school.ac.th';
   ```

### 4. การนำเข้ารายชื่อนักศึกษา
ระบบถูกตั้งค่าให้มีนักศึกษาจำลอง 6 คน (รหัส 11111111111 ถึง 66666666666)
หากต้องการใช้รายชื่อจริง:
1. ไปที่เมนู **Table Editor** ใน Supabase
2. เลือกตาราง `allowed_students`
3. กดปุ่ม **Insert > Import data from CSV** เพื่ออัปโหลดไฟล์รหัสนักศึกษาของคุณ

## 💻 การเปิดใช้งาน
ไฟล์ `courseware.html` สามารถรันได้ทันทีบนเบราว์เซอร์โดยดับเบิลคลิกที่ไฟล์ หรือสามารถนำไปโฮสต์บนบริการฟรีต่างๆ เช่น **GitHub Pages**, **Vercel**, หรือ **Netlify** เพื่อให้นักเรียนเข้าถึงผ่านอินเทอร์เน็ตได้