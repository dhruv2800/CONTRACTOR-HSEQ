# 02 — Register as a Microsoft List + the no-login upload link

This file sets up where the data and the certificate files live.

---

## Part A — Turn your register into a Microsoft List

A **Microsoft List** (same thing as a SharePoint list) is better than the raw Excel file
for this because Power Automate can read/write it cleanly, it has proper date columns for
expiry logic, and it gives you filtered views (e.g. "Expiring this month").

> Prefer to keep using the **Excel file**? You can — Flow #1/#2 work with an Excel table
> in SharePoint/OneDrive too. The List is just more robust. Steps below are for the List.

1. Go to your team SharePoint site → **New → List → From Excel** → upload
   `Recycling_Parks_Contractor_Management_Register.xlsx`.
2. Map the columns and set these **types** (important for the flows):

| List column | Type |
|-------------|------|
| Contractor Name | Single line of text *(use as Title)* |
| Services Provided | Multiple lines of text |
| Transport Operator | Choice: Yes / No |
| Contact | Single line of text |
| Email | Single line of text |
| PHB | Single line of text |
| HSE Pre-Qual Completed | Choice: Yes / No |
| CoR Pre-Qual Completed | Choice: Yes / No / N/A |
| Submission Date | **Date** |
| Public Liability Insurer | Single line of text |
| Public Liability Expiry | **Date** |
| Workers Comp Insurer | Single line of text |
| Workers Comp Expiry | **Date** |
| Plant & MV Insurer | Single line of text |
| Plant & MV Expiry | **Date** |
| Status | Choice: Compliant / Non-Compliant |
| Docs Uploaded | Choice: Yes / No |
| Approved By | Single line of text |
| Certificates Folder | Hyperlink *(link to their upload folder — optional)* |

3. Create handy **views**:
   - **Expiring ≤30 days** — filter where any expiry date is within 30 days of today.
   - **Non-Compliant** — Status = Non-Compliant.
   - **Transport Operators** — Transport Operator = Yes.

---

## Part B — Document library for the certificates

1. On the same SharePoint site → **New → Document library** → name it **`Insurances`**.
2. Inside, you can create a folder per contractor (the flow can do this, or do it manually).

---

## Part C — The "Request files" link (lets external contractors upload — NO login) ⭐

This is the piece that solves the external-upload problem.

1. Open the **`Insurances`** document library (or a specific folder inside it).
2. Click **Request files** (top toolbar — in OneDrive it's the **Request files** button;
   in SharePoint document libraries it appears once the feature is enabled by your admin).
3. Type what you're requesting, e.g. *"Insurance Certificates of Currency"*.
4. Click **Next → Done**. You get a link like
   `https://yourtenant.sharepoint.com/.../request/...`.
5. **Anyone with that link can upload** files **without signing in**. They cannot see,
   edit, or download anything already in the folder — upload only. Perfect for contractors.

> **If you don't see "Request files":** it must be enabled by a SharePoint/OneDrive admin
> (SharePoint admin center → Policies → Sharing → *"Let people request files"*). If your
> org won't enable it, fall back to **Approach B (Power Pages)** or create a simple
> **"Anyone" upload link** to a folder via **Share → Anyone with the link → ✏️ can edit**.

6. **Paste this link** into:
   - The Microsoft Form **thank-you message** (file `01`, Q17 confirmation), and
   - The **contractor email** (file `04`).

---

## Part D — Folder naming so files match the register

Ask contractors (in the email + form confirmation) to name files:

```
CompanyName_PublicLiability.pdf
CompanyName_WorkersComp.pdf
CompanyName_PlantAndMV.pdf
```

That way, even though the upload is separate from the form response, you (and the flow's
notification email) can match the certificate to the contractor by company name.

---

## Optional upgrade — one folder per contractor automatically

If you want each contractor's certs auto-sorted into their own folder, **Flow #1**
(file `03`) can create `/Insurances/<Contractor Name>/` on form submit and put that
folder's own Request-files link into the register row. Steps are in file `03`, optional
section.
