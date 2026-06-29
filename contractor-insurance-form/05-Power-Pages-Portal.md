# 05 — Power Pages contractor portal (recommended for a branded experience)

A **Power Pages** site gives your contractors a real web page (your branding) to:
enter their details, **upload insurance certificates directly** (no separate link), put in
**expiry dates**, and optionally log back in to see their compliance status.

Unlike Microsoft Forms, **Power Pages file upload works for external users** — that's the
big win. It's backed by **Dataverse** (the database) instead of an Excel/List.

```
 Contractor  ──▶  Power Pages site  ──▶  Dataverse tables  ──▶  Power Automate
 (web form +       (your branding,        (Contractor +          (expiry reminders,
  file upload)      anonymous or login)     Insurance + files)     emails, status)
```

---

## Step 1 — Create the site

1. Go to **make.powerpages.microsoft.com**.
2. **+ Create a site** → pick a blank/starter template → name it
   **"Recycling Parks Contractor Portal"**.
3. Set your logo/colours in **Styling** (top of the design studio) to match your branding.

---

## Step 2 — Build the Dataverse tables (the database)

In **make.powerapps.com → Tables → + New table**, create two tables.

### Table A — `Contractor`
| Column | Type |
|--------|------|
| Contractor Name *(primary)* | Text |
| Services Provided | Multiline text |
| Transport Operator | Choice: Yes / No |
| Contact Person | Text |
| Email | Email |
| Phone | Phone |
| HSE Pre-Qual Completed | Choice: Yes / No |
| CoR Pre-Qual Completed | Choice: Yes / No / N/A |
| Status | Choice: Compliant / Non-Compliant |
| Approved By | Text |
| Submission Date | Date |

### Table B — `Insurance` (related to Contractor — one contractor has many)
| Column | Type |
|--------|------|
| Insurance Type | Choice: Public Liability / Workers Compensation / Commercial Plant & MV |
| Insurer Name | Text |
| Expiry Date | Date |
| Certificate | **File** column (this stores the uploaded PDF/image) |
| Contractor | Lookup → Contractor table |

> Splitting insurances into their own table (rather than 3 columns) means each certificate
> has its **own expiry date and its own file** — cleaner for reminders and renewals. You can
> still show them together on one page (Step 3).

---

## Step 3 — Build the form page (no code)

In the **Power Pages design studio**:

1. **+ Add a page** → name it **"Register / Renew"**.
2. Add a **Form** component → choose the **Contractor** table → mode **Insert** (new
   submissions). Drag in the fields: Name, Services, Transport Operator, Contact, Email, Phone.
3. **Conditional CoR section:** use the form's **Advanced → JavaScript** (snippet in file
   `06`) to show the CoR question only when *Transport Operator = Yes*.
4. For the **insurances + file uploads**, add a **Multistep form** or a **Subgrid/related
   list** bound to the `Insurance` table so the contractor can add up to three insurance
   rows, each with **Insurer**, **Expiry Date**, and the **Certificate file**.
   - The **File** column renders as an upload control automatically — this is the part that
     works for external users where Forms fails.
5. Add a **Submit** button → on submit, set redirect to a **"Thank you"** page.

---

## Step 4 — Who can access it (external contractors)

Power Pages → **Security → Set up**. Two practical options:

| Option | How it works | Best when |
|--------|--------------|-----------|
| **Anonymous form** | Page set to allow **Anonymous users** to create Contractor + Insurance records. No login at all. | Quickest. Use if you just want submissions in, like a public form but with working uploads. |
| **Invite + login** | Contractors get an **invitation code** by email, set a password, and can log back in to view/update their insurances and see Status. | Best long-term — contractors self-renew and see their own compliance. |

Set **Table Permissions** (Security → Table permissions) so:
- Anonymous/Authenticated can **Create** Contractor + Insurance + file.
- (If login) Authenticated can **Read/Write their own** records only (scope = Contact/Account).

---

## Step 5 — Reminders & status (reuse Flow #2)

The **daily expiry reminder flow** from file `03` works the same — just point
*Get items* at the **Dataverse `Insurance` table** instead of the SharePoint list, and use
the `Expiry Date` column. It emails contractors before expiry and flips the parent
Contractor's **Status** to Non-Compliant when a certificate lapses.

---

## Step 6 — Your internal view

You (HSEQ/Operations) view everything in a **model-driven app** or directly in the Dataverse
table view — filter by *Status = Non-Compliant* or *Expiry within 30 days*. This replaces the
manual spreadsheet register, but you can still **export to Excel** anytime to keep the
familiar register format.

---

## Licensing note

Power Pages is licensed per site (authenticated users billed per-user/month, or anonymous
page-views in capacity packs). Check **make.powerpages.microsoft.com → site → Set up →
Pricing** for current rates before going live. For a low-volume contractor list, the
**Forms + Request-files** approach (files `01`–`04`) is the zero-extra-cost fallback if the
Power Pages licensing doesn't suit.

See file `06` for the page HTML/Liquid + JavaScript snippets.
