---
description: ทำงานแปลง Power BI (PBIP/TMDL) จาก Databricks ไป Tencent DLC ต่อจาก HANDOFF.md (จอง, ทำ, อัปเดต, commit)
argument-hint: <สิ่งที่ต้องการ หรือผลจาก Power BI Desktop หรือเว้นว่าง = ทำต่อจากขั้นต่อไป>
---
งานแปลง Power BI ใน repo นี้ (repo dashboard-migration) อ่าน `HANDOFF.md` ก่อน แล้วทำตาม:
ตาม docs/ACCOUNT_SWITCH_PROTOCOL.md: `git pull --rebase` บน `main` · ดู *Working now* (ถ้าไม่ใช่ `nobody` ให้ถามฉันก่อนแก้) ·
จองด้วยชื่อแชตนี้แล้ว push ทันที · ทำงานตาม *Plan* · อัปเดต Table map / checklist / ปัญหา / *Next step* ใน commit เดียวกัน ·
ตั้ง *Working now* = `nobody` · เพิ่มหนึ่งบรรทัดใน LOG.md · commit + push
ห้ามเก็บ credential ในไฟล์หรือแชต ตอบภาษาไทยสั้น ๆ ก่อนสร้างงานใหญ่บอก token โดยประมาณ

งาน: $ARGUMENTS

ถ้าเว้นว่าง ให้ทำต่อจาก *Next step* ในไฟล์
