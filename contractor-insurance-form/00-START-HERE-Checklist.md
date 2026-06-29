# 00 — START HERE: build checklist

Do these in order. ~1.5–2 hours total, no code. Tick each box as you go.

## Step 1 — Database (Dataverse tables) · ~15 min · ref: `05` Step 2
- [ ] make.powerapps.com → **Tables → + New table** → create **`Contractor`** (Name, Services,
      Transport Operator, Contact, Email, Phone, HSE Pre-Qual, CoR Pre-Qual, Status, Approved By, Submission Date)
- [ ] Create **`Insurance`** table (Insurance Type, Insurer Name, Expiry Date, **Certificate = File column**,
      Lookup → Contractor)

## Step 2 — Power Pages portal · ~20 min · ref: `05` Steps 1,3 + `06`
- [ ] make.powerpages.microsoft.com → **+ Create a site** → name "Recycling Parks Contractor Portal"
- [ ] Set logo + colours (Styling)
- [ ] Add page "Register / Renew" → **Form** bound to **Contractor** (Insert mode)
- [ ] Add subgrid/related list bound to **Insurance** (insurer, expiry date, **certificate upload**)
- [ ] Add Submit button → "Thank you" page

## Step 3 — External access · ~10 min · ref: `05` Step 4
- [ ] Security → **Table permissions**: allow Anonymous (or invite-code) **Create** on Contractor + Insurance + file
- [ ] Preview + do a test submission

## Step 4 — Cloud Excel · ~5 min · ref: `07`
- [ ] Upload **Contractor_Register_AUTO-POPULATE.xlsx** to SharePoint/OneDrive for Business

## Step 5 — Auto-populate flow · ~15 min · ref: `07`
- [ ] make.powerautomate.com → Automated cloud flow
- [ ] Trigger: **Dataverse — When a row is added** (Contractor)
- [ ] **Excel Online (Business) — Add a row into a table** → file → table **ContractorRegister** → map fields
- [ ] Save + Test → confirm row appears in `Register (Auto)` sheet

## Step 6 — Reminders + manager escalation · ~20 min · ref: `03` Flow #2
- [ ] Scheduled cloud flow → **every 1 day** at 7 AM
- [ ] Compose **ManagerEmail = <manager email>**
- [ ] Loop expiry dates → branches:
      `=30` → manager 30-day notice ·
      `≤10 and ≥0` → manager **daily** until expiry ·
      `<0` → manager EXPIRED + Status = Non-Compliant

## Step 7 — Go live · ~10 min · ref: `04`
- [ ] Power Pages → **Sync → Publish**
- [ ] Email contractors the portal link (template in `04`)

---
**Reference files:** `05`/`06` portal · `07` auto-populate Excel · `03` reminders+escalation · `01`–`04` Forms fallback.
