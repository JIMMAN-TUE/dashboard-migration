# HANDOFF — Power BI: Databricks → Tencent DLC

**The only shared memory is this Git repo** (branch `main`). Read this file first, update it last, in the same commit as every change.
Switching Claude accounts: [docs/ACCOUNT_SWITCH_PROTOCOL.md](docs/ACCOUNT_SWITCH_PROTOCOL.md). Run log: [LOG.md](LOG.md).
Separate from the KAM dashboard repo `JIMMAN-TUE/kam-potential-realization`: never edit that repo from this work.

**Start a chat:** `/pbi <request>` (`.claude/commands/pbi.md`), or `/pbi` alone to continue from *Next step*. Save + release: `/handover`.
Chat names: `PBI-A` (account 1), `PBI-B` (account 2), `PBI-C` (account 3).

**Working now:** nobody

## Goal (owner, 20 Oct 2026)

Move the owner's Power BI report(s) from Databricks to Tencent DLC by editing the **PBIP / TMDL** files directly
(owner: "แก้ใน tmdl เลยได้ปะ" → yes). Owner will send the file(s). On 27 Oct 2026 the work moved to this
separate repo for a new Claude account (started in the CP AXTRA DASHBOARD project, thread "Power BI Databricks to Tencent").

## Status (27 Oct 2026)

- No report file received yet. **Nothing edited yet.** Only best practice given to the owner (below).
- Open questions to the owner (asked 20 Oct, not answered yet):
  1. Atomic-level (click by click, line by line) explanation wanted? (owner preference: always ask this)
  2. Tencent view names equal to the Databricks table names for every table, or is there a mapping?
  3. Import or DirectQuery?
- Token estimate given: 40k–100k for 10–20 tables, +15k–30k if atomic level (estimate, not measured).

## Target M (from the owner, 20 Oct 2026)

```m
let
    Source = DLC.Catalogs("lb-n8mykzik-nasfor3z712xpvgb.clb.sg-tencentclb.com", "10009", "makro-analysis",
        "default-rg-k0c672y3mh", "qcs::cam::uin/200045787008:roleName/powerbi_usage", "dlc_connect",
        [Catalog="makro_shared_biz_catalog", Database="userdb", Implementation="1.0"]),
    #"Filtered Rows" = Table.SelectRows(Source, each ([Item] = "vw_pbi_b2b_overall_lc_ch_cus_it")),
    vw_pbi_b2b_overall_lc_ch_cus_it = #"Filtered Rows"{[Item="vw_pbi_b2b_overall_lc_ch_cus_it",Schema="userdb",Catalog="makro_shared_biz_catalog"]}[Data]
in
    vw_pbi_b2b_overall_lc_ch_cus_it
```

Host, port, workgroup, role ARN are not secrets. Keys / passwords never go in files or chat: they live only in
Power BI Data source settings / the gateway.

## Plan (agreed best practice, do in order)

1. Owner converts `.pbix` → `.pbip` (Power BI Desktop: File > Save as > Power BI Project; TMDL format on in
   Options > Preview features). `.pbix` cannot be edited as text.
2. Put each PBIP in `reports/<report name>/` on `main` (Claude commits it; owner never edits code). Commit the untouched
   original first (commit message `original: <report>`) so every later change is a readable diff and can be reverted.
   Never commit `cache.abf`, `localSettings.json` or `.pbix` (data / local state; see `.gitignore`).
3. Inventory: every `definition/tables/*.tmdl` `partition … = m` source → table list with the Databricks catalog/schema/table
   and the matching DLC view. Write it in *Table map* below before editing.
4. Parameters once in `definition/expressions.tmdl`: `DLC_Host`, `DLC_Port`, `DLC_Workgroup`, `DLC_ResourceGroup`,
   `DLC_Role`, `DLC_Catalog`, `DLC_Database` (`meta [IsParameterQuery=true, Type="Text", IsParameterQueryRequired=true]`).
5. In each partition replace only the `Databricks.Catalogs(...)` source + navigation steps with the DLC pattern above using the
   parameters; keep every later step (renames, type changes, filters) unchanged.
6. Column check: names (case), types (decimal/int, date/timestamp) of each DLC view vs the Databricks table. A renamed column
   breaks measures/visuals: fix with a rename step, never by editing measures.
7. Owner opens in Desktop (closed while files are edited), Refresh, sends errors back.
8. Totals check before publish: row count + main sales measure per table, Databricks file vs DLC file (owner's numbers).
   Databricks may be unavailable now (Tencent only since 23 Oct): then compare with the last published report values.
9. Service refresh: `DLC` is most likely a Tencent custom connector (inferred, not verified) → needs an on-premises gateway with the
   connector installed. Owner confirms with IT.

## Table map

| Report | Table (TMDL) | Databricks source | DLC view | Columns checked | Totals equal |
|---|---|---|---|---|---|
| | (fill after the files arrive) | | | | |

## Checklist

- [x] Repo, handoff, protocol, `/pbi` + `/handover` commands (27 Oct 2026)
- [ ] PBIP received; original committed under `reports/`
- [ ] Table map filled
- [ ] Parameters added
- [ ] All partitions switched to DLC
- [ ] Column names/types checked
- [ ] Owner refresh in Desktop OK
- [ ] Totals equal
- [ ] Gateway / Service refresh OK

## Problems

- none yet

## Next step

Wait for the owner's PBIP (zip) and answers to the 3 open questions, then steps 2–3 of the plan.

## Prompt for the next account

```text
ชื่อแชทนี้ PBI-<A|B|C>. repo JIMMAN-TUE/dashboard-migration branch main. ทำตาม docs/ACCOUNT_SWITCH_PROTOCOL.md:
pull, อ่าน HANDOFF.md, จองล็อก Working now แล้ว push ทันที, ทำต่อจาก Next step, อัปเดต HANDOFF + LOG ใน commit เดียวกัน, ปลดล็อก, push.
ไฟล์ PBIP: <แนบ zip>  คำตอบ 3 ข้อ: <atomic level? / ชื่อ view ตรงไหม / Import หรือ DirectQuery>
```
