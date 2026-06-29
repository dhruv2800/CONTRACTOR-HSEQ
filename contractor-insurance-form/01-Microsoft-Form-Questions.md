# 01 — Microsoft Form: questions to build

Go to **forms.office.com → New Form**. Name it:
**"Recycling Parks — Contractor Insurance & Pre-Qualification"**.

Add a description:
> Please complete this form to register/renew as an approved contractor. You'll be
> asked for your insurance Certificate of Currency **expiry dates** here, and given a
> secure link to **upload the certificates** at the end. Takes ~5 minutes.

Then add the questions below **in this order**. The "Required" and "Branching" columns matter.

---

## Section 1 — Contractor Details

| # | Question | Type | Required | Settings |
|---|----------|------|----------|----------|
| 1 | Contractor / Company name | Text (short) | ✅ | — |
| 2 | Services you provide | Text (long) | ✅ | "e.g. Mechanical services and welding" |
| 3 | Contact person name | Text (short) | ✅ | — |
| 4 | Contact email | Text (short) | ✅ | Turn on **Restrictions → Email** so it validates a real address. This is also the address reminder emails go to. |
| 5 | Contact phone | Text (short) | ✅ | — |
| 6 | Are you a Transport Operator? | Choice | ✅ | Options: **Yes**, **No**. Turn ON **"Add branching"** (see Section 3). |

---

## Section 2 — Insurances (Certificates of Currency)

Add a **Section header**: "Insurance Certificates of Currency".
Description: *"Enter the expiry date shown on each Certificate of Currency. You'll upload
the certificate files via a secure link at the end."*

| # | Question | Type | Required | Settings |
|---|----------|------|----------|----------|
| 7 | Public Liability — insurer name | Text (short) | ✅ | — |
| 8 | Public Liability — COC expiry date | **Date** | ✅ | — |
| 9 | Workers Compensation — insurer name | Text (short) | ✅ | — |
| 10 | Workers Compensation — COC expiry date | **Date** | ✅ | — |
| 11 | Do you operate commercial plant or motor vehicles on site? | Choice (Yes/No) | ✅ | Branch: **Yes → Q12/13**, **No → skip** |
| 12 | Commercial Plant & MV — insurer name | Text (short) | ❌ | Only shown if Q11 = Yes |
| 13 | Commercial Plant & MV — COC expiry date | **Date** | ❌ | Only shown if Q11 = Yes |

> **Why dates here but files later?** Date questions work fine for external/anonymous
> respondents. File-upload questions do **not** (Microsoft forces sign-in). So dates go in
> the Form; the certificate **files** are collected via the SharePoint *Request files*
> link (see file `02`) shown on the confirmation screen.

---

## Section 3 — Chain of Responsibility (Transport Operators only)

This whole section should only appear if **Q6 = Yes**.
On Q6, click **··· → Add branching**, and set **Yes → go to this section**,
**No → skip to "Declaration"**.

Section header: "Chain of Responsibility (CoR)".

| # | Question | Type | Required | Settings |
|---|----------|------|----------|----------|
| 14 | Do you have a documented Chain of Responsibility (CoR) management system? | Choice | ❌ | Options: Yes / No / N/A |
| 15 | Brief description of your CoR controls | Text (long) | ❌ | — |

---

## Section 4 — Declaration & upload

Section header: "Declaration".

| # | Question | Type | Required | Settings |
|---|----------|------|----------|----------|
| 16 | I confirm the information above is true and the insurances are current. | Choice | ✅ | Single option: **"I agree"** (acts as a checkbox) |
| 17 | Full name of person completing this form | Text (short) | ✅ | Acts as signature |

**Confirmation message** (Form **Settings → "Customise thank you message"**):
> ✅ Thanks! Your details are recorded. **Final step — upload your certificates here:**
> `<<PASTE YOUR SHAREPOINT "REQUEST FILES" LINK FROM FILE 02>>`
> Please name each file like: *CompanyName_PublicLiability.pdf*

---

## Form settings (top-right ··· → Settings)

- **Who can fill out this form:** **Anyone can respond** ← required for external contractors.
- ✅ "One response per person" → **OFF** (they have no login).
- ✅ "Receive email notification of each response" → ON (optional, handy).
- Customise the thank-you message as above.

---

## Tip: pre-fill / track who you sent it to

When you send the link, you can't auto-identify the contractor (anonymous form), so
**Q1 (company name) + Q4 (email)** are how Flow #1 matches the response to a register row.
Make both required (already set above).
