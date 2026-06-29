# 04 — Email to send contractors

Replace the `<<...>>` placeholders before sending. You can paste this into Outlook, or
have Power Automate send it in bulk from your register.

---

**Subject:** Action required — Insurance & contractor details for Recycling Parks

---

Hi <<Contractor contact name>>,

To keep your company approved to work on our Recycling Parks sites, we need your current
insurance details and certificates on file. This takes about **5 minutes**.

**Step 1 — Complete the short form** (your details + insurance expiry dates):
👉 <<PASTE MICROSOFT FORM LINK>>

**Step 2 — Upload your Certificates of Currency** using this secure link
(no login needed):
👉 <<PASTE SHAREPOINT "REQUEST FILES" LINK>>

Please upload a current **Certificate of Currency** for each that applies to you, and
**name the files** like this so we can match them:

- `<<CompanyName>>_PublicLiability.pdf`
- `<<CompanyName>>_WorkersComp.pdf`
- `<<CompanyName>>_PlantAndMV.pdf` *(only if you operate plant or vehicles on site)*

We'll automatically remind you before any of your insurances expire so you never lose
approved status.

Please complete both steps by **<<due date>>**. If anything's unclear, just reply to
this email.

Thanks,
<<Your name>>
<<Role / HSEQ>>, Recycling Parks
<<Phone>>

---

### Bulk-send option

To send this to every contractor in your register automatically:
**Power Automate → Recurrence or manual trigger → Get items (Register) → Apply to each →
Send an email (V2)** using the `Email` column and merging `Contractor Name` into the body.
Add a "Reminder sent" date column so you don't email the same contractor twice in a week.
