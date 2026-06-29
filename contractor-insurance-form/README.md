# Contractor Insurance & Pre-Qualification Form — Microsoft 365 Setup

A ready-to-build solution for collecting contractor **insurance certificates**, their
**expiry dates**, and pre-qualification details — using only the Microsoft apps you
already have (**Forms, Lists, Power Automate, Power Pages**).

Built to feed straight into your existing *Recycling Parks Contractor Management Register*.

---

## ⚠️ The one thing that decides your whole setup

**Microsoft Forms file-upload questions do NOT work for external people.**

If you add a "file upload" question to a Form and set it to *"Anyone can respond"*,
the upload question is **hidden / disabled** — Microsoft forces the person to sign in
to *your* tenant before they can attach a file. Your contractors don't have logins,
so they'd be stuck.

There are three clean ways around this. Pick one:

| # | Approach | External upload works? | Effort | Best for |
|---|----------|------------------------|--------|----------|
| **A** | **Microsoft Form** (details + expiry dates) **+ SharePoint "Request files" link** (certificates) | ✅ Yes — link needs no login | 🟢 Low | **Recommended.** Fastest, no extra licensing. |
| **B** | **Power Pages** external portal (login + upload + dates in one) | ✅ Yes | 🟠 Medium | A polished, branded contractor portal with a status dashboard. |
| **C** | **SharePoint List** shared externally with attachment column | ⚠️ Needs guest access per contractor | 🟠 Medium | Everything living in SharePoint with versioning. |

This package is built around **Approach A** (recommended), with notes for B and C.

---

## How Approach A works (the recommended flow)

```
                                                 ┌─────────────────────────┐
   Contractor                                    │  YOU (HSEQ / Operations)│
   ───────────                                   └─────────────────────────┘
        │                                                     ▲
        │ 1. Fills Microsoft Form                             │
        │    (name, email, services, transport Y/N,           │
        │     3 × insurance expiry dates + insurer)           │
        ▼                                                     │
   ┌──────────────┐     2. Form submit triggers      ┌────────────────────┐
   │ Microsoft    │ ───────────────────────────────▶ │  Power Automate    │
   │ Form         │                                  │  Flow #1           │
   └──────────────┘                                  └─────────┬──────────┘
        │                                                      │ writes a row to
        │ 3. Clicks "Upload your certificates"                 ▼
        │    link on the confirmation page          ┌────────────────────────┐
        ▼                                           │ Contractor Register    │
   ┌──────────────────────────┐                     │ (Microsoft List / Excel)│
   │ SharePoint "Request files"│ ──── files land ──▶│  + /Insurances doc lib │
   │  folder (no login needed) │                    └─────────┬──────────────┘
   └──────────────────────────┘                              │
                                                              │ Flow #2 (daily) checks
                                                              ▼ expiry dates
                                              ┌────────────────────────────────┐
                                              │ Reminder emails @ 30/14/7 days  │
                                              │ + auto-flip Status→Non-Compliant│
                                              └────────────────────────────────┘
```

---

## What's in this folder

| File | What it gives you |
|------|-------------------|
| `01-Microsoft-Form-Questions.md` | The exact questions to paste into Microsoft Forms, in order, with field types and settings. Maps 1:1 to your register. |
| `02-SharePoint-List-and-RequestFiles.md` | How to turn your register into a Microsoft List, build the `/Insurances` document library, and create the no-login **Request files** upload link. |
| `03-Power-Automate-Flows.md` | Step-by-step build for **Flow #1** (form → register row + email) and **Flow #2** (daily expiry reminders, **manager escalation**, status auto-update). |
| `04-Contractor-Email-Template.md` | The email you send contractors, with the Form link and upload link placeholders. |
| `05-Power-Pages-Portal.md` | **Power Pages** branded contractor portal — login/anonymous, direct certificate upload, backed by Dataverse. |
| `06-Power-Pages-Code-Snippets.md` | Optional JavaScript/Liquid snippets for the Power Pages portal (conditional CoR section, date checks, thank-you page). |
| `07-Auto-Populate-Excel.md` | **The flow that writes every submission straight into your XLSX register**, plus the write-ready workbook. |
| `Contractor_Register_AUTO-POPULATE.xlsx` | Your original workbook + a new `Register (Auto)` sheet with a proper Excel **Table** (`ContractorRegister`) that Power Automate can append rows to. |
| `register-fields.md` | The field map extracted from your spreadsheet, so nothing gets lost in translation. |

## Two ways to build it

- **Power Pages portal (your preference)** → files `05`, `06`, then `07` to auto-fill the Excel + `03` Flow #2 for reminders.
- **Microsoft Forms + Request-files (zero-cost fallback)** → files `01`–`04`, then `07`'s flow also works from Forms.

---

## Quick start (about 60–90 min, no code)

1. **List** — Import your existing register into a Microsoft List (file `02`).
2. **Library** — Add an `Insurances` document library + create the **Request files** link (file `02`).
3. **Form** — Build the Microsoft Form from file `01`, set to *Anyone can respond*.
4. **Flow #1** — Build the submit flow so each response becomes a register row (file `03`).
5. **Flow #2** — Build the daily expiry-reminder flow (file `03`).
6. **Send** — Email contractors using the template in file `04`.

---

## Why this matches your business

Your register has three insurance Certificates of Currency with separate expiry dates
(**Commercial Plant & MV**, **Public Liability**, **Workers Compensation**), a
**Transport Operator** flag that gates the **CoR pre-qualification**, and a
**Compliant / Non-Compliant** status. The Form and flows below reproduce exactly that
logic — including showing the CoR question only to transport operators, and marking a
contractor **Non-Compliant** automatically the day any certificate lapses.
