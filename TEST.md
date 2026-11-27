# Acceptance checklist
- [ ] First launch, no expenses, initial accounts visible, edit names.
- [ ] Add 410.00 / equal / first payer: shares 205.00 each, second owes first 205.00.
- [ ] Add 0.01 equal: shares 0.01 / 0.00; add 500 / units 2:3: shares 200 / 300.
- [ ] Add 500 / custom 200: second share 300. Reject custom 501, invalid decimal, empty description.
- [ ] Add 100 / second payer / first bears all: first owes second 100.
- [ ] Save and add next retains date/account/payer; edit/delete and confirm totals.
- [ ] Set card closing day 31, February 2027: range Feb 1–28. Closing day 25: Jan 26–Feb 25.
- [ ] Record settlement, verify remaining balance decreases, expense total unchanged.
- [ ] Export XLSX and CSV; compare amounts/sheets with dashboard, Thai text readable.
- [ ] Backup; add a record; restore backup; verify previous state and old-data backup download. Reject malformed JSON.
- [ ] Install on iOS/Android; reopen offline; add and export; refresh and verify persistence.
- [ ] Verify on small mobile screen: keyboard, no horizontal overflow, no unexpected zoom.

## v1.1.0
- [ ] 7-Eleven 100 บาท: ไข่ 70 หารครึ่ง, น้ำ 20 เจ, น้ำแข็ง 10 หวาน → เจ 55 หวาน 45
- [ ] หมวดของสด 70 เครื่องดื่ม 30 ไม่คิดยอดซ้ำ
- [ ] ส่วนลดท้ายใบเสร็จและยอดสุทธิตรงกัน
- [ ] ถ่ายรูปและเลือกรูปจากคลัง แก้ไข ลบ เปิดดูหลัง reload
- [ ] Backup/Restore รูปและข้อมูล; Restore backup v1
- [ ] อัปเกรดบน URL เดิมโดยไม่ล้างข้อมูล
- [ ] Excel/CSV รายการหลักและสินค้า
- [ ] Offline หลังติดตั้ง service worker
ผลตรวจใน runtime: JS syntax และ unit logic ผ่าน; browser E2E ถูกบล็อกโดย environment (ERR_BLOCKED_BY_ADMINISTRATOR) จึงยังไม่ถือว่าผ่าน UAT.

## v1.1.1 branding checks
- Verify header, browser title, PWA name, exported filenames and existing records after update.
- Existing home-screen shortcuts may retain the old name until reinstalled.

## v1.2.0 acceptance tests
- At 390 CSS px, check no horizontal overflow, safe-area navigation, floating add, keyboard and dialog scrolling.
- Create expense 500 THB (me pays, partner owes 300). Return 120 then 180: verify partial then complete, no change to expense totals. Delete one transfer: balance reopens. Try overpayment and deleting linked expense: must reject.
- Select two expenses and pay in a single action, verify each gets its own link. Opposite payer direction must be selectable separately. Verify calendar and card reports use linked payments even when transfer date is in next month.
- Restore old backup with an unlinked transfer; verify it affects calendar net, not per-item status. Allocate full amount across matching expenses; net must remain unchanged. Invalid links must reject restore.
- Test chime, mute toggle, reduced motion, refresh online/offline, update from prior service worker, export and restore with attachments.
