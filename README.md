# JAYJOD เจจด — v1.2.0

JAYJOD is a static, offline-first expense tracker for two people. No backend, no CDN, no build step.

## Deploy GitHub Pages
1. Extract ZIP and upload all files at repository root (including `.nojekyll`).
2. Settings > Pages > Deploy from a branch > main / root > Save.
3. Open the HTTPS Pages URL. On iOS: Safari > Share > Add to Home Screen. On Android: Chrome menu > Install app / Add to Home screen. Open online once so the service worker can cache assets.
4. For local testing, run `python3 -m http.server 8000` in this directory and visit `http://localhost:8000`. Do not open using file://.

## Usage
- Settings: edit names (initial placeholders เจ / หวาน), payment accounts, owners and card closing days. The initial card days are unset because actual dates are not known.
- Add a transaction with purchase date, account, actual payer and allocation. Equal split gives the first person the extra satang if needed. Custom specifies first person's amount, second gets the remainder. Percent is first person's percent. Units assume equal unit prices; unequal-price items use custom amount.
- Monthly summary uses purchase dates. Card view uses previous closing date + 1 through selected month's closing date; days beyond month's length are clamped. Actual posting date or statement adjustments are not inferred; reconcile with the bank statement manually.
- Settlements apply to the selected calendar month, independently of transfer date. They are excluded from expense totals. No automatic carry-forward across months. An overpayment reverses the remaining balance.
- Excel contains summary, all transactions, and sheets per payment account with data and static total rows. CSV is UTF-8 BOM, one flat table. Both export the selected period. Excel does not include settlement history; JSON backup includes it. No old spreadsheet import in v1.
- Backup regularly. Restore validates format and replaces all records in one database transaction. A download of the previous backup is initiated before replacement; verify a separate backup manually first because browser downloads can be blocked. Data is not encrypted. Avoid shared browser profiles and multiple tabs editing simultaneously.

## Architecture / upgrade
`index.html`: application shell and expense dialog; `style.css`: mobile-first UI; `app.js`: IndexedDB, domain logic and views; `xlsx.js`: native OOXML/ZIP export; `sw.js`: offline cache and controlled update; `manifest.webmanifest` and PNG icons: PWA.
Database `couple_expenses_db` version 1: expenses (date/account indexes), accounts, settlements, settings, metadata. Monetary values are integer satang; IDs are UUIDs. Backup schema 1. Future upgrades must retain the DB name and implement additive `onupgradeneeded` migrations; bump SW cache name and preserve previous backup compatibility explicitly. Do not clear IndexedDB as part of upgrade. Version 1 only supports its own backup schema; future migrations have not yet been implemented. Whole state is loaded into memory and written atomically, suitable for a single user/tab and modest personal datasets, not concurrent writers.

## Verification
JavaScript syntax checked; generated XLSX opened with openpyxl and verified Thai sheet names, numeric values, literal formula-like strings. Browser/device end-to-end and installation not verified here. See TEST.md for acceptance checks.

## v1.1.0 — หมวดหมู่ สินค้าย่อย และรูปใบเสร็จ
- เพิ่ม 18 หมวดหมู่ และยังไม่จัดประเภท; เพิ่ม/แก้ชื่อ/ซ่อนได้
- โหมดแยกรายละเอียด: ราคา จำนวน หมวด แบ่งเงินระดับสินค้า ส่วนลดกระจายตามราคา และตรวจยอดใบเสร็จ
- แนบรูปจากกล้องหรือคลังรูป สูงสุด 5 รูปต่อรายการ; แปลงเป็น JPEG ด้านยาวไม่เกิน 1600 px เก็บใน IndexedDB ภายในรายการ ไม่อัปโหลดออกนอกเครื่อง
- XLSX เพิ่มชีตรายละเอียดและประเภท; CSV เลือกแบบรายการหลักหรือสินค้า รูปจริงอยู่ใน JSON backup ไม่ฝังใน Excel
- อัปเกรด DB schema 1 → 2 โดยคง object stores เดิม เพิ่ม categories; รับ backup v1 และ v2
- ก่อนอัปเกรดให้ Backup JSON จาก v1.0 แล้ว deploy ทับใน repository/path เดิม และกดอัปเดตในตั้งค่า ห้ามล้าง site data
- รูปถ่ายจำนวนมากทำให้ backup และ IndexedDB ใหญ่ขึ้น; ควรเก็บไฟล์สำรองนอกเครื่องเป็นระยะ
- กล้องขึ้นอยู่กับระบบปฏิบัติการ/เบราว์เซอร์; HEIC ที่เบราว์เซอร์อ่านไม่ได้ให้เลือกรูป JPEG แทน
- ข้อจำกัด: ยังไม่มี OCR, สี/ไอคอนหมวดแบบกำหนดเอง, กราฟแนวโน้มหลายเดือน หรือ analytics เชิงลึกใน build นี้

## v1.1.1 — Branding hotfix
- เปลี่ยนชื่อแอปเป็น JAYJOD เจจด, ชื่อ PWA เป็น เจจด และชื่อไฟล์ export/backup เป็น jayjod-*
- คง DB name, schema, manifest id และไอคอนเดิม เพื่อไม่กระทบข้อมูลและการติดตั้งเดิม
- อัปเดต service worker cache; ชื่อบน Home Screen เดิมอาจต้องติดตั้ง shortcut ใหม่ตาม OS

## v1.2.0 — Pastel Blue & Linked Settlement
- Mobile-first layout tuned for ~390 CSS px (iPhone 16e), safe areas, floating add button, refresh/update button, lightweight animations respecting reduced motion, success chime with settings toggle.
- Settlement: select one or multiple expenses in the chosen date/card view, choose transfer direction and amount per expense, supports partial payments. Net balance offsets opposite directions; per-expense status only changes with explicit linked payments.
- Old unlinked transfers remain in the original calendar month and affect its net balance. Link them via the report by allocating the full old amount among matching expenses in the same month and direction; no duplicate payment. Deleting a linked payment reopens the balance. Changing payer/month or reducing debt below payments, and deleting linked expenses, requires removing their linked transfers first.
- Database schema 3; same database name and stores, additive optional expenseId on settlements; restores schemas 1, 2, 3. Backup before deploying to the same URL. Excel includes settlement sheet and transaction-level outstanding amounts; CSV transaction export includes settlement status.
- Audio is synthesized locally and may be muted by iOS/browser policies. Refresh checks the service worker and reloads without clearing data; offline uses cache. No new dependencies.
- Actual iPhone/PWA browser testing remains required; no claim of full end-to-end validation.
