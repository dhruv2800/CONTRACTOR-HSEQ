# 07 — Auto-populate your XLSX register ⭐

This is the flow that writes every Power Pages submission **straight into your Excel
register file** — so the spreadsheet you already use updates itself.

---

## First: use the write-ready file

Power Automate's Excel connector can only add rows to a **proper Excel Table with a single
header row**. Your original register has a **two-tier header** (a "Contractor Details /
Pre-Qualifications / Insurances" group row sitting above the real column row), which the
connector **cannot** write to.

So this folder includes **`Contractor_Register_AUTO-POPULATE.xlsx`** — it's your original
workbook with:

- Your original **`Contractor Management`** and **`Data`** sheets kept intact (so your
  formatted register/dropdowns still exist), **plus**
- A new first sheet **`Register (Auto)`** containing a real Excel Table named
  **`ContractorRegister`** with these 20 single-row-header columns:

```
Contractor Name | Services Provided | Transport Operator | Contact | Email | Phone | PHB |
HSE Pre-Qual Completed | CoR Pre-Qual Completed | Submission Date |
Public Liability Insurer | Public Liability Expiry |
Workers Comp Insurer | Workers Comp Expiry |
Plant & MV Insurer | Plant & MV Expiry |
Status | Docs Uploaded | Approved By | Certificates Link
```

### Do this once
1. Upload **`Contractor_Register_AUTO-POPULATE.xlsx`** to **SharePoint** or **OneDrive for
   Business** (the file must live in the cloud, not your PC, for the flow to reach it).
2. *(Optional)* Delete the empty placeholder row in `Register (Auto)` — leave the header.

> Want it to land in your **original** file instead? Open your original file in Excel,
> select your data area, **Insert → Table** (with single-row headers), name it
> `ContractorRegister` via **Table Design → Table Name**, and point the flow there. The
> two-tier header is why we made the clean sheet for you.

---

## The flow — "Power Pages submission → Excel row"

Build at **make.powerautomate.com**.

**Trigger:** *Microsoft Dataverse — When a row is added* → Table = **Contractor**.

**Action 1 (get the insurances):** *Dataverse — List rows* → Table = **Insurance**,
Filter rows = `_rp_contractor_value eq <triggerOutputs Contractor id>`.
(This pulls that contractor's up-to-three insurance records so you can flatten them into
one spreadsheet row.)

> Tip: to map the three insurer/expiry pairs into fixed columns, add a **Filter array** or
> three small **Compose** actions that pick the row where Insurance Type = "Public
> Liability", etc. Or, if you prefer one Excel row **per insurance**, skip List rows and
> just trigger on the **Insurance** table instead.

**Action 2:** *Excel Online (Business) — Add a row into a table*
- Location = the SharePoint/OneDrive site where you uploaded the file
- Document Library / File = **`Contractor_Register_AUTO-POPULATE.xlsx`**
- Table = **`ContractorRegister`**
- Map each column:

| Excel column | Value (Dataverse dynamic content) |
|--------------|-----------------------------------|
| Contractor Name | Contractor Name |
| Services Provided | Services Provided |
| Transport Operator | Transport Operator |
| Contact | Contact Person |
| Email | Email |
| Phone | Phone |
| HSE Pre-Qual Completed | HSE Pre-Qual Completed |
| CoR Pre-Qual Completed | CoR Pre-Qual Completed |
| Submission Date | `formatDateTime(triggerOutputs()?['body/createdon'],'yyyy-MM-dd')` |
| Public Liability Insurer | from the PL insurance row |
| Public Liability Expiry | `formatDateTime(<PL expiry>,'yyyy-MM-dd')` |
| Workers Comp Insurer | from the WC insurance row |
| Workers Comp Expiry | `formatDateTime(<WC expiry>,'yyyy-MM-dd')` |
| Plant & MV Insurer | from the Plant row (blank if none) |
| Plant & MV Expiry | `formatDateTime(<Plant expiry>,'yyyy-MM-dd')` |
| Status | leave blank — set by the expiry flow |
| Docs Uploaded | `Yes` (they uploaded via the portal) |
| Approved By | leave blank |
| Certificates Link | link to the Dataverse record / portal |

**Save and test:** submit a test entry on the portal → a new row should appear in
`Register (Auto)` within seconds.

---

## Keeping the spreadsheet as the single source

- **New contractor** → new row (the flow above).
- **Renewal / updated expiry** → either let it add a fresh row (full history), **or**
  swap *Add a row* for **"Update a row"** keyed on **Contractor Name** so the existing row
  is overwritten in place. Use *Update* if you want one row per contractor.
- The **daily expiry flow** (file `03`, Flow #2) can read this same Excel table and set the
  **Status** column to Compliant / Non-Compliant — so the spreadsheet stays self-maintaining.

---

## If you're NOT using Power Pages

The exact same **Action 2** works from the **Microsoft Forms** flow (file `03`, Flow #1) —
just take the dynamic content from *Get response details* instead of Dataverse. So whichever
front end you choose, the spreadsheet auto-populates the same way.
