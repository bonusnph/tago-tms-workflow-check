# งานที่ต้องทำต่อ — TMS workflow

เช็กลิสต์ให้เอเจนต์หยิบงานต่อจากรอบทดสอบ localhost เลือกข้อที่ยังไม่ติ๊กและไม่มีตัวบล็อกค้างอยู่ก่อน อย่าเริ่มข้อที่ถูกบล็อก และอย่าไปแก้ flow ที่ผ่านแล้ว

หลักฐานอยู่โฟลเดอร์เดียวกับไฟล์นี้ เปิด HTML กับภาพก่อนลงมือแก้โค้ด

## บริบท

- ทดสอบใหม่ทั้งชุดเมื่อ 2026-09-29 โดยรอ spinner และข้อมูลใน dialog นิ่งก่อนถ่ายภาพ
- Backoffice `http://localhost:4200` เรียก API ที่ `http://localhost:3000`
- บริษัท: ทาโก ทรานสปอร์ต (TAGO Transport, รหัส `573426`)
- ผู้ใช้ backoffice: `tago_transport@hotmail.com`
- คนขับ: กิต โชคดี โทร `0879362523` PIN `(ไม่เผยแพร่)`
- เลขงานของรอบนี้: `TMS29154809` ลูกค้า SCGJWD เส้นทาง LCP - TICS ตู้ 20GP Job ID `TAGO00815820GPF001`
- แอปคนขับเป็น Android emulator `emulator-5554` API `http://10.0.2.2:3000`
- ขอบเขตคือหัวข้อ TMS ใน agenda วันที่ 2026-10-05 และ Flow TMS งาน Yard ไม่อยู่ในขอบเขต
- รอบเก่า `TMS29144338` ที่ขึ้น `ids must not be empty` เกิดตอน dialog ยังโหลด ไม่ใช่ผลหลังรอข้อมูลครบ อย่าใช้เลขงานนั้นเป็นหลักฐานบั๊กบันทึกค่าใช้จ่าย

## สิ่งที่ผ่านแล้ว

อย่าไปแก้ส่วนนี้ เว้นแต่รอบทดสอบใหม่พัง

- [x] เข้าสู่ระบบและหน้าแรกได้ หลังกดภายหลังที่ dialog รหัสผ่าน — [01-system-overview.html](01-system-overview.html)
- [x] หน้า master ของบริษัทขนส่ง ยกเว้นตั้งค่าค่าใช้จ่ายซึ่งอยู่นอกชุดทดสอบ TMS — [02-master-data.html](02-master-data.html)
- [x] สร้าง order แล้วมอบหมายให้กิต โชคดี — [03-job-order-assign-driver.html](03-job-order-assign-driver.html)
- [x] บันทึก Expense & Fuel ได้ หน้าพิมพ์โชว์ `TMS29154809` — [04-expense-fuel.html](04-expense-fuel.html) ภาพ [screenshots/04-04-result.png](screenshots/04-04-result.png)
- [x] Job Monitoring แสดงงานสถานะรอเริ่มงาน และหน้า Job Tracking เปิดได้ — [06-job-monitoring-tracking.html](06-job-monitoring-tracking.html)
- [x] Complete Job มีแถวของงานนี้ สถานะ Waiting Start Job และมีปุ่ม Complete — [screenshots/07-02-complete-search.png](screenshots/07-02-complete-search.png)
- [x] หน้า Receipt, Revenue, Operation Report, Management Report, Dashboard Transport, Dashboard Operation เปิดได้ — [07-complete-job-invoice.html](07-complete-job-invoice.html), [08-reports-dashboards.html](08-reports-dashboards.html)
- [x] แอปคนขับ หลัง rebuild และดึงรายการใหม่ ค้น `TMS29154809` เจอทั้งรับตู้และส่งตู้ สถานะรอเริ่มงาน — [screenshots/05-05-search-found.png](screenshots/05-05-search-found.png)
- [x] รับตู้แล้วส่งตู้ของ `TMS29154809` จบทั้งคู่ ตู้ `CSQU3054383` — [screenshots/05-08-pickup-complete.png](screenshots/05-08-pickup-complete.png), [screenshots/05-09-drop-complete.png](screenshots/05-09-drop-complete.png)
- [x] Complete Job ของ `TMS29154809` บันทึกแล้วไม่มีปุ่ม Complete ค้าง — [screenshots/07-17-complete-done.png](screenshots/07-17-complete-done.png)
- [x] สร้าง invoice ของใบนี้ได้ เลข `INV5734262609290101` ยอด 1,712.00 — [screenshots/07-21-invoice-created.png](screenshots/07-21-invoice-created.png)

## วิธีเลือกงานถัดไป

1. เอาหมายเลขต่ำสุดที่ยังเป็น `- [ ]` และหัวข้อ Blocked by ว่างหรือข้อที่อ้างถึงถูกติ๊กแล้ว
2. วงรอบ TMS ของ `TMS29154809` ปิดแล้วตั้งแต่รับตู้จนออก invoice
3. `T1`, `T6` แยกจากโซ่นี้ ทำคู่ขนานได้ `T7` และ `T8` ตรวจซ้ำแล้ว
4. อย่าทดสอบ `/master-setting/expenses` กับบริษัทขนส่ง หน้านี้เป็น tariff ของลานตู้ ผู้ใช้ประเภท TAGO เป็นคนดูแล ต้นทุน TMS ใช้ Cost, Fuel Rate และ Route Price
5. อย่าทำ Complete Job หรือ invoice ซ้ำกับ `TMS29154809` ใบนี้ปิดและออก invoice แล้ว
6. หลังแก้ ให้ทดสอบด้วยเลขงานใหม่ของบริษัทนี้ แล้วอัปเดต HTML ที่ลิงก์ไว้กับช่องติ๊กในไฟล์นี้

## เช็กลิสต์

### T1. Dialog รหัสผ่านครบกำหนดหลังเข้าสู่ระบบ

- [ ] ตัดสินใจว่าจะปิด dialog นี้ตอนเทรนนิ่ง หรือคงไว้แล้วอธิบายให้กดภายหลัง
- ความรุนแรง: ข้อสังเกต
- จุดที่เห็น: backoffice
- หลักฐาน: [01-system-overview.html](01-system-overview.html), [screenshots/01-02-password-expiry.png](screenshots/01-02-password-expiry.png)
- URL: `http://localhost:4200/auth/login`
- อาการ: หลังล็อกอินมี dialog รหัสผ่านครบกำหนด ต้องกดภายหลังก่อนเข้าระบบ
- เสร็จเมื่อ: บัญชีเทรนนิ่งเข้าได้โดยไม่สะดุด หรือ dialog มีทางไปต่อที่ชัด และ HTML ภาพรวมถูกอัปเดต
- Blocked by: ไม่มี

### T8. ช่องค้นหาในงานของฉันพังหลังรับตู้เสร็จ

- [x] ไม่ dispose `jobBookingSearch` กับ `jobHistoryBookingSearch` ใน `onClose` เพราะ `Get.offAllNamed(home)` เรียก `onClose` บน instance ที่หน้าใหม่ยังใช้
- จุดที่แก้: `tago-express-truck-driver-flutter/lib/controller/my_job/my_job_controller.dart`
- ตรวจซ้ำแล้ว: rebuild บน emulator แล้วกลับหน้างานของฉันผ่าน `Get.offAllNamed(home)` ช่องค้นหารับ `REEFER` และกรองรายการได้ ไม่มีแถบแดง ดู [screenshots/05-11-search-after-offall.png](screenshots/05-11-search-after-offall.png)
- Blocked by: ไม่มี

### T6. แอปคนขับบน Chrome ล้มตอนสตาร์ท

- [ ] แก้การอ่าน `Platform.environment` บนเว็บ
- ความรุนแรง: บั๊ก เฉพาะตอนเปิดด้วย Chrome
- จุดที่เห็น: `tago-express-truck-driver-flutter/lib/main.dart` บรรทัด 27
- อาการ: รอบนี้ใช้ Android จึงไม่ได้เจอซ้ำ แต่โค้ดยังเรียก `Platform.environment.containsKey('FLUTTER_TEST')` ซึ่งบนเว็บโยน `Unsupported operation: Platform._environment` ก่อน `runApp`
- เสร็จเมื่อ: `flutter run -d chrome --dart-define=ENV=local` ผ่านหน้า splash ไปถึงหน้า PIN ได้
- Blocked by: ไม่มี

### T7. Aging Report ถูกส่งกลับหน้าแรก

- [x] เมนู `aging-report` อยู่ในชุดเมนูของบริษัท 66e3b50320bedeaaa3292792 แล้ว ล็อกอินใหม่แล้วเปิดหน้าได้
- ความรุนแรง: บั๊ก
- จุดที่เห็น: backoffice
- หลักฐาน: [screenshots/07-22-aging-search.png](screenshots/07-22-aging-search.png), [screenshots/07-23-aging-invoices.png](screenshots/07-23-aging-invoices.png)
- URL: `http://localhost:4200/aging-report`
- อาการเดิม: เซสชันที่ล็อกอินไว้ก่อนได้เมนูนี้ถูก `MenuGrantGuard` ส่งกลับหน้าแรก แฟล็ก `show_aging_report` ไม่ได้คุมหน้านี้
- ผลตรวจซ้ำ: SCGJWD LOGISTICS มียอด 1-30 วัน 1,712.00 และใบ `INV5734262609290101` อยู่ในรายการใบแจ้งหนี้
- Blocked by: ไม่มี
