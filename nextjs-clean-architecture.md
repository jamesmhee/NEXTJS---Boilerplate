# 📌 อธิบายแต่ละส่วนของโครงสร้าง Next.js + Clean Architecture

โครงสร้างนี้ออกแบบมาให้ **แยกเลเยอร์ชัดเจน** ตามหลัก **Clean Architecture** โดยแบ่งเป็น 4 เลเยอร์หลัก:

1. **Presentation Layer (UI)**
2. **Application Layer (Business Logic / Use Cases)**
3. **Domain Layer (Core Business / Entities & Repositories)**
4. **Infrastructure Layer (Data Sources & External Services)**

---

## 📂 0. App Router Routing

**📌 ทำหน้าที่:** จัดการ **Route สำหรับในการทำหน้าโดยใช้ folder structure** ตามหลัก app router nextjs

| 📂 โฟลเดอร์          | 📌 คำอธิบาย                                                                                                   |
| -------------------- | ------------------------------------------------------------------------------------------------------------- |
| `[locale]/`          | **เป็น slug สำหรับทำ i18next โดย locale คือตัวย่อของภาษา** เช่น TH, EN                                        |
| `[locale]/*pagename` | **สามารถเพิ่มหน้าได้ในโฟล์เดอร์ภายใต้ locale** เช่น อยากเพิ่มหน้า dashboard : app/[locale]/dashboard/page.tsx |

##### \* pagename = ชื่อหน้า

✅ **เป็นการประกาศ Route** → โดยจะมีหน้าที่แค่เป็น Route โดยจะเรียก Component ในส่วนของ Presentation Layer ต่อ

---

## 📂 1. Presentation Layer (UI Layer)

**📌 ทำหน้าที่:** จัดการ **UI และการโต้ตอบกับผู้ใช้** โดยไม่ยุ่งเกี่ยวกับ Business Logic

| 📂 โฟลเดอร์   | 📌 คำอธิบาย                                                                               |
| ------------- | ----------------------------------------------------------------------------------------- |
| `components/` | **Reusable UI Components** เช่น `Button.tsx`, `Card.tsx`, `Modal.tsx`                     |
| `hooks/`      | **Custom Hooks สำหรับ UI** เช่น `useAuth.ts`, `useUser.ts`                                |
| `layouts/`    | **Layout Components** เช่น `MainLayout.tsx` ใช้กำหนดโครงร่างของแต่ละหน้า                  |
| `styles/`     | **CSS & Styling** ไฟล์ที่ใช้จัดการ Global และ Local Styles เช่น Tailwind CSS หรือ Emotion |

✅ **แยก UI ออกจาก Business Logic** → Component ทำหน้าที่แค่ **Render UI** และใช้ Hooks เชื่อมไปที่ Application Layer

---

## 📂 2. Application Layer (Business Logic / Use Cases)

**📌 ทำหน้าที่:** จัดการ **Business Logic / Use Cases**

| 📂 โฟลเดอร์   | 📌 คำอธิบาย                                                                                |
| ------------- | ------------------------------------------------------------------------------------------ |
| `services/`   | **Service Layer** ใช้เรียก API หรือประมวลผลข้อมูล เช่น `AuthService.ts`, `UserService.ts`  |
| `state/`      | **State Management (Zustand / Redux Toolkit)** ใช้จัดการ State เช่น `authStore.ts`         |
| `validators/` | **Schema Validation (Yup)** ใช้ตรวจสอบข้อมูลที่รับเข้ามาก่อนใช้งาน เช่น `authValidator.ts` |

✅ **Application Layer เชื่อมระหว่าง UI กับ Domain**  
✅ **แยก Business Logic ออกจาก Infrastructure**  
✅ **รองรับการ Test ได้ง่าย**

---

## 📂 3. Domain Layer (Core Domain)

**📌 ทำหน้าที่:** เป็นหัวใจของแอป **กำหนดโครงสร้างข้อมูลหลัก** และ **Repositories**

| 📂 โฟลเดอร์     | 📌 คำอธิบาย                                                                                |
| --------------- | ------------------------------------------------------------------------------------------ |
| `models/`       | **Data Models / Interfaces** ใช้กำหนดโครงสร้างข้อมูล เช่น `User.ts`                        |
| `repositories/` | **Repository Interfaces** เป็นตัวกลางเชื่อม API และ Local Storage เช่น `UserRepository.ts` |

✅ **แยก Core Domain ออกจาก Infrastructure**  
✅ **ถ้าเปลี่ยน Data Source (REST → GraphQL) สามารถแก้แค่ Infrastructure Layer ได้เลย**

---

## 📂 4. Infrastructure Layer (Data Sources & External Services)

**📌 ทำหน้าที่:** เชื่อมต่อกับแหล่งข้อมูลภายนอก เช่น API และ Local Storage

| 📂 โฟลเดอร์ | 📌 คำอธิบาย                                                                              |
| ----------- | ---------------------------------------------------------------------------------------- |
| `api/`      | **API Clients (Axios / Fetch API)** เช่น `httpClient.ts`, `UserApi.ts`                   |
| `storage/`  | **Storage Handling** เช่น Local Storage, Cookies (`localStorage.ts`, `cookieService.ts`) |

✅ **Infrastructure ควรเป็น Plug & Play** → สามารถเปลี่ยน API หรือ Storage ได้ง่าย

---

## 📂 5. Config & Utilities

**📌 ทำหน้าที่:** เก็บค่าตั้งค่าต่าง ๆ ของแอป และฟังก์ชันที่ใช้บ่อย ๆ

| 📂 โฟลเดอร์ | 📌 คำอธิบาย                                                         |
| ----------- | ------------------------------------------------------------------- |
| `config/`   | **Environment & Settings** เช่น `env.ts` (API URL, Token Keys)      |
| `utils/`    | **Utilities ฟังก์ชันที่ใช้ซ้ำบ่อย ๆ** เช่น `logger.ts` (Log System) |
| `assets/`   | **Static Assets** เช่น Icons, Images                                |

---

## 📂 6. Public Directory

**📌 ทำหน้าที่:** เก็บ Static Files ที่สามารถเข้าถึงได้ผ่าน URL โดยตรง เช่น `/favicon.ico`

| 📂 โฟลเดอร์ | 📌 คำอธิบาย                                                                         |
| ----------- | ----------------------------------------------------------------------------------- |
| `public/`   | **Static Files ที่ Next.js Serve ให้** (รูปภาพ, Icons, Fonts, ข้อความสำหรับทำ i18n) |

✅ **เหมาะสำหรับไฟล์ที่ไม่ต้องผ่านการประมวลผล**

---

## 📂 7. Config Files & Dependencies

**📌 ทำหน้าที่:** ตั้งค่าการทำงานของโปรเจกต์

| 📜 ไฟล์          | 📌 คำอธิบาย                                                           |
| ---------------- | --------------------------------------------------------------------- |
| `next.config.js` | **ตั้งค่า Next.js** เช่น Custom Routing                               |
| `tsconfig.json`  | **TypeScript Config** (Alias & Path Mapping)                          |
| `package.json`   | **Dependencies & Scripts** (`npm install`, `npm run dev`)             |
| `.eslintrc.json` | **ESLint Rules** สำหรับ Linting                                       |
| `.gitignore`     | **Ignore ไฟล์ที่ไม่ต้อง track ใน Git** เช่น `node_modules/`, `.next/` |

---

## 📂 8. i18n (ส่วนเสริมหากต้องการใช้เว็บสองภาษา)

**📌 ทำหน้าที่:** สำหรับเป็น Folder ที่ไว้ใช้สำหรับ config สำหรับ library i18n

| 📜 ไฟล์      | 📌 คำอธิบาย                                   |
| ------------ | --------------------------------------------- |
| `request.ts` | **ใช้สำหรับดึงค่า locale config ของ library** |
| `routing.ts` | **Config สำหรับ i18n**                        |

---

## 🎯 สรุป

### ✅ Clean Architecture ชัดเจน

✔ แยก UI (`presentation/`) ออกจาก Business Logic (`application/`)  
✔ Domain (`domain/`) เป็นศูนย์กลางของระบบ ไม่พึ่ง API หรือ UI  
✔ Infrastructure (`infrastructure/`) ทำหน้าที่เชื่อมกับ API หรือ Storage

### ✅ ข้อดีของโครงสร้างนี้

✔ **อ่านง่าย** → แยกโค้ดออกเป็นส่วน ๆ ตามหน้าที่  
✔ **ดูแลง่าย** → เปลี่ยน API หรือ State Management ได้ง่าย  
✔ **Test ได้ง่าย** → แต่ละ Layer สามารถ Test แยกกันได้  
✔ **ขยายต่อได้ง่าย** → รองรับการเพิ่ม Feature ใหม่โดยไม่ต้อง Refactor เยอะ

---

## 🚀 Next Steps

1️⃣ **ติดตั้งโปรเจกต์**

```sh
npm install
npm run dev
```

2️⃣ **เริ่มพัฒนา API & Features เพิ่มเติม**  
3️⃣ **เพิ่มระบบ Auth (JWT, OAuth) หรือ Multi-language ถ้าจำเป็น**

🔥 **พร้อมใช้งาน! ถ้ามีอะไรให้ปรับเพิ่ม บอกได้เลย!** 😃🚀
